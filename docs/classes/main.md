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

### get_godotsteam_version

!!! function "get_godotsteam_version( )"
    Returns the current version of the GodotSteam editor you are using.

    **Returns:** String

    **Note:** This is a GodotSteam-specific function.

    ---
    [ :material-tag-plus: Added GodotSteam 4.11](../changelog/godot4.md/#version-411){ .md-button .md-button--changes target="_blank" }

### get_leaderboard_entries

!!! function "get_leaderboard_entries( )"
    Returns the current list of internally stored leaderboard entries.

    **Returns:** array

    **Note:** This is a GodotSteam-specific function.

    ---
    [ :material-tag-plus: Added GodotSteam 4.11](../changelog/godot4.md/#version-411){ .md-button .md-button--changes target="_blank" }

### getSteamID32

!!! function "getSteamID32( ```uint64_t``` steam_id )"
    Convert a SteamID64 into a SteamID32.

    **Returns:** uint32_t

    **Note:** This is a GodotSteam-specific function.

### get_steam_init_result

!!! function "get_steam_init_result( )"
    Returns the Steam initialization dictionary from either [steamInitEX](#steaminitex) or auto-initialization set in the Project Settings.  If you use [steamInit](#steaminit), this does not get filled out.

    **Returns:** dictionary

    * status (int)
    * verbal (string)

    You will receive one of these integer results:

    * 0 / "Steamworks active"
    * 1 / "Failed (generic)"
    * 2 / "Cannot connect to Steam, client probably isn't running"
    * 3 / "Steam client appears to be out of date"

    **Note:** This is a GodotSteam-specific function.

### isAnonAccount

!!! function "isAnonAccount( ```uint64_t``` steam_id )"
    Is this an anonymous account?

    **Returns:** bool

    **Note:** While this is not listed in the Steamworks docs, it is in the SDK file steamclientpublic.h

### isAnonUserAccount

!!! function "isAnonUserAccount( ```uint64_t``` steam_id )"
    Is this an anonymous user account? Used to create an account or reset a password, but do not try to do this.

    **Returns:** bool

    **Note:** While this is not listed in the Steamworks docs, it is in the SDK. 

### isChatAccount

!!! function "isChatAccount( ```uint64_t``` steam_id )"
    Is this a chat account ID?

    **Returns:** bool

    **Note:** While this is not listed in the Steamworks docs, it is in the SDK. 

### isClanAccount

!!! function "isClanAccount( ```uint64_t``` steam_id )"
    Is this a clan account ID?

    **Returns:** bool

    **Note:** While this is not listed in the Steamworks docs, it is in the SDK. 

### isConsoleUserAccount

!!! function "isConsoleUserAccount( ```uint64_t``` steam_id )"
    Is this a faked up Steam ID for a PSN friend account?

    **Returns:** bool

    **Note:** While this is not listed in the Steamworks docs, it is in the SDK. 

### isIndividualAccount

!!! function "isIndividualAccount( ```uint64_t``` steam_id )"
    Is this an individual user account ID?

    **Returns:** bool

    **Note:** While this is not listed in the Steamworks docs, it is in the SDK. 

### isLobby

!!! function "isLobby( ```uint64_t``` steam_id )"
    Is this a lobby account ID?

    **Returns:** bool

    **Note:** While this is not listed in the Steamworks docs, it is in the SDK. 

### isSteamRunning

!!! function "isSteamRunning()"
    Check if the Steam client is running. 

    **Return:** bool

    Naturally, returns **true** if the Steam client is running.

    **Note:** While this is not listed in the Steamworks docs, it is in the SDK. 

### restartAppIfNecessary

!!! function "restartAppIfNecessary( ```uint32_t``` app_id )"
    Checks if your executable was launched through Steam and relaunches it through Steam if it wasn't.

    **Return:** bool

    If this returns **true** then it starts the Steam client if required and launches your game again through it, and you should quit your process as soon as possible. If it returns **false**, then your game was launched by the Steam client and no action needs to be taken.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_api#SteamAPI_RestartAppIfNecessary){ .md-button .md-button--doc_classes target="_blank" }

### run_callbacks

!!! function "run_callbacks()"
    Enables your application to receive callbacks from Steamworks. Must be placed in your \_process function.

    Alternatively, you can check **Embed Callbacks** in the **Project Settings > Steam > Initialization** panel and this will automatically set for you.

    **Returns:** void

### set_leaderboard_details_max

!!! function "set_leaderboard_details_max( ```int``` new_leaderboard_details_max )"
    Set function for leaderboard_details_max property; which is the maximum number of details to return for leaderboard entries.

    This can only go as high as k_cLeaderboardDetailsMax / 256.

    **Note:** This is a GodotSteam-specific function.

    ---
    [ :material-tag-plus: Added GodotSteam 4.11](../changelog/godot4.md/#version-411){ .md-button .md-button--changes target="_blank" }

### steamInit

!!! function "steamInit( ```uint32_t``` app_id, ```bool``` embed_callbacks )"
    Initialize the SDK, without worrying about the cause of failure.

    You can pass your **app ID** to the _first argument_ and GodotSteam will set the OS environment for you so you do not have to do this manually anymore. If nothing is passed, it will not be set.  You can pass **true** to the _second argument_ to have GodotSteam connect and use **run_callbacks** internally so you do not have to do this manually anymore. If nothing is passed, it will not run callbacks internally.  Lastly, if you set the **app_id** and/or check **Embed Callbacks** in the **Project Settings > Steam > Initialization**, these will automatically set for you and you can just call **steamInitEx()** with no arguments filled out.

    **Return:** bool

    **Note:** This function **does not** contain the second or third argument in **GDNative** but does contain the first, deprecated argument for retrieving statistics and achievements.

    ------
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/steam_api#SteamAPI_Init){ .md-button .md-button--doc_classes target="_blank" }

### steamInitEx

!!! function "steamInitEx( ```uint32_t``` app_id, ```bool``` embed_callbacks )"
    Initialize the Steamworks SDK. On success **STEAM_API_INIT_RESULT_OK** is returned. Otherwise, if **error_message** is non-NULL, it will receive a non-localized message that explains the reason for the failure.

    You can pass your **app ID** to the _first argument_ and GodotSteam will set the OS environment for you so you do not have to do this manually anymore. If nothing is passed, it will not be set.  You can pass **true** to the _second argument_ to have GodotSteam connect and use **run_callbacks** internally so you do not have to do this manually anymore. If nothing is passed, it will not run callbacks internally.  Lastly, if you set the **app_id** and/or check **Embed Callbacks** in the **Project Settings > Steam > Initialization**, these will automatically set for you and you can just call **steamInitEx()** with no arguments filled out.

    **Returns:** dictionary

    * status (int)
    * verbal (string)

    You will receive one of these integer results:

    * 0 / "Steamworks active"
    * 1 / "Failed (generic)"
    * 2 / "Cannot connect to Steam, client probably isn't running"
    * 3 / "Steam client appears to be out of date"

    **Note:** This function **does not** contain the second or third argument in **GDNative** but does contain the first, deprecated argument for retrieving statistics and achievements.

### steamShutdown

!!! function "steamShutdown"
    Shuts down the Steamworks API, releases pointers and frees memory.

    This is called automatically by GodotSteam when the game or app shuts down.

    **Returns:** nothing


#define ACCOUNT_ID_INVALID k_uAccountIdInvalid
#define API_CALL_INVALID k_uAPICallInvalid
#define APP_ID_INVALID k_uAppIdInvalid
#define AUTH_TICKET_INVALID k_HAuthTicketInvalid
#define DEPOT_ID_INVALID k_uDepotIdInvalid
#define GAME_EXTRA_INFO_MAX k_cchGameExtraInfoMax
#define INVALID_BREAKPAD_HANDLE 0 //deprecated?
#define QUERY_PORT_ERROR 0xFFFE //deprecated?
#define QUERY_PORT_NOT_INITIALIZED 0xFFFF //deprecated?
#define STEAM_ACCOUNT_ID_MASK k_unSteamAccountIDMask
#define STEAM_ACCOUNT_INSTANCE_MASK k_unSteamAccountInstanceMask
#define STEAM_BUFFER_SIZE 255 //deprecated?
#define STEAM_ID_NIL k_steamIDNil
#define STEAM_ID_OUT_OF_DATE_GAME_SERVER k_steamIDOutofDateGS
#define STEAM_ID_LAN_MODE_GAME_SERVER k_steamIDLanModeGS
#define STEAM_ID_NOT_INIT_YET_GAME_SERVER k_steamIDNotInitYetGS
#define STEAM_ID_NON_GAME_SERVER k_steamIDNonSteamGS
#define STEAM_LARGE_BUFFER_SIZE 8160 //deprecated?
#define STEAM_MAX_ERROR_MESSAGE 1024
#define STEAM_USER_CONSOLE_INSTANCE 2 //deprecated?
#define STEAM_USER_DESKTOP_INSTANCE k_unSteamUserDefaultInstance
#define STEAM_USER_WEB_INSTANCE 4 //deprecated?

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

### AppOwnershipFlags

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
APP_OWNERSHIP_FLAGS_NONE | 0x0000
APP_OWNERSHIP_FLAGS_OWNS_LICENSE | 0x0001
APP_OWNERSHIP_FLAGS_FREE_LICENSE | 0x0002
APP_OWNERSHIP_FLAGS_REGION_RESTRICTED | 0x0004
APP_OWNERSHIP_FLAGS_LOW_VIOLENCE | 0x0008
APP_OWNERSHIP_FLAGS_INVALID_PLATFORM | 0x0010
APP_OWNERSHIP_FLAGS_SHARED_LICENSE | 0x0020
APP_OWNERSHIP_FLAGS_FREE_WEEKEND | 0x0040
APP_OWNERSHIP_FLAGS_RETAIL_LICENSE | 0x0080
APP_OWNERSHIP_FLAGS_LICENSE_LOCKED | 0x0100
APP_OWNERSHIP_FLAGS_LICENSE_PENDING | 0x0200
APP_OWNERSHIP_FLAGS_LICENSE_EXPIRED | 0x0400
APP_OWNERSHIP_FLAGS_LICENSE_PERMANENT | 0x0800
APP_OWNERSHIP_FLAGS_LICENSE_RECURRING | 0x1000
APP_OWNERSHIP_FLAGS_LICENSE_CANCELED | 0x2000
APP_OWNERSHIP_FLAGS_AUTO_GRANT | 0x4000
APP_OWNERSHIP_FLAGS_PENDING_GIFT | 0x8000
APP_OWNERSHIP_FLAGS_RENTAL_NOT_ACTIVATED | 0x10000
APP_OWNERSHIP_FLAGS_RENTAL | 0x20000
APP_OWNERSHIP_FLAGS_SITE_LICENSE | 0x40000

### AppReleaseState

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
APP_RELEASE_STATE_UNKNOWN | 0
APP_RELEASE_STATE_UNAVAILABLE | 1
APP_RELEASE_STATE_PRERELEASE | 2
APP_RELEASE_STATE_PRELOAD_ONLY | 3
APP_RELEASE_STATE_RELEASED | 4

### AppType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
APP_TYPE_INVALID | 0x000
APP_TYPE_GAME | 0x001
APP_TYPE_APPLICATION | 0x002
APP_TYPE_TOOL | 0x004
APP_TYPE_DEMO | 0x008
APP_TYPE_MEDIA_DEPRECATED | 0x010
APP_TYPE_DLC | 0x020
APP_TYPE_GUIDE | 0x040
APP_TYPE_DRIVER | 0x080
APP_TYPE_CONFIG | 0x100
APP_TYPE_HARDWARE | 0x200
APP_TYPE_FRANCHISE | 0x400
APP_TYPE_VIDEO | 0x800
APP_TYPE_PLUGIN | 0x1000
APP_TYPE_MUSIC | 0x2000
APP_TYPE_SERIES | 0x4000
APP_TYPE_SHORTCUT | 0x40000000
APP_TYPE_DEPOT_ONLY | 0X80000000

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

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
BROADCAST_UPLOAD_RESULT_NONE | 0
BROADCAST_UPLOAD_RESULT_OK | 1
BROADCAST_UPLOAD_RESULT_INIT_FAILED | 2
BROADCAST_UPLOAD_RESULT_FRAME_FAILED | 3
BROADCAST_UPLOAD_RESULT_TIME_OUT | 4
BROADCAST_UPLOAD_RESULT_BANDWIDTH_EXCEEDED | 5
BROADCAST_UPLOAD_RESULT_LOW_FPS | 6
BROADCAST_UPLOAD_RESULT_MISSING_KEYFRAMES | 7
BROADCAST_UPLOAD_RESULT_NO_CONNECTION | 8
BROADCAST_UPLOAD_RESULT_RELAY_FAILED | 9
BROADCAST_UPLOAD_RESULT_SETTINGS_CHANGED | 10
BROADCAST_UPLOAD_RESULT_MISSING_AUDIO | 11
BROADCAST_UPLOAD_RESULT_TOO_FAR_BEHIND | 12
BROADCAST_UPLOAD_RESULT_TRANSCODE_BEHIND | 13

### ChatEntryType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CHAT_ENTRY_TYPE_INVALID | 0
CHAT_ENTRY_TYPE_CHAT_MSG | 1
CHAT_ENTRY_TYPE_TYPING | 2
CHAT_ENTRY_TYPE_INVITE_GAME | 3
CHAT_ENTRY_TYPE_EMOTE | 4
CHAT_ENTRY_TYPE_LEFT_CONVERSATION | 6
CHAT_ENTRY_TYPE_ENTERED | 7
CHAT_ENTRY_TYPE_WAS_KICKED | 8
CHAT_ENTRY_TYPE_WAS_BANNED | 9
CHAT_ENTRY_TYPE_DISCONNECTED | 10
CHAT_ENTRY_TYPE_HISTORICAL_CHAT | 11
CHAT_ENTRY_TYPE_LINK_BLOCKED | 14

### ChatRoomEnterResponse

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CHAT_ROOM_ENTER_RESPONSE_SUCCESS | 1
CHAT_ROOM_ENTER_RESPONSE_DOESNT_EXIST | 2
CHAT_ROOM_ENTER_RESPONSE_NOT_ALLOWED | 3
CHAT_ROOM_ENTER_RESPONSE_FULL | 4
CHAT_ROOM_ENTER_RESPONSE_ERROR | 5
CHAT_ROOM_ENTER_RESPONSE_BANNED | 6
CHAT_ROOM_ENTER_RESPONSE_LIMITED | 7
CHAT_ROOM_ENTER_RESPONSE_CLAN_DISABLED | 8
CHAT_ROOM_ENTER_RESPONSE_COMMUNITY_BAN | 9
CHAT_ROOM_ENTER_RESPONSE_MEMBER_BLOCKED_YOU | 10
CHAT_ROOM_ENTER_RESPONSE_YOU_BLOCKED_MEMBER | 11

### ChatSteamIDInstanceFlags

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CHAT_ACCOUNT_INSTANCE_MASK | 0X00000FFF
CHAT_INSTANCE_FLAG_CLAN | ((k_unSteamAccountInstanceMask + 1) >> 1)
CHAT_INSTANCE_FLAG_LOBBY | ((k_unSteamAccountInstanceMask + 1) >> 2)
CHAT_INSTANCE_FLAG_MMS_LOBBY | ((k_unSteamAccountInstanceMask + 1) >> 3)

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

### DurationControlProgress
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
DURATION_CONTROL_PROGRESS_FULL | 0
DURATION_CONTROL_PROGRESS_HALF | 1
DURATION_CONTROL_PROGRESS_NONE | 2

### DurationControlOnlineState {
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
DURATION_CONTROL_ONLINE_STATE_INVALID | 0
DURATION_CONTROL_ONLINE_STATE_OFFLINE | 1
DURATION_CONTROL_ONLINE_STATE_ONLINE | 2
DURATION_CONTROL_ONLINE_STATE_ONLINE_HIGH_PRIORITY | 3

### DurationControlNotification
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
DURATION_CONTROL_NOTIFICATION_NONE | 0
DURATION_CONTROL_NOTIFICATION_1_HOUR | 1
DURATION_CONTROL_NOTIFICATION_3_HOURS | 3
DURATION_CONTROL_NOTIFICATION_HALF_PROGRESS | 3
DURATION_CONTROL_NOTIFICATION_NO_PROGRESS | 4


### GameIDType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
GAME_TYPE_APP | 0
GAME_TYPE_GAME_MOD | 1
GAME_TYPE_SHORTCUT | 2
GAME_TYPE_P2P | 3

### IPv6ConnectivityProtocol
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
IPV6_CONNECTIVITY_PROTOCOL_INVALID | 0
IPV6_CONNECTIVITY_PROTOCOL_HTTP | 1
IPV6_CONNECTIVITY_PROTOCOL_UDP | 2

### IPv6ConnectivityState
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
IPV6_CONNECTIVITY_STATE_UNKNOWN | 0
IPV6_CONNECTIVITY_STATE_GOOD | 1
IPV6_CONNECTIVITY_STATE_BAD | 2

### LaunchOptionType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LAUNCH_OPTION_TYPE_NONE | 0
LAUNCH_OPTION_TYPE_DEFAULT | 1
LAUNCH_OPTION_TYPE_SAFE_MODE | 2
LAUNCH_OPTION_TYPE_MULTIPLAYER | 3
LAUNCH_OPTION_TYPE_CONFIG | 4
LAUNCH_OPTION_TYPE_OPEN_VR | 5
LAUNCH_OPTION_TYPE_SERVER | 6
LAUNCH_OPTION_TYPE_EDITOR | 7
LAUNCH_OPTION_TYPE_MANUAL | 8
LAUNCH_OPTION_TYPE_BENCHMARK | 9
LAUNCH_OPTION_TYPE_OPTION1 | 10
LAUNCH_OPTION_TYPE_OPTION2 | 11
LAUNCH_OPTION_TYPE_OPTION3 | 12
LAUNCH_OPTION_TYPE_OCULUS_VR | 13
LAUNCH_OPTION_TYPE_OPEN_VR_OVERLAY | 14
LAUNCH_OPTION_TYPE_OS_VR | 15
LAUNCH_OPTION_TYPE_DIALOG | 1000

### MarketNotAllowedReasonFlags
Found in steamclientpublic.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
MARKET_NOT_ALLOWED_REASON_NONE = 0
MARKET_NOT_ALLOWED_REASON_TEMPORARY_FAILURE = (1 << 0)
MARKET_NOT_ALLOWED_REASON_ACCOUNT_DISABLED = (1 << 1)
MARKET_NOT_ALLOWED_REASON_ACCOUNT_LOCKED_DOWN = (1 << 2)
MARKET_NOT_ALLOWED_REASON_ACCOUNT_LIMITED = (1 << 3)
MARKET_NOT_ALLOWED_REASON_TRADE_BANNED = (1 << 4)
MARKET_NOT_ALLOWED_REASON_ACCOUNT_NOT_TRUSTED = (1 << 5)
MARKET_NOT_ALLOWED_REASON_STEAM_GUARD_NOT_ENABLED = (1 << 6)
MARKET_NOT_ALLOWED_REASON_STEAM_GAURD_ONLY_RECENTLY_ENABLED = (1 << 7)
MARKET_NOT_ALLOWED_REASON_RECENT_PASSWORD_RESET = (1 << 8)
MARKET_NOT_ALLOWED_REASON_NEW_PAYMENT_METHOD = (1 << 9)
MARKET_NOT_ALLOWED_REASON_INVALID_COOKIE = (1 << 10)
MARKET_NOT_ALLOWED_REASON_USING_NEW_DEVICE = (1 << 11)
MARKET_NOT_ALLOWED_REASON_RECENT_SELF_REFUND = (1 << 12)
MARKET_NOT_ALLOWED_REASON_NEW_PAYMENT_METHOD_CANNOT_BE_VERIFIED = (1 << 13)
MARKET_NOT_ALLOWED_REASON_NO_RECENT_PURCHASES = (1 << 14)
MARKET_NOT_ALLOWED_REASON_ACCEPTED_WALLET_GIFT = (1 << 15)

### NotificationPosition

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
POSITION_TOP_LEFT | 0
POSITION_TOP_RIGHT | 1
POSITION_BOTTOM_LEFT | 2
POSITION_BOTTOM_RIGHT | 3

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

### UserHasLicenseForAppResult

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
USER_HAS_LICENSE_RESULT_HAS_LICENSE | 0
USER_HAS_LICENSE_RESULT_DOES_NOT_HAVE_LICENSE | 1
USER_HAS_LICENSE_RESULT_NO_AUTH | 2

### VoiceResult

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
VOICE_RESULT_OK | 0
VOICE_RESULT_NOT_INITIALIZED | 1
VOICE_RESULT_NOT_RECORDING | 2
VOICE_RESULT_NO_DATE | 3
VOICE_RESULT_BUFFER_TOO_SMALL | 4
VOICE_RESULT_DATA_CORRUPTED | 5
VOICE_RESULT_RESTRICTED | 6

### VRHMDType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
VR_HMD_TYPE_NONE | -1
VR_HMD_TYPE_UNKNOWN | 0
VR_HMD_TYPE_HTC_DEV | 1
VR_HMD_TYPE_HTC_VIVEPRE | 2
VR_HMD_TYPE_HTC_VIVE | 3
VR_HMD_TYPE_HTC_UNKNOWN | 20
VR_HMD_TYPE_OCULUS_DK1 | 21
VR_HMD_TYPE_OCULUS_DK2 | 22
VR_HMD_TYPE_OCULUS_RIFT | 23
VR_HMD_TYPE_OCULUS_UNKNOWN | 40
