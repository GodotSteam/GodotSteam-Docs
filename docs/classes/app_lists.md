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
    Get a given app ID's build. 

    **Returns:** int

### getAppListInstallDir

!!! function "getAppListInstallDir( ```uint32_t``` app_id, ```int``` name_max )"
    Get a given app ID's install directory.

    **Returns:** string

### getAppName

!!! function "getAppName( ```uint32_t``` app_id, ```int``` name_max )"
    Get a given app ID's name. 

    **Returns:** string

### getInstalledApps

!!! function "getInstalledApps( ```uint32``` max_app_ids )"
    Get a list of app IDs for installed apps for this player. 

    **Return:** array

    Contains a list of (int) app IDs.

### getNumInstalledApps

!!! function "getNumInstalledApps()"
    Get the number of installed apps for this player. 

    **Return:** int

{==
## :material-signal: Signals
==}

These callbacks require you to run ```Steam.run_callbacks()``` in your ```_process()``` function to receive them.

### app_installed

!!! function "app_installed"
	Sent when a new app is installed.
	
	**Returns:**

	* app_id (uint32_t)
	* install_folder_index (uint32_t)

### app_uninstalled

!!! function "app_uninstalled"
	Sent when an app is uninstalled.

	**Returns:**
	
	* app_id (uint32_t)
	* install_folder_index (uint32_t)
