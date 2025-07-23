---
title: Video
description: A class reference for Steam Video.
icon: material/video
---

# Video

Provides functions to interface with the Steam video and broadcasting platforms. See [Steam Video](https://partner.steamgames.com/doc/features/streaming_video){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### getOPFSettings

!!! function "getOPFSettings( `uint32_t` app_id )"
    | :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The video app ID to get the OPF details of. |

    Get the OPF details for 360 video playback. 

    To retrieve the 360 OPF (open projection format) data to playback a 360 video, start by making a call to this, then the callback will indicate whether the request was successful. If it was successful, the actual OPF JSON data can be retrieved with a call to [getOPFStringForApp](#getopfstringforapp).

    !!! returns "Returns: void"

    !!! trigger "Triggers"
		[get_opf_settings_result](#get_opf_settings_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamVideo#GetOPFSettings){ .md-button .md-button--doc_classes target="_blank" }

### getOPFStringForApp

!!! function "getOPFStringForApp( `uint32_t` app_id )"
    | :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The video app ID to get the OPF string for. |

    Gets the OPF string for the specified video app ID.

    Once the callback for [get_opf_settings_result](#get_opf_settings_result) has been raised and the EResult indicates success, then calling this will return back the actual OPF data in a JSON format. The size of the OPF string varies, but at this time 48,000 bytes should be sufficient to contain the full string.

    **Note:** The data returned in a successful call to [getOPFStringForApp](#getopfstringforapp) can only be retrieved once. If you need to retrieve it multiple times, a call to [getOPFSettings](#getopfsettings) will need to be made each time.

    !!! returns "Returns: string"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamVideo#GetOPFStringForApp){ .md-button .md-button--doc_classes target="_blank" }

### getVideoURL

!!! function "getVideoURL( `uint32_t` app_id )"
    | :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The video app ID to receive the video stream for. |

    Asynchronously gets the URL suitable for streaming the video associated with the specified video app ID. 

    [See DASH on Wikipedia.](https://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP){ target="\_blank"}

    !!! returns "Returns: void"

    !!! trigger "Triggers"
		[get_video_result](#get_video_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamVideo#GetVideoURL){ .md-button .md-button--doc_classes target="_blank" }

### isBroadcasting

!!! function "isBroadcasting( )"
    Checks if the user is currently live broadcasting and gets the number of users.

    !!! returns "Returns: dictionary"
        Contains the following keys:

        | Key | Type | Notes |
        | --- | ---- | ----- |
        | broadcasting | bool | Returns true if user is uploading a live broadcast; otherwise, false.
        | viewers | int | The number of viewers if the user is currently broadcasting.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamVideo#IsBroadcasting){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### broadcast_upload_start

!!! function "broadcast_upload_start"
    Automatically called whenever the user starts broadcasting.

    !!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | is_rtmp | bool | Whether or not this is RTMP.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamVideo#BroadcastUploadStart_t){ .md-button .md-button--doc_classes target="_blank" }
    [ :material-tag-plus: Added GodotSteam 4.13](../changelog/godot4.md/#version-413){ .md-button .md-button--changes target="_blank" }

### broadcast_upload_stop

!!! function "broadcast_upload_stop"
    Automatically called whenever the user stops broadcasting.

    !!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| result | (int / [BroadcastUploadResult](main.md/#broadcastuploadresult)) | The reason why the broadcast has stopped.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamVideo#BroadcastUploadStop_t){ .md-button .md-button--doc_classes target="_blank" }
    [ :material-tag-plus: Added GodotSteam 4.13](../changelog/godot4.md/#version-413){ .md-button .md-button--changes target="_blank" }

### get_opf_settings_result

!!! function "get_opf_settings_result"
	Triggered when the OPF Details for 360 video playback are retrieved. After receiving this you can use [getOPFStringForApp](#getopfstringforapp) to access the OPF details.

	!!! returns "Returns"
    	| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | int / [Result enum](main.md#result) | The result of the operation.
        | app_id | uint32_t | The app ID of the video that we got the details of.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamVideo#GetOPFSettingsResult_t){ .md-button .md-button--doc_classes target="_blank" }

### get_video_result

!!! function "get_video_result"
	Provides the result of a call to [getVideoURL](#getvideourl).

    !!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | result | int / [Result enum](main.md#result) | The result of the operation. May be [RESULT_INVALID_PARAM](main.md#result) if the app ID provided is not a video app ID or if the user does not own a license for it.
        | app_id | uint32_t | The App ID provided in the original call to [getVideoURL](#getvideourl).
        | url | string | If the call was successful this contains the URL to the [MPEG-DASH Standard schema](http://en.wikipedia.org/wiki/Dynamic_Adaptive_Streaming_over_HTTP){ target="\_blank" } formatted xml document which can be used to stream the content.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamVideo#GetVideoURLResult_t){ .md-button .md-button--doc_classes target="_blank" }
