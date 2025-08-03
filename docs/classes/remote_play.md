---
title: Remote Play
description: A class reference for Remote Play.
icon: material/remote-tv
---

# Remote Play

Functions that provide information about Steam Remote Play sessions, streaming your game content to another computer or to a Steam Link app or hardware. See [Steam Remote Play](https://partner.steamgames.com/doc/features/remoteplay){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### createMouseCursor

!!! function "createMouseCursor( `int` width, `int` height, `int` hot_x, `int` hot_y, `int` pitch )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | width | int | The width of the cursor, in pixels. |
    | height | int | The height of the cursor, in pixels. |
    | hot_x | int | The X coordinate of the cursor hot spot in pixels, offset from the left of the cursor. |
    | hot_y | int | The Y coordinate of the cursor hot spot in pixels, offset from the top of the cursor. |
    | pitch | int | The distance between pixel rows in bytes, defaults to **width** * 4. |

    Create a cursor that can be used with [setMouseCursor](#setmousecursor).  This is available after calling [enableRemotePlayTogetherDirectInput](#enableremoteplaytogetherdirectinput).

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| id | uint32_t | A remote play cursor ID.
		| cursor_pixels | PackedByteArray | The cursor pixels, with the color channels in red, green, blue, alpha order.

	!!! info "Notes"
		The returned **cursor_pixels** data is used to create a texture or image much like [avatars](https://godotsteam.com/tutorials/avatars/) or [achievement icons](https://godotsteam.com/tutorials/achievement_icons/).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#CreateMouseCursor){ .md-button .md-button--doc_classes target="_blank" }

### enableRemotePlayTogetherDirectInput

!!! function "enableRemotePlayTogetherDirectInput( )"
	Make mouse and keyboard input for Remote Play Together sessions available via [getInput](#getinput) instead of being merged with local input.

	!!! returns "Returns: bool"
		Returns true if mouse and keyboard events are now available; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#BEnableRemotePlayTogetherDirectInput){ .md-button .md-button--doc_classes target="_blank" }

### disableRemotePlayTogetherDirectInput

!!! function "disableRemotePlayTogetherDirectInput( )"
	Merge Remote Play Together mouse and keyboard input with local input

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#DisableRemotePlayTogetherDirectInput){ .md-button .md-button--doc_classes target="_blank" }

### getInput

!!! function "getInput( `uint32_t` max_events )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | max_events | uint32_t | The maximum number of events to read. |

    Get input events from Remote Play Together sessions. This is available after calling [enableRemotePlayTogetherDirectInput](#enableremoteplaytogetherdirectinput). Returns an array of input events that will be filled in by this function, up to **max_events**.

	!!! returns "Returns: array"
		An array of input events, up to **max_events**.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#GetInput){ .md-button .md-button--doc_classes target="_blank" }

### getSessionClientFormFactor

!!! function "getSessionClientFormFactor( `uint32_t` session_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | session_id | uint32_t | The session ID to get information about. |

    Get the form factor of the session client device.

	!!! returns "Returns: [DeviceFormFactor enum](#deviceformfactor)"
		The form factor of the device associated with the Remote Play session, or [FORM_FACTOR_UNKNOWN](#deviceformfactor) if the session ID is not valid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#GetSessionClientFormFactor){ .md-button .md-button--doc_classes target="_blank" }

### getSessionClientName

!!! function "getSessionClientName( `uint32_t` session_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | session_id | uint32_t | The session ID to get information about. |

    Get the name of the session client device.

    !!! returns "Returns: string"
    	The name of the device associated with the Remote Play sessionor empty if the session ID is not valid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#GetSessionClientName){ .md-button .md-button--doc_classes target="_blank" }

### getSessionClientResolution

!!! function "getSessionClientResolution( `uint32_t` session_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | session_id | uint32_t | The session ID to get information about. |

    Get the resolution, in pixels, of the session client device. This is set to 0x0 if the resolution is not available.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| success | bool | True if the session ID is valid; otherwise, false.
		| x | int | The device resolution width.
		| y | int | The device resolution height.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#BGetSessionClientResolution){ .md-button .md-button--doc_classes target="_blank" }

### getSessionCount

!!! function "getSessionCount( )"
	Get the number of currently connected Steam Remote Play sessions.

	!!! returns "Returns: uint32"
		The number of currently connected Steam Remote Play sessions.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#GetSessionCount){ .md-button .md-button--doc_classes target="_blank" }

### getSessionID

!!! function "getSessionID( `uint32_t` index )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | index | uint32_t | The index of the specified session. |

	Get the currently connected Steam Remote Play session ID at the specified index.

	!!! returns "Returns: uint32"
		The session ID of the session at the specified index; may be 0 if the index is less than 0 or greater than or equal to [getSessionCount](#getsessioncount).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#GetSessionID){ .md-button .md-button--doc_classes target="_blank" }

### getSessionSteamID

!!! function "getSessionSteamID( `uint32` session_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | session_id | uint32_t | The session ID to get information about. |

    Get the Steam ID of the connected user.

	!!! returns "Returns: uint64_t"
		The Steam ID of the user associated with the Remote Play session. This would normally be the logged in user, or a friend in the case of Remote Play Together.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#GetSessionSteamID){ .md-button .md-button--doc_classes target="_blank" }

### sendRemotePlayTogetherInvite

!!! function "sendRemotePlayTogetherInvite( `uint64_t` friend_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t | The Steam ID of the friend you'd like to invite. |

    Invite a friend to join the game using Remote Play Together.

	!!! returns "Returns: bool"
		Returns true if the invite was successfully sent; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#BSendRemotePlayTogetherInvite){ .md-button .md-button--doc_classes target="_blank" }

### setMouseCursor

!!! function "setMouseCursor( `uint32_t` session_id, `uint32_t` cursor_id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | session_id | uint32_t | The session to affect. |
    | cursor_id | uint32_t | The normalized X position of the mouse cursor. |

	Set the mouse cursor for a remote player. This is available after calling [enableRemotePlayTogetherDirectInput](#enableremoteplaytogetherdirectinput). The cursor ID is a value returned by [createMouseCursor](#createmousecursor).

	!!! returns "Returns: void" 

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#SetMouseCursor){ .md-button .md-button--doc_classes target="_blank" }

### setMousePosition

!!! function "setMousePosition( `uint32_t` session_id, `float` normalized_x, `float` normalized_y )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | session_id | uint32_t | The session to affect. |
    | normalized_x | float | The normalized X position of the mouse cursor. |
    | normalized_y | float | The normalized Y position of the mouse cursor. |

	Set the mouse cursor position for a remote player. This is available after calling [enableRemotePlayTogetherDirectInput](#enableremoteplaytogetherdirectinput). This is used to warp the cursor to a specific location and isn't needed during normal event processing. The position is normalized relative to the window, where 0,0 is the upper left, and 1,1 is the lower right.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#SetMousePosition){ .md-button .md-button--doc_classes target="_blank" }

### setMouseVisibility

!!! function "setMouseVisibility( `uint32_t` session_id, `bool` visible )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | session_id | uint32_t | The session to affect. |
    | visible | bool | True to show the mouse cursor, false to hide it. |

    Set the mouse cursor visibility for a remote player. This is available after calling [enableRemotePlayTogetherDirectInput](#enableremoteplaytogetherdirectinput).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#SetMouseVisibility){ .md-button .md-button--doc_classes target="_blank" }

### showRemotePlayTogetherUI

!!! function "showRemotePlayTogetherUI( )"
	Show the Remote Play Together UI in the game overlay. This returns false if your game is not configured for Remote Play Together.

	!!! returns "Returns: bool"
		Returns true if your game is configured for Remote Play Together; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#ShowRemotePlayTogetherUI){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### remote_play_guest_invite

!!! function "remote_play_guest_invite"
	Sent when a guest invitation is created, and includes the guest invite URL.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| invite_url | string | The URL that can be used to connect to this guest session.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#SteamRemotePlayTogetherGuestInvite_t){ .md-button .md-button--doc_classes target="_blank" }

### remote_play_session_connected

!!! function "remote_play_session_connected"
	Called when a player connects to a remote play session.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| session_id | uint32_t | The session ID of the session that just connected.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#SteamRemotePlaySessionConnected_t){ .md-button .md-button--doc_classes target="_blank" }

### remote_play_session_disconnected

!!! function "remote_play_session_disconnected"
	Called when a player disconnects from a remote play session.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| session_id | uint32_t | The session ID of the session that just disconnected.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamRemotePlay#SteamRemotePlaySessionDisconnected_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-numeric: Enums
==}

### DeviceFormFactor

The form factor of a device.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
FORM_FACTOR_UNKNOWN | k_ESteamDeviceFormFactorUnknown | 0 | -
FORM_FACTOR_PHONE | k_ESteamDeviceFormFactorPhone | 1 | -
FORM_FACTOR_TABLET | k_ESteamDeviceFormFactorTablet | 2 | -
FORM_FACTOR_COMPUTER | k_ESteamDeviceFormFactorComputer | 3 | -
FORM_FACTOR_TV | k_ESteamDeviceFormFactorTV | 4 | -
FORM_FACTOR_VR_HEADSET | k_ESteamDeviceFormFactorVRHeadset | 5 | -

### RemotePlayInputType

The type of input in returned data from [getInput](#getinput).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
REMOTE_PLAY_INPUT_UNKNOWN | k_ERemotePlayInputUnknown | 0 | - 
REMOTE_PLAY_INPUT_MOUSE_MOTION | k_ERemotePlayInputMouseMotion | 1 | - 
REMOTE_PLAY_INPUT_MOUSE_BUTTON_DOWN | k_ERemotePlayInputMouseButtonDown | 2 | - 
REMOTE_PLAY_INPUT_MOUSE_BUTTON_UP | k_ERemotePlayInputMouseButtonUp | 3 | - 
REMOTE_PLAY_INPUT_MOUSE_WHEEL | k_ERemotePlayInputMouseWheel | 4 | - 
REMOTE_PLAY_INPUT_KEY_DOWN | k_ERemotePlayInputKeyDown | 5 | - 
REMOTE_PLAY_INPUT_KEY_UP | k_ERemotePlayInputKeyUp | 6 | - 

### RemotePlayKeyModifier

Key modifier in returned data from [getInput](#getinput).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
REMOTE_PLAY_KEY_MODIFIER_NONE | k_ERemotePlayKeyModifierNone | 0x0000 | -
REMOTE_PLAY_KEY_MODIFIER_LEFT_SHIFT | k_ERemotePlayKeyModifierLeftShift | 0x0001 | -
REMOTE_PLAY_KEY_MODIFIER_RIGHT_SHIFT | k_ERemotePlayKeyModifierRightShift | 0x0002 | -
REMOTE_PLAY_KEY_MODIFIER_LEFT_CONTROL | k_ERemotePlayKeyModifierLeftControl | 0x0040 | -
REMOTE_PLAY_KEY_MODIFIER_RIGHT_CONTROL | k_ERemotePlayKeyModifierRightControl | 0x0080 | -
REMOTE_PLAY_KEY_MODIFIER_LEFT_ALT | k_ERemotePlayKeyModifierLeftAlt | 0x0100 | -
REMOTE_PLAY_KEY_MODIFIER_RIGHT_ALT | k_ERemotePlayKeyModifierRightAlt | 0x0200 | -
REMOTE_PLAY_KEY_MODIFIER_LEFT_GUI | k_ERemotePlayKeyModifierLeftGUI | 0x0400 | -
REMOTE_PLAY_KEY_MODIFIER_RIGHT_GUI | k_ERemotePlayKeyModifierRightGUI | 0x0800 | -
REMOTE_PLAY_KEY_MODIFIER_NUM_LOCK | k_ERemotePlayKeyModifierNumLock | 0x1000 | -
REMOTE_PLAY_KEY_MODIFIER_CAPS_LOCK | k_ERemotePlayKeyModifierCapsLock | 0x2000 | -
REMOTE_PLAY_KEY_MODIFIER_MASK | k_ERemotePlayKeyModifierMask | 0xFFFF | -

### RemotePlayMouseButton

Mouse buttons in returned data from [getInput](#getinput).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
REMOTE_PLAY_MOUSE_BUTTON_LEFT | k_ERemotePlayMouseButtonLeft | 0x0001 | -
REMOTE_PLAY_MOUSE_BUTTON_RIGHT | k_ERemotePlayMouseButtonRight | 0x0002 | -
REMOTE_PLAY_MOUSE_BUTTON_MIDDLE | k_ERemotePlayMouseButtonMiddle | 0x0010 | -
REMOTE_PLAY_MOUSE_BUTTON_X1 | k_ERemotePlayMouseButtonX1 | 0x0020 | -
REMOTE_PLAY_MOUSE_BUTTON_X2 | k_ERemotePlayMouseButtonX2 | 0x0040 | -

### RemotePlayMouseWheelDirection 

Mouse wheel direction in returned data from [getInput](#getinput)..

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
REMOTE_PLAY_MOUSE_WHEEL_UP | k_ERemotePlayMouseWheelUp | 1 | -
REMOTE_PLAY_MOUSE_WHEEL_DOWN | k_ERemotePlayMouseWheelDown | 2 | -
REMOTE_PLAY_MOUSE_WHEEL_LEFT | k_ERemotePlayMouseWheelLeft | 3 | -
REMOTE_PLAY_MOUSE_WHEEL_RIGHT | k_ERemotePlayMouseWheelRight | 4 | -

### RemotePlayScancode 

Key scancode in returned data from [getInput](#getinput). This is a USB scancode value as defined for the Keyboard/Keypad Page (0x07).  This enumeration isn't a complete list, just the most commonly used keys.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
REMOTE_PLAYER_SCANCODE_UNKNOWN | k_ERemotePlayScancodeUnknown | 0 | -
REMOTE_PLAYER_SCANCODE_A | k_ERemotePlayScancodeA | 4 | -
REMOTE_PLAYER_SCANCODE_B | k_ERemotePlayScancodeB |  5 | -
REMOTE_PLAYER_SCANCODE_C | k_ERemotePlayScancodeC | 6 | -
REMOTE_PLAYER_SCANCODE_D | k_ERemotePlayScancodeD | 7 | -
REMOTE_PLAYER_SCANCODE_E | k_ERemotePlayScancodeE | 8 | -
REMOTE_PLAYER_SCANCODE_F | k_ERemotePlayScancodeF | 9 | -
REMOTE_PLAYER_SCANCODE_G | k_ERemotePlayScancodeG | 10 | -
REMOTE_PLAYER_SCANCODE_H | k_ERemotePlayScancodeH | 11 | -
REMOTE_PLAYER_SCANCODE_I | k_ERemotePlayScancodeI | 12 | -
REMOTE_PLAYER_SCANCODE_J | k_ERemotePlayScancodeJ | 13 | -
REMOTE_PLAYER_SCANCODE_K | k_ERemotePlayScancodeK | 14 | -
REMOTE_PLAYER_SCANCODE_L | k_ERemotePlayScancodeL | 15 | -
REMOTE_PLAYER_SCANCODE_M | k_ERemotePlayScancodeM | 16 | -
REMOTE_PLAYER_SCANCODE_N | k_ERemotePlayScancodeN | 17 | -
REMOTE_PLAYER_SCANCODE_O | k_ERemotePlayScancodeO | 18 | -
REMOTE_PLAYER_SCANCODE_P | k_ERemotePlayScancodeP | 19 | -
REMOTE_PLAYER_SCANCODE_Q | k_ERemotePlayScancodeQ | 20 | -
REMOTE_PLAYER_SCANCODE_R | k_ERemotePlayScancodeR | 21 | -
REMOTE_PLAYER_SCANCODE_S | k_ERemotePlayScancodeS | 22 | -
REMOTE_PLAYER_SCANCODE_T | k_ERemotePlayScancodeT | 23 | -
REMOTE_PLAYER_SCANCODE_U | k_ERemotePlayScancodeU | 24 | -
REMOTE_PLAYER_SCANCODE_V | k_ERemotePlayScancodeV | 25 | -
REMOTE_PLAYER_SCANCODE_W | k_ERemotePlayScancodeW | 26 | -
REMOTE_PLAYER_SCANCODE_X | k_ERemotePlayScancodeX | 27 | -
REMOTE_PLAYER_SCANCODE_Y | k_ERemotePlayScancodeY | 28 | -
REMOTE_PLAYER_SCANCODE_Z | k_ERemotePlayScancodeZ | 29 | -
REMOTE_PLAYER_SCANCODE_1 | k_ERemotePlayScancode1 | 30 | -
REMOTE_PLAYER_SCANCODE_2 | k_ERemotePlayScancode2 | 31 | -
REMOTE_PLAYER_SCANCODE_3 | k_ERemotePlayScancode3 | 32 | -
REMOTE_PLAYER_SCANCODE_4 | k_ERemotePlayScancode4 | 33 | -
REMOTE_PLAYER_SCANCODE_5 | k_ERemotePlayScancode5 | 34 | -
REMOTE_PLAYER_SCANCODE_6 | k_ERemotePlayScancode6 | 35 | -
REMOTE_PLAYER_SCANCODE_7 | k_ERemotePlayScancode7 | 36 | -
REMOTE_PLAYER_SCANCODE_8 | k_ERemotePlayScancode8 | 37 | -
REMOTE_PLAYER_SCANCODE_9 | k_ERemotePlayScancode9 | 38 | -
REMOTE_PLAYER_SCANCODE_0 | k_ERemotePlayScancode0 | 39 | -
REMOTE_PLAYER_SCANCODE_RETURN | k_ERemotePlayScancodeReturn| 40 | -
REMOTE_PLAYER_SCANCODE_ESCAPE | k_ERemotePlayScancodeEscape | 41 | -
REMOTE_PLAYER_SCANCODE_BACKSPACE | k_ERemotePlayScancodeBackspace | 42 | -
REMOTE_PLAYER_SCANCODE_TAB | k_ERemotePlayScancodeTab | 43 | -
REMOTE_PLAYER_SCANCODE_SPACE | k_ERemotePlayScancodeSpace | 44 | -
REMOTE_PLAYER_SCANCODE_MINUS | k_ERemotePlayScancodeMinus | 45 | -
REMOTE_PLAYER_SCANCODE_EQUALS | k_ERemotePlayScancodeEquals | 46 | -
REMOTE_PLAYER_SCANCODE_LEFT_BRACKET | k_ERemotePlayScancodeLeftBracket | 47 | -
REMOTE_PLAYER_SCANCODE_RIGHT_BRACKET | k_ERemotePlayScancodeRightBracket | 48 | -
REMOTE_PLAYER_SCANCODE_BACKSLASH | k_ERemotePlayScancodeBackslash | 49 | -
REMOTE_PLAYER_SCANCODE_SEMICOLON | k_ERemotePlayScancodeSemicolon | 51 | -
REMOTE_PLAYER_SCANCODE_APOSTROPHE | k_ERemotePlayScancodeApostrophe | 52 | -
REMOTE_PLAYER_SCANCODE_GRAVE | k_ERemotePlayScancodeGrave | 53 | -
REMOTE_PLAYER_SCANCODE_COMMA | k_ERemotePlayScancodeComma | 54 | -
REMOTE_PLAYER_SCANCODE_PERIOD | k_ERemotePlayScancodePeriod | 55 | -
REMOTE_PLAYER_SCANCODE_SLASH | k_ERemotePlayScancodeSlash | 56 | -
REMOTE_PLAYER_SCANCODE_CAPSLOCK | k_ERemotePlayScancodeCapsLock | 57 | -
REMOTE_PLAYER_SCANCODE_F1 | k_ERemotePlayScancodeF1 | 58 | -
REMOTE_PLAYER_SCANCODE_F2 | k_ERemotePlayScancodeF2 | 59 | -
REMOTE_PLAYER_SCANCODE_F3 | k_ERemotePlayScancodeF3 | 60 | -
REMOTE_PLAYER_SCANCODE_F4 | k_ERemotePlayScancodeF4 | 61 | -
REMOTE_PLAYER_SCANCODE_F5 | k_ERemotePlayScancodeF5 | 62 | -
REMOTE_PLAYER_SCANCODE_F6 | k_ERemotePlayScancodeF6 | 63 | -
REMOTE_PLAYER_SCANCODE_F7 | k_ERemotePlayScancodeF7 | 64 | -
REMOTE_PLAYER_SCANCODE_F8 | k_ERemotePlayScancodeF8 | 65 | -
REMOTE_PLAYER_SCANCODE_F9 | k_ERemotePlayScancodeF9 | 66 | -
REMOTE_PLAYER_SCANCODE_F10 | k_ERemotePlayScancodeF10 | 67 | -
REMOTE_PLAYER_SCANCODE_F11 | k_ERemotePlayScancodeF11 | 68 | -
REMOTE_PLAYER_SCANCODE_F12 | k_ERemotePlayScancodeF22 | 69 | -
REMOTE_PLAYER_SCANCODE_INSERT | k_ERemotePlayScancodeInsert | 73 | -
REMOTE_PLAYER_SCANCODE_HOME | k_ERemotePlayScancodeHome | 74 | -
REMOTE_PLAYER_SCANCODE_PAGE_UP | k_ERemotePlayScancodePageUp | 75 | -
REMOTE_PLAYER_SCANCODE_DELETE | k_ERemotePlayScancodeDelete | 76 | -
REMOTE_PLAYER_SCANCODE_END | k_ERemotePlayScancodeEnd | 77 | -
REMOTE_PLAYER_SCANCODE_PAGE_DOWN | k_ERemotePlayScancodePageDown | 78 | -
REMOTE_PLAYER_SCANCODE_RIGHT | k_ERemotePlayScancodeRight | 79 | -
REMOTE_PLAYER_SCANCODE_LEFT | k_ERemotePlayScancodeLeft | 80 | -
REMOTE_PLAYER_SCANCODE_DOWN | k_ERemotePlayScancodeDown | 81 | -
REMOTE_PLAYER_SCANCODE_UP | k_ERemotePlayScancodeUp | 78 | -
REMOTE_PLAYER_SCANCODE_LEFT_CONTROL | k_ERemotePlayScancodeLeftControl | 224 | -
REMOTE_PLAYER_SCANCODE_LEFT_SHIFT | k_ERemotePlayScancodeLeftShift | 225 | -
REMOTE_PLAYER_SCANCODE_LEFT_ALT | k_ERemotePlayScancodeLeftAlt | 226 | -
REMOTE_PLAYER_SCANCODE_LEFT_GUI | k_ERemotePlayScancodeLeftGUI | 227 | Windows, Command (Apple), Meta.
REMOTE_PLAYER_SCANCODE_RIGHT_CONTROL | k_ERemotePlayScancodeRightControl | 228 | -
REMOTE_PLAYER_SCANCODE_RIGHT_SHIFT | k_ERemotePlayScancodeRightShift | 229 | -
REMOTE_PLAYER_SCANCODE_RIGHT_ALT | k_ERemotePlayScancodeRightALT | 230 | -
REMOTE_PLAYER_SCANCODE_RIGHT_GUI | k_ERemotePlayScancodeRightGUI | 231 | Windows, Command (Apple), Meta.