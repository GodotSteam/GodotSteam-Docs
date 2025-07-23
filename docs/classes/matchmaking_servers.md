---
title: Matchmaking Servers
description: A class reference for Matchmaking Servers.
icon: material/server-plus
---

# Matchmaking Servers

Functions which provide access to the game server browser. See [Game Servers](https://partner.steamgames.com/doc/features/multiplayer/game_servers){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### cancelQuery

!!! function "cancelQuery( `uint64_t` server_list_request = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_list_request | uint64_t | The handle to the server list request. Defaults to 0. |

	Cancel an outstanding server list request.

	You should call this to cancel any in-progress requests before destructing a callback object that may have been passed to one of the below request calls. Not doing so may result in a crash when a callback occurs on the destructed object. Canceling a query does not release the allocated request handle. The request handle must be released using [releaseRequest](#releaserequest).

	You can pass a **server_list_request** handle or, if you do not, it will use the last internally stored one.

	!!! returns "Returns: void"
		No notes.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#CancelQuery){ .md-button .md-button--doc_classes target="_blank" }

### cancelServerQuery

!!! function "cancelServerQuery( `int` server_query )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_query | int | The server query to cancel.

	Cancel an outstanding individual server query.

	The calls that create this type of query are: [pingServer](#pingserver), [playerDetails](#playerdetails), and [serverRules](#serverrules). You should call this to cancel any in-progress requests before destructing a callback object that may have been passed to one of the above calls to avoid crashing when callbacks occur.

	!!! returns "Returns: void"
		No notes.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#CancelServerQuery){ .md-button .md-button--doc_classes target="_blank" }

### getServerCount

!!! function "getServerCount( `uint64_t` server_list_request = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_list_request | uint64_t | The handle to the server list request. Defaults to 0. |

	Gets the number of servers in the given list. This is used for iterating with [getServerDetails](#getserverdetails).

	You can pass a **server_list_request** handle or, if you do not, it will use the last internally stored one.

	!!! returns "Returns: int"
		The number of servers in a server list request object. Returns 0 if **server_list_request** is invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#GetServerCount){ .md-button .md-button--doc_classes target="_blank" }

### getServerDetails

!!! function "getServerDetails( `int` server, `uint64_t` server_list_request = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server | int | The index of the server to get the details of, from 0 to [getServerCount](#getservercount). |
    | server_list_request | uint64_t | The handle to the server list request. Defaults to 0. |

    Get the details of a given server in the list.

	You can get the valid range of index values by calling [getServerCount](#getservercount). You will also receive index values in [request_server_list_server_responded](#request_server_list_server_responded) callbacks.

	You can pass a **server_list_request** handle or, if you do not, it will use the last internally stored one.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		* ping (int)
		* success_response (bool)
		* no_refresh (bool)
		* game_dir (string)
		* map (string)
		* description (string)
		* app_id (uint32_t)
		* players (int)
		* max_players (int)
		* bot_players (int)
		* password (bool)
		* secure (bool)
		* last_played (uint32_t)
		* server_version (int)	

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#GetServerDetails){ .md-button .md-button--doc_classes target="_blank" }

### isRefreshing

!!! function "isRefreshing( `uint64_t` server_list_request = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_list_request | uint64_t | The handle to the server list request. Defaults to 0. |

    Checks if the server list request is currently refreshing.

	You can pass a *server_list_request* handle or, if you do not, it will use the last internally stored one.

	!!! returns "Returns: bool"
		Returns true if **server_list_request** is valid and is currently refreshing; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#IsRefreshing){ .md-button .md-button--doc_classes target="_blank" }

### pingServer

!!! function "pingServer( `string` ip, `uint16` port )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | ip | string | The IP of the game server you are querying in host order. |
    | port | uint16 | The port of the game server you are querying, in host order. |

	Queries an individual game server directly via IP/Port to request an updated ping time and other details from the server.

	!!! returns "Returns: int"
		Handle to the outstanding server query.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#PingServer){ .md-button .md-button--doc_classes target="_blank" }

### playerDetails

!!! function "playerDetails( `string` ip, `uint16` port )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | ip | string | The IP of the game server you are querying in host order. |
    | port | uint16 | The port of the game server you are querying, in host order. |

	Queries an individual game server directly via IP/Port to request the list of players currently playing on the server.

	!!! returns "Returns: int"
		Handle to the outstanding server query.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#PlayerDetails){ .md-button .md-button--doc_classes target="_blank" }

### refreshQuery

!!! function "refreshQuery( `uint64_t` server_list_request = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_list_request | uint64_t | The handle to the server list request. Defaults to 0. |

    Ping every server in your list again but don't update the list of servers.

	The query callback installed when the server list was requested will be used again to post notifications and [request_server_list_refresh_complete](#request_server_list_refresh_complete) will be called again, so the callback must remain valid until it completes or the request is released with [releaseRequest](#releaserequest).

	You can pass a **server_list_request** handle or, if you do not, it will use the last internally stored one.

	!!! returns "Returns: void"
		No notes.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#RefreshQuery){ .md-button .md-button--doc_classes target="_blank" }

### refreshServer

!!! function "refreshServer( `int` server, `uint64_t` server_list_request = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server | int | The index of the server to get the details of, from 0 to [getServerCount](#getservercount). |
    | server_list_request | uint64_t | The handle to the server list request. Defaults to 0. |

	Refreshes a single server inside of a query.

	If you want to refresh all of the servers then you should use [refreshQuery](#refreshquery).

	You can pass a **server_list_request** handle or, if you do not, it will use the last internally stored one.

	!!! returns "Returns: void"
		No notes.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#RefreshServer){ .md-button .md-button--doc_classes target="_blank" }

### releaseRequest

!!! function "releaseRequest( `uint64_t` server_list_request = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | server_list_request | uint64_t | The asynchronous server list request object to release. Defaults to 0. |

	Releases the asynchronous server list request object and cancels any pending query on it if there's a pending query in progress.

	You can pass a **server_list_request** handle or, if you do not, it will use the last internally stored one.

	!!! returns "Returns: void"
		No notes.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#ReleaseRequest){ .md-button .md-button--doc_classes target="_blank" }

### requestFavoritesServerList

!!! function "requestFavoritesServerList( `uint32_t` app_id, `array` filters )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app to get the server list of. |
    | filters | array | An array of filters to only retrieve servers the user cares about. |

    Request a new list of game servers from the 'favorites' server list.  This request object must be released by calling [releaseRequest](#releaserequest) when you are done using it.

	[See MatchMakingKeyValuePair_t for more information.](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#MatchMakingKeyValuePair_t){ target="_blank" }

	!!! returns "Returns: uint64_t"
		A new asynchronous server list request object.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#RequestFavoritesServerList){ .md-button .md-button--doc_classes target="_blank" }

### requestFriendsServerList

!!! function "requestFriendsServerList( `uint32_t` app_id, `array` filters )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app to get the server list of. |
    | filters | array | An array of filters to only retrieve servers the user cares about. |

    Request a new list of game servers from the 'friends' server list.  This request object must be released by calling [releaseRequest](#releaserequest) when you are done using it.

	[See MatchMakingKeyValuePair_t for more information.](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#MatchMakingKeyValuePair_t){ target="_blank" }

	!!! returns "Returns: uint64_t"
		A new asynchronous server list request object.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#RequestFriendsServerList){ .md-button .md-button--doc_classes target="_blank" }

### requestHistoryServerList

!!! function "requestHistoryServerList( `uint32_t` app_id, `array` filters )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app to get the server list of. |
    | filters | array | An array of filters to only retrieve servers the user cares about. |

    Request a new list of game servers from the 'history' server list.  This request object must be released by calling [releaseRequest](#releaserequest) when you are done using it.

	[See MatchMakingKeyValuePair_t for more information.](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#MatchMakingKeyValuePair_t){ target="_blank" }

	!!! returns "Returns: uint64_t"
		A new asynchronous server list request object.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#RequestHistoryServerList){ .md-button .md-button--doc_classes target="_blank" }

### requestInternetServerList

!!! function "requestInternetServerList( `uint32_t` app_id, `array` filters )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app to get the server list of. |
    | filters | array | An array of filters to only retrieve servers the user cares about. |

    Request a new list of game servers from the 'internet' server list.  This request object must be released by calling [releaseRequest](#releaserequest) when you are done using it.

	[See MatchMakingKeyValuePair_t for more information.](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#MatchMakingKeyValuePair_t){ target="_blank" }

	!!! returns "Returns: uint64_t"
		This returns a new asynchronous server list request object.

	!!! trigger "Triggers"
		[lobby_chat_update](matchmaking.md#lobby_chat_update) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#RequestInternetServerList){ .md-button .md-button--doc_classes target="_blank" }

### requestLANServerList

!!! function "requestLANServerList( `uint32_t` app_id )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app to get the server list of. |

    Request a new list of game servers from the 'LAN' server list.  This request object must be released by calling [releaseRequest](#releaserequest) when you are done using it.

	[See MatchMakingKeyValuePair_t for more information.](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#MatchMakingKeyValuePair_t){ target="_blank" }

	!!! returns "Returns: uint64_t"
		A new asynchronous server list request object.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#RequestLANServerList){ .md-button .md-button--doc_classes target="_blank" }

### requestSpectatorServerList

!!! function "requestSpectatorServerList( `uint32_t` app_id, `array` filters )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app to get the server list of. |
    | filters | array | An array of filters to only retrieve servers the user cares about. |

    Request a new list of game servers from the 'spectator' server list.  This request object must be released by calling [releaseRequest](#releaserequest) when you are done using it.

	[See MatchMakingKeyValuePair_t for more information.](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#MatchMakingKeyValuePair_t){ target="_blank" }

	!!! returns "Returns: uint64_t"
		A new asynchronous server list request object.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#RequestSpectatorServerList){ .md-button .md-button--doc_classes target="_blank" }

### serverRules

!!! function "serverRules( `string` ip, `uint16` port )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | ip | string | The IP of the game server you are querying in host order. |
    | port | uint16 | The port of the game server you are querying, in host order. |

	Queries an individual game server directly via IP/Port to request the list of rules that the server is running. [(See setKeyValue to set the rules on the server side.)](https://partner.steamgames.com/doc/api/ISteamGameServer#SetKeyValue){ target="\_blank" }

	!!! returns "Returns: int"
		Handle to the outstanding server query.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMatchmakingServers#ServerRules){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to run `Steam.run_callbacks()` in your `_process()` function to receive them.

### ping_server_failed_to_respond

!!! function "ping_server_failed_to_respond"
	The server has failed to respond to a ping request.

	!!! returns "Returns"
		Nothing.

### ping_server_responded

!!! function "ping_server_responded"
	The server has responded to a ping request.

	!!! returns "Returns"
		* server_details (dictionary)

		Contains the following keys:

		* name (string)
		* connection_address (string)
		* query_address (string)
		* ping (int)
		* success_response (bool)
		* no_refresh (bool)
		* game_dir (string)
		* map (string)
		* description (string)
		* app_id (int)
		* players (int)
		* max_players (int)
		* bot_players (int)
		* password (bool)
		* secure (bool)
		* last_played (int)
		* server_version (int)
		* game_tags (string)
		* steam_id (uint64_t)

### player_details_failed_to_respond

!!! function "player_details_failed_to_respond"
	The server failed to respond to a player details request.

	!!! returns "Returns"
		Nothing.

### player_details_player_added

!!! function "player_details_player_added"
	The server has responded to a player details request.

	!!! returns "Returns"
		* name (string)
		* score (int)
		* time_played (float)

### player_details_refresh_complete

!!! function "player_details_refresh_complete"
	The server has completed a player details request.

	!!! returns "Returns"
		Nothing.

### request_server_list_refresh_complete

!!! function "request_server_list_refresh_complete"
	A server list request has completed.

	!!! returns "Returns"
		* request_handle (uint64_t)
		* response (int / [MatchMakingServerResponse enum](#matchmakingserverresponse))

### request_server_list_server_failed_to_respond

!!! function "request_server_list_server_failed_to_respond"
	A server failed to respond to a list request.

	!!! returns "Returns"
		* request_handle (uint64_t)
		* server (int)

### request_server_list_server_responded

!!! function "request_server_list_server_responded"
	A server has responded to a list request.

	!!! returns "Returns"
		* request_handle (uint64_t)
		* server (int)

### server_rules_failed_to_respond

!!! function "server_rules_failed_to_respond"
	The server failed to respond with a rules request.

	!!! returns "Returns"
		Nothing.

### server_rules_refresh_complete

!!! function "server_rules_refresh_complete"
	The server has completed a rules refresh.

	!!! returns "Returns"
		Nothing.

### server_rules_responded

!!! function "server_rules_responded"
	The server responded with a rules request.

	!!! returns "Returns"
		* rule (string)
		* value (string)

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
MAX_GAME_SERVER_GAME_DATA | k_cbMaxGameServerGameData | 2048 | -
MAX_GAME_SERVER_GAME_DESCRIPTION | k_cbMaxGameServerGameDescription | 64 | -
MAX_GAME_SERVER_GAME_DIR | k_cbMaxGameServerGameDir | 32 | -
MAX_GAME_SERVER_MAP_NAME | k_cbMaxGameServerMapName | 32 | -
MAX_GAME_SERVER_NAME | k_cbMaxGameServerName | 64 | -
MAX_GAME_SERVER_TAGS | k_cbMaxGameServerTags | 128 | -

{==
## :material-numeric: Enums
==}

### MatchMakingServerResponse

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
SERVER_RESPONDED | eServerResponded | 0 | -
SERVER_FAILED_TO_RESPOND | eServerFailedToRespond | 1 | -
NO_SERVERS_LISTED_ON_MASTER_SERVER | eNoServersListedOnMasterServer | 2 | For the Internet query type; returned in response callback if no servers of this type match.
