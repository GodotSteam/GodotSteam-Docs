---
title: Exporting and Shipping
description: A guide on exporting and shipping your game on Steam.
icon: fontawesome/solid/users-rectangle
---

# Tutorials - Friends' Lobbies
:material-badge-account-horizontal: _By polypi_

---

In this quick tutorial we will cover how to get a dictionary of any lobbies belonging to friends that are visible to the user. This was created as it's a pretty common feature for multiplayer games.

??? guide "Relevant GodotSteam classes and functions"
	* [Friends class](../classes/friends.md)
		* [getFriendByIndex](../classes/friends.md#getfriendbyindex)
		* [getFriendCount](../classes/friends.md#getfriendcount)
		* [getFriendGamePlayed](../classes/friends.md#getfriendgameplayed)
	* [Matchmaking class](../classes/matchmaking.md)
		* [createLobby](../classes/matchmaking.md#createlobby)
	* [Utils class](../classes/utils.md)
		* [getAppID](../classes/utils.md#getappid)

{==
## Functions Explained
==}

We will break down the different Steam functions to call first before looking at the code samples.

```gdscript
var number_of_friends: int = Steam.getFriendCount(Steam.FRIEND_FLAG_IMMEDIATE)
```

This must be run before `getFriendByIndex()` [according to the Steamworks documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendByIndex){ target="\_blank" }. We'll be using this to iterate over each friend in our friends list.

```gdscript
var steam_id: int = Steam.getFriendByIndex(index, Steam.FRIEND_FLAG_IMMEDIATE)
```

Inside of the loop we'll call `getFriendByIndex()` to get the Steam ID of our friend at this index.

!!! warning "Notes"
	You must use the same friend flag as the one used when calling `getFriendCount()`. Refer to the [FriendFlags enum](../classes/friends.md#friendflags) for valid options and [the Steamworks documentation on the friend flags](https://partner.steamgames.com/doc/api/ISteamFriends#EFriendFlags){ target="\_blank" } for what they mean.

```gdscript
var game_info: Dictionary = Steam.getFriendGamePlayed(steam_id)
```

With our friend's Steam ID we can now get information about the game our friend may or may not be playing.

This dictionary will be empty if the friend is offline or not playing a game. Otherwise, here are the fields and possible values inside this dictionary:

- **id**: int
	- The app ID of the game they are playing.
- **lobby**: int ***or*** String
	- If the friend is in a lobby visible to this user, it will be an int of the Lobby ID
	- Otherwise, it will be a String equal to: `"No valid lobby"`
- **ip**: String
- **game_port**: int
- **query_port**: int
	- Depends on the lobby settings, but `ip`, `game_port`, and `query_port` will probably all just be zeros.

{==
## Putting It All Together
==}

```gdscript
func get_lobbies_with_friends() -> Dictionary:
	var results: Dictionary = {}

	for i in range(0, Steam.getFriendCount()):
		var steam_id: int = Steam.getFriendByIndex(i, Steam.FRIEND_FLAG_IMMEDIATE)
		var game_info: Dictionary = Steam.getFriendGamePlayed(steam_id)

		if game_info.empty():
			# This friend is not playing a game
			continue
		else:
			# They are playing a game, check if it's the same game as ours
			var app_id: int = game_info['id']
			var lobby = game_info['lobby']
			
			if app_id != Steam.getAppID() or lobby is String:
				# Either not in this game, or not in a lobby
				continue

			if not results.has(lobby):
				results[lobby] = []

			results[lobby].append(steam_id)

	return results
```

If you would rather get back a dictionary of `friend_id -> lobby_id` you can instead use:

```gdscript
func get_friends_in_lobbies() -> Dictionary:
	var results: Dictionary = {}

	for i in range(0, Steam.getFriendCount()):
		var steam_id: int = Steam.getFriendByIndex(i, Steam.FRIEND_FLAG_IMMEDIATE)
		var game_info: Dictionary = Steam.getFriendGamePlayed(steam_id)

		if game_info.empty():
			# This friend is not playing a game
			continue
		else:
			# They are playing a game, check if it's the same game as ours
			var app_id: int = game_info['id']
			var lobby = game_info['lobby']

			if app_id != Steam.getAppID() or lobby is String:
				# Either not in this game, or not in a lobby
				continue

			results[steam_id] = lobby

	return results
```

From here you can create your UI however you like, and simply call `joinLobby(lobby_id)` when the user has made a selection.

{==
## Checking if a Friend Is Still in a Lobby
==}

In the likely case that you are not running `get_lobbies_with_friends()` every frame, there's a small chance a user might click on a lobby a friend has since left, or worse, a lobby that no longer exists.

This is a little check you can do before joining the lobby:

```gdscript
# Check if a friend is in a lobby
func is_a_friend_still_in_lobby(steam_id: int, lobby_id: int) -> bool:
	var game_info: Dictionary = Steam.getFriendGamePlayed(steam_id)
	
	if game_info.empty():
		return false

	# They are in a game
	var app_id: int = game_info.id
	var lobby = game_info.lobby

	# Return true if they are in the same game and have the same lobby_id
	return app_id == Steam.getAppID() and lobby is int and lobby == lobby_id
```

{==
## Troubleshooting
==}

If you don't see any lobbies while testing, make sure to check the flags for how the lobby was created.

For instance, you won't be able to see a lobby with this flag:

```gdscript
Steam.create_lobby(Steam.LOBBY_TYPE_INVISIBLE, 8)
```

If the lobby is configured in such a way that it's invisible to you, or at max capacity, then Steam won't provide a lobby ID.
