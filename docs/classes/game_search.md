---
title: Game Search
description: A class reference for Game Search.
icon: octicons/search-24
---

# Game Search

These functions are not listed in the official Steamworks SDK documentation but do exist in the SDK in the **isteammatchmaking.h** file.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### acceptGame

!!! function "acceptGame( )"
    After receiving [search_for_game_result](#search_for_game_result), accept or decline the game. Multiple [search_for_game_result](#search_for_game_result) will follow as players accept game until the host starts or cancels the game.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### addGameSearchParams

!!! function "addGameSearchParams( `string` key, `string` value )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | key | string | The key to search for. |
    | value | string | The values to search for. |

	A keyname and a list of comma separated values: one of which is must be found in order for the match to qualify; fails if a search is currently in progress.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### cancelRequestPlayersForGame

!!! function "cancelRequestPlayersForGame( )"
	Cancel request and leave the pool of game hosts looking for players. If a set of players has already been sent to host, all players will receive **SearchForGameHostFailedToConfirm_t**. However, **SearchForGameHostFailedToConfirm_t** does not seem to exist anywhere in the SDK.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### declineGame

!!! function "declineGame( )"
	After receiving [search_for_game_result](#search_for_game_result), accept or decline the game. Multiple [search_for_game_result](#search_for_game_result) will follow as players accept game until the host starts or cancels the game.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### endGame

!!! function "endGame( `uint64_t` game_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | game_id | uint64_t | The ID of the game to end. |

    Ends the game. No further [submitPlayerResult](#submitplayerresult) for **game_id** will be accepted.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### endGameSearch

!!! function "endGameSearch( )"
	Leaves queue if still waiting.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### hostConfirmGameStart

!!! function "hostConfirmGameStart( `uint64_t` game_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | game_id | uint64_t | The ID of the game to start. |

    Accept the player list and release connection details to players. Players will only be given connection details and host steamid when this is called. Allows host to accept after all players confirm, some confirm, or none confirm. The decision is entirely up to the host.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### requestPlayersForGame

!!! function "requestPlayersForGame( `int` player_min, `int` player_max, `int` max_team_size )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | player_min | int | The minimum number of players to request. |
    | player_max | int | The maximum number of players to request. |
    | max_team_size | int | The maximum number of players per team. |

	Mark server as available for more players with **player_min**, **player_max** desired. Accept no lobbies with player-count greater than **max_team_size**.  The set of lobbies returned must be partitionable into teams of no more than **max_team_size**.

	[request_players_for_game_result](#request_players_for_game_result) callback will be sent when the search has started. Multple [request_players_for_game_result](#request_players_for_game_result) callbacks will follow when players are found.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### retrieveConnectionDetails

!!! function "retrieveConnectionDetails( `uint64_t` host_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | host_id | uint64_t | The Steam ID of the host. |

    After receiving **GameStartedByHostCallback_t** get connection details to server.

	**Note:** The **GameStartedByHostCallback_t** callback does not seem to exist in the SDK anywhere.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [GameSearchErrorCode enum](#gamesearcherrorcode) | 
        | details | string | 

### searchForGameSolo

!!! function "searchForGameSolo( `int` player_min, `int` player_max)"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | player_min | int | The minimum number of player slots open. |
    | player_max | int | The maximum number of player slots open. |

	User enter the queue and await a [search_for_game_progress](#search_for_game_progress) callback. fails if another search is currently in progress. Periodic callbacks will be sent as queue time estimates change.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### searchForGameWithLobby

!!! function "searchForGameWithLobby( `uint64_t` lobby_id, `int` player_min, `int` player_max)"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | lobby_id | uint64_t | The ID of the lobby the player is in. |
    | player_min | int | The minimum number of player slots open. |
    | player_max | int | The maximum number of player slots open. |

	All players in lobby enter the queue and await a [search_for_game_progress](#search_for_game_progress) callback. Fails if another search is currently in progress. If not the owner of the lobby or search already in progress this call fails. Periodic callbacks will be sent as queue time estimates change.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### setConnectionDetails

!!! function "setConnectionDetails( `string` connection_details )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | connection_details | string | A connection string with connection details, similar to lobby joins. |

	Set connection details for players once game is found so they can connect to this server.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### setGameHostParams

!!! function "setGameHostParams( `string` key, `string` value )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | key | string | The key to set for the game. |
    | value | string | The list of values to set for the game. |

    A keyname and a list of comma separated values: all the values you allow.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

### submitPlayerResult

!!! function "submitPlayerResult( `uint64_t` game_id, `uint64_t` player_id, `int` player_result )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | game_id | uint64_t | The ID For the game session. |
    | player_id | uint64_t | The Steam ID for the player whom we are submitting results for. |
    | player_result | [PlayerResult enum](#playerresult) | The player result to submit. |

	Submit a result for one player; does not end the game. **game_id** continues to describe this game.

	!!! returns "Returns: [GameSearchErrorCode enum](#gamesearcherrorcode)"

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### end_game_result

!!! function "end_game_result"
	This callback confirms that the game is recorded as complete on the matchmaking service, the next call to [requestPlayersForGame](#requestplayersforgame) will generate a new unique game ID.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
		| game_id | uint64_t | The unique ID for this game and call.

### request_players_for_game_final_result

!!! function "request_players_for_game_final_result"
	There are no notes about this in Valve's header files or documentation.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | **search_id** will be non-zero if this is [RESULT_OK](main.md#result).
		| search_id | uint64_t | All future callbacks referencing this search will include this search ID.
		| game_id | uint64_t | The unique ID for the game and call.

### request_players_for_game_progress

!!! function "request_players_for_game_progress"
	Callback from [requestPlayersForGame](#requestplayersforgame) when the matchmaking service has started or ended search; callback will also follow a call from [cancelRequestPlayersForGame](#cancelrequestplayersforgame) - **search_in_progress** will be false.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | **search_id** will be non-zero if this is [RESULT_OK](main.md#result).
		| search_id | uint64_t | All future callbacks referencing this search will include this search ID.

### request_players_for_game_result

!!! function "request_players_for_game_result"
	Callback from [requestPlayersForGame](#requestplayersforgame), one of these will be sent per player followed by additional callbacks when players accept or decline the game.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | **search_id** will be non-zero if this is [RESULT_OK](main.md#result).
		| search_id | uint64_t | All future callbacks referencing this search will include this search ID.
		| player_data | dictionary | Information about the player.

		**player_data** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | player_id | uint64_t | Player Steam ID.
		| lobby_id | uint64_t | If the player is in a lobby, the lobby ID.
		| player_accept_state | [PlayerAcceptState enum](#playeracceptstate) | If the player has accepted or not. 
		| player_index | int32 | Used for iterating the players.
		| total_players | int32 | Expect this many callbacks at minimum.
		| total_players_accepted_game | int32 | Total number of players who has accepted.
		| suggested_team_index | int32 | The index for the team this player should be placed on.
		| unique_game_id | uint64_t | The unique game ID.

### search_for_game_progress

!!! function "search_for_game_progress"
	There are no notes about this in Valve's header files or documentation.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | If search has started this result will be [RESULT_OK](main.md#result); any other value indicates search has failed to start or has terminated.
		| search_id | uint64_t | All future callbacks referencing this search will include this search ID.
		| search_progress | dictionary | Information about the search.

		**search_progress** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | lobby_id | uint64_t | If lobby search; invalid Steam ID otherwise.
		| ended_search_id | uint64_t | If search was terminated, Steam ID that terminated search.
		| seconds_remaining_estimate | int32 | The seconds left until the search ends.
		| players_searching | int32 | Number of players searching for games.

### search_for_game_result

!!! function "search_for_game_result"
	Notification to all players searching that a game has been found.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | If game / host was lost this will be an error value.
		| search_id | uint64_t | All future callbacks referencing this search will include this search ID.
		| search_result | dictionary | Information about the search.

		**search_result** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | count_players_ingame | int32 | If **result** is RESULT_OK this will be non-zero.
		| count_accepted_game | int32 | If **result** is RESULT_OK this will be non-zero.
		| host_id | uint64_t | If valid, the host has started the game.
		| final_callback | bool | Whether this is the final callback or not.

### submit_player_result

!!! function "submit_player_result"
	This callback confirms that results were received by the matchmaking service for this player.

	!!! returns "Returns"
          | Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
		| game_id | uint64_t | The unique game ID.
		| player_id | uint64_t | The Steam ID of this player.

{==
## :material-numeric: Enums
==}

### GameSearchErrorCode

Found in steamclientpublic.h.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
GAME_SEARCH_ERROR_CODE_OK | k_EGameSearchErrorCode_OK | 1 | -
GAME_SEARCH_ERROR_CODE_SEARCH_AREADY_IN_PROGRESS | k_EGameSearchErrorCode_Failed_Search_Already_In_Progress | 2 | -
GAME_SEARCH_ERROR_CODE_NO_SEARCH_IN_PROGRESS | k_EGameSearchErrorCode_Failed_No_Search_In_Progress | 3 | -
GAME_SEARCH_ERROR_CODE_NOT_LOBBY_LEADER | k_EGameSearchErrorCode_Failed_Not_Lobby_Leader | 4 | If not the lobby leader can not call [searchForGameWithLobby](#searchforgamewithlobby
).
GAME_SEARCH_ERROR_CODE_NO_HOST_AVAILABLE | k_EGameSearchErrorCode_Failed_No_Host_Available | 5 | No host is available that matches those search params.
GAME_SEARCH_ERROR_CODE_SEARCH_PARAMS_INVALID | k_EGameSearchErrorCode_Failed_Search_Params_Invalid | 6 | Search params are invalid.
GAME_SEARCH_ERROR_CODE_OFFLINE | k_EGameSearchErrorCode_Failed_Offline | 7 | Offline, could not communicate with server.
GAME_SEARCH_ERROR_CODE_NOT_AUTHORIZED | k_EGameSearchErrorCode_Failed_NotAuthorized | 8 | Either the user or the application does not have priveledges to do this.
GAME_SEARCH_ERROR_CODE_UNKNOWN_ERROR | k_EGameSearchErrorCode_Failed_Unknown_Error | 9 | Unknown error.

### PlayerAcceptState

Found in isteammatchmaking.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
PLAYER_ACCEPT_STATE_UNKNOWN | k_EStateUnknown | 0 | -
PLAYER_ACCEPT_STATE_ACCEPTED | k_EStatePlayerAccepted | 1 | -
PLAYER_ACCEPT_STATE_DECLINED | k_EStatePlayerDeclined | 2 | -

### PlayerResult

Found in steamclientpublic.h.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
PLAYER_RESULT_FAILED_TO_CONNECT | k_EPlayerResultFailedToConnect | 1 | Failed to connect after confirming.
PLAYER_RESULT_ABANDONED | k_EPlayerResultAbandoned | 2 | Quit game without completing it.
PLAYER_RESULT_KICKED | k_EPlayerResultKicked | 3 | Kicked by other players/moderator/server rules.
PLAYER_RESULT_INCOMPLETE | k_EPlayerResultIncomplete | 4 | Player stayed to end but game did not conclude successfully (no fault to player).
PLAYER_RESULT_COMPLETED | k_EPlayerResultCompleted | 5 | Player completed game.
