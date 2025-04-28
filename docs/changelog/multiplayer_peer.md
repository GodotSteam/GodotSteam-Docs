---
title: MultiplayerPeer Changelog
description: A history of all changes made to the multiplayer-peer branch.
icon: material/account-supervisor
---

A history of all changes to [the ***multiplayer-peer*** repo.](https://github.com/GodotSteam/MultiplayerPeer){ target="\_blank" }

---

## Version 4.15

- Changed: version bump to match Godot 4.x branch update

## Version 4.14

- Changed: version bump to match Godot 4.x branch update

## Version 4.13

- Changed: version bump to match Godot 4.x branch update

## Version 4.12

- Changed: version bump to match Godot 4.x branch update

## Version 4.11

- Changed: in-editor docs
- Changed: reorganized code-base
- Changed: more readable error for `sendPending`
- Changed: removed \_scb from callback names are they are unnecessary due to class call
- Changed: `lobby_chat_update` to just add or remove players
- Changed: chat contants names changed
- Fixed: missing argument hints for `lobby_data_update`
- Fixed: `get_peer_info` overwriting various dictionary keys
- Removed: string response to packet send failure, now returns actual error code
- Removed: matchmaking enums, were not used; can be taken from GodotSteam directly if needed
- Removed: `lobby_invite`, `lobby_match_list`, `lobby_kicked` as they are bound but not used in MP

## Version 4.10

- Changed: now using Steam Flat API, should allow compiling with MinGW
- Changed: updated in-editor docs

## Version 4.9

- Added: get_lobby_id() function to expose the lobby ID
- Removed: Mono flags from GA files

## Version 4.8

Version bump to sync with 4.x branch

## Version 4.7

- Added: in-editor docs, ***thanks to grossqx***

## Version 4.6.3

- Fixed: lobby_state breaking after calling close on peer, ***thanks to Hangman***

## Version 4.6.2

Version bump to sync with 4.x branch

## Version 4.6.1

- Added: various fixes including Linux compiling, ***thanks to friartruck***

## Version 4.6

- Changed: all-new Github Actions files with Mono attempt
- Removed: demo has been removed and added to the GodotSteam Example repo instead

## Version 4.5.1

- Added: doc_classes folder for writing
- Added: first tests for Github Actions
- Note: Version number pulled in line with godot4 branch which it depends on