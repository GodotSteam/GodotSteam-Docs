---
title: Server
description: A class reference for Steam Server.
icon: material/server-network
---

# Main Server

!!! info "Only available in the [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### get_godotsteam_version

!!! function "get_godotsteam_version( )"
	Get the GodotSteam Server version you are currently using.

	!!! returns "Returns: string"

### get_inventory_handle

!!! function "get_inventory_handle( )"
	Gets the current, internally-stored inventory handle.

	!!! returns "Returns: int32"

### get_inventory_update_handle

!!! function "get_inventory_update_handle( )"
	Gets the current, internally-stored inventory update handle.

	!!! returns "Returns: uint64_t"

### getSteamID

!!! function "getSteamID( )"
	Gets the server's Steam ID.

	!!! returns "Returns: uint64_t"

### isAnonAccount

!!! function "isAnonAccount( `uint64_t` steam_id )"
	Is this Steam Id an anonymous account?

	!!! returns "Returns: bool"

### isAnonUserAccount

!!! function "isAnonUserAccount( `uint64_t` steam_id )"
	Is this Steam ID an anonymous user account?

	!!! returns "Returns: bool"

### isChatAccount

!!! function "isChatAccount( `uint64_t` steam_id )"
	Is this Steam ID a chat account?

	!!! returns "Returns: bool"

### isClanAccount

!!! function "isClanAccount( `uint64_t` steam_id )"
	Is this Steam ID a clan / group account?

	!!! returns "Returns: bool"

### isConsoleUserAccount

!!! function "isConsoleUserAccount( `uint64_t` steam_id )"
	Is this Steam ID a console user account?

	!!! returns "Returns: bool"

### isIndividualAccount

!!! function "isIndividualAccount( `uint64_t` steam_id )"
	Is this Steam ID an individual account?

	!!! returns "Returns: bool"

### isLobby

!!! function "isLobby( `uint64_t` steam_id )"
	Is this Steam ID a lobby?

	!!! returns "Returns: bool"

### isServerSecure

!!! function "isServerSecure( )"
	Checking if the server is secured or not.

	!!! returns "Returns: bool"

### run_callbacks

!!! function "run_callbacks( )"

### serverInit

!!! function "serverInit( ```string``` ip, ```uint16``` game_port, ```uint16``` query_port, ```int``` server_mode, ```string``` version_number )"
	Initialize SteamGameServer client and interface objects, and set server properties which may not be changed.

	After calling this function, you should set any additional server parameters, and then logOnAnonymous() or logOn().

	This function is included for compatibility with older SDK.  You can use it if you don't care about decent error handling

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_gameserver#SteamGameServer_Init){ .md-button .md-button--store target="_blank" }

### serverInitEx

!!! function "serverInitEx( ```string``` ip, ```uint16``` game_port, ```uint16``` query_port, ```int``` server_mode, ```string``` version_number )"
	Initialize SteamGameServer client and interface objects, and set server properties which may not be changed.

	After calling this function, you should set any additional server parameters and then [logOnAnonymous](#logonanonymous) or [logOn](#logon).

	Things to consider:

	* **ip** will usually be zero.  If you are on a machine with multiple IP addresses, you can pass a non-zero value here and the relevant sockets will be bound to that IP.  This can be used to ensure that the IP you desire is the one used in the server browser
	* **game_port** is the port that clients will connect to for gameplay.  You will usually open up your own socket bound to this port.
	* **query_port** is the port that will manage server browser related duties and info pings from clients.  If you pass [QUERY_PORT_SHARED](#constants) for **query_port**, then it will use "GameSocketShare" mode, which means that the game is responsible for sending and receiving UDP packets for the master  server updater.  See [handleIncomingPacket](game_server.md#handleincomingpacket) and [getNextOutgoingPacket](game_server.md#getnextoutgoingpacket).
	* **version_number** should be in the form x.x.x.x, and is used by the master server to detect when the server is out of date.  Only servers with the latest version will be listed.

	On success STEAM_API_INIT_RESULT_OK is returned.  Otherwise, if error_message is non-NULL, it will receive a non-localized message that explains the reason for the failure.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| status | [SteamAPIInitResult enum](#steamapiinitresult) | Result of the initialization.
    	| verbal | string | A non-localized message that explains the reason for the failure.

### serverReleaseCurrentThreadMemory

!!! function "serverReleaseCurrentThreadMemory()"
	Frees all API-related memory associated with the calling thread.

	This memory is released automatically by [run_callbacks](#run_callbacks) so single-threaded servers do not need to call this.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_gameserver#SteamGameServer_ReleaseCurrentThreadMemory){ .md-button .md-button--store target="_blank" }

### serverShutdown

!!! function "serverShutdown()"
	Shut down the server connection to Steam.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_gameserver#SteamGameServer_Shutdown){ .md-button .md-button--store target="_blank" }

### set_inventory_handle

!!! function "set_inventory_handle( `int32` new_inventory_handle )"
	Set the current, internally-stored inventory handle.

	!!! returns "Returns: void"

### set_inventory_update_handle

!!! function "set_inventory_update_handle( `uint64_t` new_inventory_update_handle )"
	Set the current, internally-stored inventory update handle.

	!!! returns "Returns: void"

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Notes
---- | -------- | ----- | -----
ACCOUNT_ID_INVALID | k_uAccountIdInvalid | 0 | An invalid account ID.
API_CALL_INVALID | k_uAPICallInvalid | 0x0 | An invalid Steam API Call handle.
APP_ID_INVALID | k_uAppIdInvalid | 0x0 | An invalid app ID.
AUTH_TICKET_INVALID | k_HAuthTicketInvalid | An invalid user authentication ticket.
DEPOT_ID_INVALID | k_uDepotIdInvalid | 0x0 | An invalid depot ID.
GAME_EXTRA_INFO_MAX | k_cchGameExtraInfoMax | 64 | The maximum size (in UTF-8 bytes, including the null terminator) of the **extra_info** parameter of trackAppUsageEvent (since been deprecated).
INVALID_BREAKPAD_HANDLE | BREAKPAD_INVALID_HANDLE | (BREAKPAD_HANDLE)0 | -
MAX_GAME_SERVER_GAME_DATA | k_cbMaxGameServerGameData | 2048 | Max size of the GameServer game data.
MAX_GAME_SERVER_GAME_DESCRIPTION | k_cbMaxGameServerGameDescription | 64 | Max size of the GameServer game description.
MAX_GAME_SERVER_GAME_DIR | k_cbMaxGameServerGameDir | 32 | Max size of the GameServer game directory.
MAX_GAME_SERVER_MAP_NAME | k_cbMaxGameServerMapName | 32 | Max size of the GameServer map name.
MAX_GAME_SERVER_NAME | k_cbMaxGameServerName | 64 | Max size of the GameServer name.
MAX_GAME_SERVER_TAGS | k_cbMaxGameServerTags | 128 | Max size of the GameServer tags.
MAX_STEAM_DATAGRAM_GAME_COORDINATOR_SERVER_LOGIN_APP_DATA | k_cbMaxSteamDatagramGameCoordinatorServerLoginAppData | 2048 | -
MAX_STEAM_DATAGRAM_GAME_COORDINATOR_SERVER_LOGIN_SERIALIZED | k_cbMaxSteamDatagramGameCoordinatorServerLoginSerialized | 4096 | -
PARTY_BEACON_ID_INVALID k_ulPartyBeaconIdInvalid | 0 | An invalid party beacon ID.
QUERY_PORT_ERROR | QUERY_PORT_ERROR | 0xFFFE | We were unable to get the query port for this server.
QUERY_PORT_NOT_INITIALIZED | QUERY_PORT_NOT_INITIALIZED | 0xFFFF | We haven't asked the GameServer for this query port's actual value yet.
QUERY_PORT_SHARED | STEAMGAMESERVER_QUERY_PORT_SHARED | 0xffff | Pass to [serverInit](#serverinit) to indicate that the same UDP port will be used for game traffic UDP queries for server browser pings and LAN discovery.  In this case, Steam will not open up a socket to handle server browser queries, and you must use [handleIncomingPacket](#handleincomingpacket) and [getNextOutgoingPacket](#getnextoutgoingpacket) to handle packets related to server discovery on your socket.
STEAM_ACCOUNT_ID_MASK | k_unSteamAccountIDMask | 0xFFFFFFFF | Used in CSteamID to mask out the AccountID_t.
STEAM_ACCOUNT_INSTANCE_MASK | k_unSteamAccountInstanceMask | 0x000FFFFF | Used in CSteamID to mask out the account instance.
STEAM_BUFFER_SIZE | - | 255 | Custom for GodotSteam.
STEAM_DATAGRAM_MAX_SERIALIZED_TICKET k_cbSteamDatagramMaxSerializedTicket | 512 | -
STEAM_ID_NIL | k_steamIDNil | CSteamID() | Generic invalid CSteamID.
STEAM_ID_OUT_OF_DATE_GAME_SERVER | k_steamIDOutofDateGS | CSteamID() | This Steam ID comes from a user game connection to an out of date GS that hasnt implemented the protocol to provide its Steam ID.
STEAM_ID_LAN_MODE_GAME_SERVER | k_steamIDLanModeGS | CSteamID() | This Steam ID comes from a user game connection to an sv_lan GameServer.
STEAM_ID_NOT_INIT_YET_GAME_SERVER | k_steamIDNotInitYetGS | CSteamID() | This Steam ID can come from a user game connection to a GameServer that has just booted but hasnt yet even initialized its steam3 component and started logging on.
STEAM_ID_NON_GAME_SERVER | k_steamIDNonSteamGS | CSteamID() | This Steam ID can come from a user game connection to a GameServer that isn't using the steam authentication system but still wants to support the "Join Game" option in the friends list.
STEAM_LARGE_BUFFER_SIZE | - | 8160 | Custom for GodotSteam.
STEAM_MAX_ERROR_MESSAGE | k_cchMaxSteamErrMsg | 1024 | Maximum size of Steam error messages.
STEAM_USER_CONSOLE_INSTANCE | k_unSteamUserConsoleInstance | 2 | Used by CSteamID to identify users logged in from a console.
STEAM_USER_DEFAULT_INSTANCE | k_unSteamUserDefaultInstance | 1 | Used by CSteamID to identify users logged in from the desktop client.
STEAM_USER_WEB_INSTANCE | k_unSteamUserWebInstance | 4 | Used by CSteamID to identify users logged in from the web.

{==
## :material-numeric: Enums
==}

### AccountType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
ACCOUNT_TYPE_INVALID | 0
ACCOUNT_TYPE_INDIVIDUAL | 1
ACCOUNT_TYPE_MULTISEAT | 2
ACCOUNT_TYPE_GAME_SERVER | 3
ACCOUNT_TYPE_ANON_GAME_SERVER | 4
ACCOUNT_TYPE_PENDING | 5
ACCOUNT_TYPE_CONTENT_SERVER | 6
ACCOUNT_TYPE_CLAN | 7
ACCOUNT_TYPE_CHAT | 8
ACCOUNT_TYPE_CONSOLE_USER | 9
ACCOUNT_TYPE_ANON_USER | 10
ACCOUNT_TYPE_MAX | 11

### AuthSessionResponse

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
AUTH_SESSION_RESPONSE_OK | 0
AUTH_SESSION_RESPONSE_USER_NOT_CONNECTED_TO_STEAM | 1
AUTH_SESSION_RESPONSE_NO_LICENSE_OR_EXPIRED | 2
AUTH_SESSION_RESPONSE_VAC_BANNED | 3
AUTH_SESSION_RESPONSE_LOGGED_IN_ELSEWHERE | 4
AUTH_SESSION_RESPONSE_VAC_CHECK_TIMEDOUT | 5
AUTH_SESSION_RESPONSE_AUTH_TICKET_CANCELED | 6
AUTH_SESSION_RESPONSE_AUTH_TICKET_INVALID_ALREADY_USED | 7
AUTH_SESSION_RESPONSE_AUTH_TICKET_INVALID | 8
AUTH_SESSION_RESPONSE_PUBLISHER_ISSUED_BAN | 9

### BeginAuthSessionResult

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
BEGIN_AUTH_SESSION_RESULT_OK | 0
BEGIN_AUTH_SESSION_RESULT_INVALID_TICKET | 1
BEGIN_AUTH_SESSION_RESULT_DUPLICATE_REQUEST | 2
BEGIN_AUTH_SESSION_RESULT_INVALID_VERSION | 3
BEGIN_AUTH_SESSION_RESULT_GAME_MISMATCH | 4
BEGIN_AUTH_SESSION_RESULT_EXPIRED_TICKET | 5

### DenyReason

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
DENY_INVALID | 0
DENY_INVALID_VERSION | 1
DENY_GENERIC | 2
DENY_NOT_LOGGED_ON | 3
DENY_NO_LICENSE | 4
DENY_CHEATER | 5
DENY_LOGGED_IN_ELSEWHERE | 6
DENY_UNKNOWN_TEXT | 7
DENY_INCOMPATIBLE_ANTI_CHEAT | 8
DENY_MEMORY_CORRUPTION | 9
DENY_INCOMPATIBLE_SOFTWARE | 10
DENY_STEAM_CONNECTION_LOST | 11
DENY_STEAM_CONNECTION_ERROR | 12
DENY_STEAM_RESPONSE_TIMED_OUT | 13
DENY_STEAM_VALIDATION_STALLED | 14
DENY_STEAM_OWNER_LEFT_GUEST_USER | 15

### GameIDType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
GAME_TYPE_APP | 0
GAME_TYPE_GAME_MOD | 1
GAME_TYPE_SHORTCUT | 2
GAME_TYPE_P2P | 3

### Result

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
RESULT_OK | 1
RESULT_FAIL | 2
RESULT_NO_CONNECTION | 3
RESULT_INVALID_PASSWORD | 5
RESULT_LOGGED_IN_ELSEWHERE | 6
RESULT_INVALID_PROTOCOL_VER | 7
RESULT_INVALID_PARAM | 8
RESULT_FILE_NOT_FOUND | 9
RESULT_BUSY | 10
RESULT_INVALID_STATE | 11
RESULT_INVALID_NAME | 12
RESULT_INVALID_EMAIL | 13
RESULT_DUPLICATE_NAME | 14
RESULT_ACCESS_DENIED | 15
RESULT_TIMEOUT | 16
RESULT_BANNED | 17
RESULT_ACCOUNT_NOT_FOUND | 18
RESULT_INVALID_STEAM_ID | 19
RESULT_SERVICE_UNAVAILABLE | 20
RESULT_NOT_LOGGED_ON | 21
RESULT_PENDING | 22
RESULT_ENCRYPTION_FAILURE | 23
RESULT_INSUFFICIENT_PRIVILEGE | 24
RESULT_LIMIT_EXCEEDED | 25
RESULT_REVOKED | 26
RESULT_EXPIRED | 27
RESULT_ALREADY_REDEEMED | 28
RESULT_DUPLICATE_REQUEST | 29
RESULT_ALREADY_OWNED | 30
RESULT_IP_NOT_FOUND | 31
RESULT_PERSIST_FAILED | 32
RESULT_LOCKING_FAILED | 33
RESULT_LOG_ON_SESSION_REPLACED | 34
RESULT_CONNECT_FAILED | 35
RESULT_HANDSHAKE_FAILED | 36
RESULT_IO_FAILURE | 37
RESULT_REMOTE_DISCONNECT | 38
RESULT_SHOPPING_CART_NOT_FOUND | 39
RESULT_BLOCKED | 40
RESULT_IGNORED | 41
RESULT_NO_MATCH | 42
RESULT_ACCOUNT_DISABLED | 43
RESULT_SERVICE_READ_ONLY | 44
RESULT_ACCOUNT_NOT_FEATURED | 45
RESULT_ADMINISTRATOR_OK | 46
RESULT_CONTENT_VERSION | 47
RESULT_TRY_ANOTHER_CM | 48
RESULT_PASSWORD_REQUIRED_TO_KICK_SESSION | 49
RESULT_ALREADY_LOGGED_IN_ELSEWHERE | 50
RESULT_SUSPENDED | 51
RESULT_CANCELLED | 52
RESULT_DATA_CORRUPTION | 53
RESULT_DISK_FULL | 54
RESULT_REMOTE_CALL_FAILED | 55
RESULT_PASSWORD_UNSET | 56
RESULT_EXTERNAL_ACCOUNT_UNLINKED | 57
RESULT_PSN_TICKET_INVALID | 58
RESULT_EXTERNAL_ACCOUNT_ALREADY_LINKED | 59
RESULT_REMOTE_FILE_CONFLICT | 60
RESULT_ILLEGAL_PASSWORD | 61
RESULT_SAME_AS_PREVIOUS_VALUE | 62
RESULT_ACCOUNT_LOG_ON_DENIED | 63
RESULT_CANNOT_USE_OLD_PASSWORD | 64
RESULT_INVALID_LOGIN_AUTH_CODE | 65
RESULT_ACCOUNT_LOG_ON_DENIED_NO_MAIL | 66
RESULT_HARDWARE_NOT_CAPABLE_OF_IPT | 67
RESULT_IPT_INIT_ERROR | 68
RESULT_PARENTAL_CONTROL_RESTRICTED | 69
RESULT_FACEBOOK_QUERY_ERROR | 70
RESULT_EXPIRED_LOGIN_AUTH_CODE | 71
RESULT_IP_LOGIN_RESTRICTION_FAILED | 72
RESULT_ACCOUNT_LOCKED_DOWN | 73
RESULT_ACCOUNT_LOG_ON_DENIED_VERIFIED_EMAIL_REQUIRED | 74
RESULT_NO_MATCHING_URL | 75
RESULT_BAD_RESPONSE | 76
RESULT_REQUIRE_PASSWORD_REENTRY | 77
RESULT_VALUE_OUT_OF_RANGE | 78
RESULT_UNEXPECTED_ERROR | 79
RESULT_DISABLED | 80
RESULT_INVALID_CEG_SUBMISSION | 81
RESULT_RESTRICTED_DEVICE | 82
RESULT_REGION_LOCKED | 83
RESULT_RATE_LIMIT_EXCEEDED | 84
RESULT_ACCOUNT_LOGIN_DENIED_NEED_TWO_FACTOR | 85
RESULT_ITEM_DELETED | 86
RESULT_ACCOUNT_LOGIN_DENIED_THROTTLE | 87
RESULT_TWO_FACTOR_CODE_MISMATCH | 88
RESULT_TWO_FACTOR_ACTIVATION_CODE_MISMATCH | 89
RESULT_ACCOUNT_ASSOCIATED_TO_MULTIPLE_PARTNERS | 90
RESULT_NOT_MODIFIED | 91
RESULT_NO_MOBILE_DEVICE | 92
RESULT_TIME_NOT_SYNCED | 93
RESULT_SMS_CODE_FAILED | 94
RESULT_ACCOUNT_LIMIT_EXCEEDED | 95
RESULT_ACCOUNT_ACTIVITY_LIMIT_EXCEEDED | 96
RESULT_PHONE_ACTIVITY_LIMIT_EXCEEDED | 97
RESULT_REFUND_TO_WALLET | 98
RESULT_EMAIL_SEND_FAILURE | 99
RESULT_NOT_SETTLED | 100
RESULT_NEED_CAPTCHA | 101
RESULT_GSLT_DENIED | 102
RESULT_GS_OWNER_DENIED | 103
RESULT_INVALID_ITEM_TYPE | 104
RESULT_IP_BANNED | 105
RESULT_GSLT_EXPIRED | 106
RESULT_INSUFFICIENT_FUNDS | 107
RESULT_TOO_MANY_PENDING | 108

### ServerMode

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
SERVER_MODE_INVALID | eServerModeInvalid | 0 | Do not use.
SERVER_MODE_NO_AUTHENTICATION | eServerModeNoAuthentication | 1 | Don't authenticate user logins and don't list on the server list.
SERVER_MODE_AUTHENTICATION | eServerModeAuthentication | 2 | Authenticate users, list on the server list, and don't run VAC on clients that connect.
SERVER_MODE_AUTHENTICATION_AND_SECURE | eServerModeAuthenticationAndSecure | 3 | Authenticate users, list on the server list, and VAC protect clients,

### SteamAPIInitResult

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
STEAM_API_INIT_RESULT_OK | 0
STEAM_API_INIT_RESULT_FAILED_GENERIC | 1
STEAM_API_INIT_RESULT_NO_STEAM_CLIENT | 2
STEAM_API_INIT_RESULT_VERSION_MISMATCH | 3

### Universe

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
UNIVERSE_INVALID | 0
UNIVERSE_PUBLIC | 1
UNIVERSE_BETA | 2
UNIVERSE_INTERNAL | 3
UNIVERSE_DEV | 4
UNIVERSE_MAX | 5
