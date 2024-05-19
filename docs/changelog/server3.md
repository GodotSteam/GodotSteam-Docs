# Server 3.x Change-Log

A history of all changes to [the ***server3*** branch.](https://github.com/GodotSteam/GodotSteam-Server/tree/godot3){ target="\_blank" }

---

## Version 3.3

- Changed: constants list to add missing and remove unused
- Changed: Networking Messages, Sockets, and Utils now use Steam IDs instead of identity system
- Changed: various bits and pieces
- Changed: IP logic for all related functions
- Changed: UserUGCListSortOrder enums for readability
- Changed: UGCContentDescriptorID enums for readability
- Changed: `getResultStatus()` now returns the integer / enum
- Changed: `getAuthSessionTicket()` now defaults to 0 for Steam ID
- Fixed: wrong string IP conversions, ***thanks to jeremybeier***
- Fixed: typo with UGC_MATCHING_UGC_TYPE_ITEMS enum
- Fixed: minor case issue with Workshop enums
- Fixed: typo with NETWORKING_CONFIG_TYPE_STRING enum
- Removed: unnecessary enums
- Removed: Networking Types identity system and related bits
- Removed: P2P Networking constants as they are duplicates of the P2PSend enum
- Removed: `getIdentity()` as it is redundant now

## Version 3.2

- Added: new Remote Storage enum to WorkshopFileType
- Added: two new UGC enums to ItemState and ItemPreviewType
- Added: new Remote Play enum, form factor for VR headset
- Added: two new result enums; not supported and family size limit exceeded
- Added: three new enums to NetworkingConfigValue
- Added: new general constant ACCOUNT_ID_INVALID
- Changed: k_ESteamNetworkingConfig_SDRClient_DebugTicketAddress was replaced by k_ESteamNetworkingConfig_SDRClient_DevTicket, value is the same but reference changed

## Version 3.1

- Added: a missing constant
- Changed: backported various fixes from non-server GodotSteam
- Changed: updated various function arguments to match non-server GodotSteam
- Changed: constants now live in `godotsteam_server_constants.h` like non-server GodotSteam
- Changed: further fixes to initialization functions

## Version 3.0.1

- Changed: how initialization functions work, passing empty string now uses default IP (expected behavior)
- Fixed: incorrect verbal message from `serverInitEx()`

## Version 3.0

- Added: missing server functions from steam_gameserver.h
- Added: missing enums for server modes
- Added: in-editor documentation
- Changed: various improvements under-the-hood
- Changed: reorganized some constants
- Removed: unused enums, signals, functions
- Removed: unnecessary classes that are not part of the server build

## Version 2.0.1

* Changed: layout to make Git cloning easier
* Fixed: `getSessionConnectionInfo()` using old networking struct
* Removed: unused networking stricts

## Version 2.0

* Changed: separated server back into it's own module / branch
* Changed: brought server branch in line with related master branch functions

## Version 1.2.4

* Fixed: lots of compiler warnings on Linux, ***thanks to gregcsokas***

## Version 1.2.3

* Added: missing functions to Apps class
* Added: new functions and callbacks to UGC class
* Changed: EnableHeartbeats was renamed to SetAdvertiseServerActive in SDK
* Changed: various internal variable names
* Fixed: some memory allocation issues
* Removed: `setHeartbeatInterval()` and `forceHeartbeat()`; was removed from SDK

## Version 1.2.2

* Added: Added: ability to provide different locations for custom modules, ***thanks to dsnopek***

## Version 1.2.1

* Fixed: `getNumSubscribedItems()` was mislabeled as `getSubscribedItems()`

## Version 1.2

* Added: various notations and spacing
* Added: `addRequiredTagGroup()`, `getQueryUGCTag()`, `getQueryUGCTagDisplayName()`, and `getQueryUGCNumTags()` functions from UGC
* Changed: `setCookie()` to `setHTTPCookie()`
* Fixed: various delete statements
* Fixed: converting user ID in `createQueryUserUGCRequest()`
* Fixed: metadata length in `setItemMetadata()`
* Fixed: library paths in config.py
* Removed: compiling flag in config.py for OSX
* Removed: ticket struct as it causes craches

## Version 1.1.1

* Changed: includes Godot header file to allow more than 5 arguments in a function
* Fixed: `filterText()` for Steamworks SDK 1.50

## Version 1.1.0

* Added: Apps, HTTP, Inventory, Networking, UGC, and Utils classes
* Added: related callbacks, call results, constants, and enums
* Added: headless server platform for linux
* Fixed: OSX compiling config rules
