---
title: Networking
description: A guide on setting up networking with the Networking class.
icon: material/network
---

# Tutorials - Networking
:material-badge-account-horizontal: _By Gramps_

---

One of the more requested tutorials is multiplayer lobbies and networking through Steam; this tutorial specifically covers the networking portion and [our lobbies tutorial covers the other half](lobbies.md).

Please note this tutorial uses the **deprecated Networking class**, which still works perfectly fine and always will; though it may be removed from later SDKS.  Valve suggests you change over to either [Networking Messages](networking_messages.md) or [Networking Sockets](networking_sockets.md).  This tutorial is for a basic, turn-based lobby / P2P set-up. Only use this as a starting point.

I'd also like to suggest you [check out the Additional Resources section of this tutorial](#additional-resources) before continuing on.

??? guide "Relevant GodotSteam classes and functions"
	* [Networking class](../classes/networking.md)
		* [acceptP2PSessionWithUser()](../classes/networking.md#acceptp2psessionwithuser)
		* [getAvailableP2PPacketSize()](../classes/networking.md#getavailablep2ppacketsize)
		* [readP2PPacket()](../classes/networking.md#readp2ppacket)
		* [sendP2PPacket()](../classes/networking.md#sendp2ppacket)
	* [Friends class](../classes/friends.md)
		* [getFriendPersonaName()](../classes/friends.md#getfriendpersonaname)

!!! warning "Note"
	You may want to [double-check our Initialization tutorial](initializing.md) to set up initialization and callbacks functionality if you haven't done so already.

{==
## The \_ready() Function
==}

Next we'll want to set up the signal connections for Steamworks and a command line checker like so:

=== "Godot 2.x, 3.x"

	```gdscript
	func _ready() -> void:
		Steam.connect("p2p_session_request", self, "_on_p2p_session_request")
		Steam.connect("p2p_session_connect_fail", self, "_on_p2p_session_connect_fail")

		# Check for command line arguments
		check_command_line()
	```

=== "Godot 4.x"

	```gdscript
	func _ready() -> void:
		Steam.p2p_session_request.connect(_on_p2p_session_request)
		Steam.p2p_session_connect_fail.connect(_on_p2p_session_connect_fail)

		# Check for command line arguments
		check_command_line()
	```

We will get into each of these below.

{==
## The \_process() Function
==}

We'll also need to set `read_p2p_packet()` in our `_process()` function so it is always looking for new packets:

```gdscript
func _process(_delta) -> void:
	Steam.run_callbacks()

	# If the player is connected, read packets
	if lobby_id > 0:
		read_p2p_packet()
```

If you are using the `steamworks.gd` autoload singleton [we set up in the initialization tutorial](initializing.md) then you can omit the `run_callbacks()` command as they'll be running already.

Here is a nice bit of code from **tehsquidge** for handling packet reading:

```gdscript
func _process(delta):
	Steam.run_callbacks()

	if lobby_id > 0:
		read_all_p2p_packets()


func read_all_p2p_packets(read_count: int = 0):
	if read_count >= PACKET_READ_LIMIT:
		return

	if Steam.getAvailableP2PPacketSize(0) > 0:
		read_p2p_packet()
		read_all_p2p_packets(read_count + 1)
```

{==
## P2P Networking - Session Request
==}

Next we'll check out the P2P networking functionality. [Over in the lobby tutorial](lobbies.md), we did a P2P handshake when someone joins the lobby, it would trigger a `p2p_session_request` callback which would in turn trigger this function:

```gdscript
func _on_p2p_session_request(remote_id: int) -> void:
	# Get the requester's name
	var this_requester: String = Steam.getFriendPersonaName(remote_id)
	print("%s is requesting a P2P session" % this_requester)

	# Accept the P2P session; can apply logic to deny this request if needed
	Steam.acceptP2PSessionWithUser(remote_id)

	# Make the initial handshake
	make_p2p_handshake()
```

It pretty simply acknowledges the session request, accepts it, then sends a handshake back.

{==
## Reading P2P Packets
==}

Inside that handshake there was a call to the `read_p2p_packet()` function which does this:

=== "Godot 2.x, 3.x"

	```gdscript
	func read_p2p_packet() -> void:
		var packet_size: int = Steam.getAvailableP2PPacketSize(0)

		# There is a packet
		if packet_size > 0:
			var this_packet: Dictionary = Steam.readP2PPacket(packet_size, 0)

			if this_packet.empty() or this_packet == null:
				print("WARNING: read an empty packet with non-zero size!")

			# Get the remote user's ID
			var packet_sender: int = this_packet['remote_steam_id']

			# Make the packet data readable
			var packet_code: PoolByteArray = this_packet['data']
			var readable_data: Dictionary = bytes2var(packet_code)

			# Print the packet to output
			print("Packet: %s" % readable_data)

			# Append logic here to deal with packet data
	```

=== "Godot 4.x"

	```gdscript
	func read_p2p_packet() -> void:
		var packet_size: int = Steam.getAvailableP2PPacketSize(0)

		# There is a packet
		if packet_size > 0:
			var this_packet: Dictionary = Steam.readP2PPacket(packet_size, 0)

			if this_packet.is_empty() or this_packet == null:
				print("WARNING: read an empty packet with non-zero size!")

			# Get the remote user's ID
			var packet_sender: int = this_packet['remote_steam_id']

			# Make the packet data readable
			var packet_code: PackedByteArray = this_packet['data']
			var readable_data: Dictionary = bytes_to_var(packet_code)

			# Print the packet to output
			print("Packet: %s" % readable_data)

			# Append logic here to deal with packet data
	```

If the packet size is greater than zero then it will get the sender's Steam ID and the data they sent. The line `bytes2var` (Godot 2x., 3.x) or `bytes_to_var` (Godot 4.x) is very important as it decodes the data back into something you can read and use. After it is decoded you can pass that data to any number of functions for your game.

{==
## Sending P2P Packets
==}

Beyond the handshake, you will probably want to pass a lot of different pieces of data back and forth between players.

I have mine set up with two arguments: the first is the recipient as a string and the second is a dictionary. I think the dictionary is best for sending data so you can have a key / value pair to reference and make things less confusing on the receiving end. Each packet will go through the following function:

=== "Godot 2.x, 3.x"

	```gdscript
	func send_p2p_packet(this_target: int, packet_data: Dictionary) -> void:
		# Set the send_type and channel
		var send_type: int = Steam.P2P_SEND_RELIABLE
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
						Steam.sendP2PPacket(this_member['steam_id'], this_data, send_type, channel)

		# Else send it to someone specific
		else:
			Steam.sendP2PPacket(this_target, this_data, send_type, channel)
	```

=== "Godot 4.x"

	```gdscript
	func send_p2p_packet(this_target: int, packet_data: Dictionary) -> void:
				# Set the send_type and channel
		var send_type: int = Steam.P2P_SEND_RELIABLE
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
						Steam.sendP2PPacket(this_member['steam_id'], this_data, send_type, channel)

		# Else send it to someone specific
		else:
			Steam.sendP2PPacket(this_target, this_data, send_type, channel)
	```

The `send_type` variable will corresponed to these enums and integers:

Send Type Enums 				| Values	| Descriptions
---								| ---		| ---
P2P_SEND_UNRELIABLE				| 0			| Send unreliable
P2P_SEND_UNRELIABLE_NO_DELAY	| 1			| Send unreliable with no delay
P2P_SEND_RELIABLE				| 2			| Send reliable
P2P_SEND_RELIABLE_WITH_BUFFERING| 3			|Send reliable with buffering

The channel used should match for both read and send functions. You may want to use multiple channels so this should obviously be adjusted.

As your game increases in complexity, you may find the amount of data you're sending increases significantly. One of the core tenets of responsive, effective networking is reducing the amount of data you're sending, to reduce the chance of some part becoming corrupted or requiring players of your game to have a very fast internet connection to even play your game.

Luckily, we can introduce **compression** to our send function to shrink the size of the data without needing to change our whole dictionary. The concept is straightforward enough; when we call the **var2bytes** (Godot 2.x, 3.x) or **var_to_bytes** (Godot 4.x) function, we turn our dictionary (or some other variable) into a **PoolByteArray** (Godot 2.x, 3.x) or **PackedByteArray** (Godot 4.x) and send it over the internet. 

We can compress the **PoolByteArray** / **PackedByteArray** to be smaller with a single line of code:

=== "Godot 2.x, 3.x"

	```gdscript
	func send_p2p_packet(target: int, packet_data: Dictionary) -> void:
		# Set the send_type and channel
		var send_type: int = Steam.P2P_SEND_RELIABLE
		var channel: int = 0

		# Create a data array to send the data through
		var this_data: PoolByteArray
		
		# Compress the PoolByteArray we create from our dictionary  using the GZIP compression method
		var compressed_data: PoolByteArray = var2bytes(packet_data).compress(File.COMPRESSION_GZIP)
		this_data.append_array(compressed_data)

		# If sending a packet to everyone
		if target == 0:
			# If there is more than one user, send packets
			if lobby_members.size() > 1:
				# Loop through all members that aren't you
				for this_member in lobby_members:
					if this_member['steam_id'] != steam_id:
						Steam.sendP2PPacket(this_member['steam_id'], this_data, send_type, channel)
		
		# Else send it to someone specific
		else:
			Steam.sendP2PPacket(target, this_data, send_type, channel)
	```

=== "Godot 4.x"

	```gdscript
	func send_p2p_packet(target: int, packet_data: Dictionary) -> void:
		# Set the send_type and channel
		var send_type: int = Steam.P2P_SEND_RELIABLE
		var channel: int = 0

		# Create a data array to send the data through
		var this_data: PackedByteArray
		
		# Compress the PackedByteArray we create from our dictionary  using the GZIP compression method
		var compressed_data: PackedByteArray = var_to_bytes(packet_data).compress(FileAccess.COMPRESSION_GZIP)
		this_data.append_array(compressed_data)

		# If sending a packet to everyone
		if target == 0:
			# If there is more than one user, send packets
			if lobby_members.size() > 1:
				# Loop through all members that aren't you
				for this_member in lobby_members:
					if this_member['steam_id'] != steam_id:
						Steam.sendP2PPacket(this_member['steam_id'], this_data, send_type, channel)
		
		# Else send it to someone specific
		else:
			Steam.sendP2PPacket(target, this_data, send_type, channel)
	```

Of course, we've now sent a **compressed** PoolByteArray / PackedByteArray to someone else over the internet, so when they receive the packet, they will need to first **decompress** the PoolByteArray / PackedByteArray before they can decode it.
To accomplish this, we add a single line of code to our `read_p2p_packet` function like so:

=== "Godot 2.x, 3.x"

	```gdscript
	func read_p2p_packet() -> void:
		var packet_size: int = Steam.getAvailableP2PPacketSize(0)

		# There is a packet
		if packet_size > 0:
			var this_packet: Dictionary = Steam.readP2PPacket(packet_size, 0)

			if this_packet.empty() or this_packet == null:
				print("WARNING: read an empty packet with non-zero size!")

			# Get the remote user's ID
			var packet_sender: int = this_packet['remote_steam_id']

			# Make the packet data readable
			var packet_code: PoolByteArray = this_packet['data']
			
			# Decompress the array before turning it into a useable dictionary
			var readable_data: Dictionary = bytes2var(packet_code.decompress_dynamic(-1, File.COMPRESSION_GZIP))

			# Print the packet to output
			print("Packet: %s" % readable_data)

			# Append logic here to deal with packet data
	```

=== "Godot 4.x"

	```gdscript
	func read_p2p_packet() -> void:
		var packet_size: int = Steam.getAvailableP2PPacketSize(0)

		# There is a packet
		if packet_size > 0:
			var this_packet: Dictionary = Steam.readP2PPacket(packet_size, 0)

			if this_packet.is_empty() or this_packet == null:
				print("WARNING: read an empty packet with non-zero size!")

			# Get the remote user's ID
			var packet_sender: int = this_packet['remote_steam_id']

			# Make the packet data readable
			var packet_code: PackedByteArray = this_packet['data']
			
			# Decompress the array before turning it into a useable dictionary
			var readable_data: Dictionary = bytes_to_var(packet_code.decompress_dynamic(-1, FileAccess.COMPRESSION_GZIP))

			# Print the packet to output
			print("Packet: %s" % readable_data)

			# Append logic here to deal with packet data
	```

The key point to note here is the format **must be the same for sending and receiving**. There's a whole lot to read about compression in Godot, far beyond this tutorial; to learn more, [read all about it here.](https://docs.godotengine.org/en/stable/classes/class_poolbytearray.html#class-poolbytearray-method-compress)

{==
## P2P Session Failures
==}
	
For the last part of this tutorial we'll handle P2P failures with the following function which is triggered by the `p2p_session_connect_fail` callback:

```gdscript
func _on_p2p_session_connect_fail(steam_id: int, session_error: int) -> void:
	# If no error was given
	if session_error == 0:
		print("WARNING: Session failure with %s: no error given" % steam_id)

	# Else if target user was not running the same game
	elif session_error == 1:
		print("WARNING: Session failure with %s: target user not running the same game" % steam_id)

	# Else if local user doesn't own app / game
	elif session_error == 2:
		print("WARNING: Session failure with %s: local user doesn't own app / game" % steam_id)

	# Else if target user isn't connected to Steam
	elif session_error == 3:
		print("WARNING: Session failure with %s: target user isn't connected to Steam" % steam_id)

	# Else if connection timed out
	elif session_error == 4:
		print("WARNING: Session failure with %s: connection timed out" % steam_id)

	# Else if unused
	elif session_error == 5:
		print("WARNING: Session failure with %s: unused" % steam_id)

	# Else no known error
	else:
		print("WARNING: Session failure with %s: unknown error %s" % [steam_id, session_error])
```

This will print a warning message so you know why the session connection failed. From here you can add any additional functionality you want like retrying the connection or something else.

{==
## Next Up
==}

That concludes the P2P tutorial. At this point you may want to [check out the lobbies tutorial (if you haven't yet) which compliments this one](lobbies.md). Obviously this code should not be used for production and more for a very, very, very, simple guide on where to start.

{==
## :material-content-save-settings: Additional Resources
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

[Later this year you can see this tutorial in action with more in-depth information by checking out our upcoming free-to-play game Skillet on GitHub.](https://github.com/GodotSteam/Skillet){ target="\_blank" } There you will be able to view of the code used which can serve as a starting point for you to branch out from.