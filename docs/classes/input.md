---
title: Input
description: A class reference for Steam Input.
icon: material/controller
---

# Input

Steam Input API is a flexible action-based API that supports all major controller types - Xbox, PlayStation, Nintendo Switch Pro, and Steam Controllers. See the [Steam Input](https://partner.steamgames.com/doc/features/steam_controller){ target="\_blank" } documentation for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### activateActionSet

!!! function "activateActionSet( `uint64_t` input_handle, `uint64_t` action_set_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to activate an action set for.
    | action_set_handle | uint64_t | The handle of the action set you want to activate.

    Reconfigure the controller to use the specified action set (i.e. "Menu", "Walk", or "Drive").

	This is cheap, and can be safely called repeatedly. It's often easier to repeatedly call it in your state loops, instead of trying to place it in all of your state transitions.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#ActivateActionSet){ .md-button .md-button--store target="_blank" }

### activateActionSetLayer

!!! function "activateActionSetLayer( `uint64_t` input_handle, `uint64_t` action_set_layer_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to activate an action set layer for.
    | action_set_layer_handle | uint64_t | The handle of the action set layer you want to activate.

    Reconfigure the controller to use the specified action set layer.

	See the [Action Set Layers](https://partner.steamgames.com/doc/features/steam_controller/action_set_layers){ target="_blank" } article for full details and an in-depth practical example.

	!!! returns "Returns: void"
 
    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#ActivateActionSetLayer){ .md-button .md-button--store target="_blank" }

### deactivateActionSetLayer

!!! function "deactivateActionSetLayer( `uint64_t` input_handle, `uint64_t` action_set_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to deactivate an action set layer for.
    | action_set_layer_handle | uint64_t | The handle of the action set layer you want to deactivate.

    Reconfigure the controller to stop using the specified action set.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#DeactivateActionSetLayer){ .md-button .md-button--store target="_blank" }

 
### deactivateAllActionSetLayers

!!! function "deactivateAllActionSetLayers( `uint64_t` input_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to deactivate all action set layers for.

    Reconfigure the controller to stop using all action set layers.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#DeactivateAllActionSetLayers){ .md-button .md-button--store target="_blank" }

### enableDeviceCallbacks

!!! function "enableDeviceCallbacks( )"
	Enable [input_device_connected](#input_device_connected) and [input_device_disconnected](#input_device_disconnected) callbacks. Each controller that is already connected will generate a device connected callback when you enable them.

	!!! returns "Returns: void"

### enableActionEventCallbacks

!!! function "enableActionEventCallbacks( )"
	Enable SteamInputActionEvent_t callbacks. Directly calls your callback function for lower latency than standard Steam callbacks. Supports one callback at a time. This is called within either [runFrame](#runframe) or by [run_callbacks](main.md#run_callbacks).

	!!! returns "Returns: void"

### getActiveActionSetLayers

!!! function "getActiveActionSetLayers( `uint64_t` input_handle)"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to get active action set layers for.

    Fill an array with all of the currently active action set layers for a specified controller handle.

	!!! returns "Returns: array"
		Contains a list of input action set handles (uint64_t).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetActiveActionSetLayers){ .md-button .md-button--store target="_blank" }

### getActionSetHandle

!!! function "getActionSetHandle( `string` action_set_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | action_set_name | string | The string identifier of an action set defined in the game's VDF file.

	Fill an array with all of the currently active action set layers for a specified controller handle.

	!!! returns "Returns: uint64_t"
		The handle of the specified action set.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetActionSetHandle){ .md-button .md-button--store target="_blank" }
 
### getActionOriginFromXboxOrigin

!!! function "getActionOriginFromXboxOrigin( `uint64_t` input_handle, `XboxOrigin` origin )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect. You can use [getControllerForGamepadIndex](#getcontrollerforgamepadindex) to get this handle.
    | origin | [XboxOrigin enum](#xboxorigin) | This is the button you want to get the image for.

    Get the equivalent [InputActionOrigin](#inputactionorigin) for a given XBox controller origin this can be chained with [getGlyphForActionOrigin](#getglyphforactionorigin) to provide future proof glyphs for non-Steam Input API action games. This only translates the buttons directly and doesn't take into account any remapping a user has made in their configuration.

	!!! returns "Returns: [InputActionOrigin enum](#inputactionorigin)"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetActionOriginFromXboxOrigin){ .md-button .md-button--store target="_blank" }

### getAnalogActionData

!!! function "getAnalogActionData( `uint64_t` input_handle, `uint64_t` analog_action_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to query.
    | analog_action_handle | uint64_t | The handle of the analog action you want to query.

    Gets the current state of the supplied analog game action.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| mode | [InputSourceMode enum](#inputsourcemode) | The type of data coming from this action, this will match what was specified in the action set's VDF definition.
		| x | float | The current state of this action on the horizontal axis.
		| y | float | The current state of this action vertical axis.
		| active | bool | Whether or not this action is currently available to be bound in the active action set. If it is not available, OR does not belong to the active action set, this will be false.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetAnalogActionData){ .md-button .md-button--store target="_blank" }

### getAnalogActionHandle

!!! function "getAnalogActionHandle( `string` action_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | action_name | string | The string identifier of the analog action defined in the game's VDF fi

    Get the handle of the specified analog action. Best to do this once on startup and store the handles for all future API calls.

	!!! returns "Returns: uint64_t"
		The handle of the specified analog action.

	!!! info "Notes"
		This function does not take an action set handle parameter. That means that each action in your VDF file must have a unique string identifier. In other words, if you use an action called "up" in two different action sets, this function will only ever return one of them and the other will be ignored.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetAnalogActionHandle){ .md-button .md-button--store target="_blank" }

### getAnalogActionOrigins

!!! function "getAnalogActionOrigins( `uint64_t` input_handle, `uint64_t` action_set_handle, `uint64_t` analog_action_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle |uint64_t | The handle of the controller you want to query.
    | action_set_handle | uint64_t | The handle of the action set you want to query.
    | analog_action_handle | uint64_t | The handle of the analog action you want to query.

    Get the origin(s) for an analog action within an action set by filling the return array with handles. Use this to display the appropriate on-screen prompt for the action.

	!!! returns "Returns: array"
		Contains a list of [InputActionOrigin enums](#inputactionorigin).

		The [InputActionOrigin enum](#inputactionorigin) will get extended as support for new controller controllers gets added to the Steam client and will exceed the values from this header, please check bounds if you are using a look up table.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetAnalogActionOrigins){ .md-button .md-button--store target="_blank" }

### getConnectedControllers

!!! function "getConnectedControllers( )"
	Enumerate currently connected Steam Input enabled devices; developers can opt in controller by type (ex: Xbox/Playstation/etc.) via the Steam Input settings in the Steamworks site or users can opt-in in their controller settings in Steam.

	!!! returns "Returns: array"
		Contains a list of controller handles (uint64_t).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetConnectedControllers){ .md-button .md-button--store target="_blank" }

### getControllerForGamepadIndex

!!! function "getControllerForGamepadIndex( `int` index )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | index | int | The index of the emulated gamepad you want to get a controller handle for.

    Returns the associated controller handle for the specified emulated gamepad. Can be used with [getInputTypeForHandle](#getinputtypeforhandle) to determine the type of controller using Steam Input Gamepad Emulation.

	!!! returns "Returns: uint64_t"
		Returns a Steam Input handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetControllerForGamepadIndex){ .md-button .md-button--store target="_blank" }
 
### getCurrentActionSet

!!! function "getCurrentActionSet( `uint64_t` input_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to query.

    Get the currently active action set for the specified controller.

	!!! returns "Returns: uint64_t"
		The handle of the action set activated for the specified controller.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetCurrentActionSet){ .md-button .md-button--store target="_blank" }

### getDeviceBindingRevision

!!! function "getDeviceBindingRevision( `uint64_t` input_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to query.

    Get's the major and minor device binding revisions for Steam Input API configurations. Minor revisions are for small changes such as adding a new option action or updating localization in the configuration.

    When updating a Minor revision only one new configuration needs to be update with the "Use Action Block" flag set. Major revisions are to be used when changing the number of action sets or otherwise reworking configurations to the degree that older configurations are no longer usable. When a user's binding disagree's with the major revision of the current official configuration Steam will forcibly update the user to the new configuration. New configurations will need to be made for every controller when updating the Major revision.

	!!! returns "Returns: array"
		Contains a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| major | int | Major binding revision.
		| minor | int | Minor binding revision.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetDeviceBindingRevision){ .md-button .md-button--store target="_blank" }

### getDigitalActionData

!!! function "getDigitalActionData( `uint64_t` input_handle, `uint64_t` digital_action_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to query.
    | digital_action_handle | uint64_t | The handle of the digital action you want to query.

    Returns the current state of the supplied digital game action.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| state | bool | The current state of this action; true if the action is currently pressed, otherwise false.
		| active | bool | Whether or not this action is currently available to be bound in the active action set.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetDigitalActionData){ .md-button .md-button--store target="_blank" }

### getDigitalActionHandle

!!! function "getDigitalActionHandle( `string` action_name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | action_name | string | The string identifier of the digital action defined in the game's VDF file.

    Look up the handle for a digital action. Best to do this once on startup and store the handles for all future API calls.

	!!! returns "Returns: uint64_t"
		The handle of the specified digital action.

	!!! info "Notes"
		This function does not take an action set handle parameter. That means that each action in your VDF file must have a unique string identifier. In other words, if you use an action called "up" in two different action sets, this function will only ever return one of them and the other will be ignored.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetDigitalActionHandle){ .md-button .md-button--store target="_blank" }
	
 
### getDigitalActionOrigins

!!! function "getDigitalActionOrigins( `uint64_t` input_handle, `uint64_t` action_set_handle, `uint64_t` digital_action_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to query.
    | action_set_handle | uint64_t | The handle of the action set you want to query.
    | digital_action_handle | uint64_t | The handle of the digital action you want to query.

	Get the origin(s) for a digital action within an action set by filling return array with input handles. Use this to display the appropriate on-screen prompt for the action.

	!!! returns "Returns: array"
		Contains a list of [InputActionOrigin enums](#inputactionorigin).

		The [InputActionOrigin enum](#inputactionorigin) will get extended as support for new controller controllers gets added to the Steam client and will exceed the values from this header, please check bounds if you are using a look up table.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetDigitalActionOrigins){ .md-button .md-button--store target="_blank" }

### getGamepadIndexForController

!!! function "getGamepadIndexForController( `uint64_t` input_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to get a gamepad index for.

    Returns the associated gamepad index for the specified controller, if emulating a gamepad or -1 if not associated with an Xinput index.

	!!! returns "Returns: int"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetGamepadIndexForController){ .md-button .md-button--store target="_blank" }

### getGlyphForActionOrigin

!!! function "getGlyphForActionOrigin( `InputActionOrigin` origin )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | origin | [InputActionOrigin enum](#inputactionorigin) | The origin to get the glyph for.

    Get a local path to an older, Big Picture Mode-style PNG file for a particular origin

	!!! returns "Returns: string"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetGlyphForActionOrigin){ .md-button .md-button--store target="_blank" }

### getGlyphForXboxOrigin

!!! function "getGlyphForXboxOrigin( `XboxOrigin` origin )"
	Get a local path to art for on-screen glyph for a particular XBox controller origin.

	!!! returns "Returns: string"

### getGlyphPNGForActionOrigin

!!! function "getGlyphPNGForActionOrigin( `InputActionOrigin` origin, `InputGlyphSize` size, `uint32_t` flags )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | origin | [InputActionOrigin enum](#inputactionorigin) | The button glyph to get.
    | size | [InputGlyphSize enum](#inputglyphsize) | The size of the glyph.
    | flags | flags | No specifications; may be [InputGlyphStyle enum.](#inputglyphstyle)

    Get a local path to a PNG file for the provided origin's glyph.

	!!! returns "Returns: string"

### getGlyphSVGForActionOrigin

!!! function "getGlyphSVGForActionOrigin( `InputActionOrigin` origin, `uint32_t` flags )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | origin | [InputActionOrigin enum](#inputactionorigin) | The button SVG glyph to get.
    | flags | uint32_t | No specifications; may be [InputGlyphStyle enum.](#inputglyphstyle)

    Get a local path to a SVG file for the provided origin's glyph.

	!!! returns "Returns: string"

### getInputTypeForHandle

!!! function "getInputTypeForHandle( `uint64_t` input_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller whose input type (device model) you want to query

    Returns the input type for a particular handle; unlike [InputActionOrigin](#inputactionorigin) which update with Steam and may return unrecognized values. [InputType](#inputtype) will remain static and only return valid values from your SDK version.

	This tells you if a given controller is a Steam controller, Xbox 360 controller, PS4 controller, etc. XBox Series controllers are called XBox One.

	!!! returns "Returns: [InputType enum](#inputtype)"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetInputTypeForHandle){ .md-button .md-button--store target="_blank" }
 
### getMotionData

!!! function "getMotionData( `uint64_t` input_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to get motion data for.

    Returns raw motion data for the specified controller.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| rot_quat_x | float | Sensor-fused absolute rotation (will drift in heading), x axis.
		| rot_quat_y | float | Sensor-fused absolute rotation (will drift in heading), y axis.
		| rot_quat_z | float | Sensor-fused absolute rotation (will drift in heading), z axis.
		| rot_quat_w | float | Sensor-fused absolute rotation (will drift in heading), w axis.
		| pos_accel_x | float | Positional acceleration, x axis.
		| pos_accel_y | float | Positional acceleration, y axis.
		| pos_accel_z | float | Positional acceleration, z axis.
		| rot_vel_x | float | Angular velocity, x axis.
		| rot_vel_y | float | Angular velocity, y axis.
		| rot_vel_z | float | Angular velocity, z axis.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetMotionData){ .md-button .md-button--store target="_blank" }
 
### getRemotePlaySessionID

!!! function "getRemotePlaySessionID( `uint64_t` input_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to query.

    Get the Steam Remote Play session ID associated with a device or 0 if there is no session associated with it. See isteamremoteplay.h for more information on Steam Remote Play sessions.

	!!! returns "Returns: uint32_t"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetRemotePlaySessionID){ .md-button .md-button--store target="_blank" }

### getSessionInputConfigurationSettings

!!! function "getSessionInputConfigurationSettings( )"
	Get a bitmask of the Steam Input Configuration types opted in for the current session.

	!!! returns "Returns: [InputConfigurationEnableType enum](#inputconfigurationenabletype)"

	!!! info "Notes"
        User can override the settings from the Steamworks Partner site so the returned values may not exactly match your default configuration.

### getStringforActionOrigin

!!! function "getStringforActionOrigin( `InputActionOrigin` origin )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | origin | [InputActionOrigin enum](#inputactionorigin) | The origin to get the string name for.

    Returns a localized string (from Steam's InputGlyphStyle setting) for the specified origin.

	!!! returns "Returns: string"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#GetStringForActionOrigin){ .md-button .md-button--store target="_blank" }

### getStringForAnalogActionName

!!! function "getStringForAnalogActionName( `uint64_t` action_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | action_handle | uint64_t | The action handle to get the name for.

    Returns a localized string (from Steam's language setting) for the user-facing action name corresponding to the specified handle.

	!!! returns "Returns: string"

### getStringForDigitalActionName

!!! function "getStringForDigitalActionName( `uint64_t` action_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | action_handle | uint64_t | The action handle to get the name for.

    Returns a localized string (from Steam's language setting) for the user-facing action name corresponding to the specified handle.

	!!! returns "Returns: string"

### getStringForXboxOrigin

!!! function "String getStringForXboxOrigin( `XboxOrigin` origin )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | origin | [XboxOrigin enum](#xboxorigin) | -

    Returns a localized string (from Steam's language setting) for the specified Xbox controller origin.

	!!! returns "Returns: string"

### inputInit

!!! function "inputInit( `bool` explicitly_call_runframe )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | explicitly_call_runframe | bool | If false then you will need to manually call [runFrame}(#runframe).

    [inputInit](#inputinit) and [inputShutdown](#inputshutdown) must be called when starting/ending use of this interface.

	If **explicitly_call_runframe** is called then you will need to manually call [runFrame](#runframe) each frame, otherwise Steam Input will updated when [run_callbacks](main.md#run_callbacks) is called.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#Init){ .md-button .md-button--store target="_blank" }

### inputShutdown

!!! function "inputShutdown( )"
	[inputInit](#inputinit) and [inputShutdown](#inputshutdown) must be called when starting/ending use of this interface.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#Shutdown){ .md-button .md-button--store target="_blank" }

### newDataAvailable

!!! function "newDataAvailable( )"
	Returns true if new data has been received since the last time action data was accessed via [getDigitalActionData](#getdigitalactiondata) or [getAnalogActionData](#getanalogactiondata). The game will still need to call [runFrame](#runframe) or [run_callbacks](main.md#run_callbacks) before this to update the data stream.

	!!! returns "Returns: bool"

### runFrame

!!! function "runFrame( )"
	Synchronize API state with the latest Steam Controller inputs available. This is performed automatically by [run_callbacks](main.md#run_callbacks), but for the absolute lowest possible latency, you can call this directly before reading controller state.

	This must be called from somewhere before [getConnectedControllers](#getconnectedcontrollers) will return any handles.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#RunFrame){ .md-button .md-button--store target="_blank" }
 
### setDualSenseTriggerEffect

!!! function "setDualSenseTriggerEffect( `uint64_t` input_handle, `int` parameter_index, `int` trigger_mask, `SCEPadTriggerEffectMode` effect_mode, `int` position, `int` amplitude, `int` frequency )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect.
    | parameter_index | int | -
    | trigger_mask | [SCEPadTriggerEffectMode enum](#scepadtriggereffectmode) | 
    | effect_mode | int | -
    | position | int | Position where the motor arm start vibrating(0~9).
    | amplitude | int | Vibration amplitude(0~8 (0: Same as Off mode)).
    | frequency | int | Vibration frequency(0~255[Hz] (0: Same as Off mode)).

    Set the trigger effect for a DualSense controller.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#SetDualSenseTriggerEffect){ .md-button .md-button--store target="_blank" }
    [ :material-tag-remove: Removed GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### setInputActionManifestFilePath

!!! function "setInputActionManifestFilePath( `string` manifest_path )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | manifest_path | string | The absolute path to the Input Action Manifest.

    Set the absolute path to the Input Action Manifest file containing the in-game actions and file paths to the official configurations. Used in games that bundle Steam Input configurations inside of the game depot instead of using the Steam Workshop.

    !!! returns "Returns: bool"
 
### setLEDColor

!!! function "setLEDColor( `uint64_t` input_handle, `int` color_r, `int` color_g, `int` color_b, `int` flags )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect.
    | color_r | int | The red component of the color to set (0-255).
    | color_g | int | The green component of the color to set (0-255).
    | color_b | int | The blue component of the color to set (0-255).
    | flags | int | Bitmask of values from [InputLEDFlag](#inputledflag).

    Set the controller LED color on supported controllers.

	!!! returns "Returns: void"

	!!! info "Notes"
		The Steam Controller does not support any color but white, and will interpret the RGB values as a greyscale value affecting the brightness of the Steam button LED. The DualShock 4 responds to full color information and uses the values to set the color and brightness of the lightbar.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#SetLEDColor){ .md-button .md-button--store target="_blank" }
 
### showBindingPanel

!!! function "showBindingPanel( `uint64_t` input_handle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller you want to bring up the binding screen for.

    Invokes the Steam overlay and brings up the binding screen.

	!!! returns "Returns: bool"
		Returns true for success; false if overlay is disabled / unavailable. If the player is using Big Picture Mode the configuration will open in the overlay. In desktop mode a pop-up window version of Big Picture will be created and open the configuration.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#ShowBindingPanel){ .md-button .md-button--store target="_blank" }

### stopAnalogActionMomentum

!!! function "stopAnalogActionMomentum( `uint64_t` input_handle, `uint64_t` action )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect.
    | action | uint64_t | The analog action to stop momentum for.

    Stop analog momentum for the action if it is a mouse action in trackball mode.

    This will also stop all associated haptics. This is useful for situations where you want to indicate to the user that the limit of an action has been reached, such as spinning a carousel or scrolling a webpage.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#StopAnalogActionMomentum){ .md-button .md-button--store target="_blank" }

### translateActionOrigin

!!! function "translateActionOrigin( `InputType` destination_input, `InputActionOrigin` source_origin )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | destination_input | [InputType enum](#inputtype) | The controller type you want to translate to. Steam will pick the closest type from your SDK version if [INPUT_TYPE_UNKNOWN](#inputtype) is used.
    | source_origin | [InputActionOrigin enum](#inputactionorigin) | This is the button you want to translate.

    Get the equivalent origin for a given controller type or the closest controller type that existed in the SDK you built into your game if **destination_input**is 0. This action origin can be used in your glyph look up table or passed into [getGlyphForActionOrigin](#getglyphforactionorigin) or [getStringForActionOrigin](#getstringforactionorigin).

	!!! returns "Returns: [InputActionOrigin enum](#inputactionorigin)"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#TranslateActionOrigin){ .md-button .md-button--store target="_blank" }

### triggerHapticPulse

!!! function "triggerHapticPulse( `uint64_t` input_handle, `int` target_pad, `int` duration )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect.
    | target_pad | [ControllerPad enum](#controllerpad) | Which haptic touchpad to affect.
    | duration | int | Duration of the pulse, in microseconds (1/1,000,000th of a second).

	Trigger a haptic pulse on a Steam Controller; if you are approximating rumble you may want to use [triggerVibration](#triggervibration) instead.  Good uses for Haptic pulses include chimes, noises, or directional gameplay feedback (taking damage, footstep locations, etc).

	Currently only the Steam Controller supports haptic pulses. This API call will be ignored for all other controller models.

	The typical max value of an unsigned short is 65535, which means the longest haptic pulse you can trigger with this method has a duration of 0.065535 seconds (ie, less than 1/10th of a second).

	This function should be thought of as a low-level primitive meant to be repeatedly used in higher-level user functions to generate more sophisticated behavior.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#TriggerHapticPulse){ .md-button .md-button--store target="_blank" }
 
### triggerRepeatedHapticPulse

!!! function "triggerRepeatedHapticPulse( `uint64_t` input_handle, `ControllerPad` target_pad, `int` duration, `int` offset, `int` repeat, `int` flags )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect.
    | target_pad | [ControllerPad enum](#controllerpad) | Which haptic touchpad to affect.
    | duration | int | Duration of the pulse, in microseconds (1/1,000,000th of a second).
    | offset | int | Duration of the pause between pulses, in microseconds.
    | repeat | int | Number of times to repeat the **duration** / **offset** duty cycle.
    | flags | int | Currently unused and reserved for future use.

    Trigger a haptic pulse with a duty cycle of **duration** / **offset** **repeat** times. If you are approximating rumble you may want to use [triggerVibration](#triggervibration) instead.

    Currently only the Steam Controller, Steam Deck, and Nintendo Switch Pro Controller supports haptic pulses. This API call will be ignored for incompatible controller models.

    This is a more user-friendly function to call than [triggerHapticPulse](#triggerhapticpulse) as it can generate pulse patterns long enough to be actually noticed by the user. Changing the **duration** and **offset** parameters will change the "texture" of the haptic pulse.

	!!! returns "Returns: void"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#TriggerRepeatedHapticPulse){ .md-button .md-button--store target="_blank" }

### triggerSimpleHapticEvent

!!! function "triggerSimpleHapticEvent( `uint64_t` input_handle, `ControllerHapticLocation` haptic_location, `uint8_t` intensity, `string` gain_db, `uint8_t` other_intensity, `string` other_gain_db )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect.
    | haptic_location | [ControllerHapticLocation enum](#controllerhapticlocation) | -
    | intensity | uint8_t | -
    | gain_db | string | -
    | other_intensity | uint8_t | -
    | other_gain_db | string | -

    Send a haptic pulse, works on Steam Deck and Steam Controller devices

	!!! returns "Returns: void"

### triggerVibration

!!! function "triggerVibration( `uint64_t` input_handle, `uint16_t` left_speed, `uint16_t` right_speed )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect.
    | left_speed | uint16_t | The intensity value for the left rumble motor.
    | right_speed | uint16_t | The intensity value of the right rumble motor.

    Trigger a vibration event on supported controllers. Steam will translate these commands into haptic pulses for Steam Controllers.

	!!! returns "Returns: void"

	!!! info "Notes"
		This API call will be ignored for incompatible controller models. This generates the traditional "rumble" vibration effect. The Steam Controller will emulate traditional rumble using its haptics.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#TriggerVibration){ .md-button .md-button--store target="_blank" }

### triggerVibrationExtended

!!! function "triggerVibrationExtended( `uint64_t` input_handle, `uint16_t` left_speed, `uint16_t` right_speed, `uint16_t` left_trigger_speed, `uint16_t` right_trigger_speed )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | input_handle | uint64_t | The handle of the controller to affect.
    | left_speed | uint16_t | The intensity value for the left rumble motor.
    | right_speed | uint16_t | The intensity value of the right rumble motor.
    | left_trigger_speed | uint16_t | The intensity value for the left Xbox Impulse Trigger motor.
    | right_trigger_speed | uint16_t | The intensity value of the right Xbox Impulse Trigger motor.

    Trigger a vibration event on supported controllers including Xbox trigger impulse rumble. Steam will translate these commands into haptic pulses for Steam Controllers.

    On Windows support for Xbox Impulse Trigger motor values requires user installation of the Xbox Extended Feature support driver. The Steam Controller and Steam Deck will emulate traditional rumble using their haptics.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/isteaminput#TriggerVibrationExtended){ .md-button .md-button--store target="_blank" }

### waitForData

!!! function "waitForData( `bool` wait_forever, `uint32_t` timeout )"
	Waits on an IPC event from Steam sent when there is new data to be fetched from the data drop. Returns true when data was received before the timeout expires. Useful for games with a dedicated input thread.

	!!! returns "Returns: bool"

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### input_configuration_loaded

!!! function "input_configuration_loaded"
	Called when a controller configuration has been loaded, will fire once per controller per focus change for Steam Input enabled controllers.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| app_id | uint32_t | The app ID to get the configuration for.
		| device_handle | uint64_t | Handle for device.
		| config_data | dictionary | Data for the configuration.

		**config_data** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| mapping_creator | uint64_t | May differ from local user when using an unmodified community or official config.
		| major_revision | uint32 | Binding revision from In-game Action File.  Same value as queried by [getDeviceBindingRevision](#getdevicebindingrevision).
		| minor_revision | uint32 | Binding revision from In-game Action File.  Same value as queried by [getDeviceBindingRevision](#getdevicebindingrevision).
		| uses_steam_input_api | bool | Does the configuration contain any Analog / Digital actions?
		| uses_gamepad_api | bool | Does the configuration contain any Xinput bindings?

### input_device_connected

!!! function "input_device_connected"
	Called when a new controller has been connected, will fire once per controller if multiple new controllers connect in the same frame.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| input_handle | uint64_t | Handle for device.

### input_device_disconnected

!!! function "input_device_disconnected"
	Called when a new controller has been disconnected, will fire once per controller if multiple new controllers disconnect in the same frame.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| input_handle | uint64_t | Handle for device.

### input_gamepad_slot_change

!!! function "input_gamepad_slot_change"
	Called when controller gamepad slots change. On Linux / macOS these slots are shared for all running apps.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| app_id | uint32_t | The app ID for the slot change.
		| device_handle | uint64_t | andle for device
		| device_type | [InputType enum](#inputtype) | Type of device
		| old_gamepad_slot | int | Previous gamepad slot; can be -1 controller doesn't uses gamepad bindings.
		| new_gamepad_slot | int | New gamepad slot; can be -1 controller doesn't uses gamepad bindings

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
INPUT_HANDLE_ALL_CONTROLLERS | STEAM_INPUT_HANDLE_ALL_CONTROLLERS | UINT64_MAX | When sending an option to a specific controller handle, you can use this special value in the place of a handle to send the option to all controllers instead.
INPUT_MAX_ACTIVE_LAYERS | STEAM_INPUT_MAX_ACTIVE_LAYERS | 16 | -
INPUT_MAX_ANALOG_ACTIONS | STEAM_INPUT_MAX_ANALOG_ACTIONS | 24 | The maximum number of analog actions that can be performed on each controller.
INPUT_MAX_ANALOG_ACTION_DATA | STEAM_INPUT_MAX_ANALOG_ACTION_DATA | 1.0f | The maximum value that can be reported by an analog action on any given axis.
INPUT_MAX_COUNT | STEAM_INPUT_MAX_COUNT | 16 | The maximum number of controllers that can be used simultaneously with the Steam Input Configurator.
INPUT_MAX_DIGITAL_ACTIONS | STEAM_INPUT_MAX_DIGITAL_ACTIONS | 256 | The maximum number of digital actions that can be performed on each controller.
INPUT_MAX_ORIGINS | STEAM_INPUT_MAX_ORIGINS | 8 | The maximum number of input origins that can be attached to a single action.
INPUT_MIN_ANALOG_ACTION_DATA | STEAM_INPUT_MIN_ANALOG_ACTION_DATA | -1.0f | The minimum value that can be reported by an analog action on any given axis.
SONY_PAD_TRIGGER_EFFECT_CONTROL_POINT_NUM | SCE_PAD_TRIGGER_EFFECT_CONTROL_POINT_NUM | 10 | -
SONY_PAD_TRIGGER_EFFECT_PARAM_INDEX_FOR_L2 | SCE_PAD_TRIGGER_EFFECT_PARAM_INDEX_FOR_L2 | 0 | -
SONY_PAD_TRIGGER_EFFECT_PARAM_INDEX_FOR_R2 | SCE_PAD_TRIGGER_EFFECT_PARAM_INDEX_FOR_R2 | 1 | -
SONY_PAD_TRIGGER_EFFECT_TRIGGER_MASK_L2 | SCE_PAD_TRIGGER_EFFECT_TRIGGER_MASK_L2 | 0x01 | -
SONY_PAD_TRIGGER_EFFECT_TRIGGER_MASK_R2 | SCE_PAD_TRIGGER_EFFECT_TRIGGER_MASK_R2 | 0x02 | -
SONY_PAD_TRIGGER_EFFECT_TRIGGER_NUM | SCE_PAD_TRIGGER_EFFECT_TRIGGER_NUM | 2 | -

{==
## :material-numeric: Enums
==}

### controllerHapticLocation

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CONTROLLER_HAPTIC_LOCATION_LEFT | k_EControllerHapticLocation_Left | ( 1 << k_ESteamControllerPad_Left ) | -
CONTROLLER_HAPTIC_LOCATION_RIGHT | k_EControllerHapticLocation_Right | ( 1 << k_ESteamControllerPad_Right ) | -
CONTROLLER_HAPTIC_LOCATION_BOTH | k_EControllerHapticLocation_Both | ( 1 << k_ESteamControllerPad_Left | 1 << k_ESteamControllerPad_Right ) | -

### ControllerHapticType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
CONTROLLER_HAPTIC_TYPE_OFF | k_EControllerHapticType_Off | 0 | -
CONTROLLER_HAPTIC_TYPE_TICK | k_EControllerHapticType_Tick | 1 | -
CONTROLLER_HAPTIC_TYPE_CLICK | k_EControllerHapticType_Click | 2 | -

### ControllerPad

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
STEAM_CONTROLLER_PAD_LEFT | k_ESteamControllerPad_Left | 0 | The left touchpad region on a Steam Controller Device. Compatible models: Steam Controller, DualShock 4.
STEAM_CONTROLLER_PAD_RIGHT | k_ESteamControllerPad_Right | 1 | The right region on a Steam Controller Device. Compatible models: Steam Controller, DualShock 4.

### InputActionEventType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
INPUT_ACTION_EVENT_TYPE_DIGITAL_ACTION | ESteamInputActionEventType_DigitalAction | 0 | -
INPUT_ACTION_EVENT_TYPE_ANALOG_ACTION | ESteamInputActionEventType_AnalogAction | 1 | -

### InputActionOrigin
Please do not use action origins as a way to identify controller types. There is no guarantee that they will be added in a contiguous manner - use [getInputTypeForHandle](#getinputtypeforhandle) instead. Versions of Steam that add new controller types in the future will extend this enum so if you're using a lookup table please check the bounds of any origins returned by Steam.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
INPUT_ACTION_ORIGIN_NONE | k_EInputActionOrigin_None | 0 | -
INPUT_ACTION_ORIGIN_A | k_EInputActionOrigin_A | 1 | (Steam Controller) digital face button A
INPUT_ACTION_ORIGIN_B | k_EInputActionOrigin_B | 2 | (Steam Controller) digital face button B
INPUT_ACTION_ORIGIN_X | k_EInputActionOrigin_X | 3 | (Steam Controller) digital face button X
INPUT_ACTION_ORIGIN_Y | k_EInputActionOrigin_Y | 4 | (Steam Controller) digital face button Y
INPUT_ACTION_ORIGIN_LEFT_BUMPER | k_EInputActionOrigin_LeftBumper | 5 | (Steam Controller) digital left shoulder button (aka left bumper)
INPUT_ACTION_ORIGIN_RIGHT_BUMPER | k_EInputActionOrigin_RightBumper | 6 | (Steam Controller) digital right shoulder button (aka right bumper)
INPUT_ACTION_ORIGIN_LEFTGRIP | k_EInputActionOrigin_LeftGrip | 7 | (Steam Controller) digital left grip paddle
INPUT_ACTION_ORIGIN_RIGHTGRIP | k_EInputActionOrigin_RightGrip | 8 | (Steam Controller) digital right grip paddle
INPUT_ACTION_ORIGIN_START | k_EInputActionOrigin_Start | 9 | (Steam Controller) digital start button
INPUT_ACTION_ORIGIN_BACK | k_EInputActionOrigin_Back | 10 | (Steam Controller) digital back button
INPUT_ACTION_ORIGIN_LEFT_PAD_TOUCH | k_EInputActionOrigin_LeftPad_Touch | 11 | (Steam Controller) left haptic touchpad, in simple contact with a finger
INPUT_ACTION_ORIGIN_LEFT_PAD_SWIPE | k_EInputActionOrigin_LeftPad_Swipe | 12 | (Steam Controller) left haptic touchpad, touch input on any axis
INPUT_ACTION_ORIGIN_LEFT_PAD_CLICK | k_EInputActionOrigin_LeftPad_Click | 13 | (Steam Controller) left haptic touchpad, digital click (for the whole thing)
INPUT_ACTION_ORIGIN_LEFT_PAD_DPAD_NORTH | k_EInputActionOrigin_LeftPad_DPadNorth | 14 | (Steam Controller) left haptic touchpad, digital click (upper quadrant)
INPUT_ACTION_ORIGIN_LEFT_PAD_DPAD_SOUTH | k_EInputActionOrigin_LeftPad_DPadSouth | 15 | (Steam Controller) left haptic touchpad, digital click (lower quadrant)
INPUT_ACTION_ORIGIN_LEFT_PAD_DPAD_WEST | k_EInputActionOrigin_LeftPad_DPadWest | 16 | (Steam Controller) left haptic touchpad, digital click (left quadrant)
INPUT_ACTION_ORIGIN_LEFT_PAD_DPAD_EAST | k_EInputActionOrigin_LeftPad_DPadEast | 17 | (Steam Controller) left haptic touchpad, digital click (right quadrant)
INPUT_ACTION_ORIGIN_RIGHT_PAD_TOUCH | k_EInputActionOrigin_RightPad_Touch | 18 | (Steam Controller) right haptic touchpad, in simple contact with a finger
INPUT_ACTION_ORIGIN_RIGHT_PAD_SWIPE | k_EInputActionOrigin_RightPad_Swipe | 19 | (Steam Controller) right haptic touchpad, touch input on any axis
INPUT_ACTION_ORIGIN_RIGHT_PAD_CLICK | k_EInputActionOrigin_RightPad_Click | 20 | (Steam Controller) right haptic touchpad, digital click (for the whole thing)
INPUT_ACTION_ORIGIN_RIGHT_PAD_DPAD_NORTH | k_EInputActionOrigin_RightPad_DPadNorth | 21 | (Steam Controller) right haptic touchpad, digital click (upper quadrant)
INPUT_ACTION_ORIGIN_RIGHT_PAD_DPAD_SOUTH | k_EInputActionOrigin_RightPad_DPadSouth | 22 | (Steam Controller) right haptic touchpad, digital click (lower quadrant)
INPUT_ACTION_ORIGIN_RIGHT_PAD_DPAD_WEST | k_EInputActionOrigin_RightPad_DPadWest | 23 | (Steam Controller) right haptic touchpad, digital click (left quadrant)
INPUT_ACTION_ORIGIN_RIGHT_PAD_DPAD_EAST | k_EInputActionOrigin_RightPad_DPadEast | 24 | (Steam Controller) right haptic touchpad, digital click (right quadrant)
INPUT_ACTION_ORIGIN_LEFT_TRIGGER_PULL | k_EInputActionOrigin_LeftTrigger_Pull | 25 | (Steam Controller) left analog trigger, pulled by any amount (analog value)
INPUT_ACTION_ORIGIN_LEFT_TRIGGER_CLICK | k_EInputActionOrigin_LeftTrigger_Click | 26 | (Steam Controller) left analog trigger, pulled in all the way (digital value)
INPUT_ACTION_ORIGIN_RIGHT_TRIGGER_PULL | k_EInputActionOrigin_RightTrigger_Pull | 27 | (Steam Controller) right analog trigger, pulled by any amount (analog value)
INPUT_ACTION_ORIGIN_RIGHT_TRIGGER_CLICK | k_EInputActionOrigin_RightTrigger_Click | 28 | (Steam Controller) right analog trigger, pulled in all the way (digital value)
INPUT_ACTION_ORIGIN_LEFT_STICK_MOVE | k_EInputActionOrigin_LeftStick_Move | 29 | (Steam Controller) left joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_LEFT_STICK_CLICK | k_EInputActionOrigin_LeftStick_Click | 30 | (Steam Controller) left joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_LEFT_STICK_DPAD_NORTH | k_EInputActionOrigin_LeftStick_DPadNorth | 31 | (Steam Controller) left joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_LEFT_STICK_DPAD_SOUTH | k_EInputActionOrigin_LeftStick_DPadSouth | 32 | (Steam Controller) left joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_LEFT_STICK_DPAD_WEST | k_EInputActionOrigin_LeftStick_DPadWest | 33 | (Steam Controller) left joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_LEFT_STICK_DPAD_EAST | k_EInputActionOrigin_LeftStick_DPadEast | 34 | (Steam Controller) left joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_GYRO_MOVE | k_EInputActionOrigin_Gyro_Move | 35 | (Steam Controller) gyroscope, analog movement in any axis
INPUT_ACTION_ORIGIN_GYRO_PITCH | k_EInputActionOrigin_Gyro_Pitch | 36 | (Steam Controller) gyroscope, analog movement on the Pitch axis (point head up to ceiling, point head down to floor)
INPUT_ACTION_ORIGIN_GYRO_YAW | k_EInputActionOrigin_Gyro_Yaw | 37 | (Steam Controller) gyroscope, analog movement on the Yaw axis (turn head left to face one wall, turn head right to face other)
INPUT_ACTION_ORIGIN_GYRO_ROLL | k_EInputActionOrigin_Gyro_Roll | 38 | (Steam Controller) gyroscope, analog movement on the Roll axis (tilt head left towards shoulder, tilt head right towards other)
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED0 | k_EInputActionOrigin_SteamController_Reserved0 | 39 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED1 | k_EInputActionOrigin_SteamController_Reserved1 | 40 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED2 | k_EInputActionOrigin_SteamController_Reserved2 | 41 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED3 | k_EInputActionOrigin_SteamController_Reserved3 | 42 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED4 | k_EInputActionOrigin_SteamController_Reserved4 | 43 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED5 | k_EInputActionOrigin_SteamController_Reserved5 | 44 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED6 | k_EInputActionOrigin_SteamController_Reserved6 | 45 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED7 | k_EInputActionOrigin_SteamController_Reserved7 | 46 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED8 | k_EInputActionOrigin_SteamController_Reserved8 | 47 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED9 | k_EInputActionOrigin_SteamController_Reserved9 | 48 | Reserved for future use
INPUT_ACTION_ORIGIN_STEAM_CONTROLLER_RESERVED10 | k_EInputActionOrigin_SteamController_Reserved10 | 49 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_X | k_EInputActionOrigin_PS4_X | 50 | (Sony Dualshock 4) digital face button X
INPUT_ACTION_ORIGIN_PS4_CIRCLE | k_EInputActionOrigin_PS4_Circle | 51 | (Sony Dualshock 4) digital face button Circle
INPUT_ACTION_ORIGIN_PS4_TRIANGLE | k_EInputActionOrigin_PS4_Triangle | 52 | (Sony Dualshock 4) digital face button Triangle
INPUT_ACTION_ORIGIN_PS4_SQUARE | k_EInputActionOrigin_PS4_Square | 53 | (Sony Dualshock 4) digital face button Square
INPUT_ACTION_ORIGIN_PS4_LEFT_BUMPER | k_EInputActionOrigin_PS4_LeftBumper | 54 | (Sony Dualshock 4) digital left shoulder button (aka left bumper)
INPUT_ACTION_ORIGIN_PS4_RIGHT_BUMPER | k_EInputActionOrigin_PS4_RightBumper | 55 | (Sony Dualshock 4) digital right shoulder button (aka right bumper)
INPUT_ACTION_ORIGIN_PS4_OPTIONS | k_EInputActionOrigin_PS4_Options | 56 | (Sony Dualshock 4) digital options button (aka Start)
INPUT_ACTION_ORIGIN_PS4_SHARE | k_EInputActionOrigin_PS4_Share | 57 | (Sony Dualshock 4) digital share button (aka Back)
INPUT_ACTION_ORIGIN_PS4_LEFT_PAD_TOUCH | k_EInputActionOrigin_PS4_LeftPad_Touch | 58 | (Sony Dualshock 4) left half of the touchpad, in simple contact with a finger
INPUT_ACTION_ORIGIN_PS4_LEFT_PAD_SWIPE | k_EInputActionOrigin_PS4_LeftPad_Swipe | 59 | (Sony Dualshock 4) left half of the touchpad, touch input on any axis
INPUT_ACTION_ORIGIN_PS4_LEFT_PAD_CLICK | k_EInputActionOrigin_PS4_LeftPad_Click | 60 | (Sony Dualshock 4) left half of the touchpad, digital click (for the whole thing)
INPUT_ACTION_ORIGIN_PS4_LEFT_PAD_DPAD_NORTH | k_EInputActionOrigin_PS4_LeftPad_DPadNorth | 61 | (Sony Dualshock 4) left half of the touchpad, digital click (upper quadrant)
INPUT_ACTION_ORIGIN_PS4_LEFT_PAD_DPAD_SOUTH | k_EInputActionOrigin_PS4_LeftPad_DPadSouth | 62 | (Sony Dualshock 4) left half of the touchpad, digital click (lower quadrant)
INPUT_ACTION_ORIGIN_PS4_LEFT_PAD_DPAD_WEST | k_EInputActionOrigin_PS4_LeftPad_DPadWest | 63 | (Sony Dualshock 4) left half of the touchpad, digital click (left quadrant)
INPUT_ACTION_ORIGIN_PS4_LEFT_PAD_DPAD_EAST | k_EInputActionOrigin_PS4_LeftPad_DPadEast | 64 | (Sony Dualshock 4) left half of the touchpad, digital click (right quadrant)
INPUT_ACTION_ORIGIN_PS4_RIGHT_PAD_TOUCH | k_EInputActionOrigin_PS4_RightPad_Touch | 65 | (Sony Dualshock 4) right half of the touchpad, in simple contact with a finger
INPUT_ACTION_ORIGIN_PS4_RIGHT_PAD_SWIPE | k_EInputActionOrigin_PS4_RightPad_Swipe | 66 | (Sony Dualshock 4) right half of the touchpad, touch input on any axis
INPUT_ACTION_ORIGIN_PS4_RIGHT_PAD_CLICK | k_EInputActionOrigin_PS4_RightPad_Click | 67 | (Sony Dualshock 4) right half of the touchpad, digital click (for the whole thing)
INPUT_ACTION_ORIGIN_PS4_RIGHT_PAD_DPAD_NORTH | k_EInputActionOrigin_PS4_RightPad_DPadNorth | 68 | (Sony Dualshock 4) right half of the touchpad, digital click (upper quadrant)
INPUT_ACTION_ORIGIN_PS4_RIGHT_PAD_DPAD_SOUTH | k_EInputActionOrigin_PS4_RightPad_DPadSouth | 69 | (Sony Dualshock 4) right half of the touchpad, digital click (lower quadrant)
INPUT_ACTION_ORIGIN_PS4_RIGHT_PAD_DPAD_WEST | k_EInputActionOrigin_PS4_RightPad_DPadWest | 70 | (Sony Dualshock 4) right half of the touchpad, digital click (left quadrant)
INPUT_ACTION_ORIGIN_PS4_RIGHT_PAD_DPAD_EAST | k_EInputActionOrigin_PS4_RightPad_DPadEast | 71 | (Sony Dualshock 4) right half of the touchpad, digital click (right quadrant)
INPUT_ACTION_ORIGIN_PS4_CENTER_PAD_TOUCH | k_EInputActionOrigin_PS4_CenterPad_Touch | 72 | (Sony Dualshock 4) unified touchpad, in simple contact with a finger
INPUT_ACTION_ORIGIN_PS4_CENTER_PAD_SWIPE | k_EInputActionOrigin_PS4_CenterPad_Swipe | 73 | (Sony Dualshock 4) unified touchpad, touch input on any axis
INPUT_ACTION_ORIGIN_PS4_CENTER_PAD_CLICK | k_EInputActionOrigin_PS4_CenterPad_Click | 74 | (Sony Dualshock 4) unified touchpad, digital click (for the whole thing)
INPUT_ACTION_ORIGIN_PS4_CENTER_PAD_DPAD_NORTH | k_EInputActionOrigin_PS4_CenterPad_DPadNorth | 75 | (Sony Dualshock 4) unified touchpad, digital click (upper quadrant)
INPUT_ACTION_ORIGIN_PS4_CENTER_PAD_DPAD_SOUTH | k_EInputActionOrigin_PS4_CenterPad_DPadSouth | 76 | (Sony Dualshock 4) unified touchpad, digital click (lower quadrant)
INPUT_ACTION_ORIGIN_PS4_CENTER_PAD_DPAD_WEST | k_EInputActionOrigin_PS4_CenterPad_DPadWest | 77 | (Sony Dualshock 4) unified touchpad, digital click (left quadrant)
INPUT_ACTION_ORIGIN_PS4_CENTER_PAD_DPAD_EAST | k_EInputActionOrigin_PS4_CenterPad_DPadEast | 78 | (Sony Dualshock 4) unified touchpad, digital click (right quadrant)
INPUT_ACTION_ORIGIN_PS4_LEFT_TRIGGER_PULL | k_EInputActionOrigin_PS4_LeftTrigger_Pull | 79 | (Sony Dualshock 4) left analog trigger, pulled by any amount (analog value)
INPUT_ACTION_ORIGIN_PS4_LEFT_TRIGGER_CLICK | k_EInputActionOrigin_PS4_LeftTrigger_Click | 80 | (Sony Dualshock 4) left analog trigger, pulled in all the way (digital value)
INPUT_ACTION_ORIGIN_PS4_RIGHT_TRIGGER_PULL | k_EInputActionOrigin_PS4_RightTrigger_Pull | 81 | (Sony Dualshock 4) right analog trigger, pulled by any amount (analog value)
INPUT_ACTION_ORIGIN_PS4_RIGHT_TRIGGER_CLICK | k_EInputActionOrigin_PS4_RightTrigger_Click | 82 | (Sony Dualshock 4) right analog trigger, pulled in all the way (digital value)
INPUT_ACTION_ORIGIN_PS4_LEFT_STICK_MOVE | k_EInputActionOrigin_PS4_LeftStick_Move | 83 | (Sony Dualshock 4) left joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_PS4_LEFT_STICK_CLICK | k_EInputActionOrigin_PS4_LeftStick_Click | 84 | (Sony Dualshock 4) left joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_PS4_LEFT_STICK_DPAD_NORTH | k_EInputActionOrigin_PS4_LeftStick_DPadNorth | 85 | (Sony Dualshock 4) left joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_PS4_LEFT_STICK_DPAD_SOUTH | k_EInputActionOrigin_PS4_LeftStick_DPadSouth | 86 | (Sony Dualshock 4) left joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_PS4_LEFT_STICK_DPAD_WEST | k_EInputActionOrigin_PS4_LeftStick_DPadWest | 87 | (Sony Dualshock 4) left joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_PS4_LEFT_STICK_DPAD_EAST | k_EInputActionOrigin_PS4_LeftStick_DPadEast | 88 | (Sony Dualshock 4) left joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_PS4_RIGHT_STICK_MOVE | k_EInputActionOrigin_PS4_RightStick_Move | 89 | (Sony Dualshock 4) right joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_PS4_RIGHT_STICK_CLICK | k_EInputActionOrigin_PS4_RightStick_Click | 90 | (Sony Dualshock 4) right joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_PS4_RIGHT_STICK_DPAD_NORTH | k_EInputActionOrigin_PS4_RightStick_DPadNorth | 91 | (Sony Dualshock 4) right joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_PS4_RIGHT_STICK_DPAD_SOUTH | k_EInputActionOrigin_PS4_RightStick_DPadSouth | 92 | (Sony Dualshock 4) right joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_PS4_RIGHT_STICK_DPAD_WEST | k_EInputActionOrigin_PS4_RightStick_DPadWest | 93 | (Sony Dualshock 4) right joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_PS4_RIGHT_STICK_DPAD_EAST | k_EInputActionOrigin_PS4_RightStick_DPadEast | 94 | (Sony Dualshock 4) right joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_PS4_DPAD_NORTH | k_EInputActionOrigin_PS4_DPad_North | 95 | (Sony Dualshock 4) digital pad, pressed (upper quadrant)
INPUT_ACTION_ORIGIN_PS4_DPAD_SOUTH | k_EInputActionOrigin_PS4_DPad_South | 96 | (Sony Dualshock 4) digital pad, pressed (lower quadrant)
INPUT_ACTION_ORIGIN_PS4_DPAD_WEST | k_EInputActionOrigin_PS4_DPad_West | 97 | (Sony Dualshock 4) digital pad, pressed (left quadrant)
INPUT_ACTION_ORIGIN_PS4_DPAD_EAST | k_EInputActionOrigin_PS4_DPad_East | 98 | (Sony Dualshock 4) digital pad, pressed (right quadrant)
INPUT_ACTION_ORIGIN_PS4_GYRO_MOVE | k_EInputActionOrigin_PS4_Gyro_Move | 99 | (Sony Dualshock 4) gyroscope, analog movement in any axis
INPUT_ACTION_ORIGIN_PS4_GYRO_PITCH | k_EInputActionOrigin_PS4_Gyro_Pitch | 100 | (Sony Dualshock 4) gyroscope, analog movement on the Pitch axis (point head up to ceiling, point head down to floor)
INPUT_ACTION_ORIGIN_PS4_GYRO_YAW | k_EInputActionOrigin_PS4_Gyro_Yaw | 101 | (Sony Dualshock 4) gyroscope, analog movement on the Yaw axis (turn head left to face one wall, turn head right to face other)
INPUT_ACTION_ORIGIN_PS4_GYRO_ROLL | k_EInputActionOrigin_PS4_Gyro_Roll | 102 | (Sony Dualshock 4) gyroscope, analog movement on the Roll axis (tilt head left towards shoulder, tilt head right towards other shoulder)
INPUT_ACTION_ORIGIN_PS4_RESERVED0 | k_EInputActionOrigin_PS4_Reserved0 | 103 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED1 | k_EInputActionOrigin_PS4_Reserved1 | 104 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED2 | k_EInputActionOrigin_PS4_Reserved2 | 105 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED3 | k_EInputActionOrigin_PS4_Reserved3 | 106 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED4 | k_EInputActionOrigin_PS4_Reserved4 | 107 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED5 | k_EInputActionOrigin_PS4_Reserved5 | 108 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED6 | k_EInputActionOrigin_PS4_Reserved6 | 109 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED7 | k_EInputActionOrigin_PS4_Reserved7 | 110 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED8 | k_EInputActionOrigin_PS4_Reserved8 | 111 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED9 | k_EInputActionOrigin_PS4_Reserved9 | 112 | Reserved for future use
INPUT_ACTION_ORIGIN_PS4_RESERVED10 | k_EInputActionOrigin_PS4_Reserved10 | 113 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_A | k_EInputActionOrigin_XBoxOne_A | 114 | (XB1) digital face button A
INPUT_ACTION_ORIGIN_XBOX_ONE_B | k_EInputActionOrigin_XBoxOne_B | 115 | (XB1) digital face button B
INPUT_ACTION_ORIGIN_XBOX_ONE_X | k_EInputActionOrigin_XBoxOne_X | 116 | (XB1) digital face button Cross
INPUT_ACTION_ORIGIN_XBOX_ONE_Y | k_EInputActionOrigin_XBoxOne_Y | 117 | (XB1) digital face button Y
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_BUMPER | k_EInputActionOrigin_XBoxOne_LeftBumper | 118 | (XB1) digital left shoulder button (aka left bumper)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_BUMPER | k_EInputActionOrigin_XBoxOne_RightBumper | 119 | (XB1) digital right shoulder button (aka right bumper)
INPUT_ACTION_ORIGIN_XBOX_ONE_MENU | k_EInputActionOrigin_XBoxOne_Menu | 120 | (XB1) digital menu button (aka start)
INPUT_ACTION_ORIGIN_XBOX_ONE_VIEW | k_EInputActionOrigin_XBoxOne_View | 121 | (XB1) digital view button (aka back)
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_TRIGGER_PULL | k_EInputActionOrigin_XBoxOne_LeftTrigger_Pull | 122 | (XB1) left analog trigger, pulled by any amount (analog value)
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_TRIGGER_CLICK | k_EInputActionOrigin_XBoxOne_LeftTrigger_Click | 123 | (XB1) left analog trigger, pulled in all the way (digital value)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_TRIGGER_PULL | k_EInputActionOrigin_XBoxOne_RightTrigger_Pull | 124 | (XB1) right analog trigger, pulled by any amount (analog value)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_TRIGGER_CLICK | k_EInputActionOrigin_XBoxOne_RightTrigger_Click | 125 | (XB1) right analog trigger, pulled in all the way (digital value)
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_STICK_MOVE | k_EInputActionOrigin_XBoxOne_LeftStick_Move | 126 | (XB1) left joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_STICK_CLICK | k_EInputActionOrigin_XBoxOne_LeftStick_Click | 127 | (XB1) left joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_STICK_DPAD_NORTH | k_EInputActionOrigin_XBoxOne_LeftStick_DPadNorth | 128 | (XB1) left joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_STICK_DPAD_SOUTH | k_EInputActionOrigin_XBoxOne_LeftStick_DPadSouth | 129 | (XB1) left joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_STICK_DPAD_WEST | k_EInputActionOrigin_XBoxOne_LeftStick_DPadWest | 130 | (XB1) left joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_LEFT_STICK_DPAD_EAST | k_EInputActionOrigin_XBoxOne_LeftStick_DPadEast | 131 | (XB1) left joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_STICK_MOVE | k_EInputActionOrigin_XBoxOne_RightStick_Move | 132 | (XB1) right joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_STICK_CLICK | k_EInputActionOrigin_XBoxOne_RightStick_Click | 133 | (XB1) right joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_STICK_DPAD_NORTH | k_EInputActionOrigin_XBoxOne_RightStick_DPadNorth | 134 | (XB1) right joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_STICK_DPAD_SOUTH | k_EInputActionOrigin_XBoxOne_RightStick_DPadSouth | 135 | (XB1) right joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_STICK_DPAD_WEST | k_EInputActionOrigin_XBoxOne_RightStick_DPadWest | 136 | (XB1) right joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_RIGHT_STICK_DPAD_EAST | k_EInputActionOrigin_XBoxOne_RightStick_DPadEast | 137 | (XB1) right joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_DPAD_NORTH | k_EInputActionOrigin_XBoxOne_DPad_North | 138 | (XB1) digital pad, pressed (upper quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_DPAD_SOUTH | k_EInputActionOrigin_XBoxOne_DPad_South | 139 | (XB1) digital pad, pressed (lower quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_DPAD_WEST | k_EInputActionOrigin_XBoxOne_DPad_West | 140 | (XB1) digital pad, pressed (left quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_DPAD_EAST | k_EInputActionOrigin_XBoxOne_DPad_East | 141 | (XB1) digital pad, pressed (right quadrant)
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED0 | k_EInputActionOrigin_XBoxOne_Reserved0 | 142 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED1 | k_EInputActionOrigin_XBoxOne_Reserved1 | 143 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED2 | k_EInputActionOrigin_XBoxOne_Reserved2 | 144 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED3 | k_EInputActionOrigin_XBoxOne_Reserved3 | 145 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED4 | k_EInputActionOrigin_XBoxOne_Reserved4 | 146 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED5 | k_EInputActionOrigin_XBoxOne_Reserved5 | 147 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED6 | k_EInputActionOrigin_XBoxOne_Reserved6 | 148 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED7 | k_EInputActionOrigin_XBoxOne_Reserved7 | 149 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED8 | k_EInputActionOrigin_XBoxOne_Reserved8 | 150 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED9 | k_EInputActionOrigin_XBoxOne_Reserved9 | 151 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_ONE_RESERVED10 | k_EInputActionOrigin_XBoxOne_Reserved10 | 152 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_A | k_EInputActionOrigin_XBox360_A | 153 | (X360) digital face button A
INPUT_ACTION_ORIGIN_XBOX_360_B | k_EInputActionOrigin_XBox360_B | 154 | (X360) digital face button B
INPUT_ACTION_ORIGIN_XBOX_360_X | k_EInputActionOrigin_XBox360_X | 155 | (X360) digital face button X
INPUT_ACTION_ORIGIN_XBOX_360_Y | k_EInputActionOrigin_XBox360_Y | 156 | (X360) digital face button Y
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_BUMPER | k_EInputActionOrigin_XBox360_LeftBumper | 157 | (X360) digital left shoulder button (aka left bumper)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_BUMPER | k_EInputActionOrigin_XBox360_RightBumper | 158 | (X360) digital right shoulder button (aka right bumper)
INPUT_ACTION_ORIGIN_XBOX_360_START | k_EInputActionOrigin_XBox360_Start | 159 | (X360) digital start button
INPUT_ACTION_ORIGIN_XBOX_360_BACK | k_EInputActionOrigin_XBox360_Back | 160 | (X360) digital back button
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_TRIGGER_PULL | k_EInputActionOrigin_XBox360_LeftTrigger_Pull | 161 | (X360) left analog trigger, pulled by any amount (analog value)
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_TRIGGER_CLICK | k_EInputActionOrigin_XBox360_LeftTrigger_Click | 162 | (X360) left analog trigger, pulled in all the way (digital value)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_TRIGGER_PULL | k_EInputActionOrigin_XBox360_RightTrigger_Pull | 163 | (X360) right analog trigger, pulled by any amount (analog value)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_TRIGGER_CLICK | k_EInputActionOrigin_XBox360_RightTrigger_Click | 164 | (X360) right analog trigger, pulled in all the way (digital value)
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_STICK_MOVE | k_EInputActionOrigin_XBox360_LeftStick_Move | 165 | (X360) left joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_STICK_CLICK | k_EInputActionOrigin_XBox360_LeftStick_Click | 166 | (X360) left joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_STICK_DPAD_NORTH | k_EInputActionOrigin_XBox360_LeftStick_DPadNorth | 167 | (X360) left joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_STICK_DPAD_SOUTH | k_EInputActionOrigin_XBox360_LeftStick_DPadSouth | 168 | (X360) left joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_STICK_DPAD_WEST | k_EInputActionOrigin_XBox360_LeftStick_DPadWest | 169 | (X360) left joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_LEFT_STICK_DPAD_EAST | k_EInputActionOrigin_XBox360_LeftStick_DPadEast | 170 | (X360) left joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_STICK_MOVE | k_EInputActionOrigin_XBox360_RightStick_Move | 171 | (X360) right joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_STICK_CLICK | k_EInputActionOrigin_XBox360_RightStick_Click | 172 | (X360) right joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_STICK_DPAD_NORTH | k_EInputActionOrigin_XBox360_RightStick_DPadNorth | 173 | (X360) right joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_STICK_DPAD_SOUTH | k_EInputActionOrigin_XBox360_RightStick_DPadSouth | 174 | (X360) right joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_STICK_DPAD_WEST | k_EInputActionOrigin_XBox360_RightStick_DPadWest | 175 | (X360) right joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_RIGHT_STICK_DPAD_EAST | k_EInputActionOrigin_XBox360_RightStick_DPadEast | 176 | (X360) right joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_DPAD_NORTH | k_EInputActionOrigin_XBox360_DPad_North | 177 | (X360) digital pad, pressed (upper quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_DPAD_SOUTH | k_EInputActionOrigin_XBox360_DPad_South | 178 | (X360) digital pad, pressed (lower quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_DPAD_WEST | k_EInputActionOrigin_XBox360_DPad_West | 179 | (X360) digital pad, pressed (left quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_DPAD_EAST | k_EInputActionOrigin_XBox360_DPad_East | 180 | (X360) digital pad, pressed (right quadrant)
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED0 | k_EInputActionOrigin_XBox360_Reserved0 | 181 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED1 | k_EInputActionOrigin_XBox360_Reserved1 | 182 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED2 | k_EInputActionOrigin_XBox360_Reserved2 | 183 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED3 | k_EInputActionOrigin_XBox360_Reserved3 | 184 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED4 | k_EInputActionOrigin_XBox360_Reserved4 | 185 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED5 | k_EInputActionOrigin_XBox360_Reserved5 | 186 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED6 | k_EInputActionOrigin_XBox360_Reserved6 | 187 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED7 | k_EInputActionOrigin_XBox360_Reserved7 | 188 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED8 | k_EInputActionOrigin_XBox360_Reserved8 | 189 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED9 | k_EInputActionOrigin_XBox360_Reserved9 | 190 | Reserved for future use
INPUT_ACTION_ORIGIN_XBOX_360_RESERVED10 | k_EInputActionOrigin_XBox360_Reserved10 | 191 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_A | k_EInputActionOrigin_Switch_A | 192 | (Nintendo Switch Pro) digital face button A
INPUT_ACTION_ORIGIN_SWITCH_B | k_EInputActionOrigin_Switch_B | 193 | (Nintendo Switch Pro) digital face button B
INPUT_ACTION_ORIGIN_SWITCH_X | k_EInputActionOrigin_Switch_X | 194 | (Nintendo Switch Pro) digital face button X
INPUT_ACTION_ORIGIN_SWITCH_Y | k_EInputActionOrigin_Switch_Y | 195 | (Nintendo Switch Pro) digital face button Y
INPUT_ACTION_ORIGIN_SWITCH_LEFT_BUMPER | k_EInputActionOrigin_Switch_LeftBumper | 196 | (Nintendo Switch Pro) digital left shoulder button (aka left bumper)
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_BUMPER | k_EInputActionOrigin_Switch_RightBumper | 197 | (Nintendo Switch Pro) digital right shoulder button (aka right bumper)
INPUT_ACTION_ORIGIN_SWITCH_PLUS | k_EInputActionOrigin_Switch_Plus | 198 | (Nintendo Switch Pro) plus button
INPUT_ACTION_ORIGIN_SWITCH_MINUS | k_EInputActionOrigin_Switch_Minus | 199 | (Nintendo Switch Pro) minus button
INPUT_ACTION_ORIGIN_SWITCH_CAPTURE | k_EInputActionOrigin_Switch_Capture | 200 | (Nintendo Switch Pro) digital capture button
INPUT_ACTION_ORIGIN_SWITCH_LEFT_TRIGGER_PULL | k_EInputActionOrigin_Switch_LeftTrigger_Pull | 201 | (Nintendo Switch Pro) left trigger, clicked
INPUT_ACTION_ORIGIN_SWITCH_LEFT_TRIGGER_CLICK | k_EInputActionOrigin_Switch_LeftTrigger_Click | 202 | (Nintendo Switch Pro) left trigger, clicked (same as previous value)
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_TRIGGER_PULL | k_EInputActionOrigin_Switch_RightTrigger_Pull | 203 | (Nintendo Switch Pro) right trigger, clicked
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_TRIGGER_CLICK | k_EInputActionOrigin_Switch_RightTrigger_Click | 204 | (Nintendo Switch Pro) right trigger, clicked (same as previous value)
INPUT_ACTION_ORIGIN_SWITCH_LEFT_STICK_MOVE | k_EInputActionOrigin_Switch_LeftStick_Move | 205 | (Nintendo Switch Pro) left joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_SWITCH_LEFT_STICK_CLICK | k_EInputActionOrigin_Switch_LeftStick_Click | 206 | (Nintendo Switch Pro) left joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_SWITCH_LEFT_STICK_DPAD_NORTH | k_EInputActionOrigin_Switch_LeftStick_DPadNorth | 207 | (Nintendo Switch Pro) left joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_SWITCH_LEFT_STICK_DPAD_SOUTH | k_EInputActionOrigin_Switch_LeftStick_DPadSouth | 208 | (Nintendo Switch Pro) left joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_SWITCH_LEFT_STICK_DPAD_WEST | k_EInputActionOrigin_Switch_LeftStick_DPadWest | 209 | (Nintendo Switch Pro) left joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_SWITCH_LEFT_STICK_DPAD_EAST | k_EInputActionOrigin_Switch_LeftStick_DPadEast | 210 | (Nintendo Switch Pro) left joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_STICK_MOVE | k_EInputActionOrigin_Switch_RightStick_Move | 211 | (Nintendo Switch Pro) right joystick, movement on any axis (analog value)
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_STICK_CLICK | k_EInputActionOrigin_Switch_RightStick_Click | 212 | (Nintendo Switch Pro) right joystick, clicked in (digital value)
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_STICK_DPAD_NORTH | k_EInputActionOrigin_Switch_RightStick_DPadNorth | 213 | (Nintendo Switch Pro) right joystick, digital movement (upper quadrant)
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_STICK_DPAD_SOUTH | k_EInputActionOrigin_Switch_RightStick_DPadSouth | 214 | (Nintendo Switch Pro) right joystick, digital movement (lower quadrant)
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_STICK_DPAD_WEST | k_EInputActionOrigin_Switch_RightStick_DPadWest | 215 | (Nintendo Switch Pro) right joystick, digital movement (left quadrant)
INPUT_ACTION_ORIGIN_SWITCH_RIGHT_STICK_DPAD_EAST | k_EInputActionOrigin_Switch_RightStick_DPadEast | 216 | (Nintendo Switch Pro) right joystick, digital movement (right quadrant)
INPUT_ACTION_ORIGIN_SWITCH_DPAD_NORTH | k_EInputActionOrigin_Switch_DPad_North | 217 | (Nintendo Switch Pro) digital pad, pressed (upper quadrant)
INPUT_ACTION_ORIGIN_SWITCH_DPAD_SOUTH | k_EInputActionOrigin_Switch_DPad_South | 218 | (Nintendo Switch Pro) digital pad, pressed (lower quadrant)
INPUT_ACTION_ORIGIN_SWITCH_DPAD_WEST | k_EInputActionOrigin_Switch_DPad_West | 219 | (Nintendo Switch Pro) digital pad, pressed (left quadrant)
INPUT_ACTION_ORIGIN_SWITCH_DPAD_EAST | k_EInputActionOrigin_Switch_DPad_East | 220 | (Nintendo Switch Pro) digital pad, pressed (right quadrant)
INPUT_ACTION_ORIGIN_SWITCH_PRO_GYRO_MOVE | k_EInputActionOrigin_SwitchProGyro_Move | 221 | (Nintendo Switch Pro) gyroscope, analog movement in any axis
INPUT_ACTION_ORIGIN_SWITCH_PRO_GYRO_PITCH | k_EInputActionOrigin_SwitchProGyro_Pitch | 222 | (Nintendo Switch Pro) gyroscope, analog movement on the Pitch axis (point head up to ceiling, point head down to floor)
INPUT_ACTION_ORIGIN_SWITCH_PRO_GYRO_YAW | k_EInputActionOrigin_SwitchProGyro_Yaw | 223 | (Nintendo Switch Pro) gyroscope, analog movement on the Yaw axis (turn head left to face one wall, turn head right to face other)
INPUT_ACTION_ORIGIN_SWITCH_PRO_GYRO_ROLL | k_EInputActionOrigin_SwitchProGyro_Roll | 224 | (Nintendo Switch Pro) gyroscope, analog movement on the Roll axis (tilt head left towards shoulder, tilt head right towards other shoulder)
INPUT_ACTION_ORIGIN_SWITCH_RESERVED0 | k_EInputActionOrigin_Switch_Reserved0 | 225 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED1 | k_EInputActionOrigin_Switch_Reserved1 | 226 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED2 | k_EInputActionOrigin_Switch_Reserved2 | 227 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED3 | k_EInputActionOrigin_Switch_Reserved3 | 228 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED4 | k_EInputActionOrigin_Switch_Reserved4 | 229 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED5 | k_EInputActionOrigin_Switch_Reserved5 | 230 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED6 | k_EInputActionOrigin_Switch_Reserved6 | 231 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED7 | k_EInputActionOrigin_Switch_Reserved7 | 232 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED8 | k_EInputActionOrigin_Switch_Reserved8 | 233 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED9 | k_EInputActionOrigin_Switch_Reserved9 | 234 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RESERVED10 | k_EInputActionOrigin_Switch_Reserved10 | 235 | Reserved for future use
INPUT_ACTION_ORIGIN_SWITCH_RIGHTGYRO_MOVE | k_EInputActionOrigin_Switch_RightGyro_Move | 236 | Right JoyCon Gyro generally should correspond to Pro's single gyro.
INPUT_ACTION_ORIGIN_SWITCH_RIGHTGYRO_PITCH | k_EInputActionOrigin_Switch_RightGyro_Pitch | 237 | Right JoyCon Gyro generally should correspond to Pro's single gyro.
INPUT_ACTION_ORIGIN_SWITCH_RIGHTGYRO_YAW | k_EInputActionOrigin_Switch_RightGyro_Yaw | 238 | Right JoyCon Gyro generally should correspond to Pro's single gyro.
INPUT_ACTION_ORIGIN_SWITCH_RIGHTGYRO_ROLL | k_EInputActionOrigin_Switch_RightGyro_Roll | 239 | Right JoyCon Gyro generally should correspond to Pro's single gyro.
INPUT_ACTION_ORIGIN_SWITCH_LEFTGYRO_MOVE | k_EInputActionOrigin_Switch_LeftGyro_Move | 240 | -
INPUT_ACTION_ORIGIN_SWITCH_LEFTGYRO_PITCH | k_EInputActionOrigin_Switch_LeftGyro_Pitch | 241 | -
INPUT_ACTION_ORIGIN_SWITCH_LEFTGYRO_YAW | k_EInputActionOrigin_Switch_LeftGyro_Yaw | 242 | -
INPUT_ACTION_ORIGIN_SWITCH_LEFTGYRO_ROLL | k_EInputActionOrigin_Switch_LeftGyro_Roll | 243 | -
INPUT_ACTION_ORIGIN_SWITCH_LEFTGRIP_LOWER | k_EInputActionOrigin_Switch_LeftGrip_Lower | 244 | Left JoyCon SR Button.
INPUT_ACTION_ORIGIN_SWITCH_LEFTGRIP_UPPER | k_EInputActionOrigin_Switch_LeftGrip_Upper | 245 | Left JoyCon SL Button.
INPUT_ACTION_ORIGIN_SWITCH_RIGHTGRIP_LOWER | k_EInputActionOrigin_Switch_RightGrip_Lower | 246 | Right JoyCon SL Button.
INPUT_ACTION_ORIGIN_SWITCH_RIGHTGRIP_UPPER | k_EInputActionOrigin_Switch_RightGrip_Upper | 247 | Right JoyCon SR Button.
INPUT_ACTION_ORIGIN_SWITCH_JOYCON_BUTTON_N | k_EInputActionOrigin_Switch_JoyConButton_N | 248 | With a Horizontal JoyCon this will be Y or what would be Dpad Right when vertical.
INPUT_ACTION_ORIGIN_SWITCH_JOYCON_BUTTON_E | k_EInputActionOrigin_Switch_JoyConButton_E | 249 | X.
INPUT_ACTION_ORIGIN_SWITCH_JOYCON_BUTTON_S | k_EInputActionOrigin_Switch_JoyConButton_S | 250 | A.
INPUT_ACTION_ORIGIN_SWITCH_JOYCON_BUTTON_W | k_EInputActionOrigin_Switch_JoyConButton_W | 251 | B.
INPUT_ACTION_ORIGIN_SWITCH_RESERVED15 | k_EInputActionOrigin_Switch_Reserved15 | 252 | -
INPUT_ACTION_ORIGIN_SWITCH_RESERVED16 | k_EInputActionOrigin_Switch_Reserved16 | 253 | -
INPUT_ACTION_ORIGIN_SWITCH_RESERVED17 | k_EInputActionOrigin_Switch_Reserved17 | 254 | -
INPUT_ACTION_ORIGIN_SWITCH_RESERVED18 | k_EInputActionOrigin_Switch_Reserved18 | 255 | -
INPUT_ACTION_ORIGIN_SWITCH_RESERVED19 | k_EInputActionOrigin_Switch_Reserved19 | 256 | -
INPUT_ACTION_ORIGIN_SWITCH_RESERVED20 | k_EInputActionOrigin_Switch_Reserved20 | 257 | -
INPUT_ACTION_ORIGIN_PS5_X | k_EInputActionOrigin_PS5_X | 258 | -
INPUT_ACTION_ORIGIN_PS5_CIRCLE | k_EInputActionOrigin_PS5_Circle | 259 | -
INPUT_ACTION_ORIGIN_PS5_TRIANGLE | k_EInputActionOrigin_PS5_Triangle | 260 | -
INPUT_ACTION_ORIGIN_PS5_SQUARE | k_EInputActionOrigin_PS5_Square | 261 | -
INPUT_ACTION_ORIGIN_PS5_LEFTBUMPER | k_EInputActionOrigin_PS5_LeftBumper | 262 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTBUMPER | k_EInputActionOrigin_PS5_RightBumper | 263 | -
INPUT_ACTION_ORIGIN_PS5_OPTION | k_EInputActionOrigin_PS5_Option | 264 | Start.
INPUT_ACTION_ORIGIN_PS5_CREATE | k_EInputActionOrigin_PS5_Create | 265 | Back.
INPUT_ACTION_ORIGIN_PS5_MUTE | k_EInputActionOrigin_PS5_Mute | 266 | -
INPUT_ACTION_ORIGIN_PS5_LEFTPAD_TOUCH | k_EInputActionOrigin_PS5_LeftPad_Touch | 267 | -
INPUT_ACTION_ORIGIN_PS5_LEFTPAD_SWIPE | k_EInputActionOrigin_PS5_LeftPad_Swipe | 268 | -
INPUT_ACTION_ORIGIN_PS5_LEFTPAD_CLICK | k_EInputActionOrigin_PS5_LeftPad_Click | 269 | -
INPUT_ACTION_ORIGIN_PS5_LEFTPAD_DPADNORTH | k_EInputActionOrigin_PS5_LeftPad_DPadNorth | 270 | -
INPUT_ACTION_ORIGIN_PS5_LEFTPAD_DPADSOUTH | k_EInputActionOrigin_PS5_LeftPad_DPadSouth | 271 | -
INPUT_ACTION_ORIGIN_PS5_LEFTPAD_DPADWEST | k_EInputActionOrigin_PS5_LeftPad_DPadWest | 272 | -
INPUT_ACTION_ORIGIN_PS5_LEFTPAD_DPADEAST | k_EInputActionOrigin_PS5_LeftPad_DPadEast | 273 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTPAD_TOUCH | k_EInputActionOrigin_PS5_RightPad_Touch | 274 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTPAD_SWIPE | k_EInputActionOrigin_PS5_RightPad_Swipe | 275 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTPAD_CLICK | k_EInputActionOrigin_PS5_RightPad_Click | 276 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTPAD_DPADNORTH | k_EInputActionOrigin_PS5_RightPad_DPadNorth | 277 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTPAD_DPADSOUTH | k_EInputActionOrigin_PS5_RightPad_DPadSouth | 278 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTPAD_DPADWEST | k_EInputActionOrigin_PS5_RightPad_DPadWest | 279 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTPAD_DPADEAST | k_EInputActionOrigin_PS5_RightPad_DPadEast | 280 | -
INPUT_ACTION_ORIGIN_PS5_CENTERPAD_TOUCH | k_EInputActionOrigin_PS5_CenterPad_Touch | 281 | -
INPUT_ACTION_ORIGIN_PS5_CENTERPAD_SWIPE | k_EInputActionOrigin_PS5_CenterPad_Swipe | 282 | -
INPUT_ACTION_ORIGIN_PS5_CENTERPAD_CLICK | k_EInputActionOrigin_PS5_CenterPad_Click | 283 | -
INPUT_ACTION_ORIGIN_PS5_CENTERPAD_DPADNORTH | k_EInputActionOrigin_PS5_CenterPad_DPadNorth | 284 | -
INPUT_ACTION_ORIGIN_PS5_CENTERPAD_DPADSOUTH | k_EInputActionOrigin_PS5_CenterPad_DPadSouth | 285 | -
INPUT_ACTION_ORIGIN_PS5_CENTERPAD_DPADWEST | k_EInputActionOrigin_PS5_CenterPad_DPadWest | 286 | -
INPUT_ACTION_ORIGIN_PS5_CENTERPAD_DPADEAST | k_EInputActionOrigin_PS5_CenterPad_DPadEast | 287 | -
INPUT_ACTION_ORIGIN_PS5_LEFTTRIGGER_PULL | k_EInputActionOrigin_PS5_LeftTrigger_Pull | 288 | -
INPUT_ACTION_ORIGIN_PS5_LEFTTRIGGER_CLICK | k_EInputActionOrigin_PS5_LeftTrigger_Click | 289 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTTRIGGER_PULL | k_EInputActionOrigin_PS5_RightTrigger_Pull | 290 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTTRIGGER_CLICK | k_EInputActionOrigin_PS5_RightTrigger_Click | 291 | -
INPUT_ACTION_ORIGIN_PS5_LEFTSTICK_MOVE | k_EInputActionOrigin_PS5_LeftStick_Move | 292 | -
INPUT_ACTION_ORIGIN_PS5_LEFTSTICK_CLICK | k_EInputActionOrigin_PS5_LeftStick_Click | 293 | -
INPUT_ACTION_ORIGIN_PS5_LEFTSTICK_DPADNORTH | k_EInputActionOrigin_PS5_LeftStick_DPadNorth | 294 | -
INPUT_ACTION_ORIGIN_PS5_LEFTSTICK_DPADSOUTH | k_EInputActionOrigin_PS5_LeftStick_DPadSouth | 295 | -
INPUT_ACTION_ORIGIN_PS5_LEFTSTICK_DPADWEST | k_EInputActionOrigin_PS5_LeftStick_DPadWest | 296 | -
INPUT_ACTION_ORIGIN_PS5_LEFTSTICK_DPADEAST | k_EInputActionOrigin_PS5_LeftStick_DPadEast | 297 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTSTICK_MOVE | k_EInputActionOrigin_PS5_RightStick_Move | 298 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTSTICK_CLICK | k_EInputActionOrigin_PS5_RightStick_Click | 299 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTSTICK_DPADNORTH | k_EInputActionOrigin_PS5_RightStick_DPadNorth | 300 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTSTICK_DPADSOUTH | k_EInputActionOrigin_PS5_RightStick_DPadSouth | 301 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTSTICK_DPADWEST | k_EInputActionOrigin_PS5_RightStick_DPadWest | 302 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTSTICK_DPADEAST | k_EInputActionOrigin_PS5_RightStick_DPadEast | 303 | -
INPUT_ACTION_ORIGIN_PS5_DPAD_NORTH | k_EInputActionOrigin_PS5_DPad_North | 304 | -
INPUT_ACTION_ORIGIN_PS5_DPAD_SOUTH | k_EInputActionOrigin_PS5_DPad_South | 305 | -
INPUT_ACTION_ORIGIN_PS5_DPAD_WEST | k_EInputActionOrigin_PS5_DPad_West | 306 | -
INPUT_ACTION_ORIGIN_PS5_DPAD_EAST | k_EInputActionOrigin_PS5_DPad_East | 307 | -
INPUT_ACTION_ORIGIN_PS5_GYRO_MOVE | k_EInputActionOrigin_PS5_Gyro_Move | 308 | -
INPUT_ACTION_ORIGIN_PS5_GYRO_PITCH | k_EInputActionOrigin_PS5_Gyro_Pitch | 309 | -
INPUT_ACTION_ORIGIN_PS5_GYRO_YAW | k_EInputActionOrigin_PS5_Gyro_Yaw | 310 | -
INPUT_ACTION_ORIGIN_PS5_GYRO_ROLL | k_EInputActionOrigin_PS5_Gyro_Roll | 311 | -
INPUT_ACTION_ORIGIN_PS5_DPAD_MOVE | k_EInputActionOrigin_PS5_DPad_Move | 312 | -
INPUT_ACTION_ORIGIN_PS5_LEFTGRIP | k_EInputActionOrigin_PS5_LeftGrip | 313 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTGRIP | k_EInputActionOrigin_PS5_RightGrip | 314 | -
INPUT_ACTION_ORIGIN_PS5_LEFTFN | k_EInputActionOrigin_PS5_LeftFn | 315 | -
INPUT_ACTION_ORIGIN_PS5_RIGHTFN | k_EInputActionOrigin_PS5_RightFn | 316 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED5 | k_EInputActionOrigin_PS5_Reserved5 | 317 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED6 | k_EInputActionOrigin_PS5_Reserved6 | 318 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED7 | k_EInputActionOrigin_PS5_Reserved7 | 319 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED8 | k_EInputActionOrigin_PS5_Reserved8 | 320 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED9 | k_EInputActionOrigin_PS5_Reserved9 | 321 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED10 | k_EInputActionOrigin_PS5_Reserved10 | 322 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED11 | k_EInputActionOrigin_PS5_Reserved11 | 323 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED12 | k_EInputActionOrigin_PS5_Reserved12 | 324 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED13 | k_EInputActionOrigin_PS5_Reserved13 | 325 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED14 | k_EInputActionOrigin_PS5_Reserved14 | 326 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED15 | k_EInputActionOrigin_PS5_Reserved15 | 327 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED16 | k_EInputActionOrigin_PS5_Reserved16 | 328 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED17 | k_EInputActionOrigin_PS5_Reserved17 | 329 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED18 | k_EInputActionOrigin_PS5_Reserved18 | 330 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED19 | k_EInputActionOrigin_PS5_Reserved19 | 331 | -
INPUT_ACTION_ORIGIN_PS5_RESERVED20 | k_EInputActionOrigin_PS5_Reserved20 | 332 | -
INPUT_ACTION_ORIGIN_STEAMDECK_A | k_EInputActionOrigin_SteamDeck_A | 333 | -
INPUT_ACTION_ORIGIN_STEAMDECK_B | k_EInputActionOrigin_SteamDeck_B | 334 | -
INPUT_ACTION_ORIGIN_STEAMDECK_X | k_EInputActionOrigin_SteamDeck_X | 335 | -
INPUT_ACTION_ORIGIN_STEAMDECK_Y | k_EInputActionOrigin_SteamDeck_Y | 336 | -
INPUT_ACTION_ORIGIN_STEAMDECK_L1 | k_EInputActionOrigin_SteamDeck_L1 | 337 | -
INPUT_ACTION_ORIGIN_STEAMDECK_R1 | k_EInputActionOrigin_SteamDeck_R1 | 338 | -
INPUT_ACTION_ORIGIN_STEAMDECK_MENU | k_EInputActionOrigin_SteamDeck_Menu | 339 | -
INPUT_ACTION_ORIGIN_STEAMDECK_VIEW | k_EInputActionOrigin_SteamDeck_View | 340 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTPAD_TOUCH | k_EInputActionOrigin_SteamDeck_LeftPad_Touch | 341 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTPAD_SWIPE | k_EInputActionOrigin_SteamDeck_LeftPad_Swipe | 342 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTPAD_CLICK | k_EInputActionOrigin_SteamDeck_LeftPad_Click | 343 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTPAD_DPADNORTH | k_EInputActionOrigin_SteamDeck_LeftPad_DPadNorth | 344 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTPAD_DPADSOUTH | k_EInputActionOrigin_SteamDeck_LeftPad_DPadSouth | 345 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTPAD_DPADWEST | k_EInputActionOrigin_SteamDeck_LeftPad_DPadWest | 346 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTPAD_DPADEAST | k_EInputActionOrigin_SteamDeck_LeftPad_DPadEast | 347 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTPAD_TOUCH | k_EInputActionOrigin_SteamDeck_RightPad_Touch | 348 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTPAD_SWIPE | k_EInputActionOrigin_SteamDeck_RightPad_Swipe | 349 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTPAD_CLICK | k_EInputActionOrigin_SteamDeck_RightPad_Click | 350 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTPAD_DPADNORTH | k_EInputActionOrigin_SteamDeck_RightPad_DPadNorth | 351 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTPAD_DPADSOUTH | k_EInputActionOrigin_SteamDeck_RightPad_DPadSouth | 352 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTPAD_DPADWEST | k_EInputActionOrigin_SteamDeck_RightPad_DPadWest | 353 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTPAD_DPADEAST | k_EInputActionOrigin_SteamDeck_RightPad_DPadEast | 354 | -
INPUT_ACTION_ORIGIN_STEAMDECK_L2_SOFTPULL | k_EInputActionOrigin_SteamDeck_L2_SoftPull | 355 | -
INPUT_ACTION_ORIGIN_STEAMDECK_L2 | k_EInputActionOrigin_SteamDeck_L2 | 356 | -
INPUT_ACTION_ORIGIN_STEAMDECK_R2_SOFTPULL | k_EInputActionOrigin_SteamDeck_R2_SoftPull | 357 | -
INPUT_ACTION_ORIGIN_STEAMDECK_R2 | k_EInputActionOrigin_SteamDeck_R2 | 358 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTSTICK_MOVE | k_EInputActionOrigin_SteamDeck_LeftStick_Move | 359 | -
INPUT_ACTION_ORIGIN_STEAMDECK_L3 | k_EInputActionOrigin_SteamDeck_L3 | 360 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTSTICK_DPADNORTH | k_EInputActionOrigin_SteamDeck_LeftStick_DPadNorth | 361 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTSTICK_DPADSOUTH | k_EInputActionOrigin_SteamDeck_LeftStick_DPadSouth | 362 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTSTICK_DPADWEST | k_EInputActionOrigin_SteamDeck_LeftStick_DPadWest | 363 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTSTICK_DPADEAST | k_EInputActionOrigin_SteamDeck_LeftStick_DPadEast | 364 | -
INPUT_ACTION_ORIGIN_STEAMDECK_LEFTSTICK_TOUCH | k_EInputActionOrigin_SteamDeck_LeftStick_Touch | 365 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTSTICK_MOVE | k_EInputActionOrigin_SteamDeck_RightStick_Move | 366 | -
INPUT_ACTION_ORIGIN_STEAMDECK_R3 | k_EInputActionOrigin_SteamDeck_R3 | 367 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTSTICK_DPADNORTH | k_EInputActionOrigin_SteamDeck_RightStick_DPadNorth | 368 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTSTICK_DPADSOUTH | k_EInputActionOrigin_SteamDeck_RightStick_DPadSouth | 369 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTSTICK_DPADWEST | k_EInputActionOrigin_SteamDeck_RightStick_DPadWest | 370 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTSTICK_DPADEAST | k_EInputActionOrigin_SteamDeck_RightStick_DPadEast | 371 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RIGHTSTICK_TOUCH | k_EInputActionOrigin_SteamDeck_RightStick_Touch | 372 | -
INPUT_ACTION_ORIGIN_STEAMDECK_L4 | k_EInputActionOrigin_SteamDeck_L4 | 373 | -
INPUT_ACTION_ORIGIN_STEAMDECK_R4 | k_EInputActionOrigin_SteamDeck_R4 | 374 | -
INPUT_ACTION_ORIGIN_STEAMDECK_L5 | k_EInputActionOrigin_SteamDeck_L5 | 375 | -
INPUT_ACTION_ORIGIN_STEAMDECK_R5 | k_EInputActionOrigin_SteamDeck_R5 | 376 | -
INPUT_ACTION_ORIGIN_STEAMDECK_DPAD_MOVE | k_EInputActionOrigin_SteamDeck_DPad_Move | 377 | -
INPUT_ACTION_ORIGIN_STEAMDECK_DPAD_NORTH | k_EInputActionOrigin_SteamDeck_DPad_North | 378 | -
INPUT_ACTION_ORIGIN_STEAMDECK_DPAD_SOUTH | k_EInputActionOrigin_SteamDeck_DPad_South | 379 | -
INPUT_ACTION_ORIGIN_STEAMDECK_DPAD_WEST | k_EInputActionOrigin_SteamDeck_DPad_West | 380 | -
INPUT_ACTION_ORIGIN_STEAMDECK_DPAD_EAST | k_EInputActionOrigin_SteamDeck_DPad_East | 381 | -
INPUT_ACTION_ORIGIN_STEAMDECK_GYRO_MOVE | k_EInputActionOrigin_SteamDeck_Gyro_Move | 382 | -
INPUT_ACTION_ORIGIN_STEAMDECK_GYRO_PITCH | k_EInputActionOrigin_SteamDeck_Gyro_Pitch | 383 | -
INPUT_ACTION_ORIGIN_STEAMDECK_GYRO_YAW | k_EInputActionOrigin_SteamDeck_Gyro_Yaw | 384 | -
INPUT_ACTION_ORIGIN_STEAMDECK_GYRO_ROLL | k_EInputActionOrigin_SteamDeck_Gyro_Roll | 385 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED1 | k_EInputActionOrigin_SteamDeck_Reserved1 | 386 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED2 | k_EInputActionOrigin_SteamDeck_Reserved2 | 387 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED3 | k_EInputActionOrigin_SteamDeck_Reserved3 | 388 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED4 | k_EInputActionOrigin_SteamDeck_Reserved4 | 389 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED5 | k_EInputActionOrigin_SteamDeck_Reserved5 | 390 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED6 | k_EInputActionOrigin_SteamDeck_Reserved6 | 391 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED7 | k_EInputActionOrigin_SteamDeck_Reserved7 | 392 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED8 | k_EInputActionOrigin_SteamDeck_Reserved8 | 393 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED9 | k_EInputActionOrigin_SteamDeck_Reserved9 | 394 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED10 | k_EInputActionOrigin_SteamDeck_Reserved10 | 395 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED11 | k_EInputActionOrigin_SteamDeck_Reserved11 | 396 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED12 | k_EInputActionOrigin_SteamDeck_Reserved12 | 397 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED13 | k_EInputActionOrigin_SteamDeck_Reserved13 | 398 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED14 | k_EInputActionOrigin_SteamDeck_Reserved14 | 399 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED15 | k_EInputActionOrigin_SteamDeck_Reserved15 | 400 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED16 | k_EInputActionOrigin_SteamDeck_Reserved16 | 401 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED17 | k_EInputActionOrigin_SteamDeck_Reserved17 | 402 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED18 | k_EInputActionOrigin_SteamDeck_Reserved18 | 403 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED19 | k_EInputActionOrigin_SteamDeck_Reserved19 | 404 | -
INPUT_ACTION_ORIGIN_STEAMDECK_RESERVED20 | k_EInputActionOrigin_SteamDeck_Reserved20 | 405 | -
INPUT_ACTION_ORIGIN_HORIPAD_M1 | k_EInputActionOrigin_Horipad_M1 | 406 | -
INPUT_ACTION_ORIGIN_HORIPAD_M2 | k_EInputActionOrigin_Horipad_M2 | 407 | -
INPUT_ACTION_ORIGIN_HORIPAD_L4 | k_EInputActionOrigin_Horipad_L4 | 408 | -
INPUT_ACTION_ORIGIN_HORIPAD_R4 | k_EInputActionOrigin_Horipad_R4 | 409 | -
INPUT_ACTION_ORIGIN_COUNT | k_EInputActionOrigin_Count | 406 | The number of values in this enum, useful for iterating.
INPUT_ACTION_ORIGIN_MAXIMUMPOSSIBLEVALUE | k_EInputActionOrigin_MaximumPossibleValue | 32767 | The number of values in this enum, useful for iterating.

### InputConfigurationEnableType
Individual values are used by the [getSessionInputConfigurationSettings](#getsessioninputconfigurationsettings) bitmask.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
INPUT_CONFIGURATION_ENABLE_TYPE_NONE | k_ESteamInputConfigurationEnableType_None | 0x0000 | -
INPUT_CONFIGURATION_ENABLE_TYPE_PLAYSTATION | k_ESteamInputConfigurationEnableType_Playstation | 0x0001 | -
INPUT_CONFIGURATION_ENABLE_TYPE_XBOX | k_ESteamInputConfigurationEnableType_Xbox | 0x0002 | -
INPUT_CONFIGURATION_ENABLE_TYPE_GENERIC | k_ESteamInputConfigurationEnableType_Generic | 0x0004 | -
INPUT_CONFIGURATION_ENABLE_TYPE_SWITCH | k_ESteamInputConfigurationEnableType_Switch | 0x0008 | -

### InputGlyphSize
These values are passed into [getGlyphPNGForActionOrigin](#getglyphpngforactionorigin).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
INPUT_GLYPH_SIZE_SMALL | k_ESteamInputGlyphSize_Small | 0 | 32x32 pixels.
INPUT_GLYPH_SIZE_MEDIUM | k_ESteamInputGlyphSize_Medium | 1 | 128x128 pixels.
INPUT_GLYPH_SIZE_LARGE | k_ESteamInputGlyphSize_Large | 2 | 256x256 pixels.
INPUT_GLYPH_SIZE_COUNT | k_ESteamInputGlyphSize_Count | 3 | -

### InputGlyphStyle

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
INPUT_GLYPH_STYLE_KNOCKOUT | ESteamInputGlyphStyle_Knockout | 0x0 | Base-styles, cannot mix. Face buttons will have colored labels / outlines on a knocked out background. Rest of inputs will have white detail / borders on a knocked out background.
INPUT_GLYPH_STYLE_LIGHT | ESteamInputGlyphStyle_Light | 0x1 | Base-styles, cannot mix. Black detail / borders on a white background.
INPUT_GLYPH_STYLE_DARK | ESteamInputGlyphStyle_Dark | 0x2 | Base-styles, cannot mix. White detail/borders on a black background.
INPUT_GLYPH_STYLE_NEUTRAL_COLOR_ABXY | ESteamInputGlyphStyle_NeutralColorABXY | 0x10 | Modifiers. Default ABXY/PS equivalent glyphs have a solid fill w/ color matching the physical buttons on the device. ABXY Buttons will match the base style color instead of their normal associated color.
INPUT_GLYPH_STYLE_SOLID_ABXY | ESteamInputGlyphStyle_SolidABXY | 0x20 | Modifiers. Default ABXY/PS equivalent glyphs have a solid fill w/ color matching the physical buttons on the device. ABXY Buttons will have a solid fill.

### InputLEDFlag
These values are passed into SetLEDColor.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
INPUT_LED_FLAG_SET_COLOR | k_ESteamInputLEDFlag_SetColor | 0 | -
INPUT_LED_FLAG_RESTORE_USER_DEFAULT | k_ESteamInputLEDFlag_RestoreUserDefault | 1 | Restore the LED color to the user's preference setting as set in the controller personalization menu. This also happens automatically on exit of your game.

### InputSourceMode
The virtual input mode imposed by the configurator upon a controller source. For instance, the configurator can make an analog joystick behave like a Dpad with four digital inputs; the EControllerSource would be k_EInputSource_Joystick and the InputSourceMode would be INPUT_SOURCE_MODE_DPAD. The mode also changes the input data received by any associated actions.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
INPUT_SOURCE_MODE_NONE | k_EInputSourceMode_None | 0 | No input mode.
INPUT_SOURCE_MODE_DPAD | k_EInputSourceMode_Dpad | 1 | A digital pad: four digital directional buttons fused together in a cross pattern, such that only one button from each axis can be pressed at any given time.
INPUT_SOURCE_MODE_BUTTONS | k_EInputSourceMode_Buttons | 2 | -
INPUT_SOURCE_MODE_FOUR_BUTTONS | k_EInputSourceMode_FourButtons | 3 | Four digital face buttons, any of which can be pressed simultaneously.
INPUT_SOURCE_MODE_ABSOLUTE_MOUSE | k_EInputSourceMode_AbsoluteMouse | 4 | -
INPUT_SOURCE_MODE_RELATIVE_MOUSE | k_EInputSourceMode_RelativeMouse | 5 | -
INPUT_SOURCE_MODE_JOYSTICK_MOVE | k_EInputSourceMode_JoystickMove | 6 | -
INPUT_SOURCE_MODE_JOYSTICK_MOUSE | k_EInputSourceMode_JoystickMouse | 7 | -
INPUT_SOURCE_MODE_JOYSTICK_CAMERA | k_EInputSourceMode_JoystickCamera | 8 | -
INPUT_SOURCE_MODE_SCROLL_WHEEL | k_EInputSourceMode_ScrollWheel | 9 | -
INPUT_SOURCE_MODE_TRIGGER | k_EInputSourceMode_Trigger | 10 | -
INPUT_SOURCE_MODE_TOUCH_MENU | k_EInputSourceMode_TouchMenu | 11 | -
INPUT_SOURCE_MODE_MOUSE_JOYSTICK | k_EInputSourceMode_MouseJoystick | 12 | -
INPUT_SOURCE_MODE_MOUSE_REGION | k_EInputSourceMode_MouseRegion | 13 | -
INPUT_SOURCE_MODE_RADIAL_MENU | k_EInputSourceMode_RadialMenu | 14 | -
INPUT_SOURCE_MODE_SINGLE_BUTTON | k_EInputSourceMode_SingleButton | 15 | -
INPUT_SOURCE_MODE_SWITCH | k_EInputSourceMode_Switches | 16 | -

### InputType

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
INPUT_TYPE_UNKNOWN | k_ESteamInputType_Unknown | 0 | Catch-all for unrecognized devices.
INPUT_TYPE_STEAM_CONTROLLER | k_ESteamInputType_SteamController | 1 | Valve's Steam Controller.
INPUT_TYPE_XBOX360_CONTROLLER | k_ESteamInputType_XBox360Controller | 2 | Microsoft's XBox 360 Controller.
INPUT_TYPE_XBOXONE_CONTROLLER | k_ESteamInputType_XBoxOneController | 3 | Microsoft's XBox One Controller
INPUT_TYPE_GENERIC_XINPUT | k_ESteamInputType_GenericGamepad | 4 | Any generic 3rd-party XInput device.
INPUT_TYPE_PS4_CONTROLLER | k_ESteamInputType_PS4Controller | 5 | Sony's PlayStation 4 Controller.
INPUT_TYPE_APPLE_MFI_CONTROLLER | k_ESteamInputType_AppleMFiController | 6 | Unused.
INPUT_TYPE_ANDROID_CONTROLLER | k_ESteamInputType_AndroidController | 7 | Unused.
INPUT_TYPE_SWITCH_JOYCON_PAIR | k_ESteamInputType_SwitchJoyConPair | 8 | Unused.
INPUT_TYPE_SWITCH_JOYCON_SINGLE | k_ESteamInputType_SwitchJoyConSingle | 9 | Unused.
INPUT_TYPE_SWITCH_PRO_CONTROLLER | k_ESteamInputType_SwitchProController | 10 | Nintendo's Switch Pro Controller.
INPUT_TYPE_MOBILE_TOUCH | k_ESteamInputType_MobileTouch | 11 | Steam Link App on-screen virtual controller.
INPUT_TYPE_PS3_CONTROLLER | k_ESteamInputType_PS3Controller | 12 | Sony's PlayStation 3 Controller or PS3/PS4 compatible fight stick. Currently uses PS4 origins.
INPUT_TYPE_PS5_CONTROLLER | k_ESteamInputType_PS5Controller | 13 | Sony's PlayStation 5 Controller.
INPUT_TYPE_STEAM_DECK_CONTROLLER | k_ESteamInputType_SteamDeckController | 14 | Valve's Steam Deck controls.
INPUT_TYPE_COUNT | k_ESteamInputType_Count | 15 | Current number of values returned.
INPUT_TYPE_MAXIMUM_POSSIBLE_VALUE | k_ESteamInputType_MaximumPossibleValue | 255 | Maximum possible value returned.

### SCEPadTriggerEffectMode
Found in isteamdualsense.h

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
PAD_TRIGGER_EFFECT_MODE_OFF | SCE_PAD_TRIGGER_EFFECT_MODE_OFF | 0 | -
PAD_TRIGGER_EFFECT_MODE_FEEDBACK | SCE_PAD_TRIGGER_EFFECT_MODE_FEEDBACK | 1 | -
PAD_TRIGGER_EFFECT_MODE_WEAPON | SCE_PAD_TRIGGER_EFFECT_MODE_WEAPON | 2 | -
PAD_TRIGGER_EFFECT_MODE_VIBRATION | SCE_PAD_TRIGGER_EFFECT_MODE_VIBRATION | 3 | -
PAD_TRIGGER_EFFECT_MODE_MULTIPLE_POSITION_FEEDBACK | SCE_PAD_TRIGGER_EFFECT_MODE_MULTIPLE_POSITION_FEEDBACK | 4 | -
PAD_TRIGGER_EFFECT_MODE_SLOPE_FEEDBACK | SCE_PAD_TRIGGER_EFFECT_MODE_SLOPE_FEEDBACK | 5 | -
PAD_TRIGGER_EFFECT_MODE_MULTIPLE_POSITION_VIBRATION | SCE_PAD_TRIGGER_EFFECT_MODE_MULTIPLE_POSITION_VIBRATION | 6 | -

### XboxOrigin

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
XBOX_ORIGIN_A | k_EXboxOrigin_A | 0 | -
XBOX_ORIGIN_B | k_EXboxOrigin_B | 1 | -
XBOX_ORIGIN_X | k_EXboxOrigin_X | 2 | -
XBOX_ORIGIN_Y | k_EXboxOrigin_Y | 3 | -
XBOX_ORIGIN_LEFT_BUMPER | k_EXboxOrigin_LeftBumper | 4 | -
XBOX_ORIGIN_RIGHT_BUMPER | k_EXboxOrigin_RightBumper | 5 | -
XBOX_ORIGIN_MENU | k_EXboxOrigin_Menu | 6 | Start.
XBOX_ORIGIN_VIEW | k_EXboxOrigin_View | 7 | Back.
XBOX_ORIGIN_LEFT_TRIGGER_PULL | k_EXboxOrigin_LeftTrigger_Pull | 8 | -
XBOX_ORIGIN_LEFT_TRIGGER_CLICK | k_EXboxOrigin_LeftTrigger_Click | 9 | -
XBOX_ORIGIN_RIGHT_TRIGGER_PULL | k_EXboxOrigin_RightTrigger_Pull | 10 | -
XBOX_ORIGIN_RIGHT_TRIGGER_CLICK | k_EXboxOrigin_RightTrigger_Click | 11 | -
XBOX_ORIGIN_LEFT_STICK_MOVE | k_EXboxOrigin_LeftStick_Move | 12 | -
XBOX_ORIGIN_LEFT_STICK_CLICK | k_EXboxOrigin_LeftStick_Click | 13 | -
XBOX_ORIGIN_LEFT_STICK_DPAD_NORTH | k_EXboxOrigin_LeftStick_DPadNorth | 14 | -
XBOX_ORIGIN_LEFT_STICK_DPAD_SOUTH | k_EXboxOrigin_LeftStick_DPadSouth | 15 | -
XBOX_ORIGIN_LEFT_STICK_DPAD_WEST | k_EXboxOrigin_LeftStick_DPadWest | 16 | -
XBOX_ORIGIN_LEFT_STICK_DPAD_EAT | k_EXboxOrigin_LeftStick_DPadEast | 17 | -
XBOX_ORIGIN_RIGHT_STICK_MOVE | k_EXboxOrigin_RightStick_Move | 18 | -
XBOX_ORIGIN_RIGHT_STICK_CLICK | k_EXboxOrigin_RightStick_Click | 19 | -
XBOX_ORIGIN_RIGHT_STICK_DPAD_NORTH | k_EXboxOrigin_RightStick_DPadNorth | 20 | -
XBOX_ORIGIN_RIGHT_STICK_DPAD_SOUTH | k_EXboxOrigin_RightStick_DPadSouth | 21 | -
XBOX_ORIGIN_RIGHT_STICK_DPAD_WEST | k_EXboxOrigin_RightStick_DPadWest | 22 | -
XBOX_ORIGIN_RIGHT_STICK_DPAD_EAST | k_EXboxOrigin_RightStick_DPadEast | 23 | -
XBOX_ORIGIN_DPAD_NORTH | k_EXboxOrigin_DPad_North | 24 | -
XBOX_ORIGIN_DPAD_SOUTH | k_EXboxOrigin_DPad_South | 25 | -
XBOX_ORIGIN_DPAD_WEST | k_EXboxOrigin_DPad_West | 26 | -
XBOX_ORIGIN_DPAD_EAST | k_EXboxOrigin_DPad_East | 27 | -
XBOX_ORIGIN_COUNT | k_EXboxOrigin_Count | 28 | -