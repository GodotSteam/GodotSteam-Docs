---
title: Matchmaking
description: A class reference for Matchmaking.
icon: fontawesome/solid/users-rectangle
---

# Matchmaking

Functions for clients to access matchmaking services, favorite servers, and to operate on game lobbies. See [Steam Matchmaking & Lobbies](https://partner.steamgames.com/doc/features/multiplayer/matchmaking){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### addFavoriteGame

!!! function "addFavoriteGame( `string` ip, `uint16` port, `uint16` queryPort, `uint32_t` flags, `uint32_t` lastPlayed )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | ip | string | The IP address of the server in host order, i.e 127.0.0.1. |
    | port | uint16 | The port used to connect to the server, in host order. |
    | query_port | uint16 | The port used to query the server, in host order. |
    | flags | uint32_t | Sets the whether the server should be added to the favorites list or history list. See [FAVORITE_FLAG_NONE](#constants) for more information. |
    | last_played | uint32_t | This should be the current time in Unix epoch format (seconds since Jan 1st, 1970). |

	Adds the game server to the local favorites list or updates the time played of the server if it already exists in the list.  Uses the current game's app ID under-the-hood.

	!!! returns "Returns: int"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#AddFavoriteGame){ .md-button .md-button--doc_classes target="_blank" }

### addRequestLobbyListDistanceFilter

!!! function "addRequestLobbyListDistanceFilter( `int` distance_filter )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | distance_filter | int | Specifies the maximum distance. |

    Sets the physical distance for which we should search for lobbies, this is based on the user's IP address and an IP location map on the Steam backend.

	See the [LobbyDistanceFilter enum](#lobbydistancefilter) for possible values to pass in.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#AddRequestLobbyListDistanceFilter){ .md-button .md-button--doc_classes target="_blank" }

### addRequestLobbyListFilterSlotsAvailable

!!! function "addRequestLobbyListFilterSlotsAvailable( `int` slots_available )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | slots_available | int | The number of open slots that must be open. |

    Filters to only return lobbies with the specified number of open slots available.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#AddRequestLobbyListFilterSlotsAvailable){ .md-button .md-button--doc_classes target="_blank" }

### addRequestLobbyListNearValueFilter

!!! function "addRequestLobbyListNearValueFilter( `string` key_to_match, `int` value_to_be_close_to )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | key_to_match | string | The filter key name to match. This can not be longer than [MAX_LOBBY_KEY_LENGTH](#constants). |
    | value_to_match | int | The value that lobbies will be sorted on. |

	Sorts the results closest to the specified value.

	Near filters don't actually filter out values, they just influence how the results are sorted. You can specify multiple near filters, with the first near filter influencing the most, and the last near filter influencing the least.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#AddRequestLobbyListNearValueFilter){ .md-button .md-button--doc_classes target="_blank" }

### addRequestLobbyListNumericalFilter

!!! function "addRequestLobbyListNumericalFilter( `string` key_to_match, `int` value_to_match, `int` comparison_type )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | key_to_match | string | The filter key name to match. This can not be longer than [MAX_LOBBY_KEY_LENGTH](#constants). |
    | value_to_match | int | The number to match. |
    | comparison_type | [LobbyComparison enum](#lobbycomparison) | The type of comparison to make. |

	Adds a numerical comparison filter to the next [requestLobbyList](#requestlobbylist) call.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#AddRequestLobbyListNumericalFilter){ .md-button .md-button--doc_classes target="_blank" }

### addRequestLobbyListResultCountFilter

!!! function "addRequestLobbyListResultCountFilter( `int` max_results )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | max_results | int | The maximum number of lobbies to return. |

    Sets the maximum number of lobbies to return. The lower the count the faster it is to download the lobby results & details to the client.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#AddRequestLobbyListResultCountFilter){ .md-button .md-button--doc_classes target="_blank" }

### addRequestLobbyListStringFilter

!!! function "addRequestLobbyListStringFilter( `string` key_to_match, `string` value_to_match, `int` comparison_type )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | key_to_match | string | The filter key name to match. This can not be longer than [MAX_LOBBY_KEY_LENGTH](#constants). |
    | value_to_match | string | The string to match. |
    | comparison_type | [LobbyComparison enum](#lobbycomparison) | The type of comparison to make. |

	Adds a string comparison filter to the next [requestLobbyList](#requestlobbylist) call.  This compares the text in your lobby data.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#AddRequestLobbyListStringFilter){ .md-button .md-button--doc_classes target="_blank" }

### createLobby

!!! function "createLobby( `LobbyType` lobby_type, `int` max_members )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | lobby_type | [LobbyType enum](#lobbytype) | The type and visibility of this lobby. This can be changed later via [setLobbyType](#setlobbytype). |
    | max_members | int | The maximum number of players that can join this lobby. This can not be above 250. |

	Create a lobby on the Steam servers.

	If private, then the lobby will not be returned by any [requestLobbyList](#requestlobbylist) call; the lobby ID of the lobby will need to be communicated via game channels or via [inviteUserToLobby](#inviteusertolobby).

	This is an asynchronous request. Results will be returned by [lobby_created](#lobby_created) callback and call result; lobby is joined and ready to use at this point. A [lobby_joined](#lobby_joined) callback will also be received since the local user is joining their own lobby.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		All three callbacks:

		* [lobby_created](#lobby_created)
		* [lobby_joined](#lobby_joined)
		* [lobby_data_update](#lobby_data_update)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#CreateLobby){ .md-button .md-button--doc_classes target="_blank" }

### deleteLobbyData

!!! function "deleteLobbyData( `uint64_t` steam_lobby_id, `string` key )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to delete the metadata for. |
    | key | string | The key to delete the data for. |

    Removes a metadata key from the lobby.

	This can only be done by the owner of the lobby.

	This will only send the data if the key existed. There is a slight delay before sending the data so you can call this repeatedly to set all the data you need to and it will automatically be batched up and sent after the last sequential call.

	!!! returns "Returns: bool"
		This will be true if the key/value was successfully deleted; otherwise it will be false. It may also be false if the **steam_lobby_id** or **key** are invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#DeleteLobbyData){ .md-button .md-button--doc_classes target="_blank" }

### getAllLobbyData

!!! function "getAllLobbyData( `uint64_t` steam_lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to get all metadata for. |

	Get lobby data by the lobby's ID.

	!!! returns "Returns: dictionary"
		Containing the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| index | int | The index for this metadata.
		| key | string | The key name for this metadata.
		| value | string | The value for that key.

	!!! info "Notes"
		This is a GodotSteam specific function.

### getFavoriteGames

!!! function "getFavoriteGames( )"
	Gets the details of the favorite game servers.

	!!! returns "Returns: array of dictionaries"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| ret | bool | Did this call succeed?
		| app | uint32_t | The App ID this server is for.
		| ip | string | The IP address of the server in host order, ie. 127.0.0.1
		| game_port | uint16 | The port used to connect to the server, in host order.
		| query_port | uint16 | The port used to query the server, in host order.
		| flags | uint32_t | Whether the server is on the favorites list or the history list. See [FAVORITE_FLAG_NONE](#constants) for more information.
		| last_played | uint32_t | The time the server was last added to the favorites list in Unix epoch format (seconds since Jan 1st, 1970).

	!!! info "Notes"
		This function is unique to GodotSteam; incorporates [GetFavoriteGame](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetFavoriteGame){ target="\blank" } and [GetFavoriteGameCount](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetFavoriteGameCount){ target="\blank" } under-the-hood.

### getLobbyData

!!! function "getLobbyData( `uint64_t` steam_lobby_id, `string` key )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to get the metadata from.
    | key | string | The key to get the value of.

	Gets the metadata associated with the specified key from the specified lobby.

	This can only get metadata from lobbies that the client knows about, either after receiving a list of lobbies from [lobby_match_list](#lobby_match_list), retrieving the data with [requestLobbyData](#requestlobbydata), or after joining a lobby.

	!!! returns "Returns: string"
		Returns an empty string if no value is set for this key, or if **steam_lobby_id** is invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetLobbyData){ .md-button .md-button--doc_classes target="_blank" }

### getLobbyGameServer

!!! function "getLobbyGameServer( `uint64_t` steam_lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to get the game server information from.

    Gets the details of a game server set in a lobby. Either the IP/Port or the Steam ID of the game server has to be valid, depending on how you want the clients to be able to connect.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| ret | bool | True if the lobby is valid and has a valid game server set; otherwise, false.
		| ip | string | The IP address of the game server, in host order, i.e 127.0.0.1.
		| port | uint32_t | The connection port of the game server, in host order, if it's set.
		| id | uint64_t | The Steam ID of the game server, if it's set.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetLobbyGameServer){ .md-button .md-button--doc_classes target="_blank" }

### getLobbyMemberByIndex

!!! function "getLobbyMemberByIndex( `uint64_t` steam_lobby_id, `int` member )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | This **must** be the same lobby used in the previous call to [getNumLobbyMembers](#getnumlobbymembers). |

	Gets the Steam ID of the lobby member at the given index. The current user must be in the lobby to retrieve the Steam IDs of other users in that lobby.

	You must call [getNumLobbyMembers](#getnumlobbymembers) before calling this.

	!!! returns "Returns: int"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetLobbyMemberByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getLobbyMemberData

!!! function "getLobbyMemberData( `uint64_t` steam_lobby_id, `uint64_t` steam_user_id, `string` key )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby that the other player is in. |
    | steam_user_id | uint64_t | The Steam ID of the player to get the metadata from. |
    | key | string | The key to get the value of. |

    Gets per-user metadata from another player in the specified lobby.

	This can only be queried from members in lobbies that you are currently in.

	!!! returns "Returns: string"
		Returns an empty string if:

		* **steam_lobby_id** is invalid
		* **steam_user_id** is not in the lobby
		* **key** is not set for the player

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetLobbyMemberData){ .md-button .md-button--doc_classes target="_blank" }

### getLobbyMemberLimit

!!! function "getLobbyMemberLimit( `uint64_t` steam_lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to get the member limit of. |

    The current limit on the number of users who can join the lobby.

	!!! returns "Returns: int"
		Will return 0 if no limit is defined or there is no metadata available for the specified lobby.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetLobbyMemberLimit){ .md-button .md-button--doc_classes target="_blank" }

### getLobbyOwner

!!! function "getLobbyOwner( `uint64_t` steam_lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to get the owner of. |

    Returns the current lobby owner. You must be a member of the lobby to access this.

	There always one lobby owner - if the current owner leaves, another user in the lobby will become the owner automatically. It is possible (but rare) to join a lobby just as the owner is leaving, thus entering a lobby with self as the owner.

	!!! returns "Returns: uint64_t"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetLobbyOwner){ .md-button .md-button--doc_classes target="_blank" }

### getNumLobbyMembers

!!! function "getNumLobbyMembers( `uint64_t` steam_lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to get the number of members of. |

	Gets the number of users in a lobby. The current user must be in the lobby to retrieve the Steam ID's of other users in that lobby.

	This is used for iteration. After calling this then [getLobbyMemberByIndex](#getlobbymemberbyindex) can be used to get the Steam ID of each person in the lobby. Persona information for other lobby members (name, avatar, etc.) is automatically received and accessible via the [**Friends** class](friends.md).

	!!! returns "Returns: int"
		Returns the number of members in the lobby or 0 if the current user has no data from the lobby.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#GetNumLobbyMembers){ .md-button .md-button--doc_classes target="_blank" }

### inviteUserToLobby

!!! function "inviteUserToLobby( `uint64_t` steam_lobby_id, `uint64_t` steam_id_invitee )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to invite the user to. |
    | steam_id_invitee | uint64_t | The Steam ID of the person who will be invited. |

	Invite another user to the lobby.

	If the specified user clicks the join link, a [join_requested](friends.md#join_requested) callback will be posted if the user is in-game, or if the game isn't running yet then the game will be automatically launched with the command line parameter **+connect_lobby <64-bit lobby Steam ID>** instead.

	**Notes:** This call does not check if the other user was successfully invited.

	!!! returns "Returns: bool"
		Returns true if the invite was successfully sent. It will be false if:

		* the local user isn't in a lobby, no connection
		* no connection to Steam could be made
		* the specified user is invalid

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#InviteUserToLobby){ .md-button .md-button--doc_classes target="_blank" }

### joinLobby

!!! function "joinLobby( `uint64_t` steam_lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to join. |

	Joins an existing lobby.

	The lobby Steam ID can be obtained either from a search with [requestLobbyList](#requestlobbylist) joining on a friend, or from an invite.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		* [lobby_joined](#lobby_joined) callback for yourself
		* [lobby_chat_update](#lobby_chat_update) callback for any other users already in the lobby

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#JoinLobby){ .md-button .md-button--doc_classes target="_blank" }

### leaveLobby

!!! function "leaveLobby( `uint64_t` steam_lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to leave. |

	Leave a lobby that the user is currently in; this will take effect immediately on the client side, other users in the lobby will be notified by a [lobby_chat_update](#lobby_chat_update) callback.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[lobby_chat_update](#lobby_chat_update) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LeaveLobby){ .md-button .md-button--doc_classes target="_blank" }

### removeFavoriteGame

!!! function "removeFavoriteGame( `uint32_t` app_id, `string` ip, `uint16` port, `uint16` query_port, `uint32_t` flags )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID of the game. |
    | ip | string | The IP address of the server in host order, i.e 127.0.0.1. |
    | port | uint16 | The port used to connect to the server, in host order. |
    | query_port | uint16 | The port used to query the server, in host order. |
    | flags | uint32_t | Sets the whether the server should be added to the favorites list or history list. See [FAVORITE_FLAG_NONE](#constants) for more information. |

	Removes the game server from the local storage; returns true if one was removed.

	!!! returns "Returns: bool"
		Returns true if the server was removed; otherwise, false if the specified server was not on the user's local favorites list.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#RemoveFavoriteGame){ .md-button .md-button--doc_classes target="_blank" }

### requestLobbyData

!!! function "requestLobbyData( `uint64_t` steam_lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to refresh the metadata of. |

	Refreshes all of the metadata for a lobby that you're not in right now.

	You will never do this for lobbies you're a member of; that data will always be up to date. You can use this to refresh lobbies that you have obtained from [requestLobbyList](#requestlobbylist) or that are available via friends.

	!!! returns "Returns: bool"
		Returns true if the request was successfully sent to the server. False if no connection to Steam could be made, or **steam_lobby_id** is invalid.

	!!! trigger "Triggers"
		[lobby_data_update](#lobby_data_update) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#RequestLobbyData){ .md-button .md-button--doc_classes target="_blank" }

### requestLobbyList

!!! function "requestLobbyList( )"
	Get a filtered list of relevant lobbies.  This will only return lobbies that are not full, and only lobbies that are **[LOBBY_TYPE_PUBLIC (2)](#lobbytype)** or **[LOBBY_TYPE_INVISIBLE (3)](#lobbytype)**, and are set to joinable with [setLobbyJoinable](#setlobbyjoinable).

	There can only be one active lobby search at a time. The old request will be canceled if a new one is started. Depending on the user's connection to the Steam back-end, this call can take from 300ms to 5 seconds to complete, and has a timeout of 20 seconds.

	To filter the results you _must_ call the **addRequestLobbyList** functions before calling this. The filters are cleared on each call to this function.  The available lobby filters are:

	* [addRequestLobbyListDistanceFilter](#addrequestlobbylistdistancefilter)
	* [addRequestLobbyListFilterSlotsAvailable](#addrequestlobbylistfilterslotsavailable)
	* [addRequestLobbyListNearValueFilter](#addrequestlobbylistnearvaluefilter)
	* [addRequestLobbyListNumericalFilter](#addrequestlobbylistnumericalfilter)
	* [addRequestLobbyListResultCountFilter](#addrequestlobbylistresultcountfilter)
	* [addRequestLobbyListStringFilter](#addrequestlobbyliststringfilter)

	If [addRequestLobbyListDistanceFilter](#addrequestlobbylistdistancefilter) is not called, **[LOBBY_DISTANCE_FILTER_DEFAULT (1)](#lobbydistancefilter)** will be used, which will only find matches in the same or nearby regions.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[lobby_match_list](#lobby_match_list) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#RequestLobbyList){ .md-button .md-button--doc_classes target="_blank" }

### sendLobbyChatMsg

!!! function "sendLobbyChatMsg( `uint64_t` steam_lobby_id, `string` message_body )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to send the chat message to. |
    | message_body | string | The text to send to the lobby. |

	Broadcasts a chat (text or binary data) message to the all of the users in the lobby.
	All users in the lobby (including the local user) will receive a [lobby_message](#lobby_message) callback with the message.

	For communication that needs to be arbitrated (for example having a user pick from a set of characters, and making sure only one user has picked a character), you can use the lobby owner as the decision maker. [getLobbyOwner](#getlobbyowner) returns the current lobby owner. There is guaranteed to always be one and only one lobby member who is the owner. So for the choose-a-character scenario, the user who is picking a character would send the binary message 'I want to be Zoe', the lobby owner would see that message, see if it was OK, and broadcast the appropriate result (user X is Zoe).

	These messages are sent via the Steam back-end, and so the bandwidth available is limited. For higher-volume traffic like voice or game data, you'll want to use the Steam Networking API.

	!!! returns "Returns: bool"
		Returns true if the message was succesfully sent. May return false if:

		* the message was too small
		* the message was too large
		* no connection to Steam could be made

	!!! trigger "Triggers"
		[lobby_message](#lobby_message) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#SendLobbyChatMsg){ .md-button .md-button--doc_classes target="_blank" }

### setLobbyData

!!! function "setLobbyData( `uint64_t` steam_lobby_id, `string` key, `string` value )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to set the metadata for. |
    | key | string | The key to set the data for. This can not be longer than [MAX_LOBBY_KEY_LENGTH](#constants). |
    | value | string | The value to set. This can not be longer than [CHAT_METADATA_MAX](friends.md#constants). |

    Sets a key/value pair in the lobby metadata. This can be used to set the the lobby name, current map, game mode, etc.

	This can only be set by the owner of the lobby. Lobby members should use [setLobbyMemberData](#setlobbymemberdata) instead. To reset a key, just set it to "".

	Each user in the lobby will be receive notification of the lobby data change via a [lobby_data_update](#lobby_data_update) callback, and any new users joining will receive any existing data.

	This will only send the data if it has changed. There is a slight delay before sending the data so you can call this repeatedly to set all the data you need to and it will automatically be batched up and sent after the last sequential call.

	!!! returns "Returns: bool"
		Returns true if the data has been set successfully. False if **steam_lobby_id** was invalid or the key/value are too long.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#SetLobbyData){ .md-button .md-button--doc_classes target="_blank" }

### setLobbyGameServer

!!! function "setLobbyGameServer( `uint64_t` steam_lobby_id, `string` server_ip, `uint16` server_port, `uint64_t` steam_id_game_server )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to set the game server information for. |
    | server_ip | string | Sets the IP address of the game server, in host order. Defaults to "0". |
    | server_port | uint16 | Sets the connection port of the game server, in host order. Defaults to 0. |
    | steam_id_game_server | uint64_t | uint64_t | Sets the Steam ID of the game server. Use 0 if you're not setting this. Defaults to 0. |

	Sets the game server associated with the lobby. Usually at this point, the users will join the specified game server.

	This can only be set by the owner of the lobby.

	Either the IP/Port or the Steam ID of the game server must be valid, depending on how you want the clients to be able to connect.

	A [lobby_game_created](#lobby_game_created) callback will be sent to all players in the lobby, usually at this point, the users will join the specified game server.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[lobby_game_created](#lobby_game_created) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#SetLobbyGameServer){ .md-button .md-button--doc_classes target="_blank" }

### setLobbyJoinable

!!! function "setLobbyJoinable( `uint64_t` steam_lobby_id, `bool` joinable )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby. |
    | joinable | bool | Enable (true) or disable (false) allowing users to join this lobby? |

    Sets whether or not a lobby is joinable by other players. This always defaults to enabled for a new lobby.

	If joining is disabled, then no players can join, even if they are a friend or have been invited.

	Lobbies with joining disabled will not be returned from a lobby search.

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false if you're not the owner of the lobby.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#SetLobbyJoinable){ .md-button .md-button--doc_classes target="_blank" }

### setLobbyMemberData

!!! function "setLobbyMemberData( `uint64_t` steam_lobby_id, `string` key, `string` value )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to set our metadata in. |
    | key | string | The key to set the data for. This can not be longer than [MAX_LOBBY_KEY_LENGTH](#constants). |
    | value | string | The value to set. This can not be longer than [CHAT_METADATA_MAX](friends.md#constants). |

    Sets per-user metadata for the local user.

	Each user in the lobby will be receive notification of the lobby data change via a [lobby_data_update](#lobby_data_update) callback, and any new users joining will receive any existing data.

	There is a slight delay before sending the data so you can call this repeatedly to set all the data you need to and it will automatically be batched up and sent after the last sequential call.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[lobby_data_update](#lobby_data_update) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#SetLobbyMemberData){ .md-button .md-button--doc_classes target="_blank" }

### setLobbyMemberLimit

!!! function "setLobbyMemberLimit( `uint64_t` steam_lobby_id, `int` max_members )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to set the member limit for. |
    | max_members | int | The maximum number of players allowed in this lobby. This can not be above 250. |

    Set the maximum number of players that can join the lobby.

	This is also set when you create the lobby with [createLobby](#createlobby).

	This can only be set by the owner of the lobby.

	!!! returns "Returns: bool"
		Returns true if the limit was successfully set; otherwise, false if you are not the owner of the specified lobby.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#SetLobbyMemberLimit){ .md-button .md-button--doc_classes target="_blank" }

### setLobbyOwner

!!! function "setLobbyOwner( `uint64_t` steam_lobby_id, `uint64_t` steam_id_new_owner )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby where the owner change will take place. |
    | steam_id_new_owner | uint64_t | 	The Steam ID of the user that will be the new owner of the lobby, they must be in the lobby. |

    Changes who the lobby owner is.

	This can only be set by the owner of the lobby. This will trigger a [lobby_data_update](#lobby_data_update) for all of the users in the lobby, each user should update their local state to reflect the new owner. This is typically accomplished by displaying a crown icon next to the owners name.

	!!! returns "Returns: bool"
		Returns true if the owner was successfully changed. If may return false if:

		* if you're not the current owner of the lobby
		* **steam_id_new_owner** is not a member in the lobby
		* no connection to Steam could be made

	!!! trigger "Triggers"
		[lobby_data_update](#lobby_data_update) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#SetLobbyOwner){ .md-button .md-button--doc_classes target="_blank" }

### setLobbyType

!!! function "setLobbyType( `uint64_t` steam_lobby_id, `LobbyType` lobby_type )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_lobby_id | uint64_t | The Steam ID of the lobby to set the type of. |
    | lobby_type | [LobbyType enum](#lobbytype) | The new lobby type to that will be set. |

    Updates what type of lobby this is.

	This is also set when you create the lobby with [createLobby](#createlobby).

	This can only be set by the owner of the lobby.

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false if you are not the owner of the lobby.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#SetLobbyType){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### favorites_list_accounts_updated

!!! function "favorites_list_accounts_updated"
	Called when an account on your favorites list is updated.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| result | [Result enum](main.md#result) | The result of the favorites list update. 

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#FavoritesListAccountsUpdated_t){ .md-button .md-button--doc_classes target="_blank" }

### favorites_list_changed

!!! function "favorites_list_changed"
	A server was added/removed from the favorites list, you should refresh now.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| favorite | dictionary | Data about the favorite server.

		**favorite** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| ip | string | An IP of 0 means reload the whole list, any other value means just one server. This is host order; ie. 127.0.0.1
		| query_port | uint32_t | The new servers query port, in host order.
		| connection_port | uint32_t | The new servers connection port, in host order.
		| app_id | uint32_t | The App ID the game server belongs to.
		| flags | uint32_t | Whether the the server is on the favorites list or the history list. See [FAVORITE_FLAG_NONE](#constants) for more information.
		| add | bool | Whether the server was added to the list (true) or removed (false) from it.
		| account_id | uint32_t | -

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#FavoritesListChanged_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_chat_update

!!! function "lobby_chat_update"
	A lobby chat room state has changed, this is usually sent when a user has joined or left the lobby.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| lobby_id | uint64_t | The Steam ID of the lobby.
		| changed_id | uint64_t | The user who's status in the lobby just changed - can be recipient.
		| making_change_id | uint64_t | Chat member who made the change. This can be different from **changed_id** if kicking, muting, etc. For example, if one user kicks another from the lobby, this will be set to the id of the user who initiated the kick.
		| chat_state | uint32_t | Bitfield of [ChatMemberStateChange enum](#chatmemberstatechange) values.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyChatUpdate_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_created

!!! function "lobby_created"
	Result of our request to create a lobby. At this point, the lobby has been joined and is ready for use. A [lobby_joined](#lobby_joined) callback will also be received (since the user is joining their own lobby).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| connect | [Result enum](main.md#result) | The result of the operation.
		| lobby | uint64_t | The Steam ID of the lobby that was created, 0 if failed.

		Possible failure responses:

		* [RESULT_OK](main.md#result) - The lobby was successfully created.
	    * [RESULT_FAIL](main.md#result) - The server responded, but with an unknown internal error.
	    * [RESULT_TIMEOUT](main.md#result) - The message was sent to the Steam servers, but it didn't respond.
	    * [RESULT_LIMIT_EXCEEDED](main.md#result) - Your game client has created too many lobbies and is being rate limited.
	    * [RESULT_ACCESS_DENIED](main.md#result) - Your game isn't set to allow lobbies, or your client does haven't rights to play the game
	    * [RESULT_NO_CONNECTION](main.md#result) - Your Steam client doesn't have a connection to the back-end.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyCreated_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_data_update

!!! function "lobby_data_update"
	The lobby metadata has changed.

	If **member_id** is a user in the lobby, then use [getLobbyMemberData](#getlobbymemberdata) to access per-user details; otherwise, if **member_id == lobby_id**, use [getLobbyData](#getlobbydata) to access the lobby metadata.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| success | uint8 | True if the lobby data was successfully changed; otherwise, false.
		| lobby_id | uint64_t | The Steam ID of the lobby.
		| member_id | uint64_t | Steam ID of either the member whose data changed or the room itself.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyDataUpdate_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_game_created

!!! function "lobby_game_created"
	A game server has been set via [setLobbyGameServer](#setlobbygameserver) for all of the members of the lobby to join. It's up to the individual clients to take action on this; the typical game behavior is to leave the lobby and connect to the specified game server; but the lobby may stay open throughout the session if desired.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| lobby_id | uint64_t | TThe lobby that set the game server.
		| server_id | uint64_t | The Steam ID of the game server, if it's set.
		| server_ip | uint32_t | The IP address of the game server in host order; ie. 127.0.0.1.
		| port | uint16 | The connection port of the game server, in host order, if it's set.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyGameCreated_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_invite

!!! function "lobby_invite"
	Someone has invited you to join a lobby. Normally you don't need to do anything with this, as the Steam UI will also display a 'user has invited you to the lobby, join?' notification and message.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| inviter | uint64_t | Steam ID of the person that sent the invite.
		| lobby | uint64_t | Steam ID of the lobby we're invited to.
		| game | uint64_t | Game ID of the lobby we're invited to.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyGameCreated_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_joined

!!! function "lobby_joined"
	Recieved upon attempting to enter a lobby. Lobby metadata is available to use immediately after receiving this.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| lobby_id | uint64_t | The Steam ID of the lobby you have entered.
		| permissions | uint32_t |  - unused; always 0
		| locked | bool | If true, then only invited users may join.
		| response | [ChatRoomEnterResponse enum](main.md#chatroomenterresponse) | This will be set to [CHAT_ROOM_ENTER_RESPONSE_SUCCESS](main.md#chatroomenterresponse) if the lobby was successfully joined, otherwise it will be [CHAT_ROOM_ENTER_RESPONSE_ERROR](main.md#chatroomenterresponse).

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyEnter_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_kicked

!!! function "lobby_kicked"
	Posted if a user is forcefully removed from a lobby; can occur if a user loses connection to Steam.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| lobby_id | uint64_t | The Steam ID of the lobby.
		| admin_id | uint64_t | User who kicked you - possibly the ID of the lobby itself
		| due_to_disconnect | uint8 | True if you were kicked from the lobby due to the user losing connection to Steam; currently always true.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyKicked_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_match_list

!!! function "lobby_match_list"
	Result when requesting the lobby list.

	!!! returns "Returns: array"
		A list of uint64_t lobby ID's.

	!!! info "Notes"
		With the GDNative plug-in, this callback will also send back the lobby count as an integer; this fixes a very strange issue with GDNative mangling the lobbies array.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyMatchList_t){ .md-button .md-button--doc_classes target="_blank" }

### lobby_message

!!! function "lobby_message"
	A chat (text or binary) message for this lobby has been received.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| lobby_id | uint64_t | The Steam ID of the lobby this message was sent in.
		| user | uint64_t | The user who's status in the lobby just changed - can be recipient.
		| buffer | string | The message data.
		| chat_type | [ChatEntryType enum](main.md#chatentrytype) | Type of message received.

	!!! info "Notes"
		Uses getLobbyChatEntry under-the-hood.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmaking#LobbyChatMsg_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
FAVORITE_FLAG_FAVORITE | k_unFavoriteFlagFavorite | 0x01 | This game favorite entry is for the favorites list.
FAVORITE_FLAG_HISTORY | k_unFavoriteFlagHistory | 0x02 | This game favorite entry is for the history list.
FAVORITE_FLAG_NONE | k_unFavoriteFlagNone | 0x00 | -
MAX_LOBBY_KEY_LENGTH | k_nMaxLobbyKeyLength | 255 | Maximum number of characters a lobby metadata key can be.
SERVER_QUERY_INVALID | HSERVERQUERY_INVALID | 0xffffffff | -

{==
## :material-numeric: Enums
==}

### ChatMemberStateChange

Flags describing how a users lobby state has changed. This is provided from [lobby_chat_update](#lobby_chat_update).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CHAT_MEMBER_STATE_CHANGE_ENTERED | k_EChatMemberStateChangeEntered | 0x0001 | This user has joined or is joining the lobby.
CHAT_MEMBER_STATE_CHANGE_LEFT | k_EChatMemberStateChangeLeft | 0x0002 | This user has left or is leaving the lobby.
CHAT_MEMBER_STATE_CHANGE_DISCONNECTED | k_EChatMemberStateChangeDisconnected | 0x0004 | User disconnected without leaving the lobby first.
CHAT_MEMBER_STATE_CHANGE_KICKED | k_EChatMemberStateChangeKicked | 0x0008 | The user has been kicked.
CHAT_MEMBER_STATE_CHANGE_BANNED | k_EChatMemberStateChangeBanned | 0x0010 | The user has been kicked and banned.

### LobbyComparison

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LOBBY_COMPARISON_EQUAL_TO_OR_LESS_THAN | k_ELobbyComparisonEqualToOrLessThan | -2 | -
LOBBY_COMPARISON_LESS_THAN | k_ELobbyComparisonLessThan | -1 | -
LOBBY_COMPARISON_EQUAL | k_ELobbyComparisonEqual | 0 | -
LOBBY_COMPARISON_GREATER_THAN | k_ELobbyComparisonGreaterThan | 1 | -
LOBBY_COMPARISON_EQUAL_TO_GREATER_THAN | k_ELobbyComparisonEqualToOrGreaterThan | 2 | -
LOBBY_COMPARISON_NOT_EQUAL | k_ELobbyComparisonNotEqual | 3 | -

### LobbyDistanceFilter

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LOBBY_DISTANCE_FILTER_CLOSE | k_ELobbyDistanceFilterClose | 0 | Only lobbies in the same immediate region will be returned.
LOBBY_DISTANCE_FILTER_DEFAULT | k_ELobbyDistanceFilterDefault | 1 | Only lobbies in the same region or near by regions.
LOBBY_DISTANCE_FILTER_FAR | k_ELobbyDistanceFilterFar | 2 | For games that don't have many latency requirements; will return lobbies about half-way around the globe.
LOBBY_DISTANCE_FILTER_WORLDWIDE | k_ELobbyDistanceFilterWorldwide | 3 | No filtering; will match lobbies as far as India to NY. Not recommended; expect multiple seconds of latency between the clients.

### LobbyType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LOBBY_TYPE_PRIVATE | k_ELobbyTypePrivate | 0 | Only way to join the lobby is to invite to someone else.
LOBBY_TYPE_FRIENDS_ONLY | k_ELobbyTypeFriendsOnly | 1 | Shows for friends or invitees, but not in lobby list.
LOBBY_TYPE_PUBLIC | k_ELobbyTypePublic | 2 | Visible for friends and in lobby list.
LOBBY_TYPE_INVISIBLE | k_ELobbyTypeInvisible | 3 |  returned by search, but not visible to other friends useful if you want a user in two lobbies. For example, matching groups together a user can be in only one regular lobby and up to two invisible lobbies.
LOBBY_TYPE_PRIVATE_UNIQUE | k_ELobbyTypePrivateUnique | 4 | Private, unique, and does not delete when empty. Only one of these may exist per unique keypair set can only create from Web API.