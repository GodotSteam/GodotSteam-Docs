---
title: Server GDExtension Changelog
description: A history of all changes made to the server gdextension branch.
icon: material/power-plug-outline
---


A history of all changes to [the ***server gdextension*** branch.](https://github.com/GodotSteam/GodotSteam-Server/tree/gdextension){ target="\_blank" }

---

## Version 4.5.2

- Fixed: crash when GodotSteam converts IP addresses internally

## Version 4.5.1

- Changed: updated docs
- Changed: `getItemDefinitionProperty` now returns dictionary, thanks to ***gkwaerp***

## Version 4.5

- Added: missing Utils class: functions, enums, constants
- Changed: minor clean-ups
- Fixed: wrong accessor for Networking Sockets, thanks to ***Michael Janesway***
- Fixed: missing callback fro `validate_auth_ticket_response`, thanks to ***Michael Janesway***
- Fixed: all wronge accessors for all other class functions

## Version 4.4

- Added: public properties with set/get functions
- Added: failures now print to editor
- Changed: updated to Steamworks SDK 1.61
- Changed: added new enums from newest SDK, removed the now missing ones
- Changed: deprecating `serverInit` in next patch, migrate to `serverInitEx`
- Changed: return typed for `getHTTPResponseHeaderValue` and `getHTTPStreamingResponseBodyData`
- Changed: `configureConnectionLanes` now has correct type for lanes argument
- Changed: NetworkingSockets now take dictionary for options, based on godot4 branch in main GodotSteam repo
- Changed: reworked `getUserAchievement`, `getUserStatFloat`, `getUserStatInt` to mirror godot4 branch in main GodotSteam repo
- Fixed: `setHTTPRequestRawPostBody`, backport from godot4 branch in main GodotSteam repo
- Fixed: `serializeResult` now returns PackedByteArray
- Fixed: misspelled enum

## Version 4.3

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

## Version 4.2

- Added: new Remote Storage enum to WorkshopFileType
- Added: two new UGC enums to ItemState and ItemPreviewType
- Added: new Remote Play enum, form factor for VR headset
- Added: two new result enums; not supported and family size limit exceeded
- Added: three new enums to NetworkingConfigValue
- Added: new general constant ACCOUNT_ID_INVALID
- Changed: k_ESteamNetworkingConfig_SDRClient_DebugTicketAddress was replaced by k_ESteamNetworkingConfig_SDRClient_DevTicket, value is the same but reference changed

## Version 4.1

- Added: a missing constant
- Changed: backported various fixes from non-server GodotSteam
- Changed: updated various function arguments to match non-server GodotSteam
- Changed: constants now live in `godotsteam_server_constants.h` like non-server GodotSteam
- Changed: further fixes to initialization functions

## Version 4.0.1

- Changed: how initialization functions work, passing empty string now uses default IP (expected behavior)
- Fixed: incorrect verbal message from `serverInitEx()`

## Version 4.0

- Added: missing server functions from steam_gameserver.h
- Added: missing enums for server modes
- Added: in-editor documentation
- Changed: various improvements under-the-hood
- Changed: reorganized some constants
- Removed: unused enums, signals, functions
- Removed: unnecessary classes that are not part of the server build