# Tutorials - P2P Networking

One of the more requested tutorials is multiplayer lobbies and P2P networking through Steam; this tutorial specifically covers the P2P Networking portion and [our lobbies tutorial covers the other half](lobbies.md).

Please note this tutorial uses the **Steamworks Messages Networking class** and this is for a basic, turn-based lobby / P2P set-up. Only use this as a starting point.

I'd also like to suggest you [check out the Additional Resources section of this tutorial](#additional-resources) before continuing on.

??? guide "Relevant GodotSteam classes and functions"
	* [Networking Messages class](../classes/networking_messages.md)
		* [acceptsessionwithuser()](../classes/networking_messages.md#acceptsessionwithuser)
		* [receiveMessagesOnChannel()](../classes/networking.md#receiveMessagesOnChannel)
		* [sendMessageToUser()](../classes/networking.md#sendMessageToUser)
	* [Friends class](../classes/friends.md)
		* [getFriendPersonaName()](../classes/friends.md#getfriendpersonaname)

{==
## The \_ready() Function
==}

Next we'll want to set up the signal connections for Steamworks and a command line checker like so:

=== "Godot 2.x, 3.x"

	```gdscript
	func _ready() -> void:
		Steam.connect("network_messages_session_request", self, "_on_network_messages_session_request")
		Steam.connect("network_messages_session_failed", self, "_on_network_messages_session_failed")

		# Check for command line arguments
		check_command_line()
	```

=== "Godot 4.x"

	```gdscript
	func _ready() -> void:
		Steam.network_messages_session_request.connect(_on_network_messages_session_request)
		Steam.network_messages_session_failed.connect(_on_network_messages_session_failed)

		# Check for command line arguments
		check_command_line()
	```

We will get into each of these below.

{==
## The \_process() Function
==}

We'll also need to set `read_message()` in our `_process()` function so it is always looking for new packets:

```gdscript
func _process(_delta) -> void:
	Steam.run_callbacks()

	# If the player is connected, read packets
	if lobby_id > 0:
		read_messages()
```

If you are using the `global.gd` autoload singleton then you can omit the `run_callbacks()` command as they'll be running already.

Here is a nice bit of code from **Toi Lanh** based on **tehsquidge**'s networking code for handling packet reading:

```gdscript
func _process(delta):
	Steam.run_callbacks()

	if lobby_id > 0:
		read_messages()
```

{==
## P2P Networking - Session Request
==}

Next we'll check out the P2P networking functionality. [Over in the lobby tutorial](lobbies.md), we did a P2P handshake when someone joins the lobby, it would trigger a `network_messages_session_request` callback which would in turn trigger this function:

```gdscript
func _on_network_messages_session_request(remote_id: int) -> void:
	# Get the requester's name
	var this_requester: String = Steam.getFriendPersonaName(remote_id)
	print("%s is requesting a P2P session" % this_requester)

	# Accept the P2P session; can apply logic to deny this request if needed
	Steam.acceptSessionWithUser(remote_id)

	# Make the initial handshake
	make_p2p_handshake()
```

It pretty simply acknowledges the session request, accepts it, then sends a handshake back.

{==
## Reading P2P Packets
==}

Inside that handshake there was a call to the `read_messages()` function which does this:

=== "Godot 2.x, 3.x"

	```gdscript
	func read_messages() -> void:
		var messages: array = Steam.receiveMessagesOnChannel(0, 100)

		# There is a packet
		for message in messages
			if message.is_empty() or message == null:
				print("WARNING: read an empty packet with non-zero size!")
    			else:
       				#make data readable. Do NOT use bytes2var with_objects enabled.
       				message.payload = bytes_to_var(message.payload)

				# Get the remote user's ID
				var message_sender: int = message['remote_steam_id']

				# Print the packet to output
				print("Packet: " + message.payload)

				# Append logic here to deal with message data. Do NOT use call or call_v here when directly dealing with data without sanatizing your data first. Ideally don't use it at all unless you really know what you're doing.
	```

=== "Godot 4.x"

	```gdscript
	func read_messages() -> void:
		var messages: Array = Steam.receiveMessagesOnChannel(0, 1000)

		for message in messages
			if message.is_empty() or message == null:
				print("WARNING: read an empty message with non-zero size!")
     		else:
				message.payload = bytes_to_var(message.payload)
				# Get the remote user's ID
				var message_sender: int = this_packet['remote_steam_id']

				# Print the packet to output
				print("Message: " + message.payload)

				# Append logic here to deal with message data. Do NOT use call or call_v here when directly dealing with data without sanatizing your data first. Ideally don't use it at all unless you really know what you're doing.
	```

If the packet size is greater than zero then it will get the sender's Steam ID and the data they sent. The line `bytes2var` (Godot 2x., 3.x) or `bytes_to_var` (Godot 4.x) is very important as it decodes the data back into something you can read and use. Do NOT use bytes_to_var_with_objects or bytes2var with objects enabled as it can lead to remote code execution. After it is decoded you can pass that data to any number of functions for your game. It is also important to remember to not allow the other users to call functions directly as well with the packets they're sending (or else it may ALSO lead to compromised user security and remote code execution).

{==
## Sending P2P Packets
==}

Beyond the handshake, you will probably want to pass a lot of different pieces of data back and forth between players.

I have mine set up with two arguments: the first is the recipient as a string and the second is a dictionary. I think the dictionary is best for sending data so you can have a key / value pair to reference and make things less confusing on the receiving end. Each packet will go through the following function:

=== "Godot 2.x, 3.x"

	```gdscript
	func send_message(this_target: int, packet_data: Dictionary) -> void:
		# Set the send_type and channel
		var send_type: int = Steam.NETWORKING_SEND_RELIABLE_NO_NAGLE
		var channel: int = 0

		# Create a data array to send the data through
		var this_data: PoolByteArray
		this_data.append_array(var2bytes(packet_data))

		# If sending a packet to everyone
		if this_target == 0:
			# If there is more than one user, send packets
			if lobby_members.size() > 1:
				# Loop through all members that aren't you
				for this_member in lobby_members:
					if this_member['steam_id'] != steam_id:
						Steam.sendMessageToUser(this_member['steam_id'], this_data, send_type, channel)

		# Else send it to someone specific
		else:
			Steam.sendMessageToUser(this_target, this_data, send_type, channel)
	```

=== "Godot 4.x"

	```gdscript
	func send_message(this_target: int, packet_data: Dictionary) -> void:
				# Set the send_type and channel
		var send_type: int = Steam.NETWORKING_SEND_RELIABLE_NO_NAGLE
		var channel: int = 0

		# Create a data array to send the data through
		var this_data: PackedByteArray
		this_data.append_array(var_to_bytes(packet_data))

		# If sending a packet to everyone
		if this_target == 0:
			# If there is more than one user, send packets
			if lobby_members.size() > 1:
				# Loop through all members that aren't you
				for this_member in lobby_members:
					if this_member['steam_id'] != steam_id:
						Steam.sendMessageToUser(this_member['steam_id'], this_data, send_type, channel)

		# Else send it to someone specific
		else:
			Steam.sendMessageToUser(this_target, this_data, send_type, channel)
	```

For 'send_type' values please refer to this section of the [SteamWorks Api](https://partner.steamgames.com/doc/api/steamnetworkingtypes#message_sending_flags), these can also be called with their coresponding constant such as Steam.NETWORKING_SEND_RELIABLE_NO_NAGLE

The channel used should match for both read and send functions. You may want to use multiple channels so this should obviously be adjusted.

As your game increases in complexity, you may find the amount of data you're sending increases significantly. One of the core tenets of responsive, effective networking is reducing the amount of data you're sending, to reduce the chance of some part becoming corrupted or requiring players of your game to have a very fast internet connection to even play your game.

Luckily, we can introduce **compression** to our send function to shrink the size of the data without needing to change our whole dictionary. The concept is straightforward enough; when we call the **var2bytes** (Godot 2.x, 3.x) or **var_to_bytes** (Godot 4.x) function, we turn our dictionary (or some other variable) into a **PoolByteArray** (Godot 2.x, 3.x) or **PackedByteArray** (Godot 4.x) and send it over the internet. 

We can compress the **PoolByteArray** / **PackedByteArray** to be smaller with a single line of code:

=== "Godot 2.x, 3.x"

	```gdscript
	func send_message(this_target: int, packet_data: Dictionary) -> void:
		# Set the send_type and channel
		var send_type: int = Steam.NETWORKING_SEND_RELIABLE_NO_NAGLE
		var channel: int = 0

		# Create a data array to send the data through
		var this_data: PoolByteArray
		this_data.append_array(var2bytes(packet_data).compress(File.COMPRESSION_GZIP))

		# If sending a packet to everyone
		if this_target == 0:
			# If there is more than one user, send packets
			if lobby_members.size() > 1:
				# Loop through all members that aren't you
				for this_member in lobby_members:
					if this_member['steam_id'] != steam_id:
						Steam.sendMessageToUser(this_member['steam_id'], this_data, send_type, channel)

		# Else send it to someone specific
		else:
			Steam.sendMessageToUser(this_target, this_data, send_type, channel)
	```

=== "Godot 4.x"

	```gdscript
	func send_message(this_target: int, packet_data: Dictionary) -> void:
				# Set the send_type and channel
		var send_type: int = Steam.NETWORKING_SEND_RELIABLE_NO_NAGLE
		var channel: int = 0

		# Create a data array to send the data through
		var this_data: PackedByteArray
		this_data.append_array(var_to_bytes(packet_data).compress(File.COMPRESSION_GZIP))

		# If sending a packet to everyone
		if this_target == 0:
			# If there is more than one user, send packets
			if lobby_members.size() > 1:
				# Loop through all members that aren't you
				for this_member in lobby_members:
					if this_member['steam_id'] != steam_id:
						Steam.sendMessageToUser(this_member['steam_id'], this_data, send_type, channel)

		# Else send it to someone specific
		else:
			Steam.sendMessageToUser(this_target, this_data, send_type, channel)
	```

Of course, we've now sent a **compressed** PoolByteArray / PackedByteArray to someone else over the internet, so when they receive the packet, they will need to first **decompress** the PoolByteArray / PackedByteArray before they can decode it.
To accomplish this, we add a single line of code to our `read_messages` function like so:

=== "Godot 2.x, 3.x"

	```gdscript
	func read_messages() -> void:
		var messages: array = Steam.receiveMessagesOnChannel(0, 100)

		# There is a packet
		while messages.size() > 0:
			var message: Dictionary = messages.pop_front()

			if this_packet.empty() or this_packet == null:
				print("WARNING: read an empty packet with non-zero size!")
    			else:
       				#make data readable
       				message['data'] = bytes2var(message['data']).decompress_dynamic(-1, File.COMPRESSION_GZIP)

				# Get the remote user's ID
				var message_sender: int = message['remote_steam_id']

				# Print the packet to output
				print("Packet: %s" % message['data'])

				# Append logic here to deal with packet data
	```

=== "Godot 4.x"

	```gdscript
	func read_messages() -> void:
		var messages: Array = Steam.receiveMessagesOnChannel(0, 1000)

		# There is a packet
		if packet_size > 0:
  			for message in messages:
				if message.is_empty() or message == null:
					print("WARNING: read an empty message with non-zero size!")
     				else:
					message.payload = bytes_to_var(message.payload).decompress_dynamic(-1, File.COMPRESSION_GZIP)
					# Get the remote user's ID
					var message_sender: int = this_packet['remote_steam_id']

					# Print the packet to output
					print("Message: %s" % message.payload)

					# Append logic here to deal with packet data
	```

The key point to note here is the format **must be the same for sending and receiving**. There's a whole lot to read about compression in Godot, far beyond this tutorial; to learn more, [read all about it here.](https://docs.godotengine.org/en/stable/classes/class_poolbytearray.html#class-poolbytearray-method-compress)

{==
## P2P Session Failures
==}
	
For the last part of this tutorial we'll handle P2P failures with the following function which is triggered by the `network_messages_session_failed` callback:

```gdscript
func _on_network_messages_session_failed(steam_id: int, session_error: int, state: int, debug_msg: String) -> void:
	print(debug_msg)
```

This will print a warning message so you know why the session connection failed. From here you can add any additional functionality you want like retrying the connection or something else.

{==
## Next Up
==}

That concludes the P2P tutorial. At this point you may want to [check out the lobbies tutorial (if you haven't yet) which compliments this one](lobbies.md). Obviously this code should not be used for production and more for a very, very, very, simple guide on where to start.

{==
## Additional Resources
==}

### Suggested Reading Material

I highly suggest reading some or all of this to better understand networking.

[ :simple-steam: Valve's networking documentation](https://partner.steamgames.com/doc/features/multiplayer/networking){ .md-button .md-button--resource target="\_blank" }

[ :simple-github: 'Game Networking Resources' by ThusSpokeNomad](https://github.com/ThusSpokeNomad/GameNetworkingResources){ .md-button .md-button--resource target="\_blank" }

[ :simple-stackoverflow: 'How to write a network game?' on StackOverflow](https://gamedev.stackexchange.com/questions/249/how-to-write-a-network-game){ .md-button .md-button--resource target="\_blank" }

[ :simple-firefox: Networking by Gaffer On Games](https://web.archive.org/web/20180823014743/https://gafferongames.com/tags/networking){ .md-button .md-button--resource target="\_blank" }

[ :simple-firefox: 'Client/Server Game Architecture' by Gabriel Gambetta](https://www.gabrielgambetta.com/client-server-game-architecture.html){ .md-button .md-button--resource target="\_blank" }

### Video Tutorials

[ :simple-youtube: Godot 4 Steam Multiplayer' by Gwizz](https://www.youtube.com/watch?v=TPJbqA5OAmY&t=309s){ .md-button .md-button--resource target="\_blank" }

### Related Projects

[ :simple-github: 'GodotSteamHL' by JDare](https://github.com/JDare/GodotSteamHL){ .md-button .md-button--resource target="\_blank" }

### Example Project

[To see this tutorial in action, check out our GodotSteam Example Project on GitHub.](https://github.com/GodotSteam/GodotSteam-Example-Project){ target="\_blank" } There you can get a full view of the code used which can serve as a starting point for you to branch out from.

