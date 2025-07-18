---
title: App Lists
description: A class reference for App Lists.
icon: material/list-box
---

# App Lists (Deprecated Since SDK 1.59)

This is a restricted interface that can only be used by previously approved apps. It is not listed in the official Steamworks SDK documentation, either. Contact your Steam Account Manager if you believe you need access to this API.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

!!! warning "This class has been removed as of Steamworks SDK 1.59 and no longer exists in GodotSteam as of versions 3.23 / 4.6"

{==
## :material-function-variant: Functions
==}

### getAppListBuildId

!!! function "getAppListBuildId( ```uint32_t``` app_id )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID to get data for. |

    Get a given app ID's build. 

    !!! returns "Returns: int"

    ---
    [ :material-tag-remove: Removed GodotSteam 4.6](../changelog/godot4.md/#version-46){ .md-button .md-button--changes target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 3.23](../changelog/godot3.md/#version-323){ .md-button .md-button--changes target="_blank" }

### getAppListInstallDir

!!! function "getAppListInstallDir( ```uint32_t``` app_id, ```int``` name_max )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID to get data for. |
    | name_max | int | The maximum number of characters for the name. |

    Get a given app ID's install directory.

    !!! returns "Returns: string"

    ---
    [ :material-tag-remove: Removed GodotSteam 4.6](../changelog/godot4.md/#version-46){ .md-button .md-button--changes target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 3.23](../changelog/godot3.md/#version-323){ .md-button .md-button--changes target="_blank" }

### getAppName

!!! function "getAppName( ```uint32_t``` app_id, ```int``` name_max )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID to get data for. |
    | name_max | int | The maximum number of characters for the name. |

    Get a given app ID's name. 

    !!! returns "Returns: string"

    ---
    [ :material-tag-remove: Removed GodotSteam 4.6](../changelog/godot4.md/#version-46){ .md-button .md-button--changes target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 3.23](../changelog/godot3.md/#version-323){ .md-button .md-button--changes target="_blank" }

### getInstalledApps

!!! function "getInstalledApps( ```uint32_t``` max_app_ids )"
    | Argument | Type | Notes |
    | -------- | ---- | ----- |
    | max_app_ids | uint32_t | List of app IDs. |

    Get a list of app IDs for installed apps for this player. 

    !!! returns "Returns: array"
        Contains a list of (int) app IDs.

    ---
    [ :material-tag-remove: Removed GodotSteam 4.6](../changelog/godot4.md/#version-46){ .md-button .md-button--changes target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 3.23](../changelog/godot3.md/#version-323){ .md-button .md-button--changes target="_blank" }

### getNumInstalledApps

!!! function "getNumInstalledApps( )"
    Get the number of installed apps for this player. 

    !!! returns "Returns: int"

    ---
    [ :material-tag-remove: Removed GodotSteam 4.6](../changelog/godot4.md/#version-46){ .md-button .md-button--changes target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 3.23](../changelog/godot3.md/#version-323){ .md-button .md-button--changes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to run ```Steam.run_callbacks()``` in your ```_process()``` function or enabled them in ``Project Settings`` to receive them.

### app_installed

!!! function "app_installed"
	Sent when a new app is installed.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
    	| app_id | uint32_t | -
        | install_folder_index | uint32_t | -

    ---
    [ :material-tag-remove: Removed GodotSteam 4.6](../changelog/godot4.md/#version-46){ .md-button .md-button--changes target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 3.23](../changelog/godot3.md/#version-323){ .md-button .md-button--changes target="_blank" }

### app_uninstalled

!!! function "app_uninstalled"
	Sent when an app is uninstalled.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | app_id | uint32_t | -
        | install_folder_index | uint32_t | -

    ---
    [ :material-tag-remove: Removed GodotSteam 4.6](../changelog/godot4.md/#version-46){ .md-button .md-button--changes target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 3.23](../changelog/godot3.md/#version-323){ .md-button .md-button--changes target="_blank" }