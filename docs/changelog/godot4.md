---
title: Godot 4.x Changelog
description: A history of all changes made to the godot4 branch.
icon: material/gamepad-square
---

A history of all changes to [the ***godot4*** branch.](https://github.com/GodotSteam/GodotSteam/tree/godot4){ target="\_blank" }

---

## Version 4.16

- Added: missing Friends function `activateGameOverlayRemotePlayTogetherInviteDialog()` and `getNumChatsWithUnreadPriorityMessages()`
- Added: missing User function `getMarketEligibility()` and related call result `market_eligibility_response`
- Added: missing UGC functions `setAllowLegacyUpload()`, `removeAllItemKeyValueTags()`
- Added: missing Networking Sockets function `setConnectionUserData()`
- Added: missing Networking Utils functions `setDebugOutputFunction()`, `getIPv4FakeIPType()`, `getRealIdentityForFakeIP()`, `setGlobalCallbackSteamNetConnectionStatusChanged()`, `setGlobalCallbackSteamNetAuthenticationStatusChanged()`, `setGlobalCallbackSteamRelayNetworkStatusChanged()`, `setGlobalCallbackFakeIPResult()`, `setGlobalCallbackMessagesSessionRequest()`, `setGlobalCallbackMessagesSessionFailed()`, `iterateGenericEditableConfigValues()`
- Added: missing Remote Storage call results `published_file_subscribed`, `published_file_unsubscribed`
- Added: missing Remote Play callback `remote_play_guest_invite`
- Added: Steam ID constants for game servers
- Added: some missing constants
- Added: default values to `getQueryUGCContentDdescriptors()` and `getUserContentDescriptorPreferences()` for **max_entries** as there are only five values currently
- Added: missing `releaseCurrentThreadMemory()` function
- Changed: included file ID in returned callback `item_updated`
- Changed: included next cursor in returned callback `ugc_query_completed`
- Changed: `MarketNotAllowedReasonFlags` enums corrected to bitwise
- Changed: added missing result response to `steam_server_disconnected` callback
- Changed: minor swaps from integers to enums where needed
- Changed: corrected STEAM_PARTY_BEACON_LOCATION_DATA to STEAM_PARTY_BEACON_LOCATION_DATA_INVALID
- Changed: some argument or variable names for clarity
- Changed: `advertiseGame()` now has defaults to clear game advertisement is nothing is passed
- Changed: `connected_clan_chat_message` and `connected_friend_chat_message` to no longer send dictionaries
- Changed: `getLeaderboardDisplayType()` now returns the direct enum value instead of a dictionary
- Changed: `getLeaderboardSortMethod()` now returns the direct enum value instead of a dictionary
- Changed: `getMostAchievedAchievementInfo()` and `getNextMostAchievedAchievementInfo()` first key to iterator from rank to be more clear
- Changed: `global_achievement_percentages_ready` now returns the enum / int instead of string for result
- Changed: `global_stats_received` now returns the enum / int instead of string for result
- Changed: `leaderboard_ugc_set` now returns the enum /int instead of string for result
- Changed: `getAppInstallDir()` now just returns the string location of the app
- Changed: `getSyncPlatforms()` now returns the direct enum value instead of a dictionary
- Changed: `enumerate_following_list` callback to better fit the actual Steam callback
- Changed: `request_clan_officer_list` callback now returns bool instead of message for success
- Changed: `retrieveConnectionDetails()` now returns a dictionary instead of the connection details string
- Changed: **base_prices** to **base_price** in `getItemsWithPrices()`
- Fixed: added missing "app_ids" hint to `get_app_dependencies_result` callback
- Fixed: published file ID not being uint64_t in some Remote Storage signals
- Fixed: various types
- Fixed: using direct values instead of constants when the names could not be found
- Fixed: missing store_flag argument for `activateGameOverlayToStore()`
- Fixed: `downloadClanActivity()` now takes multiple clans are intended
- Fixed: constant name from LEADERBOARD_DETAIL_MAX to LEADERBOARD_DETAILS_MAX
- Fixed: `setOverlayNotificationPosition()` not using given argument
- Fixed: set limit to `requestGlobalStats()`, values over 60 defaults to 60 now
- Fixed: some incorrect variable names
- Fixed: `getAchievementProgressLimitsInt()` and `getAchievementProgressLimitsFloat()` using wrong variable name and removing unncessary name key from returned dictionary
- Fixed: incorrect spelling in enum name
- Removed: `getClanChatMessage()` as it can only be used in response to `connected_clan_chat_message` which it is called in anyway
- Removed: `getFriendMessage()` as it can only be used in response to `connected_friend_chat_message` which it is called in anyway
- Removed: `getAvailableVoice()` as it did nothing useful; was already incorporated into related functions
- Removed: **branch_size** key from returned dictionary in **getSupportedGameVersionData()** as it was misleading and useless
- Removed: **sender_user_data** key from returned dictionaries in `receiveMessageOnChannel()`
- Removed: temporarily removed `setDualSenseTriggerEffect()` until it can be overhauled
- Removed: **buffer** from returned dictionary in `getConfigValue()` as it was just the size
- Removed: unnecessary keys in certain Networking Socket functions where messages are received or sent
- Removed: `createHostedDedicatedServerListenSocket()` as it should only be in the GodotSteam Server version

## Version 4.15

- Added: missing functions `checkFileSignature()`, `getConnectedUniverse()` and `getIPv6ConnectivityState()` to Utils class
- Changed: added default of true to  `setVRHeadsetStreamingEnabled()`
- Fixed: embedding callbacks not working correctly
- Fixed: `check_file_signature` variant now int instead of string
- Removed: `storeStats()` from deconstructor as it should be storing them automatically

## Version 4.14

- Added: new functions and enums to SteamRemotePlay
- Added: Steam icon to the class
- Added: Project Settings for Steam, optional app ID, can set auto-initialization, can set embedded callbacks, thanks to ***TriMay***
- Added: new functions to UGC
- Changed: replaced previous `MouseCursor` enum with new `HTMLMouseCursor` enum
- Changed: updated docs
- Changed: updated to Steamworks SDK 1.62
- Changed: `getNumSubscribedItems` and `getSubscribedItems` now take include_locally_disabled argument
- Changed: `steamInit` now return intended boolean and first argument removed since client syncs stats/achieves at boot
- Fixed: various bits in the in-editor docs
- Fixed: `network_messages_session_failed` missing returned properties in bind
- Removed: `UserRestrictions` enum
- Removed: `SetPersonaName` function and related callback `name_changed`
- Removed: `GetUserRestrictions` function

## Version 4.13

- Added: missing functions `openDeveloperTools` and `setDPIScalingFactor`
- Added: two missing Video class callbacks: `broadcast_upload_start` and `broadcast_upload_stop`
- Changed: minor housekeeping
- Changed: updated docs
- Changed: `getItemDefinitionProperty` now returns dictionary, thanks to ***gkwaerp***
- Changed: returned dictionary of `html_needs_paint` signal key 'bgra' renamed to 'rbga', thanks to ***obscurelyme***
- Fixed: history variables not initialized in `getGlobalStatIntHistory` and `getGlobalStatFloatHistory`
- Fixed: missing argument in `activateGameOverlayToWebPage`
- Fixed: missing argument in `keyDown`
- Fixed: incorrect callback signal for `get_opf_settings_result`

## Version 4.12

- Added: new Timeline functions, call results, and enums
- Added: new Inputs enums for Horipad; `INPUT_ACTION_ORIGIN`
- Added: new Networking config enum `NETWORKING_CONFIG_SEND_TIME_SINCE_PREVIOUS_PACKET`
- Added: new Networking config enums for fake packet jitter; `NETWORKING_CONFIG_FAKE_JITTER_`
- Changed: `equipped_profile_items` callback now sends `from_cache` bool
- Changed: first argument for `steamInit` and `steamInitEx` no longer calls for stats as they are synced by client; left to prevent compatibility breakage
- Fixed: `getAchievement` and related achievement functions breaking under rare conditions
- Fixed: incorrect type for `set_inventory_update_handle`
- Removed: `setTimelineGameMode` function which was removed in 1.61
- Removed: `current_stats_received` callback removed for redundancy
- Removed: Google Stadia, Nintendo, Epic Games, and WeGame Networking identity types fully removed, from 1.61
- Removed: unncessary commenting
- Removed: `sendMessages` until it can be fixed in Windows

## Version 4.11

- Added: `getDLCData` to get all DLC information and `getDLCDataByIndex` now acts as Steam intended with an index passed
- Added: properties for all variants GodotSteam stores
- Changed: using Godot's macros for error reporting back to the editor
- Changed: buffer_size argument to `decompressVoice` with default of original value
- Changed: `steamworksError` replaced with prints to editor
- Changed: all options array parameters for all Sockets class functions changed to dictionaries, [check class docs](https://godotsteam.com/classes/networking_sockets/)
- Changed: deprecated `getAvailableVoice`, merged functionality into `getVoice`
- Changed: `setLeaderboardDetailsMax` changed to the set/get for leaderboard_details_max, now `set_leaderboard_details_max`
- Changed: Steam singleton now removed during uninitialization
- Fixed: proper type for `network_connection_status_changed`, thanks to ***stickyShift***
- Fixed: `getResultItemProperty` now takes empty string to send all property list, thanks to ***Stralor***
- Fixed: missing return value hints from `lobby_data_update`
- Fixed: `get_app_dependencies_result` now passed back app_ids array
- Fixed: missing argument hints for `item_installed` callback
- Fixed: both global stat history functions - `getGlobalStatIntHistory` and `getGlobalStatFloatHistory`
- Fixed: broken returned variable in `network_connection_status_changed`
- Fixed: a variety of small tweaks
- Removed: duplicate call on setting embedded callbacks

## Version 4.10

- Changed: now using Steam Flat API, should allow compiling with MinGW
- Changed: updated in-editor docs

## Version 4.9.1

- Fixed: fixed typo in RESULT_ADMINISTRATOR_OK, ***thanks to sepTN***
- Fixed: fixed a variety of types and code corrections, ***thanks to bobsayhilol***

## Version 4.9

- Added: missing `user_achievement_icon_fetched` callback
- Added: new functions and enums to Apps class
- Added: new Steam Timeline class functions, enums, and constants
- Added: new functions to UGC class
- Added: new enum BetaBranchFlags
- Added: new value NETWORKING_CONFIG_IP_LOCAL_HOST_ALLOW_WITHOUT_AUTH for NetworkingConfigValue enum
- Changed: updated for Steamworks SDK 1.60
- Changed: `network_messages_session_failed` callback now returns the Steam ID associated with the user whose session failed
- Changed: `global_stats_received` had call result name change under-the-hood, does not affect anything
- Changed: `sendMessages()` now returns the message result
- Changed: `getQueryUGCResult()` now passes back additional value total_files_size
- Changed: enum RemoteStoragePlatform now cast as uint32_t, fixes Rust compatibility, ***thanks to GreenFox***
- Changed: `item_installed` signal now returns additional data - legacy_content and manifest_id
- Fixed: incorrect signal name for `inventory_definition_update`, ***thanks to Foxushka***

## Version 4.8

- Added: Steam Matchmaking response handlers, ***thanks to jeremybeier***
- Added: all missing Messages and Sockets constants
- Changed: Networking Messages, Sockets, and Utils now use Steam IDs instead of identity system
- Changed: UserUGCListSortOrder enums for readability
- Changed: UGCContentDescriptorID enums for readability
- Changed: `getResultStatus()` now returns the integer / enum
- Changed: cleaned up `addItemPreviewFile()`, `check_file_signature`, and `showGamepadTextInput()`
- Changed: various bits and pieces
- Changed: IP logic for all related functions
- Changed: `addFavoriteGame()`, `initiateGameConnection()`, `terminateGameConnection()`, and `removeFavoriteGame()` now take strings for IP
- Changed: `getAuthSessionTicket()` now defaults to 0 for Steam ID
- Changed: IP address now accepted instead of IP references
- Fixed: `getFriendCount()` has correct bit-wise value
- Fixed: server browser functionality, ***thanks to jeremybeier***
- Fixed: wrong string IP conversions, ***thanks to jeremybeier***
- Fixed: server list request filters, ***thanks to jeremybeier***
- Fixed: typo with UGC_MATCHING_UGC_TYPE_ITEMS enum
- Fixed: minor case issue with Workshop enums
- Fixed: `playerDetails()`, `requestFavoritesServerList()`, `requestInternetServerList()`, `requestSpectatorServerList()`, `requestFriendsServerList()`, `requestHistoryServerList()`, and `pingServer()`, ***thanks to jeremybeier***
- Fixed: regressions caused by minor update
- Fixed: typo with NETWORKING_CONFIG_TYPE_STRING enum
- Fixed: typo with LOBBY_COMPARISON_EQUAL_TO_GREATER_THAN
- Fixed: in-editor docs
- Removed: Networking Types identity system and related bits
- Removed: P2P Networking constants as they are duplicates of the P2PSend enum
- Removed: previous, non-functioning Matchmaking Server call results
- Removed: `getIdentity()` as it is redundant now

## Version 4.7

- Changed: minor organization and readability changes
- Fixed: `htmlInit()` not returning bool for success
- Fixed: incorrect argument bind for `createBeacon()`
- Fixed: incorrect argument bind for `setItemMetadata()`

## Version 4.6.3

- Changed: returned values for getFriendGamePlayed, ***thanks to SlejmUr***
- Changed: `getItemPrice()` now returns base price and price, ***thanks to SlejmUr***
- Changed: `getFriendMessage()` and callback `connected_friend_chat_message` now returns the type, ***thanks to SlejmUr***
- Changed: updated in-editor docs with changes
- Fixed: missing info_flags key in `getSessionConnectionInfo()`, ***thanks to SlejmUr***
- Fixed: `getServerDetails()` not sending back needed struct, ***thanks to SlejmUr***

## Version 4.6.2

- Fixed: incorrect constant for PUBLISHED_FILE_UPDATE_HANDLE_INVALID

## Version 4.6.1

- Added: internal notes about where enums are found
- Added: minor extra helper functions from Steam's client header
- Added: `getSteamID32()` function to convert SteamID64 to SteamID
- Changed: replaced deprecated Controller struct with Inputs struct in `getDigitalActionData()`
- Changed: in-editor docs
- Changed: leaderboard details max now set at highest instead of zero by default

## Version 4.6

- Added: new Remote Storage enum to WorkshopFileType
- Added: two new UGC enums to ItemState and ItemPreviewType
- Added: two new Friends class constants
- Added: new function `dismissGamepadTextInput()`
- Added: new Remote Play enum, form factor for VR headset
- Added: two new result enums; not supported and family size limit exceeded
- Added: three new enums to NetworkingConfigValue
- Added: new general constant ACCOUNT_ID_INVALID
- Changed: FEATURE_KIOSK_MODE enum now deprecated
- Changed: minor housekeeping by rearranging some functions
- Changed: k_ESteamNetworkingConfig_SDRClient_DebugTicketAddress was replaced by k_ESteamNetworkingConfig_SDRClient_DevTicket, value is the same but reference changed
- Changed: updated in-editor docs
- Fixed: spelling error in `getProfileItemPropertyInt()` bind
- Removed: App Lists class functions, callbacks, etc. due to SDK 1.59 changes
- Removed: Remote Play enums mistakenly added as constants

## Version 4.5.4

- Added: missing k_nSteamNetworkingSend_AutoRestartBrokenSession to constants
- Added: two missing Input constants: INPUT_HANDLE_ALL_CONTROLLERS and INPUT_MAX_ACTIVE_LAYERS
- Changed: `getInputTypeForHandle()` now returns int / enum instead of string for device models
- Changed: updated in-editor docs for missing content
- Changed: order of constants to be alphabetic
- Changed: changed returned variable name to `need_to_accept_tos` in `item_updated` callback
- Changed: Github Actions scripts, one for Vulkan and another for version numbers

## Version 4.5.3

- Fixed: `requestClanOfficerList()` using wrong internal function, ***thanks to sepTN***

## Version 4.5.2

- Fixed: `exchangeItems()`, `generateItems()`, and `startPurchase()` using wrong array size function, ***thanks to sepTN***
- Fixed: various spelling issues in in-editor docs, ***thanks to sepTN***

## Version 4.5.1

- Fixed: app ID automatically being set to 480, now default is 0 which makes GodotSteam ignore auto-setting app ID

## Version 4.5

- Added: two new arguments to `steamInit()` and `steamInitEx()` to set your app ID and run_callbacks interally, ***thanks to GreenFox***
- Added: two Music class callbacks
- Changed: `generateItems(0`, `exchangeItems()`, `getItemsByID()`, and `startPurchase()` all list-based arguments are now PoolIntArrays
- Changed: `getItemsByID()` now takes one argument, counts the elements in the passed array instead
- Changed: `getItemsWithPrices()` no longer requires any arguments passed to it
- Changed: in-editor docs have been updated
- Fixed: `getResultItems()` now returns all item data
- Fixed: missing DEFVAL for `steamInitEx()`
- Fixed: Joy Con name in `getInputTypeForHandle()`
- Removed: `getNumItemsWithPrices()` as it was unnecessary

## Version 4.4.2

- Fixed: `requestEquippedProfileItems()` was missing method bind, ***thanks to BOTLANNER***
- Fixed: `get_ticket_for_web_api` callback for getting actual ticket buffer, ***thanks to dicarne***
- Fixed: compiler complaining about comparison between Steam enum and GodotSteam enum for `steamInitEx()`
- Fixed: `getListenSocketAddress()` fixed to provide the actual address and optional port
- Changed: different defaults in Github Actions files
- Changed: `createBrowser()` now sends proper NULL when empty string passed
- Changed: `html_browser_ready` from callback to proper call result
- Changed: cast handle in `setSize()` as Steam HHTMLBrowser
- Removed: unnecessary steam_appid.txt from zips in Github Actions

## Version 4.4.1

- Fixed: missing descriptions for some in-editor functions/signals
- Fixed: `receiveMessagesOnChannel()`, `receiveMessagesOnPollGroup()`, and `receiveMessagesOnConnection()` had overwriting dictionary keys

## Version 4.4

- Added: new enums and constant related to new Steam initialization function
- Added: new enums for NetworkingConfigValue
- Added: new intialization function `steamInitEx()` that is more verbose
- Added: new UGC function `getUserContentDescriptorPreferences()`
- Added: new Remote Play function `startRemotePlayTogether()`
- Changed: UGC function `setItemTags()` to have new argument for admin tags
- Changed: compatible with Steamworks SDK 1.58
- Changed: in-editor docs now reflect all changes

## Version 4.3.1

- Fixed: wrong variant type for join_requested

## Version 4.3

- Added: full GodotSteam documentation into the editor
- Added: `steamShutdown()` to allow Steamworks to be manually shutdown
- Added: `requestEquippedProfileItems()` function and `equipped_profile_items` callback
- Added: Steam Deck as Steam Input type
- Changed: all enums are now directly linked to their SDK counterparts
- Changed: `getDigitalActionData()` returned keys are now state and active
- Changed: names of some Steam enums to be cleaner and leaner
- Changed: `getAppInstallDir()` now returns dictionary with absolute path and install size
- Fixed: some missing enum binds
- Fixed: missing function argument binds
- Fixed: `get_ticket_for_web_api` sending back strings
- Removed: enums that are not in the SDK but Valve's docs

## Version 4.2.2

- Added: new Input callback `input_gamepad_slot_change`
- Added: new User callback `get_ticket_for_web_api`
- Added: new User function `getAuthTicketForWebApi()`
- Changed: `getAuthSessionTicket()` argument is now optional, defaults to NULL

## Version 4.2.1

- Added: new return values for `overlay_toggled`; this will break compatibility with this
- Added: new Input and Parental Settings enums
- Added: new UGC Content Descriptor ID enums
- Added: new UGC functions `removeContentDescriptor()`, `addContentDescriptor()`, and `getQueryUGCContentDescriptors()`
- Added: new signal `filter_text_dictionary_changed`
- Changed: `getAuthSessionTicket()` now uses networking identities
- Changed: `gamepad_text_input_dismissed` now passes back the app ID
- Changed: Steam Input max analog and digital actions values
- Removed: ERegisterActivationCodeResult due to removal in SDK

## Version 4.2

* Added: various backports from Godot 3.x branch
* Fixed: options array size for new Networking classes and memory leaks, ***thanks to profour***
* Fixed: need for godotsteam.sh file on some Linux systems, ***thanks to mikix***

## Version 4.1.5

* Added: networking type message constants
* Added: more descriptions and tutorial links to in-editor docs
* Added: `avatar_image_loaded` callback to get raw response from Steamworks
* Changed: brought 4.x branch up-to-speed with master / 3.x branch
* Changed: enums now bound with BIND_ENUM_CONSTANT, ***thanks to raulsntos***
* Changed: bitwise enums now bound with BIND_ENUM_BITWISE_CONSTANT, ***thanks to raulsntos***
* Changed: platform of 'osx' to new 'macos'
* Fixed: platform of 'osx' not being recognized so the module doesn't get added
* Fixed: `microtransaction_auth_response` spelling for callback
* Fixed: `getLobbyData()` not returning UTF-8 encoded string
* Fixed: `sendLobbyChatMsg()` truncating non-English strings
* Fixed: `filterText()` truncating input; ***thanks to _tcoxon***
* Removed: MarketingMessageFlags as they don't exist in the header files

## Version 4.1.4

* Changed: layout to make Git cloning easier
* Changed: `submitItemUpdate()` to use null if no notes are passed, ***thanks to mashumafi***
* Fixed: incorrect use of io failure
* Fixed: DEFVAL type mismatch, ***thanks to raulsntos***
* Fixed: `getSessionConnectionInfo()` using old networking struct
* Removed: unused networking stricts

## Version 4.1.3

* Fixed: config.py and SCsub so that Linux compiling will link Steam correctly

## Version 4.1.2

* Changed: brought branch up to speed with GodotSteam 3.17.4

## Version 4.1.1

* Fixed: wrong arch in SCsub file
* Fixed: registering class in register_types.cpp

## Version 4.1

* Changed: brought branch up to speed with GodotSteam 3.17.1
* Fixed: various small issues to get it running with Godot alpha 1

## Version 4.0

* Added: Initial build, highly experimental!
