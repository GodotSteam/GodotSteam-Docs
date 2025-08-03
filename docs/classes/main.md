---
title: Main
description: A class reference for Steam main.
icon: material/home
---

# Main

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### get_browser_handle

!!! function "get_browser_handle( )"
    Gets the internally-stored browser handle.

    !!! returns "Returns: uint32_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### get_current_app_id

!!! function "get_current_app_id( )"
    Gets the internally-stored, current app ID.

    !!! returns "Returns: uint32_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### get_current_clan_id

!!! function "get_current_clan_id( )"
    Gets the internally-stored, current clan ID.

    !!! returns "Returns: uint64_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### get_current_steam_id

!!! function "get_current_steam_id( )"
    Gets the interally-stored, local Steam ID.

    !!! returns "Returns: uint64_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### get_godotsteam_version

!!! function "get_godotsteam_version( )"
    Gets the current version of the GodotSteam editor you are using.

    !!! returns "Returns: String"

    !!! info "Notes"
        This is a GodotSteam-specific function.

    ---
    [ :material-tag-plus: Added GodotSteam 4.11](../changelog/godot4.md/#version-411){ .md-button .md-button--changes target="_blank" }
   
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

### get_leaderboard_handle

!!! function "get_leaderboard_handle( )"
    Gets the internally-stored leaderboard handle.

    !!! returns "Returns: uint64_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### get_leaderboard_details_max

!!! function "get_leaderboard_details_max( )"
    Gets the internally-stored maximum amount of leaderboard details to fetch.

    !!! returns "Returns: int"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### get_leaderboard_entries

!!! function "get_leaderboard_entries( )"
    Gets the current list of internally-stored leaderboard entries.

    !!! returns "Returns: array"

    !!! info "Notes"
        This is a GodotSteam-specific function.

    ---
    [ :material-tag-plus: Added GodotSteam 4.11](../changelog/godot4.md/#version-411){ .md-button .md-button--changes target="_blank" }

### get_server_list_request

!!! function "get_server_list_request( )"

    !!! returns "Returns: uint64_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### getSteamID32

!!! function "getSteamID32( `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | steam_id | uint64_t | The SteamID64 to convert.

    Convert a SteamID64 into a SteamID32.

    !!! returns "Returns:uint32_t"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### get_steam_init_result

!!! function "get_steam_init_result( )"
    Returns the Steam initialization dictionary from either [steamInitEX](#steaminitex) or auto-initialization set in the Project Settings.  If you use [steamInit](#steaminit), this does not get filled out.

    !!! returns "Returns: dictionary"
        Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | status | [SteamAPIInitResult enum](#steamapiinitresult)| The status code of the initialization attempt.
        | verbal | string | The error message if initialization fails.

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

### isSteamRunning

!!! function "isSteamRunning( )"
    Check if the Steam client is running. 

    !!! returns "Returns: bool"

### releaseCurrentThreadMemory

!!! function "releaseCurrentThreadMemory( )"
    Frees the internal Steamworks API memory associated with the calling thread.

    Most Steamworks API functions allocate a small amount of thread-local memory for parameter storage, calling this will manually free such memory. This function is called automatically by [run_callbacks](#run_callbacks), so a program that only ever accesses the Steamworks API from a single thread never needs to explicitly call this function.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_api#SteamAPI_ReleaseCurrentThreadMemory){ .md-button .md-button--doc_classes target="_blank" }

### restartAppIfNecessary

!!! function "restartAppIfNecessary( `uint32_t` app_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | app_id | uint32_t | The app ID to restart from Steam.

    Checks if your executable was launched through Steam and relaunches it through Steam if it wasn't. See [Valve's docs initialization and shutdown for additionanl information.](https://partner.steamgames.com/doc/sdk/api#initialization_and_shutdown){ target="\_blank"} For something more relevant to GodotSteam, [check out our initialization docs.](../tutorials/initializing.md)

    !!! returns "Returns: bool"
        If this returns true then it starts the Steam client if required and launches your game again through it, and you should quit your process as soon as possible. This effectively runs `steam://run/<AppId>` so it may not relaunch the exact executable that called it, as it will always relaunch from the version installed in your Steam library folder.

        If it returns false then your game was launched by the Steam client and no action needs to be taken. One exception is if a **steam_appid.txt** file is present then this will return false regardless. This allows you to develop and test without launching your game through the Steam client. Make sure to remove the **steam_appid.txt** file when uploading the game to your Steam depot.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_api#SteamAPI_RestartAppIfNecessary){ .md-button .md-button--doc_classes target="_blank" }

### run_callbacks

!!! function "run_callbacks( )"
    Enables your application to receive callbacks from Steamworks. Must be placed in your \_process function.

    Alternatively, you can check **Embed Callbacks** in the **Project Settings > Steam > Initialization** panel and this will automatically set for you.

    !!! returns "Returns: void"

### set_browser_handle

!!! function "set_browser_handle( `uint32_t` new_browser_handle )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_browser_handle | uint32_t | The browser handle to set.

    Sets the internally-stored browser handle.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### set_current_app_id

!!! function "set_current_app_id( `uint32_t` new_current_app_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_current_app_id | uint32_t | The app ID to set.

    Sets the internally-stored, current app ID.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### set_current_clan_id

!!! function "set_current_clan_id( `uint64_t` new_current_clan_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_current_clan_id | uint64_t | The clan ID to set.

    Sets the internally-stored, current clan ID.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### set_current_steam_id

!!! function "set_current_steam_id( `uint64_t` new_current_steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_current_steam_id | uint64_t | The Steam ID to set.

    Sets the internally-stored, local Steam ID.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

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

### set_leaderboard_details_max

!!! function "set_leaderboard_details_max( `int` new_leaderboard_details_max )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_leaderboard_details_max | int | The maximum number of leaderboard details to fetch.

    Set function for leaderboard_details_max property; which is the maximum number of details to return for leaderboard entries.

    This can only go as high as [LEADERBOARD_DETAILS_MAX (256)](user_stats.md#constants).

    !!! info "Notes"
        This is a GodotSteam-specific function.

    ---
    [ :material-tag-plus: Added GodotSteam 4.11](../changelog/godot4.md/#version-411){ .md-button .md-button--changes target="_blank" }

### set_leaderboard_entries

!!! function "set_leaderboard_entries( `Array` new_leaderboard_entries )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_leaderboard_entries | Array | An array of leaderboard entries.

    Sets the internally-stored leaderboard entries array.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### set_leaderboard_handle

!!! function "set_leaderboard_handle( `uint64_t` new_leaderboard_handle )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_leaderboard_handle | uint64_t | The leaderboard handle to set.

    Sets the internally-stored leaderboard handle.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### set_server_list_request

!!! function "set_server_list_request( `uint64_t` new_server_list_request )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | new_server_list_request | uint64_t | The server list request handle to set.

    Sets the internally-stored server list request handle.

    !!! returns "Returns: void"

    !!! info "Notes"
        This is a GodotSteam-specific function.

### steamInit

!!! function "steamInit( `uint32_t` app_id = 0, `bool` embed_callbacks = false )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | app_id | uint32_t | The app ID for the game we are initializing.  Defaults to 0.
    | embed_callbacks | bool | Whether or not to call [run_callbacks](#run_callbacks) interally.  Defaults to false.

    Initialize the SDK, without worrying about the cause of failure.

    You can pass your **app ID** to the _first argument_ and GodotSteam will set the OS environment for you so you do not have to do this manually anymore. If nothing is passed, it will not be set.  You can pass **true** to the _second argument_ to have GodotSteam connect and use **run_callbacks** internally so you do not have to do this manually anymore. If nothing is passed, it will not run callbacks internally.  Lastly, if you set the **app_id** and/or check **Embed Callbacks** in the **Project Settings > Steam > Initialization**, these will automatically set for you and you can just call **steamInitEx()** with no arguments filled out.

    !!! returns "Returns: bool"

    !!! info "Notes"
        This function **does not** contain a second parameter in **GDNative** and the first parameter is for the deprecated retrieving statistics and achievements.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_api#SteamAPI_Init){ .md-button .md-button--doc_classes target="_blank" }

### steamInitEx

!!! function "steamInitEx( `uint32_t` app_id = 0, `bool` embed_callbacks = false )"
    | :material-variable: Parameter | Type | Notes |
    | --- | ---- | ----- |
    | app_id | uint32_t | The app ID for the game we are initializing.  Defaults to 0.
    | embed_callbacks | bool | Whether or not to call [run_callbacks](#run_callbacks) interally.  Defaults to false.

    Initialize the Steamworks SDK. On success **STEAM_API_INIT_RESULT_OK** is returned. Otherwise, if **verbal** is non-NULL, it will receive a non-localized message that explains the reason for the failure.

    You can pass your **app ID** to the _first argument_ and GodotSteam will set the OS environment for you so you do not have to do this manually anymore. If nothing is passed, it will not be set.  You can pass **true** to the _second argument_ to have GodotSteam connect and use **run_callbacks** internally so you do not have to do this manually anymore. If nothing is passed, it will not run callbacks internally.  Lastly, if you set the **app_id** and/or check **Embed Callbacks** in the **Project Settings > Steam > Initialization**, these will automatically set for you and you can just call **steamInitEx()** with no arguments filled out.

    !!! returns "Returns: dictionary"
        Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | status | [SteamAPIInitResult enum] (#steamapiinitresult)| The status code of the initialization attempt.
        | verbal | string | The error message if initialization fails.

    !!! info "Notes"
        This function **does not** contain a second parameter in **GDNative** and the first parameter is for the deprecated retrieving statistics and achievements.

### steamShutdown

!!! function "steamShutdown( )"
    Shuts down the Steamworks API, releases pointers and frees memory.

    This is called automatically by GodotSteam when the game or app shuts down.

    !!! returns "Returns: void"

{==
## :material-variable: Properties
==}

Name | Variant | Set | Get
---- | ------- | --- | ---
current_app_id | uint32_t | [set_current_app_id](#set_current_app_id) | [get_current_app_id](#get_current_app_id)
current_browser_handle | uint32_t | [set_browser_handle](#set_browser_handle) | [get_browser_handle](#get_browser_handle)
current_clan_id | uint64_t | [set_current_clan_id](#set_current_clan_id) | [get_current_clan_id](#get_current_clan_id)
current_steam_id | uint64_t | [set_current_steam_id](#set_current_steam_id) | [get_current_steam_id](#set_current_steam_id)
inventory_handle | int32 | [set_inventory_handle](#set_inventory_handle) | [get_inventory_handle](#get_inventory_handle)
inventory_update_handle | uint64_t | [set_inventory_update_handle](#set_inventory_update_handle) | [get_inventory_update_handle](#get_inventory_update_handle)
leaderboard_details_max | int | [set_leaderboard_details_max](#set_leaderboard_details_max) | [get_leaderboard_details_max](#get_leaderboard_details_max)
leaderboard_entries_array | Array | [set_leaderboard_entries](#set_leaderboard_entries) | [get_leaderboard_entries](#get_leaderboard_entries)
leaderboard_handle | uint64_t | [set_leaderboard_handle](#set_leaderboard_handle) | [get_leaderboard_handle](#get_leaderboard_handle)

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
ACCOUNT_ID_INVALID | k_uAccountIdInvalid | 0 | An invalid account ID.
API_CALL_INVALID | k_uAPICallInvalid | 0x0 | An invalid Steam API Call handle.
APP_ID_INVALID | k_uAppIdInvalid | 0x0 | An invalid app ID.
AUTH_TICKET_INVALID | k_HAuthTicketInvalid | An invalid user authentication ticket.
DEPOT_ID_INVALID | k_uDepotIdInvalid | 0x0 | An invalid depot ID.
GAME_EXTRA_INFO_MAX | k_cchGameExtraInfoMax | 64 | The maximum size (in UTF-8 bytes, including the null terminator) of the **extra_info** parameter of trackAppUsageEvent (since been deprecated).
MAX_STEAM_DATAGRAM_GAME_COORDINATOR_SERVER_LOGIN_APP_DATA | k_cbMaxSteamDatagramGameCoordinatorServerLoginAppData | 2048 | -
MAX_STEAM_DATAGRAM_GAME_COORDINATOR_SERVER_LOGIN_SERIALIZED | k_cbMaxSteamDatagramGameCoordinatorServerLoginSerialized | 4096 | -
PARTY_BEACON_ID_INVALID | k_ulPartyBeaconIdInvalid | 0 | An invalid party beacon ID.
QUERY_PORT_ERROR | k_usFriendGameInfoQueryPort_Error | 0xFFFE | We were unable to get the query port for this server.
QUERY_PORT_NOT_INITIALIZED | k_usFriendGameInfoQueryPort_NotInitialized | 0xFFFF | We haven't asked the GameServer for this query port's actual value yet.
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

### BetaBranchFlags
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
BETA_BRANCH_NONE | k_EBetaBranch_None | 0 | - 
BETA_BRANCH_DEFAULT | k_EBetaBranch_Default | 1 | This is the default branch ("public").
BETA_BRANCH_AVAILABLE | k_EBetaBranch_Available | 2 | This branch can be selected (available).
BETA_BRANCH_PRIVATE | k_EBetaBranch_Private | 4 | This is a private branch (password protected).
BETA_BRANCH_SELECTED | k_EBetaBranch_Selected | 8 | This is the currently selected branch (active).
BETA_BRANCH_INSTALLED | k_EBetaBranch_Installed | 16 | This is the currently installed branch (mounted).

### BroadcastUploadResult
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
BROADCAST_UPLOAD_RESULT_NONE | k_EBroadcastUploadResultNone | 0 | Broadcast state unknown.
BROADCAST_UPLOAD_RESULT_OK | k_EBroadcastUploadResultOK | 1 | Broadcast was good, no problems.
BROADCAST_UPLOAD_RESULT_INIT_FAILED | k_EBroadcastUploadResultInitFailed | 2 | Broadcast init failed.
BROADCAST_UPLOAD_RESULT_FRAME_FAILED | k_EBroadcastUploadResultFrameFailed | 3 | Broadcast frame upload failed.
BROADCAST_UPLOAD_RESULT_TIME_OUT | k_EBroadcastUploadResultTimeout | 4 | Broadcast upload timed out.
BROADCAST_UPLOAD_RESULT_BANDWIDTH_EXCEEDED | k_EBroadcastUploadResultBandwidthExceeded | 5 | Broadcast send too much data.
BROADCAST_UPLOAD_RESULT_LOW_FPS | k_EBroadcastUploadResultLowFPS | 6 | Broadcast FPS too low.
BROADCAST_UPLOAD_RESULT_MISSING_KEYFRAMES | k_EBroadcastUploadResultMissingKeyFrames | 7 | Broadcast sending not enough key frames.
BROADCAST_UPLOAD_RESULT_NO_CONNECTION | k_EBroadcastUploadResultNoConnection | 8 | Broadcast client failed to connect to relay.
BROADCAST_UPLOAD_RESULT_RELAY_FAILED | k_EBroadcastUploadResultRelayFailed | 9 | Relay dropped the upload.
BROADCAST_UPLOAD_RESULT_SETTINGS_CHANGED | k_EBroadcastUploadResultSettingsChanged | 10 | The client changed broadcast settings.
BROADCAST_UPLOAD_RESULT_MISSING_AUDIO | k_EBroadcastUploadResultMissingAudio | 11 | Client failed to send audio data.
BROADCAST_UPLOAD_RESULT_TOO_FAR_BEHIND | k_EBroadcastUploadResultTooFarBehind | 12 | Clients was too slow uploading.
BROADCAST_UPLOAD_RESULT_TRANSCODE_BEHIND | k_EBroadcastUploadResultTranscodeBehind | 13 | Server failed to keep up with transcode.
BROADCAST_UPLOAD_RESULT_NOT_ALLOWED_TO_PLAY | k_EBroadcastUploadResultNotAllowedToPlay | 14 | Broadcast does not have permissions to play game.
BROADCAST_UPLOAD_RESULT_BUSY | k_EBroadcastUploadResultBusy | 15 | RTMP host to busy to take new broadcast stream, choose another.
BROADCAST_UPLOAD_RESULT_BANNED | k_EBroadcastUploadResultBanned | 16 | Account banned from community broadcast.
BROADCAST_UPLOAD_RESULT_ALREADY_ACTIVE | k_EBroadcastUploadResultAlreadyActive | 17 | We already already have an stream running.
BROADCAST_UPLOAD_RESULT_FORCED_OFF | k_EBroadcastUploadResultForcedOff | 18 | We explicitly shutting down a broadcast.
BROADCAST_UPLOAD_RESULT_AUDIO_BEHIND | k_EBroadcastUploadResultAudioBehind | 19 | Audio stream was too far behind video .
BROADCAST_UPLOAD_RESULT_SHUTDOWN | k_EBroadcastUploadResultShutdown | 20 | Broadcast server was shut down.
BROADCAST_UPLOAD_RESULT_DISCONNECT | k_EBroadcastUploadResultDisconnect | 21 | Broadcast uploader TCP disconnected.
BROADCAST_UPLOAD_RESULT_VIDEO_INIT_FAILED | k_EBroadcastUploadResultVideoInitFailed | 22 | Invalid video settings .
BROADCAST_UPLOAD_RESULT_AUDIO_INIT_FAILED | k_EBroadcastUploadResultAudioInitFailed | 23 | Invalid audio settings.

### ChatEntryType
Previously was only friend-to-friend message types. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CHAT_ENTRY_TYPE_INVALID | k_EChatEntryTypeInvalid | 0 | -
CHAT_ENTRY_TYPE_CHAT_MSG | k_EChatEntryTypeChatMsg | 1 | Normal text message from another user.
CHAT_ENTRY_TYPE_TYPING | k_EChatEntryTypeTyping | 2 | Another user is typing; not used in multi-user chat.
CHAT_ENTRY_TYPE_INVITE_GAME | k_EChatEntryTypeInviteGame | 3 | Invite from other user into that users current game.
CHAT_ENTRY_TYPE_EMOTE | k_EChatEntryTypeEmote | 4 | Text emote message; deprecated, should be treated as ChatMsg.
CHAT_ENTRY_TYPE_LEFT_CONVERSATION | k_EChatEntryTypeLeftConversation | 6 | User has left the conversation; closed chat window.
CHAT_ENTRY_TYPE_ENTERED | k_EChatEntryTypeEntered | 7 | Uer has entered the conversation; used in multi-user chat and group chat.
CHAT_ENTRY_TYPE_WAS_KICKED | k_EChatEntryTypeWasKicked | 8 | User was kicked; data: 64-bit steamid of actor performing the kick.
CHAT_ENTRY_TYPE_WAS_BANNED | k_EChatEntryTypeWasBanned | 9 | User was banned; data: 64-bit steamid of actor performing the ban.
CHAT_ENTRY_TYPE_DISCONNECTED | k_EChatEntryTypeDisconnected | 10 | User disconnected.
CHAT_ENTRY_TYPE_HISTORICAL_CHAT | k_EChatEntryTypeHistoricalChat | 11 | A chat message from user's chat history or offilne message.
CHAT_ENTRY_TYPE_LINK_BLOCKED | k_EChatEntryTypeLinkBlocked | 14 | A link was removed by the chat filter.

### ChatRoomEnterResponse
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CHAT_ROOM_ENTER_RESPONSE_SUCCESS | k_EChatRoomEnterResponseSuccess | 1 | Success.
CHAT_ROOM_ENTER_RESPONSE_DOESNT_EXIST | k_EChatRoomEnterResponseDoesntExist | 2 | Chat doesn't exist; probably closed.
CHAT_ROOM_ENTER_RESPONSE_NOT_ALLOWED | k_EChatRoomEnterResponseNotAllowed | 3 | General denied; you don't have the permissions needed to join the chat.
CHAT_ROOM_ENTER_RESPONSE_FULL | k_EChatRoomEnterResponseFull | 4 | Chat room has reached its maximum size.
CHAT_ROOM_ENTER_RESPONSE_ERROR | k_EChatRoomEnterResponseError | 5 | Unexpected error.
CHAT_ROOM_ENTER_RESPONSE_BANNED | k_EChatRoomEnterResponseBanned | 6 | You are banned from this chat room and may not join.
CHAT_ROOM_ENTER_RESPONSE_LIMITED | k_EChatRoomEnterResponseLimited | 7 | Joining this chat is not allowed because you are a limited user; no value on account.
CHAT_ROOM_ENTER_RESPONSE_CLAN_DISABLED | k_EChatRoomEnterResponseClanDisabled | 8 | Attempt to join a clan chat when the clan is locked or disabled.
CHAT_ROOM_ENTER_RESPONSE_COMMUNITY_BAN | k_EChatRoomEnterResponseCommunityBan | 9 | Attempt to join a chat when the user has a community lock on their account.
CHAT_ROOM_ENTER_RESPONSE_MEMBER_BLOCKED_YOU | k_EChatRoomEnterResponseMemberBlockedYou | 10 | Join failed; some member in the chat has blocked you from joining.
CHAT_ROOM_ENTER_RESPONSE_YOU_BLOCKED_MEMBER | k_EChatRoomEnterResponseYouBlockedMember | 11 | Join failed; you have blocked some member already in the chat.
CHAT_ROOM_ENTER_RESPONSE_RATE_LIMIT_EXCEEDED | k_EChatRoomEnterResponseRatelimitExceeded | 15 | Join failed; too many join attempts in a very short period of time.

### ChatSteamIDInstanceFlags
Special flags for Chat accounts; they go in the top 8 bits of the Steam ID's "instance", leaving 12 for the actual instances. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CHAT_ACCOUNT_INSTANCE_MASK | k_EChatAccountInstanceMask | 0X00000FFF | Top 8 bits are flags.
CHAT_INSTANCE_FLAG_CLAN | k_EChatInstanceFlagClan | ((k_unSteamAccountInstanceMask + 1) >> 1) | Top bit.
CHAT_INSTANCE_FLAG_LOBBY | k_EChatInstanceFlagLobby | ((k_unSteamAccountInstanceMask + 1) >> 2) | Next one down, etc.
CHAT_INSTANCE_FLAG_MMS_LOBBY | k_EChatInstanceFlagMMSLobby | ((k_unSteamAccountInstanceMask + 1) >> 3) | Next one down, etc.

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

### DurationControlProgress
Describes XP / progress restrictions to apply for games with duration control / anti-indulgence enabled for minor Steam China users. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
DURATION_CONTROL_PROGRESS_FULL | k_EDurationControlProgress_Full | 0 | Full progress.
DURATION_CONTROL_PROGRESS_HALF | k_EDurationControlProgress_Half | 1 | Deprecated; XP or persistent rewards should be halved.
DURATION_CONTROL_PROGRESS_NONE | k_EDurationControlProgress_None | 2 | Deprecated; XP or persistent rewards should be stopped.
DURATION_CONTROL_EXIT_SOON_3H | k_EDurationControl_ExitSoon_3h | 3 | Allowed 3h time since 5h gap/break has elapsed, game should exit; Steam will terminate the game soon.
DURATION_CONTROL_EXIT_SOON_5H | k_EDurationControl_ExitSoon_5h | 4 | Allowed 5h time in calendar day has elapsed, game should exit; Steam will terminate the game soon.
DURATION_CONTROL_EXIT_SOON_NIGHT | k_EDurationControl_ExitSoon_Night | 5 | Game running after day period, game should exit; Steam will terminate the game soon.

### DurationControlOnlineState
Specifies a game's online state in relation to duration control. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
DURATION_CONTROL_ONLINE_STATE_INVALID | k_EDurationControlOnlineState_Invalid | 0 | Nil value.
DURATION_CONTROL_ONLINE_STATE_OFFLINE | k_EDurationControlOnlineState_Offline | 1 | Currently in offline play; single-player, offline co-op, etc.
DURATION_CONTROL_ONLINE_STATE_ONLINE | k_EDurationControlOnlineState_Online | 2 | Currently in online play.
DURATION_CONTROL_ONLINE_STATE_ONLINE_HIGH_PRIORITY | k_EDurationControlOnlineState_OnlineHighPri | 3 | Currently in online play and requests not to be interrupted.

### DurationControlNotification
Describes which notification timer has expired, for steam china duration control feature. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
DURATION_CONTROL_NOTIFICATION_NONE | k_EDurationControlNotification_None | 0 | Just informing you about progress, no notification to show.
DURATION_CONTROL_NOTIFICATION_1_HOUR | k_EDurationControlNotification_1Hour | 1 | "You've been playing for N hours".
DURATION_CONTROL_NOTIFICATION_3_HOURS | k_EDurationControlNotification_3Hours | 2 | Deprecated; "you've been playing for 3 hours; take a break".
DURATION_CONTROL_NOTIFICATION_HALF_PROGRESS | k_EDurationControlNotification_HalfProgress | 3 | Deprecated; "your XP / progress is half normal".
DURATION_CONTROL_NOTIFICATION_NO_PROGRESS | k_EDurationControlNotification_NoProgress | 4 | Deprecated; "your XP / progress is zero".
DURATION_CONTROL_NOTIFICATION_EXIT_SOON_3H | k_EDurationControlNotification_ExitSoon_3h | 5 | Allowed 3h time since 5h gap/break has elapsed, game should exit; Steam will terminate the game soon.
DURATION_CONTROL_NOTIFICATION_EXIT_SOON_5H | k_EDurationControlNotification_ExitSoon_5h | 6 | Allowed 5h time in calendar day has elapsed, game should exit; Steam will terminate the game soon.
DURATION_CONTROL_NOTIFICATION_EXIT_SOON_NIGHT | k_EDurationControlNotification_ExitSoon_Night | 7 | Game running after day period, game should exit; Steam will terminate the game soon.

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

### IPv6ConnectivityProtocol
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
IPV6_CONNECTIVITY_PROTOCOL_INVALID | k_ESteamIPv6ConnectivityProtocol_Invalid | 0 | -
IPV6_CONNECTIVITY_PROTOCOL_HTTP | k_ESteamIPv6ConnectivityProtocol_HTTP | 1 | Because a proxy may make this different than other protocols.
IPV6_CONNECTIVITY_PROTOCOL_UDP | k_ESteamIPv6ConnectivityProtocol_UDP | 2 | Test UDP connectivity. Uses a port that is commonly needed for other Steam stuff. If UDP works, TCP probably works.

### IPv6ConnectivityState
For the above transport protocol, what do we think the local machine's connectivity to the internet over IPv6 is like. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
IPV6_CONNECTIVITY_STATE_UNKNOWN | k_ESteamIPv6ConnectivityState_Unknown | 0 | We haven't run a test yet.
IPV6_CONNECTIVITY_STATE_GOOD | k_ESteamIPv6ConnectivityState_Good | 1 | We have recently been able to make a request on ipv6 for the given protocol.
IPV6_CONNECTIVITY_STATE_BAD | k_ESteamIPv6ConnectivityState_Bad | 2 | We failed to make a request, either because this machine has no ipv6 address assigned or it has no upstream connectivity.

### MarketNotAllowedReasonFlags
Reasons a user may not use the Community Market. Used in [market_eligibility_response](user.md#market_eligibility_response). Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
MARKET_NOT_ALLOWED_REASON_NONE | k_EMarketNotAllowedReason_None | 0 | -
MARKET_NOT_ALLOWED_REASON_TEMPORARY_FAILURE | k_EMarketNotAllowedReason_TemporaryFailure | (1 << 0) | A back-end call failed or something that might work again on retry.
MARKET_NOT_ALLOWED_REASON_ACCOUNT_DISABLED | k_EMarketNotAllowedReason_AccountDisabled | (1 << 1) | Disabled account.
MARKET_NOT_ALLOWED_REASON_ACCOUNT_LOCKED_DOWN | k_EMarketNotAllowedReason_AccountLockedDown | (1 << 2) | Locked account.
MARKET_NOT_ALLOWED_REASON_ACCOUNT_LIMITED | k_EMarketNotAllowedReason_AccountLimited | (1 << 3) | Limited account; no purchases.
MARKET_NOT_ALLOWED_REASON_TRADE_BANNED | k_EMarketNotAllowedReason_TradeBanned | (1 << 4) | The account is banned from trading items.
MARKET_NOT_ALLOWED_REASON_ACCOUNT_NOT_TRUSTED | k_EMarketNotAllowedReason_AccountNotTrusted | (1 << 5) | Wallet funds aren't tradable because the user has had no purchase activity in the last year or has had no purchases prior to last month.
MARKET_NOT_ALLOWED_REASON_STEAM_GUARD_NOT_ENABLED | k_EMarketNotAllowedReason_SteamGuardNotEnabled | (1 << 6) | The user doesn't have Steam Guard enabled.
MARKET_NOT_ALLOWED_REASON_STEAM_GUARD_ONLY_RECENTLY_ENABLED | k_EMarketNotAllowedReason_SteamGuardOnlyRecentlyEnabled | (1 << 7) | The user has Steam Guard but it hasn't been enabled for the required number of days.
MARKET_NOT_ALLOWED_REASON_RECENT_PASSWORD_RESET | k_EMarketNotAllowedReason_RecentPasswordReset | (1 << 8) | The user has recently forgotten their password and reset it.
MARKET_NOT_ALLOWED_REASON_NEW_PAYMENT_METHOD | k_EMarketNotAllowedReason_NewPaymentMethod | (1 << 9) | The user has recently funded his or her wallet with a new payment method.
MARKET_NOT_ALLOWED_REASON_INVALID_COOKIE | k_EMarketNotAllowedReason_InvalidCookie | (1 << 10) | An invalid cookie was sent by the user.
MARKET_NOT_ALLOWED_REASON_USING_NEW_DEVICE | k_EMarketNotAllowedReason_UsingNewDevice | (1 << 11) | The user has Steam Guard but is using a new computer or web browser.
MARKET_NOT_ALLOWED_REASON_RECENT_SELF_REFUND | k_EMarketNotAllowedReason_RecentSelfRefund | (1 << 12) | The user has Steam Guard, but is using a new computer or web browser.
MARKET_NOT_ALLOWED_REASON_NEW_PAYMENT_METHOD_CANNOT_BE_VERIFIED | k_EMarketNotAllowedReason_NewPaymentMethodCannotBeVerified | (1 << 13) | The user has recently funded his or her wallet with a new payment method that cannot be verified.
MARKET_NOT_ALLOWED_REASON_NO_RECENT_PURCHASES | k_EMarketNotAllowedReason_NoRecentPurchases | (1 << 14) | Not only is the account not trusted but they have no recent purchases at all.
MARKET_NOT_ALLOWED_REASON_ACCEPTED_WALLET_GIFT | k_EMarketNotAllowedReason_AcceptedWalletGift | (1 << 15) | User accepted a wallet gift that was recently purchased.

### NotificationPosition
Possible positions to tell the overlay to show notifications in. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
POSITION_INVALID | k_EPositionInvalid |  -1| -
POSITION_TOP_LEFT | k_EPositionTopLeft | 0 | -
POSITION_TOP_RIGHT | k_EPositionTopRight | 1 | -
POSITION_BOTTOM_LEFT | k_EPositionBottomLeft | 2 | -
POSITION_BOTTOM_RIGHT | k_EPositionBottomRight | 3 | -

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

### VoiceResult
Error codes for use with the voice functions. Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
VOICE_RESULT_OK | k_EVoiceResultOK | 0 | -
VOICE_RESULT_NOT_INITIALIZED | k_EVoiceResultNotInitialized | 1 | -
VOICE_RESULT_NOT_RECORDING | k_EVoiceResultNotRecording | 2 | -
VOICE_RESULT_NO_DATE | k_EVoiceResultNoData | 3 | -
VOICE_RESULT_BUFFER_TOO_SMALL | k_EVoiceResultBufferTooSmall | 4 | -
VOICE_RESULT_DATA_CORRUPTED | k_EVoiceResultDataCorrupted | 5 | -
VOICE_RESULT_RESTRICTED | k_EVoiceResultRestricted | 6 | -
VOICE_RESULT_UNSUPPORTED_CODEC | k_EVoiceResultUnsupportedCodec | 7 | -
VOICE_RESULT_RECEIVER_OUT_OF_DATE | k_EVoiceResultReceiverOutOfDate | 8 | -
VOICE_RESULT_RECEIVER_DID_NOT_ANSWER | k_EVoiceResultReceiverDidNotAnswer | 9 | -