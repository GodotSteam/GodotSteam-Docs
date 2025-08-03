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
    Gets the internally-stored inventory handle.

    !!! returns "Returns: int32"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### get_inventory_update_handle

!!! function "get_inventory_update_handle( )"
    Gets the internally-stored inventory update handle.

    !!! returns "Returns: uint64_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### getSteamID

!!! function "getSteamID( )"
	Gets the server's Steam ID.

	!!! returns "Returns: uint64_t"

### getSteamID32

!!! function "getSteamID32( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The SteamID64 to convert.

    Convert a SteamID64 into a SteamID32.

    !!! returns "Returns:uint32_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### isAnonAccount

!!! function "isAnonAccount( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to check.

    Is this an anonymous account?

    !!! returns "Returns: bool"

### isAnonUserAccount

!!! function "isAnonUserAccount( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to check.

    Is this an anonymous user account? Used to create an account or reset a password, but do not try to do this.

    !!! returns "Returns: bool"

### isChatAccount

!!! function "isChatAccount( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to check.

    Is this a chat account ID?

    !!! returns "Returns: bool"

### isClanAccount

!!! function "isClanAccount( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to check.

    Is this a clan account ID?

    !!! returns "Returns: bool"

### isConsoleUserAccount

!!! function "isConsoleUserAccount( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to check.

    Is this a faked up Steam ID for a PSN friend account?

    !!! returns "Returns: bool"

### isIndividualAccount

!!! function "isIndividualAccount( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to check.

    Is this an individual user account ID?

    !!! returns "Returns: bool"

### isLobby

!!! function "isLobby( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to check.

    Is this a lobby account ID?

    !!! returns "Returns: bool"

### isServerSecure

!!! function "isServerSecure( )"
	Checking if the server is secured or not.

	!!! returns "Returns: bool"

### run_callbacks

!!! function "run_callbacks( )"
    Enables your application to receive callbacks from Steamworks. Must be placed in your \_process function.

    Alternatively, you can check **Embed Callbacks** in the **Project Settings > Steam > Initialization** panel and this will automatically set for you.

    !!! returns "Returns: void"

### serverInit

!!! function "serverInit( ```string``` ip, ```uint16_t``` game_port, ```uint16_t``` query_port, ```int``` server_mode, ```string``` version_number )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | ip | string | Will usually be zero. If you are on a machine with multiple IP addresses, you can pass a non-zero value here and the relevant sockets will be bound to that IP.  This can be used to ensure that the IP you desire is the one used in the server browser.
    | game_port | uint16_t | The port that clients will connect to for gameplay.  You will usually open up your own socket bound to this port.
    | query_port | uint16_t | The port that will manage server browser related duties and info pings from clients.  If you pass [QUERY_PORT_SHARED](#constants) for **query_port**, then it will use "GameSocketShare" mode, which means that the game is responsible for sending and receiving UDP packets for the master  server updater.  See [handleIncomingPacket](game_server.md#handleincomingpacket) and [getNextOutgoingPacket](game_server.md#getnextoutgoingpacket).
    | server_mode | [ServerMode enum](#servermode) | Whether to run authentication, VAC, and such or not.
    | version_number | string | Should be in the form x.x.x.x, and is used by the master server to detect when the server is out of date.  Only servers with the latest version will be listed.

    Initialize SteamGameServer client and interface objects, and set server properties which may not be changed.

	After calling this function, you should set any additional server parameters, and then logOnAnonymous() or logOn().

	This function is included for compatibility with older SDK.  You can use it if you don't care about decent error handling

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_gameserver#SteamGameServer_Init){ .md-button .md-button--store target="_blank" }

### serverInitEx

!!! function "serverInitEx( ```string``` ip, ```uint16_t``` game_port, ```uint16_t``` query_port, ```int``` server_mode, ```string``` version_number )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | ip | string | Will usually be zero. If you are on a machine with multiple IP addresses, you can pass a non-zero value here and the relevant sockets will be bound to that IP.  This can be used to ensure that the IP you desire is the one used in the server browser.
    | game_port | uint16_t | The port that clients will connect to for gameplay.  You will usually open up your own socket bound to this port.
    | query_port | uint16_t | The port that will manage server browser related duties and info pings from clients.  If you pass [QUERY_PORT_SHARED](#constants) for **query_port**, then it will use "GameSocketShare" mode, which means that the game is responsible for sending and receiving UDP packets for the master  server updater.  See [handleIncomingPacket](game_server.md#handleincomingpacket) and [getNextOutgoingPacket](game_server.md#getnextoutgoingpacket).
    | server_mode | [ServerMode enum](#servermode) | Whether to run authentication, VAC, and such or not.
    | version_number | string | Should be in the form x.x.x.x, and is used by the master server to detect when the server is out of date.  Only servers with the latest version will be listed.

    Initialize SteamGameServer client and interface objects, and set server properties which may not be changed.

	After calling this function, you should set any additional server parameters and then [logOnAnonymous](game_server.md#logonanonymous) or [logOn](game_server.md#logon).

	On success STEAM_API_INIT_RESULT_OK is returned.  Otherwise, if error_message is non-NULL, it will receive a non-localized message that explains the reason for the failure.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| :material-variable: Parameter | Type | Notes |
        | --- | ---- | ----- |
		| status | [SteamAPIInitResult enum](#steamapiinitresult) | Result of the initialization.
    	| verbal | string | A non-localized message that explains the reason for the failure.

### releaseCurrentThreadMemory

!!! function "releaseCurrentThreadMemory( )"
    Frees the internal Steamworks API memory associated with the calling thread.

    Most Steamworks API functions allocate a small amount of thread-local memory for parameter storage, calling this will manually free such memory. This function is called automatically by [run_callbacks](#run_callbacks), so a program that only ever accesses the Steamworks API from a single thread never needs to explicitly call this function.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_api#SteamAPI_ReleaseCurrentThreadMemory){ .md-button .md-button--doc_classes target="_blank" }

### serverShutdown

!!! function "serverShutdown( )"
	Shut down the server connection to Steam.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_gameserver#SteamGameServer_Shutdown){ .md-button .md-button--store target="_blank" }

### set_inventory_handle

!!! function "set_inventory_handle( `int32` new_inventory_handle )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_inventory_handle | int32 | The inventory handle to set.

    Sets the internally-stored inventory handle.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### set_inventory_update_handle

!!! function "set_inventory_update_handle( `uint64_t` new_inventory_update_handle )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_inventory_update_handle | uint64_t | The inventory update handle to set.

    Sets the internally-stored inventory update handle.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

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
MAX_GAME_SERVER_GAME_DATA | k_cbMaxGameServerGameData | 2048 | Max size of the GameServer game data.
MAX_GAME_SERVER_GAME_DESCRIPTION | k_cbMaxGameServerGameDescription | 64 | Max size of the GameServer game description.
MAX_GAME_SERVER_GAME_DIR | k_cbMaxGameServerGameDir | 32 | Max size of the GameServer game directory.
MAX_GAME_SERVER_MAP_NAME | k_cbMaxGameServerMapName | 32 | Max size of the GameServer map name.
MAX_GAME_SERVER_NAME | k_cbMaxGameServerName | 64 | Max size of the GameServer name.
MAX_GAME_SERVER_TAGS | k_cbMaxGameServerTags | 128 | Max size of the GameServer tags.
MAX_STEAM_DATAGRAM_GAME_COORDINATOR_SERVER_LOGIN_APP_DATA | k_cbMaxSteamDatagramGameCoordinatorServerLoginAppData | 2048 | -
MAX_STEAM_DATAGRAM_GAME_COORDINATOR_SERVER_LOGIN_SERIALIZED | k_cbMaxSteamDatagramGameCoordinatorServerLoginSerialized | 4096 | -
PARTY_BEACON_ID_INVALID | k_ulPartyBeaconIdInvalid | 0 | An invalid party beacon ID.
QUERY_PORT_ERROR | k_usFriendGameInfoQueryPort_Error | 0xFFFE | We were unable to get the query port for this server.
QUERY_PORT_NOT_INITIALIZED | k_usFriendGameInfoQueryPort_NotInitialized | 0xFFFF | We haven't asked the GameServer for this query port's actual value yet.
QUERY_PORT_SHARED | STEAMGAMESERVER_QUERY_PORT_SHARED | 0xffff | Pass to [serverInit](#serverinit) to indicate that the same UDP port will be used for game traffic UDP queries for server browser pings and LAN discovery.  In this case, Steam will not open up a socket to handle server browser queries, and you must use [handleIncomingPacket](game_server.md#handleincomingpacket) and [getNextOutgoingPacket](game_server.md#getnextoutgoingpacket) to handle packets related to server discovery on your socket.
STEAM_ACCOUNT_ID_MASK | k_unSteamAccountIDMask | 0xFFFFFFFF | Used in CSteamID to mask out the AccountID_t.
STEAM_ACCOUNT_INSTANCE_MASK | k_unSteamAccountInstanceMask | 0x000FFFFF | Used in CSteamID to mask out the account instance.
STEAM_BUFFER_SIZE | - | 255 | Custom for GodotSteam.
STEAM_DATAGRAM_MAX_SERIALIZED_TICKET | k_cbSteamDatagramMaxSerializedTicket | 512 | -
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
Steam account types. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
ACCOUNT_TYPE_INVALID | k_EAccountTypeInvalid | 0 | -
ACCOUNT_TYPE_INDIVIDUAL | k_EAccountTypeIndividual | 1 | Single user account.
ACCOUNT_TYPE_MULTISEAT | k_EAccountTypeMultiseat | 2 | Multiseat (e.g. cybercafe) account.
ACCOUNT_TYPE_GAME_SERVER | k_EAccountTypeGameServer | 3 | Game server account.
ACCOUNT_TYPE_ANON_GAME_SERVER | k_EAccountTypeAnonGameServer | 4 | Anonymous game server account.
ACCOUNT_TYPE_PENDING | k_EAccountTypePending | 5 | Pending.
ACCOUNT_TYPE_CONTENT_SERVER | k_EAccountTypeContentServer | 6 | Content server.
ACCOUNT_TYPE_CLAN | k_EAccountTypeClan | 7 | -
ACCOUNT_TYPE_CHAT | k_EAccountTypeChat | 8 | -
ACCOUNT_TYPE_CONSOLE_USER | k_EAccountTypeConsoleUser | 9 | Fake Steam ID for local PSN account on PS3 or Live account on 360, etc.
ACCOUNT_TYPE_ANON_USER | k_EAccountTypeAnonUser | 10 | -
ACCOUNT_TYPE_MAX | k_EAccountTypeMax | - | Max of 16 items in this field.

### AuthSessionResponse
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
AUTH_SESSION_RESPONSE_OK | k_EAuthSessionResponseOK | 0 | Steam has verified the user is online, the ticket is valid and ticket has not been reused.
AUTH_SESSION_RESPONSE_USER_NOT_CONNECTED_TO_STEAM | k_EAuthSessionResponseUserNotConnectedToSteam | 1 | The user in question is not connected to Steam.
AUTH_SESSION_RESPONSE_NO_LICENSE_OR_EXPIRED | k_EAuthSessionResponseNoLicenseOrExpired | 2 | The license has expired.
AUTH_SESSION_RESPONSE_VAC_BANNED | k_EAuthSessionResponseVACBanned | 3 | The user is VAC banned for this game.
AUTH_SESSION_RESPONSE_LOGGED_IN_ELSEWHERE | k_EAuthSessionResponseLoggedInElseWhere | 4 | The user account has logged in elsewhere and the session containing the game instance has been disconnected.
AUTH_SESSION_RESPONSE_VAC_CHECK_TIMED_OUT | k_EAuthSessionResponseVACCheckTimedOut | 5 | VAC has been unable to perform anti-cheat checks on this user.
AUTH_SESSION_RESPONSE_AUTH_TICKET_CANCELED | k_EAuthSessionResponseAuthTicketCanceled | 6 | The ticket has been canceled by the issuer.
AUTH_SESSION_RESPONSE_AUTH_TICKET_INVALID_ALREADY_USED | k_EAuthSessionResponseAuthTicketInvalidAlreadyUsed | 7 | This ticket has already been used, it is not valid.
AUTH_SESSION_RESPONSE_AUTH_TICKET_INVALID | k_EAuthSessionResponseAuthTicketInvalid | 8 | This ticket is not from a user instance currently connected to Steam.
AUTH_SESSION_RESPONSE_PUBLISHER_ISSUED_BAN | k_EAuthSessionResponsePublisherIssuedBan | 9 | The user is banned for this game. The ban came via the web api and not VAC.
AUTH_SESSION_RESPONSE_AUTH_TICKET_NETWORK_IDENTITY_FAILURE | k_EAuthSessionResponseAuthTicketNetworkIdentityFailure | 10 | The network identity in the ticket does not match the server authenticating the ticket.

### BeginAuthSessionResult
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
BEGIN_AUTH_SESSION_RESULT_OK | k_EBeginAuthSessionResultOK | 0 | Ticket is valid for this game and this steamID.
BEGIN_AUTH_SESSION_RESULT_INVALID_TICKET | k_EBeginAuthSessionResultInvalidTicket | 1 | Ticket is not valid.
BEGIN_AUTH_SESSION_RESULT_DUPLICATE_REQUEST | k_EBeginAuthSessionResultDuplicateRequest | 2 | A ticket has already been submitted for this Steam ID.
BEGIN_AUTH_SESSION_RESULT_INVALID_VERSION | k_EBeginAuthSessionResultInvalidVersion | 3 | Ticket is from an incompatible interface version.
BEGIN_AUTH_SESSION_RESULT_GAME_MISMATCH | k_EBeginAuthSessionResultGameMismatch | 4 | Ticket is not for this game.
BEGIN_AUTH_SESSION_RESULT_EXPIRED_TICKET | k_EBeginAuthSessionResultExpiredTicket | 5 | Ticket has expired.

### DenyReason
Result codes to GSHandleClientDeny / Kick. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
DENY_INVALID | k_EDenyInvalid | 0 | -
DENY_INVALID_VERSION | k_EDenyInvalidVersion | 1 | -
DENY_GENERIC | k_EDenyGeneric | 2 | -
DENY_NOT_LOGGED_ON | k_EDenyNotLoggedOn | 3 | -
DENY_NO_LICENSE | k_EDenyNoLicense | 4 | -
DENY_CHEATER | k_EDenyCheater | 5 | -
DENY_LOGGED_IN_ELSEWHERE | k_EDenyLoggedInElseWhere | 6 | -
DENY_UNKNOWN_TEXT | k_EDenyUnknownText | 7 | -
DENY_INCOMPATIBLE_ANTI_CHEAT | k_EDenyIncompatibleAnticheat | 8 | -
DENY_MEMORY_CORRUPTION | k_EDenyMemoryCorruption | 9 | -
DENY_INCOMPATIBLE_SOFTWARE | k_EDenyIncompatibleSoftware | 10 | -
DENY_STEAM_CONNECTION_LOST | k_EDenySteamConnectionLost | 11 | -
DENY_STEAM_CONNECTION_ERROR | k_EDenySteamConnectionError | 12 | -
DENY_STEAM_RESPONSE_TIMED_OUT | k_EDenySteamResponseTimedOut | 13 | -
DENY_STEAM_VALIDATION_STALLED | k_EDenySteamValidationStalled | 14 | -
DENY_STEAM_OWNER_LEFT_GUEST_USER | k_EDenySteamOwnerLeftGuestUser | 15 | -

### GameIDType
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
GAME_TYPE_APP | k_EGameIDTypeApp | 0 | -
GAME_TYPE_GAME_MOD | k_EGameIDTypeGameMod | 1 | -
GAME_TYPE_SHORTCUT | k_EGameIDTypeShortcut | 2 | -
GAME_TYPE_P2P | k_EGameIDTypeP2P | 3 | -

### IPType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
IP_TYPE_IPV4 | k_ESteamIPTypeIPv4 | 0 | -
IP_TYPE_IPV6 |k_ESteamIPTypeIPv6 | 1 | -

### Result
General result codes. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
RESULT_NONE | k_EResultNone | 0 | No result.
RESULT_OK | k_EResultOK | 1 | Success.
RESULT_FAIL | k_EResultFail | 2 | Generic failure.
RESULT_NO_CONNECTION | k_EResultNoConnection | 3 | No / failed network connection.
RESULT_INVALID_PASSWORD | k_EResultInvalidPassword | 5 | Password / ticket is invalid.
RESULT_LOGGED_IN_ELSEWHERE | k_EResultLoggedInElsewhere | 6 | Same user logged in elsewhere.
RESULT_INVALID_PROTOCOL_VER | k_EResultInvalidProtocolVer | 7 | Protocol version is incorrect.
RESULT_INVALID_PARAM | k_EResultInvalidParam | 8 | A parameter is incorrect.
RESULT_FILE_NOT_FOUND | k_EResultFileNotFound | 9 | File was not found.
RESULT_BUSY | k_EResultBusy | 10 | Called method busy; action not taken.
RESULT_INVALID_STATE | k_EResultInvalidState | 11 | Called object was in an invalid state.
RESULT_INVALID_NAME | k_EResultInvalidName | 12 | Name is invalid.
RESULT_INVALID_EMAIL | k_EResultInvalidEmail | 13 | E-mail is invalid.
RESULT_DUPLICATE_NAME | k_EResultDuplicateName | 14 | Name is not unique.
RESULT_ACCESS_DENIED | k_EResultAccessDenied | 15 | Access is denied.
RESULT_TIMEOUT | k_EResultTimeout | 16 | Operation timed out.
RESULT_BANNED | k_EResultBanned | 17 | VAC2 banned.
RESULT_ACCOUNT_NOT_FOUND | k_EResultAccountNotFound | 18 | Account not found.
RESULT_INVALID_STEAMID | k_EResultInvalidSteamID | 19 | Steam ID is invalid.
RESULT_SERVICE_UNAVAILABLE | k_EResultServiceUnavailable | 20 | The requested service is currently unavailable.
RESULT_NOT_LOGGED_ON | k_EResultNotLoggedOn | 21 | The requested service is currently unavailable.
RESULT_PENDING | k_EResultPending | 22 | Request is pending; may be in process or waiting on third party.
RESULT_ENCRYPTION_FAILURE | k_EResultEncryptionFailure | 23 | Encryption or decryption failed.
RESULT_INSUFFICIENT_PRIVILEGE | k_EResultInsufficientPrivilege | 24 | Insufficient privilege.
RESULT_LIMIT_EXCEEDED | k_EResultLimitExceeded | 25 | Too much of a good thing.
RESULT_REVOKED | k_EResultRevoked | 26 | Access has been revoked; used for revoked guest passes.
RESULT_EXPIRED | k_EResultExpired | 27 | License / guest pass the user is trying to access is expired.
RESULT_ALREADY_REDEEMED | k_EResultAlreadyRedeemed | 28 | Guest pass has already been redeemed by account, cannot be acked again.
RESULT_DUPLICATE_REQUEST | k_EResultDuplicateRequest | 29 | The request is a duplicate and the action has already occurred in the past; ignored this time.
RESULT_ALREADY_OWNED | k_EResultAlreadyOwned | 30 | All the games in this guest pass redemption request are already owned by the user.
RESULT_IP_NOT_FOUND | k_EResultIPNotFound | 31 | IP address not found.
RESULT_PERSIST_FAILED | k_EResultPersistFailed | 32 | Failed to write change to the data store.
RESULT_LOCKING_FAILED | k_EResultLockingFailed | 33 | Failed to acquire access lock for this operation.
RESULT_LOG_ON_SESSION_REPLACED | k_EResultLogonSessionReplaced | 34 | -
RESULT_CONNECT_FAILED | k_EResultConnectFailed | 35 | -
RESULT_HANDSHAKE_FAILED | k_EResultHandshakeFailed | 36 | -
RESULT_IO_FAILURE | k_EResultIOFailure | 37 | -
RESULT_REMOTE_DISCONNECT | k_EResultRemoteDisconnect | 38 | -
RESULT_SHOPPING_CART_NOT_FOUND | k_EResultShoppingCartNotFound | 39 | Failed to find the shopping cart requested.
RESULT_BLOCKED | k_EResultBlocked | 40 | A user didn't allow it.
RESULT_IGNORED | k_EResultIgnored | 41 | Target is ignoring sender.
RESULT_NO_MATCH | k_EResultNoMatch | 42 | Nothing matching the request found.
RESULT_ACCOUNT_DISABLED | k_EResultAccountDisabled | 43 | -
RESULT_SERVICE_READ_ONLY | k_EResultServiceReadOnly | 44 | This service is not accepting content changes right now.
RESULT_ACCOUNT_NOT_FEATURED | k_EResultAccountNotFeatured | 45 | Account doesn't have value, so this feature isn't available.
RESULT_ADMINISTRATOR_OK | k_EResultAdministratorOK | 46 | Allowed to take this action but only because requester is admin.
RESULT_CONTENT_VERSION | k_EResultContentVersion | 47 | A version mismatch in content transmitted within the Steam protocol.
RESULT_TRY_ANOTHER_CM | k_EResultTryAnotherCM | 48 | The current CM can't service the user making a request; user should try another.
RESULT_PASSWORD_REQUIRED_TO_KICK_SESSION | k_EResultPasswordRequiredToKickSession | 49 | You are already logged in elsewhere, this cached credential login has failed.
RESULT_ALREADY_LOGGED_IN_ELSEWHERE | k_EResultAlreadyLoggedInElsewhere | 50 | You are already logged in elsewhere; you must wait.
RESULT_SUSPENDED | k_EResultSuspended | 51 | Long running operation (content download) suspended / paused.
RESULT_CANCELLED | k_EResultCancelled | 52 | Operation canceled; typically by user: content download.
RESULT_DATA_CORRUPTION | k_EResultDataCorruption | 53 | Operation canceled because data is ill formed or unrecoverable.
RESULT_DISK_FULL | k_EResultDiskFull | 54 | Operation canceled; not enough disk space.
RESULT_REMOTE_CALL_FAILED | k_EResultRemoteCallFailed | 55 | A remote call or IPC call failed.
RESULT_PASSWORD_UNSET | k_EResultPasswordUnset | 56 | Password could not be verified as it's unset server side.
RESULT_EXTERNAL_ACCOUNT_UNLINKED | k_EResultExternalAccountUnlinked | 57 | External account (PSN, Facebook, etc.) is not linked to a Steam account.
RESULT_PSN_TICKET_INVALID | k_EResultPSNTicketInvalid | 58 | PSN ticket was invalid.
RESULT_EXTERNAL_ACCOUNT_ALREADY_LINKED | k_EResultExternalAccountAlreadyLinked | 59 | External account (PSN, Facebook, etc.) is already linked to some other account, must explicitly request to replace / delete the link first.
RESULT_REMOTE_FILE_CONFLICT | k_EResultRemoteFileConflict | 60 | The sync cannot resume due to a conflict between the local and remote files.
RESULT_ILLEGAL_PASSWORD | k_EResultIllegalPassword | 61 | The requested new password is not legal.
RESULT_SAME_AS_PREVIOUS_VALUE | k_EResultSameAsPreviousValue | 62 | New value is the same as the old one; secret question and answer.
RESULT_ACCOUNT_LOG_ON_DENIED | k_EResultAccountLogonDenied | 63 | Account login denied due to 2nd factor authentication failure.
RESULT_CANNOT_USE_OLD_PASSWORD | k_EResultCannotUseOldPassword | 64 | The requested new password is not legal.
RESULT_INVALID_LOG_IN_AUTH_CODE | k_EResultInvalidLoginAuthCode | 65 | Account login denied due to auth code invalid.
RESULT_ACCOUNT_LOG_ON_DENIED_NO_MAIL | k_EResultAccountLogonDeniedNoMail | 66 | Account login denied due to 2nd factor auth failure and no mail has been sent; partner site specific.
RESULT_HARDWARE_NOT_CAPABLE_OF_IPT | k_EResultHardwareNotCapableOfIPT | 67 | -
RESULT_IPT_INIT_ERROR | k_EResultIPTInitError | 68 | -
RESULT_PARENTAL_CONTROL_RESTRICTED | k_EResultParentalControlRestricted | 69 | Operation failed due to parental control restrictions for current user.
RESULT_FACEBOOK_QUERY_ERROR | k_EResultFacebookQueryError | 70 | Facebook query returned an error.
RESULT_EXPIRED_LOGIN_AUTH_CODE | k_EResultExpiredLoginAuthCode | 71 | Account login denied due to auth code expired.
RESULT_IP_LOGIN_RESTRICTION_FAILED | k_EResultIPLoginRestrictionFailed | 72 | -
RESULT_ACCOUNT_LOCKED_DOWN | k_EResultAccountLockedDown | 73 | -
RESULT_ACCOUNT_LOG_ON_DENIED_VERIFIED_EMAIL_REQUIRED | -k_EResultAccountLogonDeniedVerifiedEmailRequired | 74 | -
RESULT_NO_MATCHING_URL | k_EResultNoMatchingURL | 75 | -
RESULT_BAD_RESPONSE | k_EResultBadResponse | 76 | Parse failure, missing field, etc.
RESULT_REQUIRE_PASSWORD_REENTRY | k_EResultRequirePasswordReEntry | 77 | The user cannot complete the action until they re-enter their password.
RESULT_VALUE_OUT_OF_RANGE | k_EResultValueOutOfRange | 78 | The value entered is outside the acceptable range.
RESULT_UNEXPECTED_ERROR | k_EResultUnexpectedError | 79 | Something happened that we didn't expect to ever happen.
RESULT_DISABLED | k_EResultDisabled | 80 | The requested service has been configured to be unavailable.
RESULT_INVALID_CEG_SUBMISSION | k_EResultInvalidCEGSubmission | 81 | The set of files submitted to the CEG server are not valid.
RESULT_RESTRICTED_DEVICE | k_EResultRestrictedDevice | 82 | The device being used is not allowed to perform this action.
RESULT_REGION_LOCKED | k_EResultRegionLocked | 83 | The action could not be complete because it is region restricted.
RESULT_RATE_LIMIT_EXCEEDED | k_EResultRateLimitExceeded | 84 | Temporary rate limit exceeded, try again later. Different from RESULT_LIMIT_EXCEEDED which may be permanent.
RESULT_ACCOUNT_LOGIN_DENIED_NEED_TWO_FACTOR | k_EResultAccountLoginDeniedNeedTwoFactor | 85 | Need two-factor code to login.
RESULT_ITEM_DELETED | k_EResultItemDeleted | 86 | The thing we're trying to access has been deleted.
RESULT_ACCOUNT_LOGIN_DENIED_THROTTLE | k_EResultAccountLoginDeniedThrottle | 87 | Login attempt failed, try to throttle response to possible attacker.
RESULT_TWO_FACTOR_CODE_MISMATCH | k_EResultTwoFactorCodeMismatch | 88 | Two-factor code mismatch.
RESULT_TWO_FACTOR_ACTIVATION_CODE_MISMATCH | k_EResultTwoFactorActivationCodeMismatch | 89 | Activation code for two-factor didn't match.
RESULT_ACCOUNT_ASSOCIATED_TO_MULTIPLE_PARTNERS | k_EResultAccountAssociatedToMultiplePartners | 90 | Account has been associated with multiple partners.
RESULT_NOT_MODIFIED | k_EResultNotModified | 91 | Data not modified.
RESULT_NO_MOBILE_DEVICE | k_EResultNoMobileDevice | 92 | The account does not have a mobile device associated with it.
RESULT_TIME_NOT_SYNCED | k_EResultTimeNotSynced | 93 | The time presented is out of range or tolerance.
RESULT_SMS_CODE_FAILED | k_EResultSmsCodeFailed | 94 | SMS code failure; no match, none pending, etc.
RESULT_ACCOUNT_LIMIT_EXCEEDED | k_EResultAccountLimitExceeded | 95 | Too many accounts access this resource.Too many changes to this account.
RESULT_ACCOUNT_ACTIVITY_LIMIT_EXCEEDED | k_EResultAccountActivityLimitExceeded | 96 | Too many changes to this account.
RESULT_PHONE_ACTIVITY_LIMIT_EXCEEDED | k_EResultPhoneActivityLimitExceeded | 97 | Too many changes to this phone.
RESULT_REFUND_TO_WALLET | k_EResultRefundToWallet | 98 | Cannot refund to payment method, must use wallet.
RESULT_EMAIL_SEND_FAILURE | k_EResultEmailSendFailure | 99 | Cannot send an e-mail.
RESULT_NOT_SETTLED | k_EResultNotSettled | 100 | Can't perform operation till payment has settled.
RESULT_NEED_CAPTCHA | k_EResultNeedCaptcha | 101 | Needs to provide a valid captcha.
RESULT_GSLT_DENIED | k_EResultGSLTDenied | 102 | A game server login token owned by this token's owner has been banned.
RESULT_GS_OWNER_DENIED | k_EResultGSOwnerDenied | 103 | Game server owner is denied for other reason: account lock, community ban, vac ban, missing phone, etc.
RESULT_INVALID_ITEM_TYPE | k_EResultInvalidItemType | 104 | The type of thing we were requested to act on is invalid.
RESULT_IP_BANNED | k_EResultIPBanned | 105 | The IP address has been banned from taking this action.
RESULT_GSLT_EXPIRED | k_EResultGSLTExpired | 106 | This token has expired from disuse; can be reset for use.
RESULT_INSUFFICIENT_FUNDS | k_EResultInsufficientFunds | 107 | User doesn't have enough wallet funds to complete the action.
RESULT_TOO_MANY_PENDING | k_EResultTooManyPending | 108 | There are too many of this thing pending already.
RESULT_NO_SITE_LICENSES_FOUND | k_EResultNoSiteLicensesFound | 109 | No site licenses found.
RESULT_WG_NETWORK_SEND_EXCEEDED | k_EResultWGNetworkSendExceeded | 110 | The WG couldn't send a response because we exceeded max network send size.
RESULT_ACCOUNT_NOT_FRIENDS | k_EResultAccountNotFriends | 111 | The user is not mutually friends.
RESULT_LIMITED_USER_ACCOUNT | k_EResultLimitedUserAccount | 112 | The user is limited.
RESULT_CANT_REMOVE_ITEM | k_EResultCantRemoveItem | 113 | Item can't be removed.
RESULT_ACCOUNT_DELETED | k_EResultAccountDeleted | 114 | Account has been deleted.
RESULT_EXISTING_USER_CANCELLED_LICENSE | k_EResultExistingUserCancelledLicense | 115 | A license for this already exists but cancelled.
RESULT_COMMUNITY_COOLDOWN | k_EResultCommunityCooldown | 116 | Access is denied because of a community cooldown; probably from support profile data resets.
RESULT_NO_LAUNCHER_SPECIFIED | k_EResultNoLauncherSpecified | 117 | No launcher was specified, but a launcher was needed to choose correct realm for operation.
RESULT_MUST_AGREE_TO_SSA | k_EResultMustAgreeToSSA | 118 | User must agree to china SSA or global SSA before login.
RESULT_LAUNCHER_MIGRATED | k_EResultLauncherMigrated | 119 | The specified launcher type is no longer supported; the user should be directed elsewhere.
RESULT_STEAM_REALM_MISMATCH | k_EResultSteamRealmMismatch | 120 | The user's realm does not match the realm of the requested resource.
RESULT_INVALID_SIGNATURE | k_EResultInvalidSignature | 121 | Signature check did not match.
RESULT_PARSE_FAILURE | k_EResultParseFailure | 122 | Failed to parse input.
RESULT_NO_VERIFIED_PHONE | k_EResultNoVerifiedPhone | 123 | Account does not have a verified phone number.
RESULT_INSUFFICIENT_BATTERY | k_EResultInsufficientBattery | 124 | User device doesn't have enough battery charge currently to complete the action.
RESULT_CHARGER_REQUIRED | k_EResultChargerRequired | 125 | The operation requires a charger to be plugged in, which wasn't present.
RESULT_CACHED_CREDENTIAL_INVALID | k_EResultCachedCredentialInvalid | 126 | Cached credential was invalid; user must reauthenticate.
RESULT_PHONE_NUMBER_IS_VOIP | K_EResultPhoneNumberIsVOIP | 127 | The phone number provided is a voice-over-IP number.
RESULT_NOT_SUPPORTED | k_EResultNotSupported | 128 | The data being accessed is not supported by this API.
RESULT_FAMILY_SIZE_LIMIT_EXCEEDED | k_EResultFamilySizeLimitExceeded | 129 | Reached the maximum size of the family.
RESULT_OFFLINE_APP_CACHE_INVALID | k_EResultOfflineAppCacheInvalid | 130 | The local data for the offline mode cache is insufficient to login.

### ServerMode

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
SERVER_MODE_INVALID | eServerModeInvalid | 0 | Do not use.
SERVER_MODE_NO_AUTHENTICATION | eServerModeNoAuthentication | 1 | Don't authenticate user logins and don't list on the server list.
SERVER_MODE_AUTHENTICATION | eServerModeAuthentication | 2 | Authenticate users, list on the server list, and don't run VAC on clients that connect.
SERVER_MODE_AUTHENTICATION_AND_SECURE | eServerModeAuthenticationAndSecure | 3 | Authenticate users, list on the server list, and VAC protect clients,

### SteamAPIInitResult
Found in steam_api.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
STEAM_API_INIT_RESULT_OK | k_ESteamAPIInitResult_OK | 0 | Initialization was successful.
STEAM_API_INIT_RESULT_FAILED_GENERIC | k_ESteamAPIInitResult_FailedGeneric | 1 | Some other failure.
STEAM_API_INIT_RESULT_NO_STEAM_CLIENT | k_ESteamAPIInitResult_NoSteamClient | 2 | We cannot connect to Steam, it probably isn't running.
STEAM_API_INIT_RESULT_VERSION_MISMATCH | k_ESteamAPIInitResult_VersionMismatch | 3 | Steam client appears to be out of date.

### Universe
Steam universes.  Each universe is a self-contained Steam instance. Found in steamuniverse.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
UNIVERSE_INVALID | k_EUniverseInvalid | 0 | -
UNIVERSE_PUBLIC | k_EUniversePublic | 1 | -
UNIVERSE_BETA | k_EUniverseBeta | 2 | -
UNIVERSE_INTERNAL | k_EUniverseInternal | 3 | -
UNIVERSE_DEV | k_EUniverseDev | 4 | -
UNIVERSE_MAX | k_EUniverseMax | 5 | -

### UserHasLicenseForAppResult
Results from [userHasLicenseForApp](user.md#userhaslicenseforapp). Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
USER_HAS_LICENSE_RESULT_HAS_LICENSE | k_EUserHasLicenseResultHasLicense | 0 | User has a license for specified app.
USER_HAS_LICENSE_RESULT_DOES_NOT_HAVE_LICENSE | k_EUserHasLicenseResultDoesNotHaveLicense | 1 | User does not have a license for the specified app.
USER_HAS_LICENSE_RESULT_NO_AUTH | k_EUserHasLicenseResultNoAuth | 2 | User has not been authenticated.
