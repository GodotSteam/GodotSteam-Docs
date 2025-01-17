---
title: Server GDNative Changelog
description: A history of all changes made to the server gdnative branch.
icon: material/power-plug-outline
---

A history of all changes to [the ***server gdnative*** branch;](https://github.com/GodotSteam/GodotSteam-Server/tree/gdnative){ target="\_blank" } which is now retired. This branch still works fine but will not receive any further updates.

---

## Version 3.3
- Changed: constants list to add missing and remove unused
- Changed: Networking Messages, Sockets, and Utils now use Steam IDs instead of identity system
- Changed: various bits and pieces
- Changed: IP logic for all related functions
- Changed: `getResultStatus()` now returns the integer / enum
- Changed: `getAuthSessionTicket()` now defaults to 0 for Steam ID
- Fixed: wrong string IP conversions, ***thanks to jeremybeier***
- Removed: Networking Types identity system and related bits
- Removed: P2P Networking constants as they are duplicates of the P2PSend enum
- Removed: `getIdentity()` as it is redundant now

## Version 3.2

- Added: new general constant ACCOUNT_ID_INVALID
- Changed: k_ESteamNetworkingConfig_SDRClient_DebugTicketAddress was replaced by k_ESteamNetworkingConfig_SDRClient_DevTicket, value is the same but reference changed

## Version 3.1

- Added: a missing constant
- Changed: backported various fixes from non-server GodotSteam
- Changed: updated various function arguments to match non-server GodotSteam
- Changed: constants now live in `godotsteam_server_constants.h` like non-server GodotSteam
- Changed: further fixes to initialization functions

## Version 3.0.1

- Changed: updated included Steam server script
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