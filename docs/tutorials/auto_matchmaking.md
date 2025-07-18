---
title: Lobby Auto-Matchmaking
description: A guide on setting up automatic matchmaking for lobbies.
icon: material/account-group
---

# Tutorials - Lobby Auto-Matchmaking
:material-badge-account-horizontal: _By Gramps_

---

A quick, requested tutorial is auto-matchmaking lobbies. While this example does not show how to match specific criteria, it will be noted where you can place such matches. This tutorial is basically an extension of the [lobbies](lobbies.md) and the various networking tutorials. Because of that we'll focus only on what's different; please refer to the aforementioned tutorials for additional information and layouts.

??? guide "Relevant GodotSteam classes and functions"
	* [Matchmaking class](../classes/matchmaking.md)
		* [addRequestLobbyListDistanceFilter()](../classes/matchmaking.md#addrequestlobbylistdistancefilter)
		* [getLobbyData()](../classes/matchmaking.md#getlobbydata)
		* [getNumLobbyMembers()](../classes/matchmaking.md#getnumlobbymembers)
		* [joinLobby()](../classes/matchmaking.md#joinlobby)
		* [requestLobbyList()](../classes/matchmaking.md#requestlobbylist)

!!! warning "Note"
	You may want to [double-check our Initialization tutorial](initializing.md) to set up initialization and callbacks functionality if you haven't done so already.

{==
## Set Up
==}

First let's set up some variables to fill in later:

```gdscript
var lobby_id: int = 0
var lobby_max_players: int = 2
var lobby_members: Array = []
var matchmaking_phase: int = 0
```

Carried over from the lobby tutorial is `lobby_id`, which obviously houses the lobby's ID, and `lobby_members`, which will be an array of dictionaries of lobby members and their Steam ID 64's.

New to this tutorial is `matchmaking_phase` which keeps track of which iteration of the matchmaking search our code does. Also new is `lobby_max_players` which is used to check if the lobby has space for our player.

All of the `_ready()` function callback connections will be the same.

{==
## Finding a Lobby
==}

For our purposes, we will create a button named "Auto Matchmake" and connect a `on_pressed` signal to it called `_on_auto_matchmake_pressed()`. Here is the function that triggers when that button is pressed:

```gdscript
# Start the auto matchmaking process.
func _on_auto_matchmake_pressed() -> void:
	# Set the matchmaking process over
	matchmake_phase = 0

	# Start the loop!
	matchmaking_loop()
```			

This starts the main loop that looks for a matching lobby for your player to join:

```gdscript
# Iteration for trying different distances
func matchmaking_loop() -> void:
	# If this matchmake_phase is 3 or less, keep going
	if matchmake_phase < 4:
		###
		# Add other filters for things like game modes, etc.
		# Since this is an example, we cannot set game mode or text match features.
		# However you could use addRequestLobbyListStringFilter to look for specific
		# text in lobby metadata to match different criteria.

		###
		# Set the distance filter
		Steam.addRequestLobbyListDistanceFilter(matchmake_phase)

		# Request a list
		Steam.requestLobbyList()

	else:
		print("[STEAM] Failed to automatically match you with a lobby. Please try again.")
```

As it notes in the code above, you could have a list of different filters the player can pick from before searching for a lobby. These can be applied to terms that `addRequestLobbyListStringFilter()` looks for ahead of time. Things like game modes, maps, difficulties, etc.

Very important is our `addRequestLobbyListDistanceFilter()` and `matchmake_phase` variable. We are starting with "close" / 0 and ending with "worldwide" 3; hence at 4 it fails to find anything and prompts the user to try again.

This loop function will trigger a callback once it finds some lobbies to check through. Sorting through our matches should look like this:

```gdscript
# A lobby list was created, find a possible lobby
func _on_lobby_match_list(lobbies: Array) -> void:
	# Set attempting_join to false
	var attempting_join: bool = false

	# Show the list 
	for this_lobby in lobbies:
		# Pull lobby data from Steam
		var lobby_name: String = Steam.getLobbyData(this_lobby, "name")
		var lobby_nums: int = Steam.getNumLobbyMembers(this_lobby)

		###
		# Add other filters for things like game modes, etc.
		# Since this is an example, we cannot set game mode or text match features.
		# However, much like lobby_name, you can use Steam.getLobbyData to get other
		# preset lobby defining data to append to the next if statement.
		###

		# Attempt to join the first lobby that fits the criteria
		if lobby_nums < lobby_max_players and not attempting_join:
			# Turn on attempting_join
			attempting_join = true
			print("Attempting to join lobby...")
			Steam.joinLobby(this_lobby)

	# No lobbies that matched were found, go onto the next phase
	if not attempting_join:
		# Increment the matchmake_phase
		matchmake_phase += 1
		matchmaking_loop()
```

This will go through every lobby returned and, if none of them match, it will iterate the `matchmake_phase` variable and start the loop again but moving one step further in the distance filter.

Much like the previous additional filters, you can sort by other lobby data in the manner the `lobby_name` data is acquired. These bits of information can be added to a lobby when it is created, making this process easier.

The first lobby that matches your criteria and has space for the user, triggers the `joinLobby()` function to fire and the player should soon join their automatically found lobby.

{==
## :material-content-save-settings: Additional Resources
==}

### Example Project

[Later this year you can see this tutorial in action with more in-depth information by checking out our upcoming free-to-play game Skillet on GitHub.](https://github.com/GodotSteam/Skillet){ target="\_blank" } There you will be able to view of the code used which can serve as a starting point for you to branch out from.