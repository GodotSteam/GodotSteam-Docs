---
title: Music
description: A class reference for Music.
icon: material/music
---

# Music

Functions to control music playback in the Steam client. This gives games the opportunity to do things like pause the music or lower the volume when an important cut scene is shown, and then start playing again afterwards. These functions only work with soundtracks you purchased or own on Steam.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### getPlaybackStatus

!!! function "getPlaybackStatus( )"
	Gets the current status of the Steam Music player.

	!!! returns "Returns: [AudioPlaybackStatus enum](#audioplaybackstatus)"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#GetPlaybackStatus){ .md-button .md-button--store target="_blank" }

### musicIsEnabled

!!! function "musicIsEnabled( )"
	Checks if Steam Music is enabled.

	!!! returns "Returns: bool"
		Returns true if it is enabled; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#BIsEnabled){ .md-button .md-button--store target="_blank" }

### musicIsPlaying

!!! function "musicIsPlaying( )"
	Checks if Steam Music is active. This does not necessarily a song is currently playing, it may be paused.

	For finer grain control use [getPlaybackStatus](#getplaybackstatus).

	!!! returns "Returns: bool"
		Returns true if a song is currently playing, paused, or queued up to play; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#BIsPlaying){ .md-button .md-button--store target="_blank" }

### musicGetVolume

!!! function "musicGetVolume( )"
	Gets the current volume of the Steam Music player.

	!!! returns "Returns: float"
		The volume is returned as a percentage between 0.0 and 1.0.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#GetVolume){ .md-button .md-button--store target="_blank" }

### musicPause

!!! function "musicPause( )"
	Pause the Steam Music player.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#Pause){ .md-button .md-button--store target="_blank" }

### musicPlay

!!! function "musicPlay( )"
	Have the Steam Music player resume playing.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#Play){ .md-button .md-button--store target="_blank" }

### musicPlayNext

!!! function "musicPlayNext( )"
	Have the Steam Music player skip to the next song.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#PlayNext){ .md-button .md-button--store target="_blank" }

### musicPlayPrev

!!! function "musicPlayPrev( )"
	Have the Steam Music player play the previous song.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#PlayPrevious){ .md-button .md-button--store target="_blank" }

### musicSetVolume

!!! function "musicSetVolume( `float` value )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | value | float | The volume percentage to set from 0.0 to 1.0.

	Sets the volume of the Steam Music player.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusic#SetVolume){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### music_playback_status_has_changed

!!! function "music_playback_status_has_changed"
	No notes about this in the Steam docs, but we can assume it just updates us about the playback status.

	!!! returns "Returns"
		Nothing.

### music_volume_has_changed

!!! function "music_volume_has_changed"
	No notes about this in the Steam docs, but we can assume it just updates us about the volume changes.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| new_volume | float | -

{==
## :material-numeric: Enums
==}

### AudioPlaybackStatus

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
AUDIO_PLAYBACK_UNDEFINED | AudioPlayback_Undefined | 0 | The Steam music interface probably isn't enabled.
AUDIO_PLAYBACK_PLAYING | AudioPlayback_Playing | 1 | Steam Music is currently playing.
AUDIO_PLAYBACK_PAUSED | AudioPlayback_Paused | 2 | Steam Music is currently paused.
AUDIO_PLAYBACK_IDLE | AudioPlayback_Idle | 3 | Steam Music is currently stopped.
