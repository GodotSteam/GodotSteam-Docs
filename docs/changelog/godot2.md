# Godot 2.x Change-Log

A history of all to [the ***godot2*** branch.](https://github.com/GodotSteam/GodotSteam/tree/godot2){ target="\_blank" }

---

## Version 1.10

- Added: more verbose response to `steamInit()`, now returns a dictionary
- Added: missing initialization constants
- Changed: `steamInit()` to give actual response on Steamworks status (from bool to int)
- Changed: minor formatting to match Godot 3 version
- Fixed: `currentAppID` not utilized correctly
- Removed: unneeded `gameInfo` struct

## Version 1.9.3

- Added: networking functions, ***thanks to Antokolos***
- Added: compiling flag for Linux and Mac, need for new Steamworks 1.44
- Changed: linked against Steamworks 1.44
- Fixed: issue with leaderboard upload returning false no matter what

## Version 1.9.2

- Added: `persona_state_change` callback
- Changed: `getFriendAvatar()` to `getPlayerAvatar()`
- Changed: `avatar_loaded` now sends back Steam ID of avatar, ***thanks to avencherus***
- Fixed: issue with avatar and Steam ID on Linux compile
- Fixed: `join_requested` signal, ***thanks to Fischer96***

## Version 1.9.1

- Added: additional user statistics and achievement signals
- Changed: minor notations
- Fixed: Linux not compiling correctly with new Friends and Matchmaking updates
- Fixed: various Friends functions not providing correct data
- Fixed: missing bind methods for integers

## Version 1.9.0

- Added: all remaining matchmaking functions
- Added: all remaining friend functions
- Changed: `getRecentPlayers()` to include timestamp
- Changed: naming of `leaderboard_handle` and `leaderboard_entries` for consistency
- Changed: `getAchievement()` to dictionary, ***thanks to jandrewlong***
- Fixed: invite functions giving incorrect steam ids
- Fixed: `getInstalledDepots()`, `getDLCDownloadProgress()`, `getItemUpdateProgress()`, `getSubscribedItems()`
- Removed: `setGameInfo()`, `clearGameInfo()`, `inviteFriend()`

## Version 1.8.0

- Added: `getAchievementDisplayAttribute()`, `getAchievementName()`, `getAchievementIcon()`, `getImageRGBA()`, and `getImageSize()`, ***thanks to marcelofg55***
- Added: all missing SteamApps functions
- Changed: NULL statements for achievement functions
- Changed: cleaned up and organized signal functions in godotteam.h
- Fixed: issue with `getAchievement()` failing to compile
- Removed: `hasOtherApp()` function

## Version 1.7.0

- Added: `getCurrentBetaName()`, `addScreenshotToLibrary()`, and `setLocation()`, ***thanks to marcelofg55***
- Added: Steam controller functionality, ***thanks to marcelofg55***
- Added: more workshop functionality
- Changed: various small maintenance
- Fixed: compiling error on Linux

## Version 1.6.0

- Added: `getFileNameAndSize()`, `getQuota()`, `getSyncPlatforms()` functions
- Changed: small corrections with Steam ID variable
- Fixed: small things with `getQuota()`

## Version 1.5.1

- Added: `getNumAchievements()`, `getAchievementAchievedPercent()`, `requestGlobalAchievementPercentages()` functions
- Added: related signals to new functions
- Added: minor notes
- Removed: is_valid for `updateLeaderboardHandle()`
- Removed: deprecated function `requestAppProofOfPurchaseKey()`
- Removed: callback for `requestAppProofOfPurchaseKey()`
- Removed: commented out deprecated functions

## Version 1.5.0

- Added: more Screenshot features
- Added: notes for callback
- Changed: documentation to reflect new features and signals
- Fixed: types in `validate_auth_ticket_response`

## Version 1.4.0

- Added: Auth Session functions added to Godot 3 branch, ***thanks to marcelofg55***
- Added: Auth Session constants

## Version 1.3.0

- Added: `getNumberOfCurrentPlayers()`, ***thanks to marcelofg55***
- Added: `leaderboard_uploaded` and `number_of_current_players` callbacks, ***thanks to marcelofg55***
- Changed: signals in godotsteam.h with CCallback instead of SteamCallback

## Version 1.2.1

- Changed: instances of int32 and int64 to int32_t and int64_t respectively; mostly for Linux compilation
- Changed: readme to reflect Godot 3 release

## Version 1.2.0

- Added: Remote Storage functionality for Steam Cloud, ***thanks to marcelofg55***
- Added: new functions to documentation
- Changed: SCsub file to include "no-pie" fix for Ubuntu 16.10 and higher

## Version 1.1.0

- Added: `getCurrentGameLanguage()`
- Added: Pre-compiled engines and templates for Windows
- Added: change log to documentation
- Changed: minor things in godotsteam.cpp
