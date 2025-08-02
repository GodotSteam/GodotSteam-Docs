---
title: Music Remote
description: A class reference for Music Remote.
icon: material/music-box-multiple
---

# Music Remote

Allows direct interaction with the Steam Music player. These functions only work with soundtracks you purchased or own on Steam. [See features / music_player for more information.](https://partner.steamgames.com/doc/features/music_player){ target="\_blank" }

This class has absolutely no notes in either the SDK nor Valve's online documentation; everything here is assumed.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### activationSuccess

!!! function "activationSuccess( `bool` activate )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | activate | bool | -

	Was the Music Remote activation successful?  It is unclear what **activate** actually refers to though or if this is setting the activation as successful.

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#BActivationSuccess){ .md-button .md-button--store target="_blank" }

### isCurrentMusicRemote

!!! function "isCurrentMusicRemote( )"
	Is a remote music client / host connected?

	!!! returns "Returns: bool"
		Return true if Music Remote is currently active; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#BIsCurrentMusicRemote){ .md-button .md-button--store target="_blank" }

### currentEntryDidChange

!!! function "currentEntryDidChange( )"
	Did the currenty music entry just change?

	!!! returns "Returns: bool"
		Returns true if the current music entry did change; otherwise, false if not.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#CurrentEntryDidChange){ .md-button .md-button--store target="_blank" }

### currentEntryIsAvailable

!!! function "currentEntryIsAvailable( `bool` available )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | available | bool | Whether or not the current entry is available.

	Is the current music entry available?

	!!! returns "Returns: bool"
		Returns true if the current music entry is available; otherwise, false if not.
 
    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#CurrentEntryIsAvailable){ .md-button .md-button--store target="_blank" }

### currentEntryWillChange

!!! function "currentEntryWillChange( )"
	Will the current music entry change?

	!!! returns "Returns: bool"
		Returns true if the current music entry will change; otherwise, false if not.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#CurrentEntryWillChange){ .md-button .md-button--store target="_blank" }

### deregisterSteamMusicRemote

!!! function "deregisterSteamMusicRemote( )"
	Disconnect from remote music client / host.

	!!! returns "Returns: bool"
		Returns true if Steam Music Remote was deregistered; otherwise, false if not.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#DeregisterSteamMusicRemote){ .md-button .md-button--store target="_blank" }

### enableLooped

!!! function "enableLooped( `bool` enable_loop )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | enable_loop | bool | Whether or not to enable track looping.

    Enable track looping.

	!!! returns "Returns: bool"
		Returns true if track looping was enabled; otherwise, false if not.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#EnableLooped){ .md-button .md-button--store target="_blank" }

### enablePlaylists

!!! function "enablePlaylists( `bool` enable_playlists )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | enable_playlists | bool | Whether or not to enable playlists.

	Enable playlists on client.

	!!! returns "Returns: bool"
		Returns true if playlists were enabled; otherwise, false if not.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#EnablePlaylists){ .md-button .md-button--store target="_blank" }

### enablePlayNext

!!! function "enablePlayNext( `bool` enable_next )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | enable_next | bool | Whether or not to enable play next.

	Enable the play next command?

	!!! returns "Returns: bool"
		Returns true if play next is enabled; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#EnablePlayNext){ .md-button .md-button--store target="_blank" }

### enablePlayPrevious

!!! function "enablePlayPrevious( `bool` enable_previous )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | enable_previous | bool | Whether or not to enable play previous.

	Play previous track on client.

	!!! returns "Returns: bool"
		Returns true if play previous is enabled; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#EnablePlayPrevious){ .md-button .md-button--store target="_blank" }

### enableQueue

!!! function "enableQueue( `bool` enable_queue )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | enable_queue | bool | Whether or not to enable the queue.

	Enable the music queue on the client.

	!!! returns "Returns: bool"
		Returns true if the queue is enabled; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#EnableQueue){ .md-button .md-button--store target="_blank" }

### enableShuffled

!!! function "enableShuffled( `bool` enable_shuffle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | enable_shuffle | bool | Whether or not to enable shuffle.

	Enable shuffle on the client.

	!!! returns "Returns: bool"
		Returns true if shuffle is enabled; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#EnableShuffled){ .md-button .md-button--store target="_blank" }

### playlistDidChange

!!! function "playlistDidChange( )"
	Has the playlist changed?

	!!! returns "Returns: bool"
		Returns true if the playlist changed; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#PlaylistDidChange){ .md-button .md-button--store target="_blank" }

### playlistWillChange

!!! function "playlistWillChange( )"
	Will the playlist change?

	!!! returns "Returns: bool"
		Returns true if the playlist will change; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#PlaylistWillChange){ .md-button .md-button--store target="_blank" }

### queueDidChange

!!! function "queueDidChange( )"
	Did the song queue change?

	!!! returns "Returns: bool"
		Returns true if the queue did change; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#QueueDidChange){ .md-button .md-button--store target="_blank" }

### queueWillChange

!!! function "queueWillChange( )"
	Will the song queue change?

	!!! returns "Returns: bool"
		Returns true if the queue will change; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#QueueWillChange){ .md-button .md-button--store target="_blank" }

### registerSteamMusicRemote

!!! function "registerSteamMusicRemote( `string` name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | name | string | THe name of this Music Remote session?

	Connect to a music remote client / host?

	!!! returns "Returns: bool"
		Returns true if the Music Remote session was registered successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#RegisterSteamMusicRemote){ .md-button .md-button--store target="_blank" }

### resetPlaylistEntries

!!! function "resetPlaylistEntries( )"
	Reset the playlist entries.

	!!! returns "Returns: bool"
		Returns true if the playlist entries were reset; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#ResetPlaylistEntries){ .md-button .md-button--store target="_blank" }

### resetQueueEntries

!!! function "resetQueueEntries( )"
	Reset the song queue entries.

	!!! returns "Returns: bool"
		Returns true if the queue entries were reset; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#ResetQueueEntries){ .md-button .md-button--store target="_blank" }

### setCurrentPlaylistEntry

!!! function "setCurrentPlaylistEntry( `int` id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | id | int | The current playlist entry to set.

	Set a new current playlist.

	!!! returns "Returns: bool"
		Returns true if the playlist entry was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#SetCurrentPlaylistEntry){ .md-button .md-button--store target="_blank" }

### setCurrentQueueEntry

!!! function "setCurrentQueueEntry( `int` id )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | id | int | The current queue entry to set.

	Set a new current song queue.

	!!! returns "Returns: bool"
		Returns true if the queue entry was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#SetCurrentQueueEntry){ .md-button .md-button--store target="_blank" }

### setDisplayName

!!! function "setDisplayName( `string` name )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | name | string | The display name to set.

	Set a new display name.

	!!! returns "Returns: bool"
		Returns true if the display name was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#SetDisplayName){ .md-button .md-button--store target="_blank" }

### setPlaylistEntry

!!! function "setPlaylistEntry( `int` id, `int` position, `string` entry_text )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | id | int | The ID of the entry to set.
    | position | int | The position in the playlist to set it to.
    | entry_text | string | The name of the entry.

	Set a new playlist entry.

	!!! returns "Returns: bool"
		Returns true if the playlist entry was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#SetPlaylistEntry){ .md-button .md-button--store target="_blank" }

### setPNGIcon64x64

!!! function "setPNGIcon64x64( `PackedByteArray` icon )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | icon | PackedByteArray | The icon data to set.

	Set a PNG icon for a song? A playlist?

	!!! returns "Returns: bool"
		Returns true if the icon was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#SetPNGIcon_64x64){ .md-button .md-button--store target="_blank" }

### setQueueEntry

!!! function "setQueueEntry( `int` id, `int` position, `string` entry_text )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | id | int | The ID of the entry to set.
    | position | int | The position in the queue to set it to.
    | entry_text | string | The name of the entry.

	Set a new queue entry.

	!!! returns "Returns: bool"
		Returns true if the queue entry was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#SetQueueEntry){ .md-button .md-button--store target="_blank" }

### updateCurrentEntryCoverArt

!!! function "updateCurrentEntryCoverArt( `PackedByteArray` art )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | art | PackedByteArray | The icon data to set.

	Update the current song entry's cover art.

	!!! returns "Returns: bool"
		Returns true if the current entry's cover art was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#UpdateCurrentEntryCoverArt){ .md-button .md-button--store target="_blank" }

### updateCurrentEntryElapsedSeconds

!!! function "updateCurrentEntryElapsedSeconds( `int` seconds )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | seconds | int | The amount of elapsed seconds to set.

	Update the current seconds that have elapsed for an entry.

	!!! returns "Returns: bool"
		Returns true if the elapsed seconds were set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#UpdateCurrentEntryElapsedSeconds){ .md-button .md-button--store target="_blank" }

### updateCurrentEntryText

!!! function "updateCurrentEntryText( `string` text )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | text | string | The text to set for the current entry.

	Update the current song entry's text?

	!!! returns "Returns: bool"
		Returns true if the current entry's text was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#UpdateCurrentEntryText){ .md-button .md-button--store target="_blank" }

### updateLooped

!!! function "updateLooped( `bool` looped )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | looped | bool | Whether or not to update the looped status?

	Update looped or not.

	!!! returns "Returns: bool"
		Returns true if the looped status was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#UpdateLooped){ .md-button .md-button--store target="_blank" }

### updatePlaybackStatus

!!! function "updatePlaybackStatus( `AudioPlaybackStatus` status )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | status | [AudioPlaybackStatus enum](music.md#audioplaybackstatus) | The current playback status to set.

	Update the current playback status.

	!!! returns "Returns: bool"
		Returns true if the playback status was set successfully; otherwise, false.
 
    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#UpdatePlaybackStatus){ .md-button .md-button--store target="_blank" }

### updateShuffled

!!! function "updateShuffled( `bool` shuffle )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | shuffle | bool | Whether or not to update shuffle?

	Update whether to shuffle or not.

	!!! returns "Returns: bool"
		Returns true if shuffled was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#UpdateShuffled){ .md-button .md-button--store target="_blank" }

### updateVolume

!!! function "updateVolume( `float` volume )"
	| :material-variable: Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | volume | float | The updated volume level to set.

	Volume is between 0.0 and 1.0.

	!!! returns "Returns: bool"
		Returns true if the volume was set successfully; otherwise, false.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#UpdateVolume){ .md-button .md-button--store target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### music_player_remote_to_front

!!! function "music_player_remote_to_front"
	There are no notes in the Steamworks documentation.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerRemoteToFront_t){ .md-button .md-button--store target="_blank" }

### music_player_remote_will_activate
!!! function "music_player_remote_will_activate"
	Signals when the Music Remote session will activate.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerRemoteWillActivate_t){ .md-button .md-button--store target="_blank" }
	
### music_player_remote_will_deactivate

!!! function "music_player_remote_will_deactivate"
	Signals when the Music Remote session will deactivate.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerRemoteWillDeactivate_t){ .md-button .md-button--store target="_blank" }

### music_player_selects_playlist_entry

!!! function "music_player_selects_playlist_entry"
	Signals when a playlist entry has been selected.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | entry | int | The entry that was selected.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerSelectsPlaylistEntry_t){ .md-button .md-button--store target="_blank" }

### music_player_selects_queue_entry

!!! function "music_player_selects_queue_entry"
	Signals when a queue entry has been selected.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | entry | int | The entry that was selected.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerSelectsQueueEntry_t){ .md-button .md-button--store target="_blank" }

### music_player_wants_looped

!!! function "music_player_wants_looped"
	Signals when the remote session wants a track looped.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| looped | bool | Whether or not the remote session selected looped.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWantsLooped_t){ .md-button .md-button--store target="_blank" }

### music_player_wants_pause

!!! function "music_player_wants_pause"
	Signals when the remote session wants music paused.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWantsPause_t){ .md-button .md-button--store target="_blank" }

### music_player_wants_play

!!! function "music_player_wants_play"
	Signals when the remote session wants music played.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWantsPlay_t){ .md-button .md-button--store target="_blank" }

### music_player_wants_play_next

!!! function "music_player_wants_play_next"
	Signals when the remote session wants the next entry played.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWantsPlayNext_t){ .md-button .md-button--store target="_blank" }

### music_player_wants_play_previous

!!! function "music_player_wants_play_previous"
	Signals when the remote session wants the previous entry played.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWantsPlay_t){ .md-button .md-button--store target="_blank" }

### music_player_wants_playing_repeat_status

!!! function "music_player_wants_playing_repeat_status"
	Signals when the remote session has a playing repeat status update.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
        | status | int | -

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWantsPlayingRepeatStatus_t){ .md-button .md-button--store target="_blank" }

### music_player_wants_shuffled

!!! function "music_player_wants_shuffled"
	Signals when the Steam music interface wants to change shuffled status.

	!!! returns "Returns"
        | Key | Type | Notes |
        | --- | ---- | ----- |
		| shuffled | bool | Whether it is shuffled or not.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWantsShuffled_t){ .md-button .md-button--store target="_blank" }

### music_player_wants_volume

!!! function "music_player_wants_volume"
	Signals the Steam music interface has a new volume.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| new_volume | float | The new volume level; between 0.0 and 1.0.
	
	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWantsVolume_t){ .md-button .md-button--store target="_blank" }

### music_player_will_quit

!!! function "music_player_will_quit"
	Signals that the Steam music interface will now quit.

	!!! returns "Returns"
		Nothing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamMusicRemote#MusicPlayerWillQuit_t){ .md-button .md-button--store target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
MUSIC_NAME_MAX_LENGTH | k_SteamMusicNameMaxLength | 255 | -
MUSIC_PNG_MAX_LENGTH | k_SteamMusicPNGMaxLength | 65535 | -