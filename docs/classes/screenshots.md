---
title: Screenshots
description: A class reference for Screenshots.
icon: material/monitor-screenshot
---

# Screenshots

Functions for adding screenshots to the user's screenshot library. See [Steam Screenshots](https://partner.steamgames.com/doc/features/screenshots){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### addScreenshotToLibrary

!!! function "addScreenshotToLibrary( `string` filename, `string` thumbnail_filename, `int` width, `int` height )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | filename | string | The absolute file path to the JPG, PNG, or TGA screenshot. |
    | thumbnail_filename | string | The absolute file path to an optional thumbnail image. This must be 200px wide, as described by [SCREENSHOT_THUMB_WIDTH](#constants) and the same aspect ratio. Pass NULL if there is no thumbnail, one will be created automatically. |
    | width | int | The width of the screenshot. |
    | height | int | The height of the screenshot. |

    Adds a screenshot to the user's screenshot library from disk.  If a thumbnail is provided, it must be 200 pixels wide and the same aspect ratio as the screenshot, otherwise a thumbnail will be generated if the user uploads the screenshot.

    The screenshots must be in either JPEG or TGA format. The return value is a handle that is valid for the duration of the game process and can be used to apply tags; if that handle is 0 then the screenshot could not be saved. JPEG, TGA, and PNG formats are supported.

    The given path must be an absolute path to the file.

    !!! returns "Returns: uint32_t"
        Returns a screenshot handle that is valid for the duration of the game process and can be used to apply tags.  May return [SCREENSHOT_INVALID_HANDLE](#constants) if the file could not be found.

    !!! trigger "Triggers"
		[screenshot_ready](#screenshot_ready) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#AddScreenshotToLibrary){ .md-button .md-button--doc_classes target="_blank" }

### addVRScreenshotToLibrary

!!! function "addVRScreenshotToLibrary( `VRScreenshotType` type, `string` filename, `string` vr_filename )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | type | [VRScreenshotType enum](#vrscreenshottype) | The type of VR screenshot that this is. |
    | filename | string | The absolute file path to a 2D JPG, PNG, or TGA version of the screenshot for the library view. |
    | vr_filename | string | The absolute file path to the VR screenshot, this should be the same type of screenshot specified in **type**. |

    Adds a VR screenshot to the user's Steam screenshot library from disk in the supported type. 

    !!! returns "Returns: uint32_t"
        Returns a screenshot handle that is valid for the duration of the game process and can be used to apply tags.  May return [SCREENSHOT_INVALID_HANDLE](#constants) if the file could not be found.

    !!! trigger "Triggers"
		[screenshot_ready](#screenshot_ready) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#AddVRScreenshotToLibrary){ .md-button .md-button--doc_classes target="_blank" }

### hookScreenshots

!!! function "hookScreenshots( `bool` hook )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | hook | bool | Enable (true) or disable (false) hooking? |

    Toggles whether the overlay handles screenshots when the user presses the screenshot hotkey, or if the game handles them. 
  
	Hooking is disabled by default, and only ever enabled if you do so with this function. 
  
	If the hooking is enabled, then the [screenshot_requested](#screenshot_requested) callback will be sent if the user presses the hotkey or when [triggerScreenshot](#triggerscreenshot) is called. Then the game is expected to call [writeScreenshot](#writescreenshot) or [addScreenshotToLibrary](#addscreenshottolibrary) in response. 
 
	You can check if hooking is enabled with [isScreenshotsHooked](#isscreenshotshooked).

    !!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#HookScreenshots){ .md-button .md-button--doc_classes target="_blank" }

### isScreenshotsHooked

!!! function "isScreenshotsHooked( )"
    Checks if the app is hooking screenshots. 

    !!! returns "Returns: bool"
        Returns true if the game is hooking screenshots and is expected to handle them; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#IsScreenshotsHooked){ .md-button .md-button--doc_classes target="_blank" }

### setLocation

!!! function "setLocation( `uint32_t` screenshot, `string` location )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | screenshot | uint32_t | The handle to the screenshot to tag. |
    | location | string | The location in the game where this screenshot was taken. This can not be longer than [UFS_TAG_VALUE_MAX](#constants). |

    Sets optional metadata about a screenshot's location. For example, the name of the map it was taken on.

    You can get the handle to tag the screenshot once it has been successfully saved from the [screenshot_ready](#screenshot_ready) callback or via the [writeScreenshot](#writescreenshot, [addScreenshotToLibrary](#addscreenshottolibrary), [addVRScreenshotToLibrary](#addvrscreenshottolibrary) calls.

    !!! returns "Returns: bool"
        Returns true if the location was successfully added to the screenshot; otherwise, false if **screenshot** was invalid, or **location** is invalid or too long.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#SetLocation){ .md-button .md-button--doc_classes target="_blank" }

### tagPublishedFile

!!! function "tagPublishedFile( `uint32_t` screenshot, `uint64_t` file_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | screenshot | uint32_t | The handle to the screenshot to tag. |
    | file_id | uint64_t | The workshop item ID that is in the screenshot. |

    Tags a published file as being visible in the screenshot.

    You can tag up to the value declared by [MAX_TAGGED_PUBLISHED_FILES](#constants) in a single screenshot. Tagging more items than that will just be discarded.

    This function has a built in delay before saving the tag which allows you to call it repeatedly for each item.

    You can get the handle to tag the screenshot once it has been successfully saved from the [screenshot_ready](#screenshot_ready) callback or via the [writeScreenshot](#writescreenshot, [addScreenshotToLibrary](#addscreenshottolibrary), [addVRScreenshotToLibrary](#addvrscreenshottolibrary) calls.

    !!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#TagPublishedFile){ .md-button .md-button--doc_classes target="_blank" }

### tagUser

!!! function "tagUser( `uint32_t` screenshot, `uint64_t` steam_id )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | screenshot | uint32_t | The handle to the screenshot to tag. |
    | steam_id | uint64_t | The Steam ID of a user that is in the screenshot. |

    Tags a Steam user as being visible in the screenshot. You can tag up to the value declared by [MAX_TAGGED_USERS](#constants) in a single screenshot. Tagging more users than that will just be discarded. This function has a built in delay before saving the tag which allows you to call it repeatedly for each item.

    You can get the handle to tag the screenshot once it has been successfully saved from the [screenshot_ready](#screenshot_ready) callback or via the [writeScreenshot](#writescreenshot), [addScreenshotToLibrary](#addscreenshottolibrary), [addVRScreenshotToLibrary](#addvrscreenshottolibrary) calls. 

    !!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#TagUser){ .md-button .md-button--doc_classes target="_blank" }

### triggerScreenshot

!!! function "triggerScreenshot( )"
    Either causes the Steam Overlay to take a screenshot, or tells your screenshot manager that a screenshot needs to be taken. Depending on the value of [isScreenshotsHooked](#isscreenshotshooked).  Triggers a [screenshot_requested](#screenshot_requested) callback if hooking has been enabled with [hookScreenshots](#hookscreenshots). Otherwise [screenshot_ready](#screenshot_ready) will be called when the screenshot has been saved and added to the library.

    !!! returns "Returns: void"

    !!! trigger "Triggers"
		* [screenshot_ready](#screenshot_ready) callback
        * [screenshot_requested](#screenshot_requested) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#TriggerScreenshot){ .md-button .md-button--doc_classes target="_blank" }

### writeScreenshot

!!! function "writeScreenshot( `PoolByteArray` rgb, `int` width, `int` height )"
    | :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | rgb | PoolByteArray | The buffer containing the raw RGB data from the screenshot. |
    | width | int | The width of the screenshot in pixels. |
    | height | int | The height of the screenshot in pixels. |

    Writes a screenshot to the user's screenshot library given the raw image data, which must be in RGB format. The return value is a handle that is valid for the duration of the game process and can be used to apply tags.

    !!! returns "Returns: uint32_t"
        Returns a screenshot handle that is valid for the duration of the game process and can be used to apply tags.  May return [SCREENSHOT_INVALID_HANDLE](#constants) if the file could not be found.

    !!! trigger "Triggers"
		 [screenshot_ready](#screenshot_ready) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#WriteScreenshot){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### screenshot_ready

!!! function "screenshot_ready"
	A screenshot successfully written or otherwise added to the library and can now be tagged.
	
	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| handle | uint32_t | The screenshot handle that has been written.
    	| result | [Result enum](main.md#result) | The result of the operation. Possible values are: [RESULT_OK](main.md#result) if the screenshot was successfully added to the user's library. [RESULT_FAIL](main.md#result) if the screenshot could not be loaded or parsed. [RESULT_OF_FAILURE](main.md#result) if screenshot could not be saved to disk.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#ScreenshotReady_t){ .md-button .md-button--doc_classes target="_blank" }

### screenshot_requested

!!! function "screenshot_requested"
	A screenshot has been requested by the user from the Steam screenshot hotkey. This will only be called if [hookScreenshots](#hookscreenshots) has been enabled, in which case Steam will not take the screenshot itself.

	!!! returns "Returns"
		 Nothing.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamScreenshots#ScreenshotRequested_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
MAX_TAGGED_PUBLISHED_FILES | k_nScreenshotMaxTaggedPublishedFiles | 32 | The maximum number of workshop items that can be tagged in a screenshot using [tagPublishedFile](#tagpublishedfile).
MAX_TAGGED_USERS | k_nScreenshotMaxTaggedUsers | 32 | The maximum number of users that can be tagged in a screenshot using [tagUser](#taguser).
SCREENSHOT_INVALID_HANDLE | INVALID_SCREENSHOT_HANDLE | 0 | An invalid screenshot handle, this is returned when writing or adding a screenshot has failed.
SCREENSHOT_THUMB_WIDTH | k_ScreenshotThumbWidth | 200 | Required width of a thumbnail provided to [addScreenshotToLibrary](#addscreenshottolibrary). If you do not provide a thumbnail then one will be generated automatically.
UFS_TAG_TYPE_MAX | k_cubUFSTagTypeMax | 255 | Unused.
UFS_TAG_VALUE_MAX | k_cubUFSTagValueMax | 255 | The maximum length in bytes of a location metadata string set on a screenshot using [setLocation](#setlocation).

{==
## :material-numeric: Enums
==}

### VRScreenshotType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
VR_SCREENSHOT_TYPE_NONE | k_EVRScreenshotType_None | 0 | -
VR_SCREENSHOT_TYPE_MONO | k_EVRScreenshotType_Mono | 1 | -
VR_SCREENSHOT_TYPE_STEREO | k_EVRScreenshotType_Stereo | 2 | -
VR_SCREENSHOT_TYPE_MONO_CUBE_MAP | k_EVRScreenshotType_MonoCubemap | 3 | -
VR_SCREENSHOT_TYPE_MONO_PANORAMA | k_EVRScreenshotType_MonoPanorama | 4 | -
VR_SCREENSHOT_TYPE_STEREO_PANORAMA | k_EVRScreenshotType_StereoPanorama | 5 | -
