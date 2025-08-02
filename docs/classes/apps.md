---
title: Apps
description: A class reference for Apps.
icon: material/application
---

# Apps

Exposes a wide range of information and actions for applications and [Downloadable Content (DLC)](https://partner.steamgames.com/doc/store/application/dlc){ target="\_blank" }.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### getAppBuildId

!!! function "getAppBuildId( )"
    Gets the build ID of this app, may change at any time based on backend updates to the game. 

    !!! returns "Returns: int"
        The current build ID of this app. Defaults to 0 if you're not running a build downloaded from Steam.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetAppBuildId){ .md-button .md-button--doc_classes target="_blank" }

### getAppInstallDir

!!! function "getAppInstallDir( `uint32_t` app_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID to get the install directory for. |

    Gets the install folder for a specific app ID. This works even if the application is not installed, based on where the game would be installed with the default Steam library location. 

    !!! returns "Returns: string"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetAppInstallDir){ .md-button .md-button--doc_classes target="_blank" }

### getAppOwner

!!! function "getAppOwner( )"
    Gets the Steam ID of the original owner of the current app. If it's different from the current user then it is borrowed.

    !!! returns "Returns: uint64_t"
        The original owner of the current app.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetAppOwner){ .md-button .md-button--doc_classes target="_blank" }

### getAvailableGameLanguages

!!! function "getAvailableGameLanguages( )"
    Gets a comma separated list of the languages the current app supports.  For the full list of languages that may be returned [see Localization and Languages](https://partner.steamgames.com/doc/store/localization){ target="\_blank" }.

    !!! returns "Returns: string"
        Returns a comma separated list of languages.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetAvailableGameLanguages){ .md-button .md-button--doc_classes target="_blank" }

### getBetaInfo

!!! function "getBetaInfo( )"
    Get details about an app beta branch like name, description and state.

    !!! returns "Returns: dictionary"
        Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | index | int | Branch index starting at 0 which is always the default branch.
        | flags | uint32_t | Set of [BetaBranchFlags flags](main.md#betabranchflags) describing current branch state.
        | build_id | uint32_t | Content BuildID set live on this branch.
        | name | string | Beta branch name.
        | description | string | Beta branch description.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetBetaInfo){ .md-button .md-button--doc_classes target="_blank" }

### getCurrentBetaName

!!! function "getCurrentBetaName( )"
    Checks if the user is running from a beta branch, and gets the name of the branch if they are.

    !!! returns "Returns: string"
        Empty if the user is not on a beta branch.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetCurrentBetaName){ .md-button .md-button--doc_classes target="_blank" }

### getCurrentGameLanguage

!!! function "getCurrentGameLanguage( )"
    Gets the current language that the user has set.  This falls back to the Steam UI language if the user hasn't explicitly picked a language for the title.

    For the full list of languages [see Supported Languages](https://partner.steamgames.com/doc/store/localization/languages){ target="\_blank" }.

    !!! returns "Returns: string"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetCurrentGameLanguage){ .md-button .md-button--doc_classes target="_blank" }

### getDLCCount

!!! function "getDLCCount( )"
    Gets the number of DLC pieces for the current app. This is typically used to loop through each piece of DLC and get the info about each one with [getDLCDataByIndex](#getdlcdatabyindex).

    !!! returns "Returns: int"
        The number of DLC pieces for the current app. Note that this value may max out at 64, depending on how much unowned DLC the user has. If your app has a large number of DLC, you should set your own internal list of known DLC to check against.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetDLCCount){ .md-button .md-button--doc_classes target="_blank" }


### getDLCData

!!! function "getDLCData( )"
    Returns an array of dictionaries containing information about all available DLC for the current game.

    !!! returns "Returns: array"
        Contains dictionaries (dlc) which contain the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | id | int | The app ID for the DLC.
        | available | bool | Whether the DLC is available on the Steam store currently.
        | name | string | The name of the DLC.

    !!! info "Notes"
        This function is unique to GodotSteam.

### getDLCDataByIndex

!!! function "getDLCDataByIndex( `uint32_t` this_dlc_index )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | this_dlc_index | uint32_t | Index of the DLC to get between 0 and [getDLCCount](#getdlccount). |

    Returns metadata for a DLC by index. 

    !!! returns "Returns: dictionary"
        Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | id | int | The app ID for the DLC.
        | available | bool | Whether the DLC is available on the Steam store currently.
        | name | string | The name of the DLC.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BGetDLCDataByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getDLCDownloadProgress

!!! function "getDLCDownloadProgress( `uint32_t` dlc_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | dlc_id | uint32_t | The App ID of the DLC to monitor. |

    Gets the download progress for optional DLC.

    !!! returns "Returns: dictionary"
        Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | ret | bool | Returns true if the specified DLC exists and is currently downloading; otherwise, false.
        | downloaded | uint64_t | Returns the number of bytes downloaded.
        | total | uint64_t | Returns the total size of the download in bytes

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetDlcDownloadProgress){ .md-button .md-button--doc_classes target="_blank" }

### getEarliestPurchaseUnixTime

!!! function "getEarliestPurchaseUnixTime( `uint32_t` app_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The App ID to get the purchase time for. |

    Gets the time of purchase of the specified app in Unix epoch format (time since Jan 1st, 1970).

    !!! returns "Returns: uint32_t"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetEarliestPurchaseUnixTime){ .md-button .md-button--doc_classes target="_blank" }

### getFileDetails

!!! function "getFileDetails( `string` filename )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | filename | string | The absolute path and name to the file. |

    Asynchronously retrieves metadata details about a specific file in the depot manifest. 

    !!! returns "Returns: void"

    !!! trigger "Triggers"
        [file_details_result](#file_details_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetFileDetails){ .md-button .md-button--doc_classes target="_blank" }

### getInstalledDepots

!!! function "getInstalledDepots( `uint32_t` app_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app to list the depots for. |

    Gets a list of all installed depots for a given App ID. 

    !!! returns "Returns: array"
        Contains the installed depots, returned in mount order. 

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetInstalledDepots){ .md-button .md-button--doc_classes target="_blank" }

### getLaunchCommandLine

!!! function "getLaunchCommandLine( )"
    Gets the command line if the game was launched via Steam URL, e.g. `steam://run/<appid>//<command line>/`. This method is preferable to launching with a command line via the operating system, which can be a security risk. In order for rich presence joins to go through this and not be placed on the OS command line, you must enable "Use launch command line" from the Installation > General page on your app.

    If game was already running and launched again, the [new_launch_url_parameters](#new_launch_url_parameters) callback will be fired.

    !!! returns "Returns: string"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetLaunchCommandLine){ .md-button .md-button--doc_classes target="_blank" }

### getLaunchQueryParam

!!! function "getLaunchQueryParam( `string` key )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | key | string | The launch key to test for. Ex: param1 |

    Returns the associated launch param if the game is run via `steam://run/<appid>//?param1=value1&param2=value2&param3=value3` etc.

    Parameter names starting with the character '@' are reserved for internal use and will always return an empty string.  Parameter names starting with an underscore '_' are reserved for Steam features; they can be queried by the game but it is advised that you not use param names beginning with an underscore for your own features.

    Check for new launch parameters on [new_launch_url_parameters](#new_launch_url_parameters) callbacks.

    !!! returns "Returns: string"
        The value associated with the key provided. Returns an empty string if the specified key does not exist.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetLaunchQueryParam){ .md-button .md-button--doc_classes target="_blank" }

### getNumBetas

!!! function "getNumBetas( )"
    Returns total number of known app beta branches; including the default public branch.

    !!! returns "Returns: dictionary"
        Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | available | int | Number of beta branches available to current user.
        | private | int | How many of these are private betas.
        | total | int | Number of known app branches.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#GetNumBetas){ .md-button .md-button--doc_classes target="_blank" }
     <span class="sdk">Added SDK 1.60</span>


### installDLC

!!! function "installDLC( `uint32_t` dlc_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | dlc_id | uint32_t | The DLC you want to install. |

    Allows you to install an optional DLC.

    !!! returns "Returns: void"

    !!! trigger "Triggers"
        [dlc_installed](#dlc_installed) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#InstallDLC){ .md-button .md-button--doc_classes target="_blank" }


### isAppInstalled

!!! function "isAppInstalled( `uint32_t` app_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The App ID of the application to check. |

    Checks if a specific app is installed. The app may not actually be owned by the current user, they may have it left over from a free weekend, etc. This only works for base applications, not [downloadable content (DLC).](https://partner.steamgames.com/doc/store/application/dlc){ target="\_blank" }

    Use [isDLCInstalled](https://partner.steamgames.com/doc/api/ISteamApps#BIsAppInstalled){ target="\_blank" } for DLC instead. 

    !!! returns "Returns: bool"
        True if the specified app ID is installed; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsAppInstalled){ .md-button .md-button--doc_classes target="_blank" }

### isCyberCafe

!!! function "isCyberCafe( )"
    Checks whether the current App ID is for Cyber Cafes.

    !!! returns "Returns: bool"
        Returns true if the license if for cyber cafe's; otherwise, false.

    !!! info "Notes"
        Listed as deprecated in the Steam documentation but the SDK makes no mention of this.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsCybercafe){ .md-button .md-button--doc_classes target="_blank" }

### isDLCInstalled

!!! function "isDLCInstalled( `uint32_t` dlc_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | dlc_id | uint32_t | The DLC ID to check. |

    Checks if the user owns a specific DLC and if the DLC is installed. 

    !!! returns "Returns: bool"
        True if the user owns the DLC and it's currently installed, otherwise false.

    !!! info "Notes"
        Should only be used for simple client side checks; not intended for granting in-game items.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsDlcInstalled){ .md-button .md-button--doc_classes target="_blank" }

### isLowViolence

!!! function "isLowViolence( )"
    Checks if the license owned by the user provides low violence depots. Low violence depots are useful for copies sold in countries that have content restrictions.

    !!! returns "Returns: bool"
        Returns true if the license owned by the user provides low violence depots; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsLowViolence){ .md-button .md-button--doc_classes target="_blank" }

### isSubscribed

!!! function "isSubscribed( )"
    Checks if the active user is subscribed to the current app ID. 

    !!! returns "Returns: bool"
        Returns true if the active user owns the current app ID; otherwise, false.

    !!! info "Notes"
        This will always return true if you're using Steam DRM or calling [restartAppIfNecessary](main.md#restartappifnecessary).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsSubscribed){ .md-button .md-button--doc_classes target="_blank" }

### isSubscribedApp

!!! function "isSubscribedApp( `uint32_t` app_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID to check. |

    Checks if the active user is subscribed to a specified app ID. Only use this if you need to check ownership of another game related to yours, a demo for example. 

    !!! returns "Returns: bool"
        Returns true if the active user is subscribed to the specified app ID; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsSubscribedApp){ .md-button .md-button--doc_classes target="_blank" }

### isSubscribedFromFamilySharing

!!! function "isSubscribedFromFamilySharing( )"
    Checks if the active user is accessing the current app ID via a temporary Family Shared license owned by another user.

    If you need to determine the Steam ID of the permanent owner of the license, use [getAppOwner](#getappowner).

    !!! returns "Returns: bool"
        Returns true if the active user is accessing the current app ID via family sharing; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsSubscribedFromFamilySharing){ .md-button .md-button--doc_classes target="_blank" }


### isSubscribedFromFreeWeekend

!!! function "isSubscribedFromFreeWeekend( )"
    Checks if the user is subscribed to the current app through a free weekend. This function will return false for users who have a retail or other type of license. Suggested you contact Valve on how to package and secure your free weekend properly. 

    !!! returns "Returns: bool"
        Returns true if the active user is subscribed to the current app ID via a free weekend; otherwise, false for any other type of license.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsSubscribedFromFreeWeekend){ .md-button .md-button--doc_classes target="_blank" }

### isTimedTrial

!!! function "isTimedTrial( )"
    Checks if the user is subscribed to the current app ID through a timed trial. If so, it gives back the total time the timed trial is allowed to play, along with the current amount of time the user has played. 

    If the active user is not subscribed to the current app ID via a timed trial, the returned values will both be 0.

    !!! returns "Returns: dictionary"
        Containing these keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | seconds_allowed | uint32_t | Returns the number of seconds the timed trial will list.
        | seconds_played | uint32_t | Returns the number of seconds that the user has played so far.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsTimedTrial){ .md-button .md-button--doc_classes target="_blank" }

### isVACBanned

!!! function "isVACBanned( )"
    Checks if the user has a VAC ban on their account. 

    !!! returns "Returns: bool"
        Returns true if the user has a VAC ban on their account; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#BIsVACBanned){ .md-button .md-button--doc_classes target="_blank" }

### markContentCorrupt

!!! function "markContentCorrupt( `bool` missing_files_only )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | missing_files_only | bool | Only scan for missing files, don't verify the checksum of each file. |

    Allows you to force verify game content on next launch.

    If you detect the game is out-of-date (for example, by having the client detect a version mismatch with a server), you can call use [markContentCorrupt](#markcontentcorrupt) to force a verify, show a message to the user, and then quit.

    !!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#MarkContentCorrupt){ .md-button .md-button--doc_classes target="_blank" }

### setActiveBeta

!!! function "setActiveBeta( `string` beta_name )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | beta_name | string | Beta name the game wants to switch to. |

    Select this beta branch for this app as active, might need the game to restart so Steam can update to that branch.

    !!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#SetActiveBeta){ .md-button .md-button--doc_classes target="_blank" }

### setDLCContext

!!! function "setDLCContext( `uint32_t` app_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The DLC app ID to track. |

    Set current DLC app ID being played (or 0 if none). Allows Steam to track usage of major DLC extensions.

    !!! returns "Returns: bool"

### uninstallDLC

!!! function "uninstallDLC( `uint32_t` dlc_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | dlc_id | uint32_t | The DLC you want to uninstall. |

    Allows you to uninstall an optional DLC.

    !!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#UninstallDLC){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### dlc_installed

!!! function "dlc_installed"
    Triggered after the current user gains ownership of DLC and that DLC is installed.

    !!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | app_id | uint32_t | App ID of the DLC that was installed.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#DlcInstalled_t){ .md-button .md-button--doc_classes target="_blank" }

### file_details_result

!!! function "file_details_result"
    Called after requesting the details of a specific file.

    !!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | uint32_t | Was the call successful? [RESULT_OK](main.md#result) if it was; otherwise, [RESULT_FILE_NOT_FOUND](main.md#result) if the file was not found. None of the other fields are filled out if the call was not successful.
        | file_size | uint64_t | The original file size in bytes.
        | file_hash | uint8 | The original file SHA1 hash.
        | flags | uint32_t | No notes about what these are.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#FileDetailsResult_t){ .md-button .md-button--doc_classes target="_blank" }

### new_launch_url_parameters

!!! function "new_launch_url_parameters"
    Triggered after the user executes a steam url with command line or query parameters such as `steam://run/(app_id)//?param1=value1;param2=value2;param3=value3;` while the game is already running. The new params can be queried with [getLaunchCommandLine](#getlaunchcommandline) and [getLaunchQueryParam](#getlaunchqueryparam).

    !!! returns "Returns"
		 Nothing.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#NewUrlLaunchParameters_t){ .md-button .md-button--doc_classes target="_blank" }

### timed_trial_status

!!! function "timed_trials_status"
    Sent every minute when a appID is owned via a timed trial.

    !!! returns "Returns""
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | app_id | uint32_t | App ID that is in a timed trial.
        | is_offline | bool | If true, the user is currently offline. Time allowed / played refers to offline time, not total time.
        | seconds_allowed | uint32_t | How many seconds the app can be played in total.
        | seconds_played | uint32_t | How many seconds the app was already played.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamApps#TimedTrialStatus_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
APP_PROOF_OF_PURCHASE_KEY_MAX | k_cubAppProofOfPurchaseKeyMax | 240 | Max supported length of a legacy cd key.