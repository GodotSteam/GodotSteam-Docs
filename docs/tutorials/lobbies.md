# Tutorials - Lobbies

One of the more requested tutorials is multiplayer lobbies and P2P networking through Steam; this tutorial specifically covers the lobby portion and [our P2P tutorial covers the other half](p2p.md). Please note, only use this as a starting point.

I'd also like to suggest you [check out the Additional Resources section of this tutorial](#additional-resources) before continuing on.

??? guide "Relevant GodotSteam classes and functions"
	* [Matchmaking class](../classes/matchmaking.md)
		* [addRequestLobbyListFilterSlotsAvailable()](../classes/matchmaking.md#addrequestlobbylistfilterslotsavailable)
		* [addRequestLobbyListNearValueFilter()](../classes/matchmaking.md#addrequestlobbylistnearvaluefilter)
		* [addRequestLobbyListNumericalFilter()](../classes/matchmaking.md#addrequestlobbylistnumericalfilter)
		* [addRequestLobbyListResultCountFilter()](../classes/matchmaking.md#addrequestlobbylistresultcountfilter)
		* [addRequestLobbyListStringFilter()](../classes/matchmaking.md#addrequestlobbyliststringfilter)
		* [createLobby()](../classes/matchmaking.md#createlobby)
		* [getLobbyData()](../classes/matchmaking.md#getlobbydata)
		* [getLobbyMemberByIndex()](../classes/matchmaking.md#getlobbymemberbyindex)
		* [getNumLobbyMembers()](../classes/matchmaking.md#getnumlobbymembers)
		* [joinLobby()](../classes/matchmaking.md#joinlobby)
		* [leaveLobby()](../classes/matchmaking.md#leavelobby)
		* [requestLobbyList()](../classes/matchmaking.md#requestlobbylist)
		* [sendLobbyChatMsg()](../classes/matchmaking.md#sendlobbychatmsg)
		* [setLobbyData()](../classes/matchmaking.md#setlobbydata)
		* [setLobbyJoinable()](../classes/matchmaking.md#setlobbyjoinable)
	* [Networking class](../classes/networking.md)
		* [allowP2PPacketRelay()](../classes/networking.md#allowp2ppacketrelay)
		* [closeP2PSessionWithUser()](../classes/networking.md#closep2psessionwithuser)
	* [Friends class](../classes/friends.md)
		* [getFriendPersonaName()](../classes/friends.md#getfriendpersonaname)

{==
## Set Up
==}

First let's set up some variables to fill in later:

```gdscript
const PACKET_READ_LIMIT: int = 32

var lobby_data
var lobby_id: int = 0
var lobby_members: Array = []
var lobby_members_max: int = 10
var lobby_vote_kick: bool = false
var steam_id: int = 0
var steam_username: String = ""
```

Your Steam ID and username may actually be in a different GDScript, especially if you use the `global.gd` the way I do [mentioned in the initialization tutorial](initializing.md). The most important will be the `lobby_id`, which obviously houses the lobby's ID, and `lobby_members`, which will be an array of dictionaries of lobby members and their Steam ID 64's.

{==
## The \_ready() Function
==}

Next we'll want to set up the signal connections for Steamworks and a command line checker like so:

=== "Godot 2.x, 3.x"

	```gdscript
	func _ready() -> void:
		Steam.connect("join_requested", self, "_on_lobby_join_requested")
		Steam.connect("lobby_chat_update", self, "_on_lobby_chat_update")
		Steam.connect("lobby_created", self, "_on_lobby_created")
		Steam.connect("lobby_data_update", self, "_on_lobby_data_update")
		Steam.connect("lobby_invite", self, "_on_lobby_invite")
		Steam.connect("lobby_joined", self, "_on_lobby_joined")
		Steam.connect("lobby_match_list", self, "_on_lobby_match_list")
		Steam.connect("lobby_message", self, "_on_lobby_message")
		Steam.connect("persona_state_change", self, "_on_persona_change")

		# Check for command line arguments
		check_command_line()
	```

=== "Godot 4.x"

	```gdscript
	func _ready() -> void:
		Steam.join_requested.connect(_on_lobby_join_requested)
		Steam.lobby_chat_update.connect(_on_lobby_chat_update)
		Steam.lobby_created.connect(_on_lobby_created)
		Steam.lobby_data_update.connect(_on_lobby_data_update)
		Steam.lobby_invite.connect(_on_lobby_invite)
		Steam.lobby_joined.connect(_on_lobby_joined)
		Steam.lobby_match_list.connect(_on_lobby_match_list)
		Steam.lobby_message.connect(_on_lobby_message)
		Steam.persona_state_change.connect(_on_persona_change)

		# Check for command line arguments
		check_command_line()
	```

We will get into each of these below. You noticed we added a check for command line arguments. Here is how our basic function will look:

```gdscript
func check_command_line() -> void:
	var these_arguments: Array = OS.get_cmdline_args()

	# There are arguments to process
	if these_arguments.size() > 0:

		# A Steam connection argument exists
		if these_arguments[0] == "+connect_lobby":
		
			# Lobby invite exists so try to connect to it
			if int(these_arguments[1]) > 0:

				# At this point, you'll probably want to change scenes
				# Something like a loading into lobby screen
				print("Command line lobby ID: %s" % these_arguments[1])
				join_lobby(int(these_arguments[1]))
```

This is important if the player is accepting a Steam invite or right-clicks a friend's name then selects 'Join Game' or 'Join Lobby' and doesn't have the game open. Doing either action will launch the game with the additional command `+connect_lobby <Steam Lobby ID>`. Sadly Godot doesn't really understand this command argument so our `check_command_line()` function has to be written to work within those constraints.

Additionally, you'll need to add the appropriate scene name to your Steamworks launch options on the Steamworks website. You'll want to add the full scene path (res://your-scene.tscn) on the **Arguments** line in your launch option. [You can read more about that, with details, in this link.](https://github.com/CoaguCo-Industries/GodotSteam/issues/100){ target="\_blank" } Big thanks to **Antokolos** for answering this issue and providing a solid example.

{==
## Creating Lobbies
==}

Next we'll set up our lobby creation functions. You'll probably want to connect this function to a button somewhere in your game:

```gdscript
func create_lobby() -> void:
	# Make sure a lobby is not already set
	if lobby_id == 0:
		Steam.createLobby(Steam.LOBBY_TYPE_PUBLIC, lobby_max_members)
```

In this example we have `createLobby()` using our variables and enum. The first variables covers the type of lobby; we are using a public lobby open to all. There are, of course, four settings in total you can use:

Lobby Type Enums 		| Values	| Descriptions
---						| ---		| ---
LOBBY_TYPE_PRIVATE 		| 0  		| The only way to join the lobby is from an invite.
LOBBY_TYPE_FRIENDS_ONLY | 1 		| Joinable by friends and invitees, but does not show up in the lobby list.
LOBBY_TYPE_PUBLIC 		| 2 		| Returned by search and visible to friends.
LOBBY_TYPE_INVISIBLE 	| 3 		| Returned by search, but not visible to other friends.

The second variable is the maximum number of players allowed to join the lobby. This cannot be set higher than 250.

Next we'll cover the callback from Steam saying the lobby has been created:

```gdscript
func _on_lobby_created(connect: int, this_lobby_id: int) -> void:
	if connect == 1:
		# Set the lobby ID
		lobby_id = this_lobby_id
		print("Created a lobby: %s" % lobby_id)

		# Set this lobby as joinable, just in case, though this should be done by default
		Steam.setLobbyJoinable(lobby_id, true)

		# Set some lobby data
		Steam.setLobbyData(lobby_id, "name", "Gramps' Lobby")
		Steam.setLobbyData(lobby_id, "mode", "GodotSteam test")

		# Allow P2P connections to fallback to being relayed through Steam if needed
		var set_relay: bool = Steam.allowP2PPacketRelay(true)
		print("Allowing Steam to be relay backup: %s" % set_relay)
```

Once this callback fires, you'll have your lobby ID which you can pass off to our `lobby_id` variable for later use. As the note says, the lobby should be set to joinable by default but, just in case, we add it here. You can make the lobby unjoinable too.

You can also set some lobby data now; which can be whatever **key / value** pair you want. I'm not aware of a maximum amount of pairs you can set.

And you'll notice I set `allowP2PPacketRelay()` to true at this point; this allows, as the note mentions, P2P connections to fallback to being relayed through Steam if needed. This usually happens if you have NAT or firewall issues.

{==
## Get Lobby Lists
==}

Now that we can create lobbies, let's query and pull a list of lobbies. I usually have a button that will open a lobby interface which is a list of buttons, one per lobby:

```gdscript
func _on_open_lobby_list_pressed() -> void:
	# Set distance to worldwide
	Steam.addRequestLobbyListDistanceFilter(Steam.LOBBY_DISTANCE_FILTER_WORLDWIDE)

	print("Requesting a lobby list")
	Steam.requestLobbyList()
```

Before requesting the lobby list with `requestLobbyList()` you can add more search queries like:

`addRequestLobbyListStringFilter()`<br />Allows you to look for specific works in the lobby metadata

`addRequestLobbyListNumericalFilter()`<br />Adds a numerical comparions filter (<=, <, =, >, >=, !=)

`addRequestLobbyListNearValueFilter()`<br />Gives results closes to the specified value you give

`addRequestLobbyListFilterSlotsAvailable()`<br />Only returns lobbies with a specified amount of open slots available

`addRequestLobbyListResultCountFilter()`<br/>Sets how many results you want returned

`addRequestLobbyListDistanceFilter()`<br/>Sets the distance to search for lobbies, like:

Lobby Distance Enums			| Values	| Checking Distances
---								| ---		| ---
LOBBY_DISTANCE_FILTER_CLOSE		| 0			| Close
LOBBY_DISTANCE_FILTER_DEFAULT	| 1			| Default
LOBBY_DISTANCE_FILTER_FAR		| 2			| Far
LOBBY_DISTANCE_FILTER_WORLDWIDE	| 3			| Worldwide

Once you set all, some, or none of these, you can then call `requestLobbyList()`. Once it pulls your lobby list it will fire a callback `_on_lobby_match_list()`. You can then loop through the lobbies however you want.

In our example code, I do something like this to make buttons for each lobby:

=== "Godot 2.x, 3.x"

	```gdscript
	func _on_lobby_match_list(these_lobbies: Array) -> void:
		for this_lobby in these_lobbies:
			# Pull lobby data from Steam, these are specific to our example
			var lobby_name: String = Steam.getLobbyData(this_lobby, "name")
			var lobby_mode: String = Steam.getLobbyData(this_lobby, "mode")

			# Get the current number of members
			var lobby_num_members: int = Steam.getNumLobbyMembers(this_lobby)

			# Create a button for the lobby
			var lobby_button: Button = Button.new()
			lobby_button.set_text("Lobby %s: %s [%s] - %s Player(s)" % [this_lobby, lobby_name, lobby_mode, lobby_num_members])
			lobby_button.set_size(Vector2(800, 50))
			lobby_button.set_name("lobby_%s" % this_lobby)
			lobby_button.connect("pressed", self, "join_lobby", [this_lobby])

			# Add the new lobby to the list
			$Lobbies/Scroll/List.add_child(lobby_button)
	```

=== "Godot 4.x"

	```gdscript
	func _on_lobby_match_list(these_lobbies: Array) -> void:
		for this_lobby in these_lobbies:
			# Pull lobby data from Steam, these are specific to our example
			var lobby_name: String = Steam.getLobbyData(this_lobby, "name")
			var lobby_mode: String = Steam.getLobbyData(this_lobby, "mode")
			
			# Get the current number of members
			var lobby_num_members: int = Steam.getNumLobbyMembers(this_lobby)

			# Create a button for the lobby
			var lobby_button: Button = Button.new()
			lobby_button.set_text("Lobby %s: %s [%s] - %s Player(s)" % [this_lobby, lobby_name, lobby_mode, lobby_num_members])
			lobby_button.set_size(Vector2(800, 50))
			lobby_button.set_name("lobby_%s" % this_lobby)
			lobby_button.connect("pressed", Callable(self, "join_lobby").bind(this_lobby))

			# Add the new lobby to the list
			$Lobbies/Scroll/List.add_child(lobby_button)
	```

You should now have a way to call lobby lists and display them.

{==
## Joining Lobbies
==}
	
Next we'll tackle joining a lobby. Clicking one of the lobby buttons we created in the last step will fire this function:

```gdscript
func join_lobby(this_lobby_id: int) -> void:
	print("Attempting to join lobby %s" % lobby_id)

	# Clear any previous lobby members lists, if you were in a previous lobby
	lobby_members.clear()

	# Make the lobby join request to Steam
	Steam.joinLobby(this_lobby_id)
```

This will attempt to join the lobby you click on and, when it succeeds, it will fire the `_on_lobby_joined()` callback:

```gdscript
func _on_lobby_joined(this_lobby_id: int, _permissions: int, _locked: bool, response: int) -> void:
	# If joining was successful
	if response == Steam.CHAT_ROOM_ENTER_RESPONSE_SUCCESS:
		# Set this lobby ID as your lobby ID
		lobby_id = this_lobby_id

		# Get the lobby members
		get_lobby_members()

		# Make the initial handshake
		make_p2p_handshake()

	# Else it failed for some reason
	else:
		# Get the failure reason
		var fail_reason: String
	
		match response:
			Steam.CHAT_ROOM_ENTER_RESPONSE_DOESNT_EXIST: fail_reason = "This lobby no longer exists."
			Steam.CHAT_ROOM_ENTER_RESPONSE_NOT_ALLOWED: fail_reason = "You don't have permission to join this lobby."
			Steam.CHAT_ROOM_ENTER_RESPONSE_FULL: fail_reason = "The lobby is now full."
			Steam.CHAT_ROOM_ENTER_RESPONSE_ERROR: fail_reason = "Uh... something unexpected happened!"
			Steam.CHAT_ROOM_ENTER_RESPONSE_BANNED: fail_reason = "You are banned from this lobby."
			Steam.CHAT_ROOM_ENTER_RESPONSE_LIMITED: fail_reason = "You cannot join due to having a limited account."
			Steam.CHAT_ROOM_ENTER_RESPONSE_CLAN_DISABLED: fail_reason = "This lobby is locked or disabled."
			Steam.CHAT_ROOM_ENTER_RESPONSE_COMMUNITY_BAN: fail_reason = "This lobby is community locked."
			Steam.CHAT_ROOM_ENTER_RESPONSE_MEMBER_BLOCKED_YOU: fail_reason = "A user in the lobby has blocked you from joining."
			Steam.CHAT_ROOM_ENTER_RESPONSE_YOU_BLOCKED_MEMBER: fail_reason = "A user you have blocked is in the lobby."

		print("Failed to join this chat room: %s" % fail_reason)

		#Reopen the lobby list
		_on_open_lobby_list_pressed()
```

For a more clear explanation of these chat room responses, [check out the enums listings in the Friends class](../classes/main.md#chatroomenterresponse).

If the player is already in-game and accepts a Steam invite or clicks on a friend in their friend list then selects 'Join Game' from there, it will trigger the `join_requested` callback. This function will handle that:

```gdscript
func _on_Lobby_Join_Requested(this_lobby_id: int, friend_id: int) -> void:
	# Get the lobby owner's name
	var owner_name: String = Steam.getFriendPersonaName(friend_id)

	print("Joining %s's lobby..." % owner_name)

	# Attempt to join the lobby
	join_lobby(this_lobby_id)
```

It will then follow the normal `join_lobby()` process of setting up all lobby members, handshakes, etc. Not to sound repetitive, but note again that if the player is not in-game and accepts a Steam invite or joins a game through the friends list then we are back to the command line situation talked about earlier.

{==
## Getting Lobby Members
==}
	
Depending on how you set up your lobby interface, you'll probably want the player to see some kind of chat window with a player list. Our `get_lobby_members()` will help with finding out who all is in this lobby:

```gdscript
func get_lobby_members() -> void:
	# Clear your previous lobby list
	lobby_members.clear()

	# Get the number of members from this lobby from Steam
	var num_of_members: int = Steam.getNumLobbyMembers(lobby_id)

	# Get the data of these players from Steam
	for this_member in range(0, num_of_members):
		# Get the member's Steam ID
		var member_steam_id: int = Steam.getLobbyMemberByIndex(lobby_id, this_member)

		# Get the member's Steam name
		var member_steam_name: String = Steam.getFriendPersonaName(member_steam_id)

		# Add them to the list
		lobby_members.append({"steam_id":member_steam_id, "steam_name":member_steam_name})
```

This will get the lobby members from Steam then loop through and get their names and Steam ID's then append them to our `lobby_members` array for later use. You can then display this list in your lobby room.

{==
## Persona Changes / Avatars / Names
==}
	
Sometimes you will see that a user's name and avatar, sometimes one or the other, won't immediately show up correctly. This is because our local user only really knows about friends and players they have played with; whatever is stored in local cache.

A bit after a lobby is joined, this data will be sent by Steam which triggers a `persona_state_change` callback. You will want to update your player list to reflect this and get the correct name and avatar for unknown players. Our connect `_on_persona_change()` function will do that:

```gdscript
# A user's information has changed
func _on_persona_change(this_steam_id: int, _flag: int) -> void:
	# Make sure you're in a lobby and this user is valid or Steam might spam your console log
	if lobby_id > 0:
		print("A user (%s) had information change, update the lobby list" % this_steam_id)

		# Update the player list
		get_lobby_members()
```

All this really does is refresh our lobby list information to get the avatar and name right by re-calling `get_lobby_Members()` again.

{==
## P2P Handshakes
==}

You'll also note in the joining lobbies part we fire the initial P2P handshake; this just opens and checks our P2P session:

```gdscript
func make_p2p_handshake() -> void:
	print("Sending P2P handshake to the lobby")
	
	send_p2p_packet(0, {"message": "handshake", "from": steam_id})
```

We won't get into what all this means just yet, but I wanted to show the code for the handshake function here since it is referenced; [more on that in the P2P tutorial](p2p.md). Your handshake messages can be anything and disregarded for the most part. Again, it is just to test our P2P session.

{==
## Lobby Updates / Changes
==}

Now that a player has joined the lobby, everyone in the lobby will receive a callback notifying of the change. We will handle it like this:

```gdscript
func _on_lobby_chat_update(this_lobby_id: int, change_id: int, making_change_id: int, chat_state: int) -> void:
	# Get the user who has made the lobby change
	var changer_name: String = Steam.getFriendPersonaName(change_id)

	# If a player has joined the lobby
	if chat_state == Steam.CHAT_MEMBER_STATE_CHANGE_ENTERED:
		print("%s has joined the lobby." % changer_name)

	# Else if a player has left the lobby
	elif chat_state == Steam.CHAT_MEMBER_STATE_CHANGE_LEFT:
		print("%s has left the lobby." % changer_name)

	# Else if a player has been kicked
	elif chat_state == Steam.CHAT_MEMBER_STATE_CHANGE_KICKED:
		print("%s has been kicked from the lobby." % changer_name)

	# Else if a player has been banned
	elif chat_state == Steam.CHAT_MEMBER_STATE_CHANGE_BANNED:
		print("%s has been banned from the lobby." % changer_name)

	# Else there was some unknown change
	else:
		print("%s did... something." % changer_name)

	# Update the lobby now that a change has occurred
	get_lobby_members()
```

For the most part this will update when players join or leave the lobby. However, if you add functionality to kick or ban players, it will show that too. At the end of this function, I always update the player list so we can show the correct list of players in the lobby.

{==
## Lobby Chat / Messages
==}

You may also want players to be able to chat while in the lobby and waiting for a game to start. If you have a **LineEdit** node for messaging, clicking a "send" button should trigger something like this:

```gdscript
func _on_send_chat_pressed() -> void:
	# Get the entered chat message
	var this_message: String = $Chat.get_text()

	# If there is even a message
	if this_message.length() > 0:
		# Pass the message to Steam
		var was_sent: bool = Steam.sendLobbyChatMsg(lobby_id, this_message)

		# Was it sent successfully?
		if not was_sent:
			print("ERROR: Chat message failed to send.")

	# Clear the chat input
	$Chat.clear()
```

The $Chat is your **LineEdit** and will probably be different in your project. Most importantly is your get the text and send it to `sendLobbyChatMsg()`.

{==
## Leaving A Lobby
==}

Next we'll handle leaving a lobby. If you have a button do to so, have it connect to this function:

```gdscript
func leave_lobby() -> void:
	# If in a lobby, leave it
	if lobby_id != 0:
		# Send leave request to Steam
		Steam.leaveLobby(lobby_id)

		# Wipe the Steam lobby ID then display the default lobby ID and player list title
		lobby_id = 0

		# Close session with all users
		for this_member in lobby_members:
			# Make sure this isn't your Steam ID
			if this_member['steam_id'] != steam_id:

				# Close the P2P session
				Steam.closeP2PSessionWithUser(this_member['steam_id'])

		# Clear the local lobby list
		lobby_members.clear()
```

This will inform Steam you have left the lobby then clear your `lobby_id` variable, as well as your `lobby_members` array after it closes your P2P sessions with all players in the lobby. You'll notice at this point we don't have any functions to handle invites through Steam. This will be added in the second half of the lobby tutorial at a later time.

{==
## Up Next
==}

That concludes the lobby tutorial. At this point you may want to [check out the P2P tutorial which compliments this one](p2p.md). Obviously this code should not be used for production and more for a very, very, very, simple guide on where to start.

{==
## Additional Resources
==}

### Video Tutorials

Prefer video tutorials? Feast your eyes and ears!

[ :simple-youtube: 'Godot Tutorial: GodotSteam Lobby System' by DawnsCrow Games](https://youtu.be/si50G3S1XGU){ .md-button .md-button--resource target="\_blank" }

### Related Projects

[ :simple-github: 'GodotSteamHL' by JDare](https://github.com/JDare/GodotSteamHL){ .md-button .md-button--resource target="\_blank" }

### Example Project

[To see this tutorial in action, check out our GodotSteam Example Project on GitHub.](https://github.com/CoaguCo-Industries/GodotSteam-Example-Project){ target="\_blank" } There you can get a full view of the code used which can serve as a starting point for you to branch out from.
