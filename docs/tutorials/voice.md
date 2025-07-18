---
title: Voice
description: A guide on setting up voice chat in your game.
icon: material/account-voice
---

# Tutorials - Voice
:material-badge-account-horizontal: _By Gramps_

---

In case you want to Steam's Voice functionality in your game, we might as well cover that too! This example is based partialy on this [Github repo for networked voice chat in Godot](https://github.com/ikbencasdoei/godot-voip/){ target="\_blank" } and Valve's SpaceWar example. There are additional ideas, details, and such from users **Punny** and **ynot01**.

??? guide "Relevant GodotSteam classes and functions"
	* [Friends class](../classes/friends.md)
		* [setInGameVoiceSpeaking()](../classes/friends.md#setingamevoicespeaking)
	* [User class](../classes/user.md)
		* [decompressVoice()](../classes/user.md#decompressvoice)
		* [getAvailableVoice()](../classes/user.md#getavailablevoice)
		* [getVoice()](../classes/user.md#getvoice)
		* [getVoiceOptimalSampleRate()](../classes/user.md#getvoiceoptimalsamplerate)
		* [startVoiceRecording()](../classes/user.md#startvoicerecording)
		* [stopVoiceRecording()](../classes/user.md#stopvoicerecording)

!!! warning "Note"
	You may want to [double-check our Initialization tutorial](initializing.md) to set up initialization and callbacks functionality if you haven't done so already.

{==
## :material-file-document-check: Preparations
==}

First we will set up a bunch of variables that will get used later on.

=== "Godot 2.x, 3.x"

	```gdscript
	var current_sample_rate: int = 48000
	var has_loopback: bool = false
	var local_playback: AudioStreamGeneratorPlayback = null
	var local_voice_buffer: PoolByteArray = PoolByteArray()
	var network_playback: AudioStreamGeneratorPlayback = null
	var network_voice_buffer: PoolByteArray = PoolByteArray()
	var packet_read_limit: int = 5
	```

=== "Godot 4.x"

	```gdscript
	var current_sample_rate: int = 48000
	var has_loopback: bool = false
	var local_playback: AudioStreamGeneratorPlayback = null
	var local_voice_buffer: PoolByteArray = PackedByteArray()
	var network_playback: AudioStreamGeneratorPlayback = null
	var network_voice_buffer: PoolByteArray = PackedByteArray()
	var packet_read_limit: int = 5
	```

A few quick things to mention, `has_loopback` will toggle whether or not you can hear yourself in voice chat. While not great in-game, it is handy when testing. You also notice `local_` / `network_` prefixed variables; these are to house data on your end and someone else's. In theory, the `network_` will cover all audio output from the network / Steam (anything that isn't you).

Also, somewhere in the root of our test, we need to create two `AudioStreamPlayer` nodes. One named **Local** and one named **Network** which we can use as `$Local` and `$Network` respectively.

In order to add the voice data to the audio stream, we need to set up the `AudioStreamGeneratorPlayback`. Make sure the `AudioStreamPlayer` nodes both have and `AudioStreamGenerator` as their streams. Set the stream's `buffer_length` to something like 0.1:

```gdscript
func _ready() -> void:
    $Local.stream.mix_rate = current_sample_rate
    $Local.play()
    local_playback = $Local.get_stream_playback()

    $Network.stream.mix_rate = current_sample_rate
    $Network.play()
    network_playback = $Network.get_stream_playback()
```

{==
## Getting Voice Data
==}

Also, in our `_process()` function we will add the call to our `check_for_voice()` function:

```gdscript
func _process(_delta: float) -> void:
	check_for_voice()
```

This function basically just looks to see if there is voice data from Steam available then gets and sends it off to be processed.

```gdscript
func check_for_voice() -> void:
	var available_voice: Dictionary = Steam.getAvailableVoice()

	# Seems there is voice data
	if available_voice['result'] == Steam.VOICE_RESULT_OK and available_voice['buffer'] > 0:
		# Valve's getVoice uses 1024 but GodotSteam's is set at 8192?
		# Our sizes might be way off; internal GodotSteam notes that Valve suggests 8kb
		# However, this is not mentioned in the header nor the SpaceWar example but -is- in Valve's docs which are usually wrong
		var voice_data: Dictionary = Steam.getVoice()
		if voice_data['result'] == Steam.VOICE_RESULT_OK and voice_data['written']:
			print("Voice message has data: %s / %s" % [voice_data['result'], voice_data['written']])
			
			# Here we can pass this voice data off on the network
			Networking.send_message(voice_data['buffer'])

			# If loopback is enable, play it back at this point
			if has_loopback:
				print("Loopback on")
				process_voice_data(voice_data, "local")
```

Our `Networking.send_message` function can be whatever P2P networking function you have for sending packets / data. At the time of writing this, I wonder how necessary this is since Steam is recording voice data and we are checking if there is any available; surely it knows who we are talking to, if anyone. Do we need to send this data back as a packet?

{==
## Processing Voice Data
==}

OK, now that we have something, let's hear it. We may want to use the optimal sample rate instead of whatever we set in our `current_sample_rate` variable.  In which case we can use this function:

```gdscript
func get_sample_rate() -> void:
	current_sample_rate = Steam.getVoiceOptimalSampleRate()
	print("Current sample rate: %s" % current_sample_rate)
```

This function can be attached to a button. We can also add a toggle to this button to change between the optimal rate or back to our default. When changing the sample rate, remember to change the `AudioStreamGenerator`'s mix rate too:

```gdscript
func get_sample_rate(is_toggled: bool) -> void:
	if is_toggled:
		current_sample_rate = Steam.getVoiceOptimalSampleRate()
	else:
		current_sample_rate = 48000
    $Local.stream.mix_rate = current_sample_rate
    $Network.stream.mix_rate = current_sample_rate
	print("Current sample rate: %s" % current_sample_rate)
```

We have our sample rates figured out so let's try to actual play this data. Since we are just testing things, we will use the `local_` variables and nodes.

=== "Godot 2.x, 3.x"

	```gdscript
	func process_voice_data(voice_data: Dictionary, voice_source: String) -> void:
		# Our sample rate function above without toggling
		get_sample_rate()

		var decompressed_voice: Dictionary = Steam.decompressVoice(voice_data['buffer'], current_sample_rate)
		
		if decompressed_voice['result'] == Steam.VOICE_RESULT_OK and decompressed_voice['size'] > 0:
			print("Decompressed voice: %s" % decompressed_voice['size'])
			
			if voice_source == "local":
				local_voice_buffer = decompressed_voice['uncompressed']
				local_voice_buffer.resize(decompressed_voice['size'])
				
				# We now create an audio stream to put our data into
				var local_audio: AudioStreamSample = AudioStreamSample.new()
				local_audio.mix_rate = current_sample_rate
				local_audio.data = local_voice_buffer
				local_audio.format = AudioStreamSample.FORMAT_16_BITS

				# Lastly, put this into a node and play it
				$Local.stream = local_audio
				$Local.play()
	```

=== "Godot 4.x"

	```gdscript
	func process_voice_data(voice_data: Dictionary, voice_source: String) -> void:
		# Our sample rate function above without toggling
		get_sample_rate()
        
		var decompressed_voice: Dictionary = Steam.decompressVoice(voice_data['buffer'], current_sample_rate)
		
		if decompressed_voice['result'] == Steam.VOICE_RESULT_OK and decompressed_voice['size'] > 0:
			print("Decompressed voice: %s" % decompressed_voice['size'])
			
			if voice_source == "local":
				local_voice_buffer = decompressed_voice['uncompressed']
				local_voice_buffer.resize(decompressed_voice['size'])
				
                # We now iterate through the local_voice_buffer and push the samples to the audio generator
                for i: int in local_playback.get_frames_available():
                    # Steam's audio data is represented as 16-bit single channel PCM audio, so we need to convert it to amplitudes
                    # Combine the low and high bits to get full 16-bit value
		            var raw_value: int = LOCAL_VOICE_BUFFER[0] | (LOCAL_VOICE_BUFFER[1] << 8)
		            # Make it a 16-bit signed integer
		            raw_value = (raw_value + 32768) & 0xffff
		            # Convert the 16-bit integer to a float on from -1 to 1
		            var amplitude: float = float(raw_value - 32768) / 32768.0
                    
                    # push_frame() takes a Vector2. The x represents the left channel and the y represents the right channel
                    local_playback.push_frame(Vector2(amplitude, amplitude))
                    
                    # Delete the used samples
                    local_voice_buffer.remove_at(0)
                    local_voice_buffer.remove_at(0)
	```

{==
## Recording Voice
==}

So how do we actually get our voice data to Steam? We will need to set up a button that can be toggled and attached to the following function:

```gdscript
func record_voice(is_recording: bool) -> void:
	# If talking, suppress all other audio or voice comms from the Steam UI
	Steam.setInGameVoiceSpeaking(steam_id, is_recording)

	if is_recording:
		Steam.startVoiceRecording()
	else:
		Steam.stopVoiceRecording()
```

Easy-peasy! You will notice our `setInGameVoiceSpeaking` has a note that it is used to suppress any sounds or whatnot coming from the Steam UI; you'll definitely want to have that.

You may want to provide the option for always-on voice chat, in which case you'd probably want to fire this function once in your `_ready()` or somewhere else to start recording until the player turns it off.

---

{==
## :material-content-save-settings: Additional Resources
==}

### Related Projects

[:simple-github: 'Godot VOIP' by ikbencasdoei](https://github.com/ikbencasdoei/godot-voip){ .md-button .md-button--resource target="\_blank" }

### Example Project

### Example Project

[Later this year you can see this tutorial in action with more in-depth information by checking out our upcoming free-to-play game Skillet on GitHub.](https://github.com/GodotSteam/Skillet){ target="\_blank" } There you will be able to view of the code used which can serve as a starting point for you to branch out from.