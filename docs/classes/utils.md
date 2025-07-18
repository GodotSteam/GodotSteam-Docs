---
title: Utils
description: A class reference for Utilities.
icon: material/tools
---

# Utils

Interface which provides access to a range of miscellaneous utility functions.

!!! info "Available in both the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } and [GodotSteam Server branches](https://github.com/GodotSteam/GodotSteam-Server){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### checkFileSignature

!!! function "checkFileSignature( ```string``` filename )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| filename | string | Name of the file you want to check the signature on. |

	Asynchronous call to check if an executable file has been signed using the public key set on the signing tab of the partner site; for example, to refuse to load modified executable files.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		* [check_file_signature](#check_file_signature)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#CheckFileSignature){ .md-button .md-button--doc_classes target="_blank" }
	[ :material-tag-plus: Added GodotSteam 4.15](../changelog/godot4.md/#version-415){ .md-button .md-button--changes target="_blank" }

### dismissFloatingGamepadTextInput

!!! function "dismissFloatingGamepadTextInput( )"
	Dismisses the floating keyboard.

	!!! returns "Returns: bool"

### dismissGamepadTextInput

!!! function "dismissGamepadTextInput( )"
	Dismisses the full-screen text input dialog.

	!!! returns "Returns: bool"

### filterText

!!! function "filterText( ```TextFilteringContext``` context, ```uint64_t``` steam_id, ```string``` message )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| context | [TextFilteringContext enum](#textfilteringcontext) | The type of content in the input string. |
	| steam_id | uint64_t | Steam ID of the input string source; the player who entered the chat text. |
	| message | string | The input string that should be filtered. |

	Filters the provided input message and returns the filtered result. Legally required filtering is always applied. Additional filtering may occur, based on the context and user settings.

	!!! returns "Returns: string"
		Returns the filtered **message**, even if no filtering is performed.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#FilterText){ .md-button .md-button--doc_classes target="_blank" }

### getAPICallFailureReason

!!! function "getAPICallFailureReason( )"
	Used to get the failure reason of a call result.

	The primary usage for this function is debugging. The failure reasons are typically out of your control and tend to not be very important. Just keep retrying your API Call until it works.

	!!! returns "Returns: string"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetAPICallFailureReason){ .md-button .md-button--doc_classes target="_blank" }

### getAppID

!!! function "getAppID( )"
	Gets the App ID of the current process.

	!!! returns "Returns: uint32_t"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetAppID){ .md-button .md-button--doc_classes target="_blank" }

### getConnectedUniverse

!!! function "getConnectedUniverse( )"
	Gets the uinverse the current client is connected to.  Supposed to be for Valve's use only.

	!!! returns "Returns: int / [Universe enum](main.md#universe)"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetConnectedUniverse){ .md-button .md-button--doc_classes target="_blank" }
	[ :material-tag-plus: Added GodotSteam 4.15](../changelog/godot4.md/#version-415){ .md-button .md-button--changes target="_blank" }

### getCurrentBatteryPower

!!! function "getCurrentBatteryPower( )"
	Gets the current amount of battery power on the computer.

	!!! returns "Returns: int"
		The current battery power ranging from 0 to 100%. Returns 255 when the user in on AC power.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetCurrentBatteryPower){ .md-button .md-button--doc_classes target="_blank" }

### getImageRGBA

!!! function "getImageRGBA( ```int``` image_handle )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| image_handle | int | The handle of the image that will be obtained; usually retrieved from an avatar function. |

	Gets the image bytes from an image handle.

	Prior to calling this you must get the size of the image by calling [getImageSize](#getimagesize) so that you can create your buffer with an appropriate size. You can then allocate your buffer with the width and height as: width * height * 4. The image is provided in RGBA format.

	This call can be somewhat expensive as it converts from the compressed type (JPG, PNG, TGA) and provides no internal caching of returned buffer, thus it is highly recommended to only call this once per image handle and cache the result. This function is only used for Steam Avatars and Achievement images and those are not expected to change mid game.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
		| --- | ---- | ----- |
		| buffer  | PackedByteArray | The image buffer to translate into a texture.
		| success | bool |  Returns true upon success if the image handle is valid and the buffer was filled out; otherwise, false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetImageRGBA){ .md-button .md-button--doc_classes target="_blank" }

### getImageSize

!!! function "getImageSize( ```int``` image_handle )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| image_handle | int | The handle of the image that will be obtained; usually retrieved from an avatar function. |

	Gets the size of a Steam image handle.

	This must be called before calling [getImageRGBA](#getimagergba) to create an appropriately sized buffer that will be filled with the raw image data.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
		| --- | ---- | ----- |
		| width | uint32_t | Returns the width of the image.
		| height | uint32_t | Returns the height of the image.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetImageSize){ .md-button .md-button--doc_classes target="_blank" }

### getIPCCallCount

!!! function "getIPCCallCount( )"
	Used for perf debugging so you can determine how many IPC (Inter-Process Communication) calls your game makes per frame.

	Every IPC call is at minimum a thread context switch if not a process one so you want to rate control how often you do them.

	!!! returns "Returns: uint32_t"
		Returns the number of IPC calls made since the last time this function was called.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetIPCCallCount){ .md-button .md-button--doc_classes target="_blank" }

### getIPCountry

!!! function "getIPCountry( )"
	This is looked up via an IP-to-location database.

	!!! returns "Returns: string"
		Returns the 2 digit ISO 3166-1-alpha-2 format country code which client is running in. e.g "US" or "UK".

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetIPCountry){ .md-button .md-button--doc_classes target="_blank" }

### getIPv6ConnectivityState

!!! function "getIPv6ConnectivityState( ```IPv6ConnectivityProtocol``` protocol )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| protocol | [IPv6ConnectivityProtocol enum](main.md#ipv6connectivityprotocol) | The IPv6 protocol you are getting the state for; typically HTTP or UDP. |

	Return what we believe your current ipv6 connectivity to "the internet" is on the specified protocol. This **does not** tell you if the Steam client is currently connected to Steam via IPv6.

	!!! returns "Returns: int / [IPv6ConnectivityState enum](main.md#ipv6connectivitystate)"

	---
	[ :material-tag-plus: Added GodotSteam 4.15](../changelog/godot4.md/#version-415){ .md-button .md-button--changes target="_blank" }

### getSecondsSinceAppActive

!!! function "getSecondsSinceAppActive( )"
	Returns the number of seconds since the application was active.

	!!! returns "Returns: int"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetSecondsSinceAppActive){ .md-button .md-button--doc_classes target="_blank" }

### getSecondsSinceComputerActive

!!! function "getSecondsSinceComputerActive( )"
	Returns the number of seconds since the user last moved the mouse.

	!!! returns "Returns: int"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetSecondsSinceComputerActive){ .md-button .md-button--doc_classes target="_blank" }

### getServerRealTime

!!! function "getServerRealTime( )"
	Returns the Steam server time in Unix epoch format. (Number of seconds since Jan 1, 1970 UTC).

	!!! returns "Returns: int"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetServerRealTime){ .md-button .md-button--doc_classes target="_blank" }

### getSteamUILanguage

!!! function "getSteamUILanguage( )"
	Returns the language the steam client is running in.

	You probably want [getCurrentGameLanguage](apps.md#getcurrentgamelanguage) instead, this should only be used in very special cases.

	For a full list of languages see [Supported Languages](https://partner.steamgames.com/doc/store/localization#supported_languages){ target="_blank" }.

	!!! returns "Returns: string"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GetSteamUILanguage){ .md-button .md-button--doc_classes target="_blank" }

### initFilterText

!!! function "initFilterText( )"
	Initializes text filtering, loading dictionaries for the language the game is running in.

	Users can customize the text filter behavior in their Steam Account preferences.

	!!! returns "Returns: bool"
		Returns true if initialization succeeds. False if filtering is unavailable for the game's language, in which case [filtetText](#filtertext) will act as a passthrough.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#InitFilterText){ .md-button .md-button--doc_classes target="_blank" }

### isAPICallCompleted

!!! function "isAPICallCompleted( )"
	Checks if an API Call is completed. Provides the backend of the CallResult wrapper.

	It's generally not recommended that you use this yourself.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | completed | bool | Returns true if the API Call is valid and has completed; otherwise, false.
		| failed | bool | Returns whether the API call has encountered a failure (true) or not (false).

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IsAPICallCompleted){ .md-button .md-button--doc_classes target="_blank" }

### isOverlayEnabled

!!! function "isOverlayEnabled( )"
	Checks if the Steam Overlay is running & the user can access it.

	The overlay process could take a few seconds to start & hook the game process, so this function will initially return false while the overlay is loading.

	!!! returns "Returns: bool"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IsOverlayEnabled){ .md-button .md-button--doc_classes target="_blank" }

### isSteamChinaLauncher

!!! function "isSteamChinaLauncher( )"
	Returns whether the current launcher is a Steam China launcher. You can cause the client to behave as the Steam China launcher by adding ```-dev -steamchina``` to the command line when running Steam.

	!!! returns "Returns: bool"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IsSteamChinaLauncher){ .md-button .md-button--doc_classes target="_blank" }

### isSteamInBigPictureMode

!!! function "isSteamInBigPictureMode( )"
	Checks if Steam & the Steam Overlay are running in Big Picture mode.

	Games must be launched through the Steam client to enable the Big Picture overlay. During development, a game can be added as a non-steam game to the developers library to test this feature.

	!!! returns "Returns: bool"
		Returns true if the Big Picture overlay is visible; otherwise, false.  This will always return false if your app is not the ["game" application type.](https://partner.steamgames.com/doc/store/application#types_of_applications){ target="\_blank" }

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IsSteamInBigPictureMode){ .md-button .md-button--doc_classes target="_blank" }

### isSteamRunningInVR

!!! function "isSteamRunningInVR( )"
	Checks if Steam is running in VR mode.

	!!! returns "Returns: bool"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IsSteamRunningInVR){ .md-button .md-button--doc_classes target="_blank" }

### isSteamRunningOnSteamDeck

!!! function "isSteamRunningOnSteamDeck( )"
	Checks if Steam is running on a Steam Deck device.

	!!! returns "Returns: bool"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IsSteamRunningOnSteamDeck){ .md-button .md-button--doc_classes target="_blank" }

### isVRHeadsetStreamingEnabled

!!! function "isVRHeadsetStreamingEnabled( )"
	Checks if the HMD view will be streamed via Steam In-Home Streaming.

	!!! returns "Returns: bool"
		Returns true if VR is enabled and the HMD view is currently being streamed; otherwise, false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IsVRHeadsetStreamingEnabled){ .md-button .md-button--doc_classes target="_blank" }

### overlayNeedsPresent

!!! function "overlayNeedsPresent( )"
	Checks if the Overlay needs a present. Only required if using event driven render updates.

	Typically this call is unneeded if your game has a constantly running frame loop that calls the D3D Present API, or OGL SwapBuffers API every frame as is the case in most games. However, if you have a game that only refreshes the screen on an event driven basis then that can break the overlay, as it uses your Present/SwapBuffers calls to drive it's internal frame loop and it may also need to Present() to the screen any time a notification happens or when the overlay is brought up over the game by a user.

	You can use this API to ask the overlay if it currently need a present in that case, and then you can check for this periodically (roughly 33hz is desirable) and make sure you refresh the screen with Present or SwapBuffers to allow the overlay to do it's work.

	!!! returns "Returns: bool"
		Returns true if the overlay needs you to refresh the screen; otherwise, false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#BOverlayNeedsPresent){ .md-button .md-button--doc_classes target="_blank" }

### setGameLauncherMode

!!! function "setGameLauncherMode( ```bool``` mode )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| mode | bool | Whether a launcher is active or not. |

	In game launchers that don't have controller support you can call this to have Steam Input translate the controller input into mouse / keyboard to navigate the launcher.

	!!! returns "Returns: void"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#SetGameLauncherMode){ .md-button .md-button--doc_classes target="_blank" }

### setOverlayNotificationInset

!!! function "setOverlayNotificationInset( ```int``` horizontal, ```int``` vertical )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| horizontal | int | The horizontal (left-right) distance in pixels from the corner. |
	| vertical | int | The vertical (up-down) distance in pixels from the corner. |

	Sets the inset of the overlay notification from the corner specified by [setOverlayNotificationPosition](#setoverlaynotificationposition). Integer should be number of pixels.

	A value of (0, 0) resets the position into the corner.

	This position is per-game and is reset each launch.

	!!! returns "Returns: void"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#SetOverlayNotificationInset){ .md-button .md-button--doc_classes target="_blank" }

### setOverlayNotificationPosition

!!! function "setOverlayNotificationPosition( ```NotificationPosition``` position )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| position | [NotificationPosition enum](main.md#notificationposition) | The corner the notification displays from. |

	Set the position where overlay shows notifications.

	You can also set the distance from the specified corner by using [setOverlayNotificationInset](#setoverlaynotificationinset).

	This position is per-game and is reset each launch.

	!!! returns "Returns: void"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#SetOverlayNotificationPosition){ .md-button .md-button--doc_classes target="_blank" }

### setVRHeadsetStreamingEnabled

!!! function "setVRHeadsetStreamingEnabled( ```bool``` enabled = true )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| enabled | bool | Turns VR HMD Streaming on (true) or off (false); defaults to true. |

	Set whether the HMD content will be streamed via Steam In-Home Streaming.

	If this is enabled, then the scene in the HMD headset will be streamed, and remote input will not be allowed. Otherwise if this is disabled, then the application window will be streamed instead, and remote input will be allowed. VR games default to enabled unless "VRHeadsetStreaming" "0" is in the extended appinfo for a game.

	This is useful for games that have asymmetric multiplayer gameplay.

	!!! returns "Returns: void"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IsVRHeadsetStreamingEnabled){ .md-button .md-button--doc_classes target="_blank" }

### showFloatingGamepadTextInput

!!! function "showFloatingGamepadTextInput( ```FloatingGamepadTextInputMode``` input_mode, ```int``` text_field_x_position, ```int``` text_field_y_position, ```int``` text_field_width, ```int``` text_field_height )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| input_mode | [FloatingGamepadTextInputMode enum](#floatinggamepadtextinputmode) | Selects the keyboard type to use. |
	| text_field_x_position | int | X coordinate of text field which shouldn't be obscured by the floating keyboard. |
	| text_field_y_position | int | Y coordinate of text field which shouldn't be obscured by the floating keyboard. |
	| text_field_width | int | Width of text field which shouldn't be obscured by the floating keyboard. |
	| text_field_height | int | Height of text field which shouldn't be obscured by the floating keyboard. |

	Opens a floating keyboard over the game content and sends OS keyboard keys directly to the game.

	The text field position is specified in pixels relative the origin of the game window and is used to position the floating keyboard in a way that doesn't cover the text field.

	!!! returns "Returns: bool"
		Returns true if the floating keyboard was shown; otherwise, false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#ShowFloatingGamepadTextInput){ .md-button .md-button--doc_classes target="_blank" }

### showGamepadTextInput

!!! function "showGamepadTextInput( ```GamepadTextInputMode``` input_mode, ```GamepadTextInputLineMode``` line_input_mode, ```string``` description, ```uint32_t``` max_text, ```string``` preset_text )"
	| :material-variable: Argument | Type | Notes |
	| -------- | ---- | ----- |
	| input_mode | [GamepadTextInputMode enum](#gamepadtextinputmode) | Selects the input mode to use, either Normal or Password (hidden text). |
	| line_input_mode | [GamepadTextInputLineMode enum](#gamepadtextinputlinemode) | Controls whether to use single or multi line input. |
	| description | string | Sets the description that should inform the user what the input dialog is for. |
	| max_text | uint32_t | The maximum number of characters that the user can input. |
	| preset_text | string | Sets the preexisting text which the user can edit. |

	Activates the Big Picture text input dialog which only supports gamepad input.

	!!! returns "Returns: bool"
		Returns true if the Big Picture overlay is running; otherwise, false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#ShowGamepadTextInput){ .md-button .md-button--doc_classes target="_blank" }

### startVRDashboard

!!! function "startVRDashboard( )"
	Ask SteamUI to create and render its OpenVR dashboard.

	!!! returns "Returns: void"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#StartVRDashboard){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### app_resuming_from_suspend

!!! function "app_resuming_from_suspend"
	Sent after the device returns from sleep/suspend mode.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#AppResumingFromSuspend_t){ .md-button .md-button--doc_classes target="_blank" }

### check_file_signature

!!! function "check_file_signature"
	Call result for [checkFileSignature](#checkfilesignature).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | signature | string | Contains the result of the file signature check.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#CheckFileSignature_t){ .md-button .md-button--doc_classes target="_blank" }

### filter_text_dictionary_changed

!!! function "filter_text_dictionary_changed"
	Sent when the text filtering dictionary has changed languages.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | language | int | Numerical code for the new language.

### floating_gamepad_text_input_dismissed

!!! function "floating_gamepad_text_input_dismissed"
	Called when the floating keyboard invoked from [showFloatingGamepadTextInput](#showfloatinggamepadtextinput) has been closed.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#FloatingGamepadTextInputDismissed_t){ .md-button .md-button--doc_classes target="_blank" }

### gamepad_text_input_dismissed

!!! function "gamepad_text_input_dismissed"
	Called when the Big Picture gamepad text input has been closed.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| submitted | bool | Returns true if the user entered and accepted text; call [getEnteredGamepadTextInput](#getenteredgamepadtextinput) to receive the text. Otherwise, false if the input was canceled.
		| text | string | Contains the text that was submitted.
		| app_id | uint32_t | The app ID for the game this text was submitted for.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#GamepadTextInputDismissed_t){ .md-button .md-button--doc_classes target="_blank" }

### ip_country

!!! function "ip_country"
	Called when the country of the user changed. The country should be updated with [getIPCountry](#getipcountry).

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#IPCountry_t){ .md-button .md-button--doc_classes target="_blank" }

### low_power

!!! function "low_power"
	Called when running on a laptop and less than 10 minutes of battery is left, and then fires then every minute afterwards.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| power | uint8 | The estimated amount of battery life left in minutes.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#LowBatteryPower_t){ .md-button .md-button--doc_classes target="_blank" }


### steam_api_call_completed

!!! function "steam_api_call_completed"
	Called when a SteamAPICall_t has completed or failed.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| async_call | uint64_t | The handle of the Steam API Call that completed.
		| callback | int | This is the callback constant which uniquely identifies the completed callback.
		| parameter | uint32_t | The size in bytes of the completed callback.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#SteamAPICallCompleted_t){ .md-button .md-button--doc_classes target="_blank" }

### steam_shutdown

!!! function "steam_shutdown"
	Called when Steam wants to shut down.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUtils#SteamShutdown_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-numeric: Enums
==}

### CheckFileSignature

Results for [checkFileSignature](#checkfilesignature).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CHECK_FILE_SIGNATURE_INVALID_SIGNATURE | k_ECheckFileSignatureInvalidSignature | 0 | The file exists, and the signing tab has been set for this file, but the file is either not signed or the signature does not match.
CHECK_FILE_SIGNATURE_VALID_SIGNATURE | k_ECheckFileSignatureValidSignature | 1 | The file is signed and the signature is valid.
CHECK_FILE_SIGNATURE_FILE_NOT_FOUND | k_ECheckFileSignatureFileNotFound | 2 | The file does not exist on disk.
CHECK_FILE_SIGNATURE_NO_SIGNATURES_FOUND_FOR_THIS_APP | k_ECheckFileSignatureNoSignaturesFoundForThisApp | 3 | This app has not been configured on the signing tab of the partner site to enable this function.
CHECK_FILE_SIGNATURE_NO_SIGNATURES_FOUND_FOR_THIS_FILE | k_ECheckFileSignatureNoSignaturesFoundForThisFile | 4 | This file is not listed on the signing tab for the partner site.

### GamepadTextInputLineMode

Controls number of allowed lines for the Big Picture gamepad text entry.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
GAMEPAD_TEXT_INPUT_LINE_MODE_SINGLE_LINE | k_EGamepadTextInputLineModeSingleLine | 0 | -
GAMEPAD_TEXT_INPUT_LINE_MODE_MULTIPLE_LINES | k_EGamepadTextInputLineModeMultipleLines | 1 | -

### GamepadTextInputMode

Input modes for the Big Picture gamepad text entry.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
GAMEPAD_TEXT_INPUT_MODE_NORMAL | k_EGamepadTextInputModeNormal | 0 | -
GAMEPAD_TEXT_INPUT_MODE_PASSWORD | k_EGamepadTextInputModePassword | 1 | -

### FloatingGamepadTextInputMode

Controls number of allowed lines for the Big Picture gamepad text entry.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
FLOATING_GAMEPAD_TEXT_INPUT_MODE_SINGLE_LINE | k_EFloatingGamepadTextInputModeModeSingleLine | 0 | Enter dismisses the keyboard.
FLOATING_GAMEPAD_TEXT_INPUT_MODE_MULTIPLE_LINES | k_EFloatingGamepadTextInputModeModeMultipleLines | 1 | User needs to explictly close the keyboard.
FLOATING_GAMEPAD_TEXT_INPUT_MODE_EMAIL | k_EFloatingGamepadTextInputModeModeEmail | 2 | Keyboard layout is email, enter dismisses the keyboard.
FLOATING_GAMEPAD_TEXT_INPUT_MODE_NUMERIC | k_EFloatingGamepadTextInputModeModeNumeric | 3 | Keyboard layout is numeric, enter dismisses the keyboard.

### SteamAPICallFailure

Steam API call failure results.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
STEAM_API_CALL_FAILURE_NONE |k_ESteamAPICallFailureNone | -1 | No failure.
STEAM_API_CALL_FAILURE_STEAM_GONE | k_ESteamAPICallFailureSteamGone | 0 | The local Steam process has gone away.
STEAM_API_CALL_FAILURE_NETWORK_FAILURE |k_ESteamAPICallFailureNetworkFailure | 1 | The network connection to Steam has been broken or was already broken. [steam_server_disconnected](user.md#steam_server_disconnected) callback will be sent around the same time. [steam_server_connected](user.md#steam_server_connected) will be sent when the client is able to talk to the Steam servers again.
STEAM_API_CALL_FAILURE_INVALID_HANDLE | k_ESteamAPICallFailureInvalidHandle | 2 | The SteamAPICall_t handle passed in no longer exists.
STEAM_API_CALL_FAILURE_MISMATCHED_CALLBACK | k_ESteamAPICallFailureMismatchedCallback | 3 | getAPICallResult was called with the wrong callback type for this API call.

### TextFilteringContext

The context where text filtering is being done.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
TEXT_FILTERING_CONTEXT_UNKNOWN | k_ETextFilteringContextUnknown | 0 | Unknown context.
TEXT_FILTERING_CONTEXT_GAME_CONTENT | k_ETextFilteringContextGameContent | 1 | Game content, only legally required filtering is performed.
TEXT_FILTERING_CONTEXT_CHAT | k_ETextFilteringContextChat | 2 | Chat from another player.
TEXT_FILTERING_CONTEXT_NAME | k_ETextFilteringContextName | 3 | Character or item name.
