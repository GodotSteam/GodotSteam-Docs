---
title: HTML Surface
description: A class reference for HTML Surface.
icon: octicons/browser-16
---

# HTML Surface

Interface for rendering and interacting with HTML pages. You can use this interface to render and display HTML pages directly inside your game or application. You must call [htmlInit](#htmlinit) prior to using this interface, and [htmlShutdown](#htmlshutdown) when you're done using it. See [Steam HTML Surface](https://partner.steamgames.com/doc/features/html_surface){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

!!! info "Notes"
	The **browser_handle** argument is optional. If you do not pass one, GodotSteam will use the internally-stored handle instead.

### addHeader

!!! function "addHeader( `string` key, `string` value, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | key | string | The header name to add.
    | value | string | The header value to associate with the key.
    | browser_handle | uint32_t | The handle of the surface to add the header to.

	Add a header to any HTTP requests from this browser.  A full list of standard request fields are available here on [Wikipedia](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields){ target="\_blank" }.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#AddHeader){ .md-button .md-button--store target="_blank" }

### allowStartRequest

!!! function "allowStartRequest( `bool` allowed, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | allowed | bool | Allow or deny the navigation to the current start request.
    | browser_handle | uint32_t | The handle of the surface that is navigating.

	Sets whether a pending load is allowed or if it should be canceled.  You can use this feature to limit the valid pages allowed in your HTML surface.

	!!! returns "Returns: void"

	!!! info "Notes"
		You **must** call this in response to a [html_start_request](#html_start_request) callback.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#AllowStartRequest){ .md-button .md-button--store target="_blank" }

### copyToClipboard

!!! function "copyToClipboard( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to copy the text from.

    Copy the currently selected text from the current page in an HTML surface into the local clipboard.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#CopyToClipboard){ .md-button .md-button--store target="_blank" }

### createBrowser

!!! function "createBrowser( `string` user_agent = "", `string` user_css = "" )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | user_agent | string | Appends the string to the general user agent string of the browser, allowing you to detect your client on webservers. Use "" if you do not require this functionality.
    | user_css | string | This allows you to set a CSS style to every page displayed by this browser. Use "" if you do not require this functionality.

	Create a browser object for displaying of an HTML page. Both arguments used are optional.  When creation is complete the call handle will return a [html_browser_ready](#html_browser_ready) callback for the HTML Browser of your new browser.

	The **user_agent** string is a substring to be added to the general user agent string so you can identify your client on web servers.

	The **user_css** string lets you apply a CSS style sheet to every displayed page; leave null if you do not require this functionality.

	You **must** have implemented handlers for [html_browser_ready](#html_browser_ready), [html_start_request](#html_start_request), [html_js_alert](#html_js_alert), [html_js_confirm](#html_js_confirm), and [html_file_open_dialog](#html_file_open_dialog) callbacks.  If you do not implement these callback handlers, the browser may appear to hang instead of navigating to new pages or triggering Javascript popups.

	!!! returns "Returns: void"

	!!! info "Notes"
		You **must** call [removeBrowser](#removebrowser) when you are done using this browser to free up the resources associated with it. Failing to do so will result in a memory leak.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#CreateBrowser){ .md-button .md-button--store target="_blank" }

### executeJavascript

!!! function "executeJavascript( `string` script, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | script | string | The javascript script to run.
    | browser_handle | uint32_t | The handle of the surface that is navigating.

	Run a javascript script in the currently loaded page.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#ExecuteJavascript){ .md-button .md-button--store target="_blank" }

### find

!!! function "find( `string` search, `bool` currently_in_find, `bool` reverse, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | search | string | The string to search for.
    | currently_in_find | bool | Set this to true on subsequent calls to cycle through to the next matching string.
    | reverse | bool | Search from the bottom up?
    | browser_handle | uint32_t | The handle of the surface to find the string in.

    Find a string in the current page of an HTML surface.  This is the equivalent of "ctrl+f" in your browser of choice. It will highlight all of the matching strings. You should call [stopFind](#stopfind) when the input string has changed or you want to stop searching.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[html_search_results](#html_search_results) callback.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#Find){ .md-button .md-button--store target="_blank" }

### getLinkAtPosition

!!! function "getLinkAtPosition( `int` x, `int` y, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | x | int | The X (width) position in pixels within the surface. (0, 0) is the top left corner.
    | y | int | The Y (height) position in pixels within the surface. (0, 0) is the top left corner.
    | browser_handle | uint32_t | The handle of the surface to get a link from.

	Retrieves details about a link at a specific position on the current page in an HTML surface.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[html_link_at_position](#html_link_at_position) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#GetLinkAtPosition){ .md-button .md-button--store target="_blank" }

### goBack

!!! function "goBack( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to navigate back on

	Navigate back in the page history.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#GoBack){ .md-button .md-button--store target="_blank" }

### goForward

!!! function "goForward( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to navigate forward on.

	Navigate forward in the page history.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#GoForward){ .md-button .md-button--store target="_blank" }

### htmlInit

!!! function "htmlInit( )"
	Initializes the HTML Surface API. This must be called prior to using any other functions in this interface.

	You **must** call [htmlShutdown](#htmlshutdown) when you are done using the interface to free up the resources associated with it. Failing to do so will result in a memory leak.

	!!! returns "Returns: bool"
		Returns true if the API was successfully initialized; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#Init){ .md-button .md-button--store target="_blank" }

### htmlShutdown

!!! function "htmlShutdown( )"
	Shutdown the ISteamHTMLSurface interface, releasing the memory and handles. You **must** call this when you are done using this interface to prevent memory and handle leaks. After calling this then all of the functions provided in this interface will fail until you call [htmlInit](#htmlinit) to reinitialize again.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#Shutdown){ .md-button .md-button--store target="_blank" }

### jsDialogResponse

!!! function "jsDialogResponse( `bool` result, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | result | bool | Set this to true to simulate pressing the "OK" button, otherwise false for "Cancel".
    | browser_handle | uint32_t | The handle of the surface that is spawning a dialog.

    Allows you to react to a page wanting to open a javascript modal dialog notification.

	!!! returns "Returns: void"

	!!! info "Notes"
		You **must** call this in response to [html_js_alert](#html_js_alert) and [html_js_confirm](#html_js_confirm) callbacks.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#JSDialogResponse){ .md-button .md-button--store target="_blank" }

### keyChar

!!! function "keyChar( `uint32_t` unicode_char, `HTMLKeyModifiers` key_modifiers, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | unicode_char | uint32_t | The unicode character point for this keypress; and potentially multiple characters per press.
    | key_modifiers | [HTMLKeyModifiers enum](#htmlkeymodifiers) | This should be set to a bitmask of the modifier keys that the user is currently pressing.
    | browser_handle | uint32_t | The handle of the surface to send the interaction to.

	**unicode_char** is the unicode character point for this keypress (and potentially multiple characters per press).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#KeyChar){ .md-button .md-button--store target="_blank" }

### keyDown

!!! function "keyDown( `uint32_t` native_key_code, `HTMLKeyModifiers` key_modifiers, `uint32_t` browser_handle = 0, `bool` is_system_key = false )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | native_key_code | uint32_t | This is the virtual keycode value from the OS.
    | key_modifiers | [HTMLKeyModifiers enum](#htmlkeymodifiers) | This should be set to a bitmask of the modifier keys that the user is currently pressing.
    | browser_handle | uint32_t | The handle of the surface to send the interaction to.
    | is_system_key | bool | Flags the key to not be sent as a typed character as well as a key down.

	Keyboard interactions, native keycode is the virtual key code value from your OS.  **is_system_key** flags the key to not be sent as a typed character as well as a key down.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#KeyDown){ .md-button .md-button--store target="_blank" }

### keyUp

!!! function "keyUp( `uint32_t` native_key_code, `HTMLKeyModifiers` key_modifiers, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | native_key_code | uint32_t | This is the virtual keycode value from the OS.
    | key_modifiers | [HTMLKeyModifiers enum](#htmlkeymodifiers) | This should be set to a bitmask of the modifier keys that the user is currently pressing.
    | browser_handle | uint32_t | The handle of the surface to send the interaction to.

	Keyboard interactions, native keycode is the virtual key code value from your OS.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#KeyUp){ .md-button .md-button--store target="_blank" }

### loadURL

!!! function "loadURL( `string` url, `string` post_data, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | url | string | The URL to load.
    | post_data | string | Optionally send a POST request with this data, set this to NULL to not send any data.
    | browser_handle | uint32_t | The handle of the surface to load this URL in.

	Navigate to a specified URL. If you send POST data with `post_data` then the data should be formatted as: `name1=value1&name2=value2`. You can load any URI scheme supported by Chromium Embedded Framework including but not limited to: `http://, https://, ftp://, and file:///`. If no scheme is specified then `http://` is used.

	If no handle is passed, it will use the internally-store handle.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[html_start_request](#html_start_request) callback.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#LoadURL){ .md-button .md-button--store target="_blank" }

### mouseDoubleClick

!!! function "mouseDoubleClick( `HTMLMouseButton` mouse_button, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | mouse_button | [HTMLMouseButton enum](#htmlmousebutton) | The mouse button which was double clicked.
    | browser_handle | uint32_t | The handle of the surface to send the interaction to.

	Tells an HTML surface that a mouse button has been double clicked. The click will occur where the surface thinks the mouse is based on the last call to [mouseMove](#mousemove).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#MouseDoubleClick){ .md-button .md-button--store target="_blank" }

### mouseDown

!!! function "mouseDown( `HTMLMouseButton` mouse_button, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | mouse_button | [HTMLMouseButton enum](#htmlmousebutton) | The mouse button which was pressed.
    | browser_handle | uint32_t | The handle of the surface to send the interaction to.

	Tells an HTML surface that a mouse button has been pressed. The click will occur where the surface thinks the mouse is based on the last call to [mouseMove](#mousemove).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#MouseDown){ .md-button .md-button--store target="_blank" }

### mouseMove

!!! function "mouseMove( `int` x, `int` y, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | x | int | X (width) coordinate in pixels relative to the position of the HTML surface. (0, 0) is the top left.
    | y | int | Y (height) coordinate in pixels relative to the position of the HTML surface. (0, 0) is the top left.
    | browser_handle | uint32_t | The handle of the surface to send the interaction to.

	Tells an HTML surface where the mouse is.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#MouseMove){ .md-button .md-button--store target="_blank" }

### mouseUp

!!! function "mouseUp( `HTMLMHouseButton` mouse_button, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | mouse_button | [HTMLMouseButton enum](#htmlmousebutton) | The mouse button which was released.
    | browser_handle | uint32_t | The handle of the surface to send the interaction to.

	Tells an HTML surface that a mouse button has been released. The click will occur where the surface thinks the mouse is based on the last call to [mouseMove](#mousemove).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#MouseUp){ .md-button .md-button--store target="_blank" }

### mouseWheel

!!! function "mouseWheel( `int32` delta, `uint32_t` browser_handle = 0)"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | delta | int32 | The number of pixels to scroll.
    | browser_handle | uint32_t | The handle of the surface to send the interaction to.

	Tells an HTML surface that the mouse wheel has moved.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#MouseWheel){ .md-button .md-button--store target="_blank" }

### openDeveloperTools

!!! function "openDeveloperTools( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to open the developer tools for.

    Open HTML / JavaScript developer tools.

    !!! returns "Returns: void"

### pasteFromClipboard

!!! function "pasteFromClipboard( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to paste into.

    Paste from the local clipboard to the current page in an HTML surface.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#PasteFromClipboard){ .md-button .md-button--store target="_blank" }

### reload

!!! function "reload( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to reload.

	Refreshes the current page. The reload will most likely hit the local cache instead of going over the network. This is equivalent to F5 or Ctrl+R in your browser of choice.

	If no handle is passed, it will use the internally-stored handle.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#Reload){ .md-button .md-button--store target="_blank" }

### removeBrowser

!!! function "removeBrowser( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The browser handle to release.

	You **must** call this when you are done with an HTML surface, freeing the resources associated with it. Failing to call this will result in a memory leak.

	If no handle is passed, it will use the internally-stored handle.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#RemoveBrowser){ .md-button .md-button--store target="_blank" }

### setBackgroundMode

!!! function "setBackgroundMode( `bool` background_mode, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | background_mode | bool | Toggle background mode on or off.
    | browser_handle | uint32_t | The handle of the surface set background mode on.

    Enable/disable low-resource background mode, where Javascript and repaint timers are throttled, resources are more aggressively purged from memory, and audio/video elements are paused.

	When background mode is enabled, all HTML5 video and audio objects will execute ".pause()" and gain the property ".\_steam_background_paused = 1". When background mode is disabled, any video or audio objects with that property will resume with ".play()".

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#SetBackgroundMode){ .md-button .md-button--store target="_blank" }

### setCookie

!!! function "setCookie( `string` hostname, `string` key, `string` value, `string` path, `uint32_t` expires, `bool` secure, `bool` http_only )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | hostname | string | The hostname of the server to set the cookie for. ('Host' attribute).
    | key | string | The cookie name to set.
    | value | string | The cookie value to set.
    | path | string | Sets the 'Path' attribute on the cookie. You can use this to restrict the cookie to a specific path on the domain. e.g. "/accounts"
    | expires | uint32_t | Sets the 'Expires' attribute on the cookie to the specified timestamp in Unix epoch format (seconds since Jan 1st 1970).
    | secure | bool | Sets the 'Secure' attribute.
    | http_only | bool | Sets the 'HttpOnly' attribute.

    Set a webcookie for a specific hostname. You can read more about the specifics of setting cookies here on [Wikipedia](https://en.wikipedia.org/wiki/HTTP_cookie#Implementation){ target="\_blank" }.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#SetCookie){ .md-button .md-button--store target="_blank" }

### setDPIScalingFactor

!!! function "setDPIScalingFactor( `float` dpi_scaling, `uint32_t` browser_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | dpi_scaling | float | The scaling factor to set.
    | browser_handle | uint32_t | The handle of the surface set the DPI scaling factor for.

    Scale the output display space by this factor, this is useful when displaying content on high-DPI devices.

	Specifies the ratio between physical and logical pixels.

	!!! returns "Returns: void"

### setHorizontalScroll

!!! function "setHorizontalScroll( `uint32_t` absolute_pixel_scroll, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | absolute_pixel_scroll | uint32_t | The absolute pixel position to scroll to. 0 is the left and [html_horizontal_scroll](#html_horizontal_scroll) **scroll_max** is the right side.
    | browser_handle | uint32_t | The handle of the surface to set the horizontal scroll position.

	Scroll the current page horizontally.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[html_horizontal_scroll](#html_horizontal_scroll) callback.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#SetHorizontalScroll){ .md-button .md-button--store target="_blank" }

### setKeyFocus

!!! function "setKeyFocus( `bool` has_key_focus, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | has_key_focus | bool | Turn key focus on or off?
    | browser_handle | uint32_t | The handle of the surface to set key focus on.

	Tell a HTML surface if it has key focus currently, controls showing the I-beam cursor in text controls amongst other things.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#SetKeyFocus){ .md-button .md-button--store target="_blank" }

### setPageScaleFactor

!!! function "setPageScaleFactor( `float` zoom, `int` point_x, `int` point_y, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | zoom | float | The amount to zoom, this can range from 1 (100% and the default) to 2 (200%).
    | point_x | int | The X point in pixels to zoom around. Use 0 if you don't care.
    | point_y | int | The Y point in pixels to zoom around. Use 0 if you don't care.
    | browser_handle | uint32_t | The handle of the surface to scale.

    Zoom the current page in an HTML surface by by **zoom**; from 0.0 to 2.0.  So to zoom to 120%, use 1.2.  Zooming around point x,y in the page; use 0,0 if you don't care.

    The current scale factor is available from [html_needs_paint](#html_needs_paint) **page_scale**, [html_horizontal_scroll](#html_horizontal_scroll) **page_scale**, and [html_vertical_scroll](#html_vertical_scroll) **page_scale**.

	!!! returns "Returns: void"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#SetPageScaleFactor){ .md-button .md-button--store target="_blank" }

### setSize

!!! function "setSize( `uint32_t` width, `uint32_t` height, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | width | uint32_t | The width of the surface in pixels.
    | height | uint32_t | The height of the surface in pixels.
    | browser_handle | uint32_t | The handle of the surface to set the size of.

	Sets the display size of a surface in pixels.

	If no handle is passed, it will use the internally-stored handle.

	!!! returns "Returns: void"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#SetSize){ .md-button .md-button--store target="_blank" }

### setVerticalScroll

!!! function "setVerticalScroll( `uint32_t` absolute_pixel_scroll, `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | absolute_pixel_scroll | uint32_t | The absolute pixel position to scroll to. 0 is the top and [html_vertical_scroll](#html_vertical_scroll) **scroll_max** is the bottom
    | browser_handle | uint32_t | The handle of the surface to set the vertical scroll position.

	Scroll the current page vertically.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[html_vertical_scroll](#html_vertical_scroll) callback.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#SetVerticalScroll){ .md-button .md-button--store target="_blank" }

### stopFind

!!! function "stopFind( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to stop the find results.

    Cancel a currently running find.

	!!! returns "Returns: void"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#StopFind){ .md-button .md-button--store target="_blank" }

### stopLoad

!!! function "stopLoad( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to stop loading.

	Stop the load of the current HTML page.

	If no handle is passed, it will use the internally-stored handle.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#StopLoad){ .md-button .md-button--store target="_blank" }

### viewSource

!!! function "viewSource( `uint32_t` browser_handle = 0 )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | browser_handle | uint32_t | The handle of the surface to view its current pages source.

    Open the current pages HTML source code in default local text editor, used for debugging.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#ViewSource){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### html_browser_ready

!!! function "html_browser_ready"
	A new browser was created and is ready for use.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | This is the handle to the browser that was just created, which you can use with future calls.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_BrowserReady_t){ .md-button .md-button--store target="_blank" }

### html_browser_restarted

!!! function "html_browser_restarted"
	The browser has restarted due to an internal failure; use this new handle value.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | This is the new browser handle after the restart.
		| old_browser_handle | uint32_t | The handle for the browser before the restart; if your handle was this then switch to using **browser_handle** for API calls.

### html_can_go_backandforward

!!! function "html_can_go_backandforward"
	Called when page history status has changed the ability to go backwards and forward.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| go_back | bool | Returns whether you can navigate backwards.
		| go_forward | bool | Returns whether you can navigate forward.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_CanGoBackAndForward_t){ .md-button .md-button--store target="_blank" }		

### html_changed_title

!!! function "html_changed_title"
	Called when the current page in a browser gets a new title.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| title | string | The new title of the page.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_ChangedTitle_t){ .md-button .md-button--store target="_blank" }

### html_close_browser

!!! function "html_close_browser"
	Called when the browser has been requested to close due to user interaction; usually because of a javascript window.close() call.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_CloseBrowser_t){ .md-button .md-button--store target="_blank" }

### html_file_open_dialog

!!! function "html_file_open_dialog"
	Called when a browser surface has received a file open dialog from a `input type="file"` click or similar.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that is navigating.
		| title | string | The name of the dialog; eg. "Upload Images".
		| initial_file | string | The filename that the page wants you to set by default. It may be expecting a file with that name or that was the file the user previously uploaded.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_FileOpenDialog_t){ .md-button .md-button--store target="_blank" }

### html_finished_request

!!! function "html_finished_request"
	Called when a browser has finished loading a page.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this call was for.
		| url | string | The URL that was loaded.
		| title | string | The title of the page that got loaded.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_FinishedRequest_t){ .md-button .md-button--store target="_blank" }

### html_hide_tooltip

!!! function "html_hide_tooltip"
	Called when a a browser wants to hide a tooltip.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_HideToolTip_t){ .md-button .md-button--store target="_blank" }

### html_horizontal_scroll

!!! function "html_horizontal_scroll"
	Provides details on the visibility and size of the horizontal scrollbar.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| scroll_data | dictionary | Contains data about the horizontal scroll.

		**scroll_data** contains the follow keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| scroll_max | uint32_t | The maximum amount you can scroll horizontally.
		* scroll_current | uint32_t | The current horizontal scroll position.
		* page_scale | float | The current page scale.
		* visible | bool | Whether the horizontal scrollbar is visible.
		* page_size | uint32_t | The total width of the page in pixels.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_HorizontalScroll_t){ .md-button .md-button--store target="_blank" }

### html_js_alert

!!! function "html_js_alert"
	Called when the browser wants to display a Javascript alert dialog, call [jsDialogResponse](#jsdialogresponse) when the user dismisses this dialog; or right away to ignore it.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this call was for.
		| message | string | The message associated with the dialog.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_JSAlert_t){ .md-button .md-button--store target="_blank" }

### html_js_confirm

!!! function "html_js_confirm"
	Called when the browser wants to display a Javascript confirmation dialog, call [jsDialogResponse](#jsdialogresponse) when the user dismisses this dialog; or right away to ignore it.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this call was for.
		| message | string | The message associated with the dialog.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_JSConfirm_t){ .md-button .md-button--store target="_blank" }	

### html_link_at_position

!!! function "html_link_at_position"
	Result of a call to [getLinkAtPosition](#getlinkatposition).

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this call was for.
		| link_data | dictionary | Data about the link.

		**link_data** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| x | uint32_t | Unused.
		| y | uint32_t | Unused.
		| url | string | The URL found at this position. Empty string if no link was found.
		| input | bool | Was the position an input field?
		| live_link | bool | -

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_LinkAtPosition_t){ .md-button .md-button--store target="_blank" }

### html_needs_paint

!!! function "html_needs_paint"
	Called when a browser surface has a pending paint. This is where you get the actual image data to render to the screen.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| page_data | dictionary | Data about the page.

		**page_data** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| rgba | string | B8G8R8A8 image data for this surface, valid until [run_callbacks](main.md#run_callbacks) is next called.
		| wide | uint32_t | The total width of the **rgba** texture.
		| tall | uint32_t | The total height of the **rgba** texture.
		| update_x | uint32_t | The offset in X for the damage rect for this update.
		| update_y | uint32_t | The offset in Y for the damage rect for this update.
		| update_wide | uint32_t | The width of the damage rect for this update.
		| update_tall | uint32_t | The height of the damage rect for this update.
		| scroll_x | uint32_t | The horizontal scroll position the browser was at when this texture was rendered.
		| scroll_y | uint32_t | The veritcal scroll position the browser was at when this texture was rendered.
		| page_scale | float | The scale factor the browser was at when this texture was rendered.
		| page_serial | uint32_t | Incremented on each new page load, you can use this to reject draws while navigating to new pages.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_NeedsPaint_t){ .md-button .md-button--store target="_blank" }

### html_new_window

!!! function "html_new_window"
	A browser has created a new HTML window.

	At this time, the API does not allow you to acknowledge or render the contents of this new window, so the new window is always destroyed immediately.  The URL and other parameters of the new window are passed here to give your application the opportunity to call [createBrowser](#createbrowser) and set up a new browser in response to the attempted popup, if you wish to do so.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| window_data | dictionary | Data about the window.

		**window_data** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| url | string | The URL to load in the new window.
		| x | uint32_t | The x pos into the page to display the popup.
		| y | uint32_t | The y pos into the page to display the popup.
		| wide | uint32_t | The total width of the window texture.
		| tall | uint32_t | The total height of the window texture.
		| new_handle | uint32_t | The handle of the new window surface.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_NewWindow_t){ .md-button .md-button--store target="_blank" }

### html_open_link_in_new_tab

!!! function "html_open_link_in_new_tab"
	The browser has requested to load a url in a new tab.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| url | string | The URL that the browser wants to load.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_OpenLinkInNewTab_t){ .md-button .md-button--store target="_blank" }

### html_search_results

!!! function "html_search_results"
	Results from a search.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| results | uint32_t | The number of matching results found.
		| current_match | uint32_t | The ordinal of the current match relative to **results**.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_SearchResults_t){ .md-button .md-button--store target="_blank" }

### html_set_cursor

!!! function "html_set_cursor"
	Called when a browser wants to change the mouse cursor.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| mouse_cursor | uint32_t | The [HTMLMouseCursor enum](#htmlmousecursor) to display.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_SetCursor_t){ .md-button .md-button--store target="_blank" }

### html_show_tooltip

!!! function "html_show_tooltip"
	Called when a browser wants to display a tooltip.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| message | string | The text of the tooltip that wants to be displayed.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_ShowToolTip_t){ .md-button .md-button--store target="_blank" }

### html_start_request

!!! function "html_start_request"
	Called when a browser wants to navigate to a new page.

	!!! returns "Returns"        
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that is navigating.
		| url | string | The url it wants to navigate to.
		| target | string | The html link target type; i.e _blank, _self, _parent, _top.
		| post_data | string | Any posted data for the request.
		| redirect | bool | True if this was a HTTP / HTML redirect from the last load request.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_StartRequest_t){ .md-button .md-button--store target="_blank" }

### html_status_text

!!! function "html_status_text"
	Called when a browser wants you to display an informational message. This is most commonly used when you hover over links.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| message | string | The text of the status message to display.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_StatusText_t){ .md-button .md-button--store target="_blank" }

### html_update_tooltip

!!! function "html_update_tooltip"
	Called when the text of an existing tooltip has been updated.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| message | string | The new text of the tooltip.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_UpdateToolTip_t){ .md-button .md-button--store target="_blank" }

### html_url_changed

!!! function "html_url_changed"
	Called when the browser is navigating to a new url.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| url_data | dictionary | Data about the new URL.

		**url_data** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| url | string | The url that the browser is navigating to.
		| post_data | string | Any posted data for the request, empty string if there was none.
		| redirect | bool | True if this was a http/html redirect from the last load request; otherwise, false.
		| title | string | The title of the page.
		| new_navigation | bool | This is true if the page has changed rather than just a call to the browser history API.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_URLChanged_t){ .md-button .md-button--store target="_blank" }

### html_vertical_scroll

!!! function "html_vertical_scroll"
	Provides details on the visibility and size of the vertical scrollbar.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| browser_handle | uint32_t | The handle of the surface that this callback is for.
		| scroll_data | dictionary | Data about the vertical scrollbar.

		**scroll_data** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| scroll_max | uint32_t | The maximum amount you can scroll vertically.
		| scroll_current | uint32_t | The current vertical scroll position.
		| page_scale | float | The current page scale.
		| visible | bool | Whether the vertical scrollbar is visible.
		| page_size | uint32_t | The total height of the page in pixels.

		---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamHTMLSurface#HTML_VerticalScroll_t){ .md-button .md-button--store target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
INVALID_HTMLBROWSER | INVALID_HTMLBROWSER | 0 | Indicates that a browser handle is invalid. You should initialize your own HTML Browser handles to this value and then set it back to this when the page closes.

{==
## :material-numeric: Enums
==}

### HTMLKeyModifiers

Used to let the browser know what keys are pressed with: [keyChar](#keychar), [keyUp](#keyup) and [keyDown](#keydown). These flags can be added together using bitwise OR.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
HTML_KEY_MODIFIER_NONE | k_eHTMLKeyModifier_None | 0 | No modifiers are pressed.
HTML_KEY_MODIFIER_ALT_DOWN | k_eHTMLKeyModifier_AltDown | 1 << 0 | One of the alt keys are pressed.
HTML_KEY_MODIFIER_CTRL_DOWN | k_eHTMLKeyModifier_CtrlDown | 1 << 1 | One of the ctrl keys are pressed.
HTML_KEY_MODIFIER_SHIFT_DOWN | k_eHTMLKeyModifier_ShiftDown | 1 << 2 | One of the shift keys are pressed.

### HTMLMouseButton

Used to let the browser know when a mouse button is pressed with: [mouseUp](#mouseup), [mouseDown](#mousedown) and [mouseDoubleClick](#mousedoubleclick).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
HTML_MOUSE_BUTTON_LEFT | eHTMLMouseButton_Left | 0 | The left button is pressed.
HTML_MOUSE_BUTTON_RIGHT | eHTMLMouseButton_Right | 1 | The right button is pressed.
HTML_MOUSE_BUTTON_MIDDLE | eHTMLMouseButton_Middle | 2 | The middle button is pressed.

### HTMLMouseCursor

This lists the mouse cursors that the HTML surface will tell you to render.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
HTML_MOUSE_CURSOR_USER | k_EHTMLMouseCursor_User | 0 | -
	HTML_MOUSE_CURSOR_NONE | k_EHTMLMouseCursor_None | 1 | -
	HTML_MOUSE_CURSOR_ARROW | k_EHTMLMouseCursor_Arrow | 2 | -
	HTML_MOUSE_CURSOR_IBEAM | k_EHTMLMouseCursor_IBeam | 3 | -
	HTML_MOUSE_CURSOR_HOURGLASS | k_EHTMLMouseCursor_Hourglass | 4 | -
	HTML_MOUSE_CURSOR_WAIT_ARROW | k_EHTMLMouseCursor_WaitArrow | 5 | -
	HTML_MOUSE_CURSOR_CROSSHAIR | k_EHTMLMouseCursor_Crosshair | 6 | -
	HTML_MOUSE_CURSOR_UP | k_EHTMLMouseCursor_Up | 7 | -
	HTML_MOUSE_CURSOR_SIZE_NW | k_EHTMLMouseCursor_SizeNW | 8 | -
	HTML_MOUSE_CURSOR_SIZE_SE | k_EHTMLMouseCursor_SizeSE | 9 | -
	HTML_MOUSE_CURSOR_SIZE_NE | k_EHTMLMouseCursor_SizeNE | 10 | -
	HTML_MOUSE_CURSOR_SIZE_SW | k_EHTMLMouseCursor_SizeSW | 11 | -
	HTML_MOUSE_CURSOR_SIZE_W | k_EHTMLMouseCursor_SizeW | 12 | -
	HTML_MOUSE_CURSOR_SIZE_E | k_EHTMLMouseCursor_SizeE | 13 | -
	HTML_MOUSE_CURSOR_SIZE_N | k_EHTMLMouseCursor_SizeN | 14 | -
	HTML_MOUSE_CURSOR_SIZE_S | k_EHTMLMouseCursor_SizeS | 15 | -
	HTML_MOUSE_CURSOR_SIZE_WE | k_EHTMLMouseCursor_SizeWE | 16 | -
	HTML_MOUSE_CURSOR_SIZE_NS | k_EHTMLMouseCursor_SizeNS | 17 | -
	HTML_MOUSE_CURSOR_SIZE_ALL | k_EHTMLMouseCursor_SizeAll | 18 | -
	HTML_MOUSE_CURSOR_CURSOR_NO | k_EHTMLMouseCursor_No | 19 | -
	HTML_MOUSE_CURSOR_CURSOR_HAND | k_EHTMLMouseCursor_Hand | 20 | -
	HTML_MOUSE_CURSOR_CURSOR_BLANK | k_EHTMLMouseCursor_Blank | 21 | Don't show any custom cursor, just use your default.
	HTML_MOUSE_CURSOR_MIDDLE_PAN | k_EHTMLMouseCursor_MiddlePan | 22 | -
	HTML_MOUSE_CURSOR_NORTH_PAN | k_EHTMLMouseCursor_NorthPan | 23 | -
	HTML_MOUSE_CURSOR_NORTH_EAST_PAN | k_EHTMLMouseCursor_NorthEastPan | 24 | -
	HTML_MOUSE_CURSOR_EAST_PAN | k_EHTMLMouseCursor_EastPan | 25 | -
	HTML_MOUSE_CURSOR_SOUTH_EAST_PAN | k_EHTMLMouseCursor_SouthEastPan | 26 | -
	HTML_MOUSE_CURSOR_SOUTH_PAN | k_EHTMLMouseCursor_SouthPan | 27 | -
	HTML_MOUSE_CURSOR_SOUTH_WEST_PAN | k_EHTMLMouseCursor_SouthWestPan | 28 | -
	HTML_MOUSE_CURSOR_WEST_PAN | k_EHTMLMouseCursor_WestPan | 29 | -
	HTML_MOUSE_CURSOR_NORTH_WEST_PAN | k_EHTMLMouseCursor_NorthWestPan | 30 | -
	HTML_MOUSE_CURSOR_ALIAS | k_EHTMLMouseCursor_Alias | 31 | -
	HTML_MOUSE_CURSOR_CELL | k_EHTMLMouseCursor_Cell | 32 | -
	HTML_MOUSE_CURSOR_COL_RESIZE | k_EHTMLMouseCursor_ColResize | 33 | -
	HTML_MOUSE_CURSOR_COPY_CUR | k_EHTMLMouseCursor_CopyCur | 34 | -
	HTML_MOUSE_CURSOR_VERTICAL_TEXT | k_EHTMLMouseCursor_VerticalText | 35 | -
	HTML_MOUSE_CURSOR_ROW_RESIZE | k_EHTMLMouseCursor_RowResize | 36 | -
	HTML_MOUSE_CURSOR_ZOOM_IN | k_EHTMLMouseCursor_ZoomIn | 37 | -
	HTML_MOUSE_CURSOR_ZOOM_OUT | k_EHTMLMouseCursor_ZoomOut | 38 | -
	HTML_MOUSE_CURSOR_HELP | k_EHTMLMouseCursor_Help | 39 | -
	HTML_MOUSE_CURSOR_CUSTOM | k_EHTMLMouseCursor_Custom | 40 | -
	HTML_MOUSE_CURSOR_SIZE_NWSE | k_EHTMLMouseCursor_SizeNWSE | 41 | -
	HTML_MOUSE_CURSOR_SIZE_NESW | k_EHTMLMouseCursor_SizeNESW | 42 | -
	HTML_MOUSE_CURSOR_LAST | k_EHTMLMouseCursor_last | 43 | Only used to iterate over all cursors. Custom cursors start from this value and up.