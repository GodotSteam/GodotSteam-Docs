---
title: Timeline
description: A class reference for Timeline.
icon: material/timeline
---

# Timeline

Functions that allow the game to add events to the timeline that is displayed alongside recorded video.  See [Steam Timelines](https://partner.steamgames.com/doc/features/timeline){ target="\_blank" } for more information.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### addGamePhaseTag

!!! function "addGamePhaseTag( `string` tag_name, `string` tag_icon, `string` tag_group, `uint32_t` priority )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | tag_name | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage).
    | tag_icon | string | The name of the icon to show when the tag is shown in the UI. This can be one of the icons uploaded through the Steamworks partner site for your title, or one of the provided icons that start with steam_. [The Steam Timelines overview includes a list of available icons.](https://partner.steamgames.com/doc/features/timeline#icons){ target="_blank" } |
    | tag_group | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). Tags within the same group will be shown together in the UI. |
    | priority | uint32_t | Provide the priority to use when the UI is deciding which icons to display. Tags with larger priority values will be displayed more prominently than tags with smaller priority values. This value must be between 0 and [MAX_TIMELINE_PRIORITY](#constants). |

	Use this to add a game phase tag (F). Phase tags represent data with a well defined set of options, which could be data such as match resolution, hero played, game mode, etc. Tags can have an icon in addition to a text name. Multiple tags within the same group may be added per phase and all will be remembered.

	For example, addGamePhaseTag may be called multiple times for a "Bosses Defeated" group, with different names and icons for each boss defeated during the phase, all of which will be shown to the user.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#AddGamePhaseTag){ .md-button .md-button--doc_classes target="_blank" }

### addInstantaneousTimelineEvent

!!! function "addInstantaneousTimelineEvent( `string` title, `string` description, `string` icon, `uint32_t` icon_priority, `float` start_offset_seconds, `TimelineEventClipPriority` possible_clip = TIMELINE_EVENT_CLIP_PRIORITY_NONE )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | title | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | description | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | icon | string | The name of the icon to show at the timeline at this point. This can be one of the icons uploaded through the Steamworks partner site for your title, or one of the provided icons that start with steam_. [The Steam Timelines overview includes a list of available icons.](https://partner.steamgames.com/doc/features/timeline#icons){ target="_blank" } |
    | icon_priority | uint32_t | Provide the priority to use when the UI is deciding which icons to display in crowded parts of the timeline. Events with larger priority values will be displayed more prominently than events with smaller priority values. This value must be between 0 and [MAX_TIMELINE_PRIORITY](#constants). |
    | start_offset_seconds | float | The time offset in seconds to apply to the start of the event. Negative times indicate an event that happened in the past. |
	| possible_clip | [TimelineEventClipPriority enum](#timelineeventclippriority) | Allows the game to describe events that should be suggested to the user as possible video clips. | 

	Use this to mark an event (A) on the Timeline. This event will be instantaneous. (See [addRangeTimelineEvent](#addrangetimelineevent) to add events that happened over time.)

	Examples could include:

	* picking up a new weapon or ammo
	* scoring a goal

	One use of **start_offset_seconds** is to handle events whose significance is not clear until after the fact. For instance if the player starts a damage over time effect on another player, which kills them 3.5 seconds later, the game could pass -3.5 as the start offset and cause the event to appear in the timeline where the effect started.

	The game can nominate an event as being suitable for a clip by passing [TIMELINE_EVENT_CLIP_PRIORITY_STANDARD](#timelineeventclippriority) or [TIMELINE_EVENT_CLIP_PRIORITY_FEATURED](#timelineeventclippriority) to **possible_clip**. Players can make clips of their own at any point, but this lets the game suggest some options to Steam to make that process easier for players.

	!!! returns "Returns: uint64_t"
        Returns a timeline event handle that can be used with [removeTimelineEvent](#removetimelineevent) or the overlay functions [doesEventRecordingExist](#doeseventrecordingexist) and [openOverlayToTimelineEvent](#openoverlaytotimelineevent).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#AddInstantaneousTimelineEvent){ .md-button .md-button--doc_classes target="_blank" }

### addRangeTimelineEvent

!!! function "addRangeTimelineEvent( `string` title, `string` description, `string` icon, `uint32_t` icon_priority, `float` start_offset_seconds, `float` duration, `TimelineEventClipPriority` possible_clip = TIMELINE_EVENT_CLIP_PRIORITY_NONE )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | title | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | description | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | icon | string | The name of the icon to show at the timeline at this point. This can be one of the icons uploaded through the Steamworks partner site for your title, or one of the provided icons that start with steam_. [The Steam Timelines overview includes a list of available icons.](https://partner.steamgames.com/doc/features/timeline#icons){ target="_blank" } |
    | icon_priority | uint32_t | Provide the priority to use when the UI is deciding which icons to display in crowded parts of the timeline. Events with larger priority values will be displayed more prominently than events with smaller priority values. This value must be between 0 and [MAX_TIMELINE_PRIORITY](#constants). |
    | start_offset_seconds | float | The time offset in seconds to apply to the start of the event. Negative times indicate an event that happened in the past. |
	| duration | float | The duration of the event, in seconds. Pass 0 for instantaneous events. |
	| possible_clip | [TimelineEventClipPriority enum](#timelineeventclippriority) | Allows the game to describe events that should be suggested to the user as possible video clips. |

	Use this to mark an event (A) on the Timeline that takes some amount of time to complete.

	Examples could include:

	* a boss battle
	* an impressive combo
	* a large team fight

	One use of **start_offset_seconds** is to handle events whose significance is not clear until after the fact. For instance if the player starts a damage over time effect on another player, which kills them 3.5 seconds later, the game could pass -3.5 as the start offset and cause the event to appear in the timeline where the effect started.

	The game can nominate an event as being suitable for a clip by passing [TIMELINE_EVENT_CLIP_PRIORITY_STANDARD](#timelineeventclippriority) or [TIMELINE_EVENT_CLIP_PRIORITY_FEATURED](#timelineeventclippriority) to **possible_clip**. Players can make clips of their own at any point, but this lets the game suggest some options to Steam to make that process easier for players.

	!!! returns "Returns: uint64_t"
        Returns a timeline event handle that can be used with [removeTimelineEvent](#removetimelineevent) or the overlay functions [doesEventRecordingExist](#doeseventrecordingexist) and [openOverlayToTimelineEvent](#openoverlaytotimelineevent).

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#AddRangeTimelineEvent){ .md-button .md-button--doc_classes target="_blank" }

### clearTimelineTooltip

!!! function "clearTimelineTooltip( `float` time_delta )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | time_delta | float | The time offset in seconds to apply to this state change. Negative times indicate an event that happened in the past. |

    Clears the previous set game state in the timeline.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#ClearTimelineTooltip){ .md-button .md-button--doc_classes target="_blank" }

### doesEventRecordingExist

!!! function "doesEventRecordingExist( `uint64_t` timeline_event_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | timeline_event_handle | uint64_t | Handle of the event to check for recordings. |

    Use this to determine if video recordings exist for the specified event. Steam will sent a [timeline_event_recording_exists](#timeline_event_recording_exists) callback with the result. This can be useful when the game needs to decide whether or not to show a control that will call [openOverlayToTimelineEvent](#openoverlaytotimelineevent).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		 [timeline_event_recording_exists](#timeline_event_recording_exists) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#DoesEventRecordingExist){ .md-button .md-button--doc_classes target="_blank" }

### doesGamePhaseRecordingExist

!!! function "doesGamePhaseRecordingExist( `string` phase_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | phase_id | string | A game-provided persistent ID for a game phase. |

    Use this to determine if video recordings exist for the specified game phase. Steam will sent a [timeline_game_phase_recording_exists](#timeline_game_phase_recording_exists) callback with the result. This can be useful when the game needs to decide whether or not to show a control that will call [openOverlayToGamePhase](#openoverlaytogamephase).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		 [timeline_game_phase_recording_exists](#timeline_game_phase_recording_exists) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#DoesGamePhaseRecordingExist){ .md-button .md-button--doc_classes target="_blank" }

### endGamePhase

!!! function "endGamePhase( )"
	Use this to end a game phase that was started with [startGamePhase](#startgamephase).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#EndGamePhase){ .md-button .md-button--doc_classes target="_blank" }

### endRangeTimelineEvent

!!! function "endRangeTimelineEvent( `uint64_t` this_event, `float` end_offset_seconds )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | timeline_event_handle | uint64_t | Handle of the event to end. |
    | end_offset_seconds | float | The time offset in seconds to apply to the end of the event. Negative times indicate an event that happened in the past. |

	Ends a range timeline event and shows it in the UI.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#EndRangeTimelineEvent){ .md-button .md-button--doc_classes target="_blank" }

### openOverlayToGamePhase

!!! function "openOverlayToGamePhase( `string` phase_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | phase_id | string | A game-provided persistent ID for a game phase. |

	Opens the Steam overlay to the section of the timeline represented by the game phase.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#OpenOverlayToGamePhase){ .md-button .md-button--doc_classes target="_blank" }

### openOverlayToTimelineEvent

!!! function "openOverlayToTimelineEvent( `uint64_t` timeline_event_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | timeline_event_handle | uint64_t | Handle of the event to end. |

    Opens the Steam overlay to the section of the timeline represented by the timeline event. This event must be in the current game session, since timeline event handle values are not valid for future runs of the game.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#OpenOverlayToTimelineEvent){ .md-button .md-button--doc_classes target="_blank" }

### removeTimelineEvent

!!! function "removeTimelineEvent( `uint64_t` timeline_event_handle )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | timeline_event_handle | uint64_t | Handle of the event to end. |

    Use this to remove an event that was added with [addInstantaneousTimelineEvent](#addinstantaneoustimelineevent) or [addRangeTimelineEvent](#addrangetimelineevent) or an event that is in progress and was started with [startRangeTimelineEvent](#startrangetimelineevent).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#RemoveTimelineEvent){ .md-button .md-button--doc_classes target="_blank" }

### setGamePhaseAttribute

!!! function "setGamePhaseAttribute( `string` attribute_group, `string` attribute_value, `uint32_t` priority )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | attribute_group | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | attribute_value | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | priority | uint32_t | Provide the priority to use when the UI is deciding which icons to display. Tags with larger priority values will be displayed more prominently than tags with smaller priority values. This value must be between 0 and [MAX_TIMELINE_PRIORITY](#constants). |

	Use this to add a game phase attribute (E). Phase attributes represent generic text fields that can be updated throughout the duration of the phase. They are meant to be used for phase metadata that is not part of a well defined set of options.

    For example, a KDA attribute that starts with the value "0/0/0" and updates as the phase progresses, or something like a played-entered character name. Attributes can be set as many times as the game likes with [setGamePhaseAttribute](#setgamephaseattribute), and only the last value will be shown to the user.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#SetGamePhaseAttribute){ .md-button .md-button--doc_classes target="_blank" }

### setGamePhaseID

!!! function "setGamePhaseID( `string` phase_id )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | phase_id | string | A game-provided persistent ID for a game phase. |

	The phase ID is used to let the game identify which phase it is referring to in calls to [doesGamePhaseRecordingExist](#doesgamephaserecordingexist) or [openOverlayToGamePhase](#openoverlaytogamephase). It may also be used to associated multiple phases with each other.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#SetGamePhaseID){ .md-button .md-button--doc_classes target="_blank" }

### setTimelineGameMode

!!! function "setTimelineGameMode( `TimelineGameMode` mode )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | mode | [TimelineGameMode enum](#timelinegamemode) | The mode that the game is in. |

    Changes the color of the timeline bar (C). See [TimelineGameMode](#timelinegamemode) for how to use each value.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#SetTimelineGameMode){ .md-button .md-button--doc_classes target="_blank" }

### setTimelineTooltip

!!! function "setTimelineTooltip( `string` description, `float` time_delta )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | description | string | A localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | time_delta | float | The time offset in seconds to apply to this state change. Negative times indicate an event that happened in the past. |

    Sets a description (B) for the current game state in the timeline. These help the user to find specific moments in the timeline when saving clips. Setting a new state description replaces any previous description.

	Examples could include:

	* Where the user is in the world in a single player game
	* Which round is happening in a multiplayer game
	* The current score for a sports game

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#SetTimelineTooltip){ .md-button .md-button--doc_classes target="_blank" }

### startGamePhase

!!! function "startGamePhase( )"
	Use this to start a game phase. Game phases allow the user to navigate their background recordings and clips. Exactly what a game phase means will vary game to game, but the game phase should be a section of gameplay that is usually between 10 minutes and a few hours in length, and should be the main way a user would think to divide up the game. These are presented to the user in a UI that shows the date the game was played, with one row per game slice. Game phases should be used to mark sections of gameplay that the user might be interested in watching.

	Examples could include:
	* A single match in a multiplayer PvP game
	* A chapter of a story-based singleplayer game
	* A single run in a roguelike

	Game phases are started with [startGamePhase](#startgamephase), and while a phase is still happening, they can have tags and attributes added to them with [addGamePhaseTag](#addgamephasetag) or [setGamePhaseAttribute](#setgamephaseattribute). Only one game phase can be active at a time.

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#StartGamePhase){ .md-button .md-button--doc_classes target="_blank" }

### startRangeTimelineEvent

!!! function "startRangeTimelineEvent( `string` title, `string` description, `string` icon, `uint32_t` priority, `float` start_offset_seconds, `TimelineEventClipPriority` possible_clip = TIMELINE_EVENT_CLIP_PRIORITY_NONE )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | title | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | description | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | icon | string | The name of the icon to show at the timeline at this point. This can be one of the icons uploaded through the Steamworks partner site for your title, or one of the provided icons that start with steam_. [The Steam Timelines overview includes a list of available icons.](https://partner.steamgames.com/doc/features/timeline#icons){ target="_blank" } |
    | priority | uint32_t | Provide the priority to use when the UI is deciding which icons to display. Tags with larger priority values will be displayed more prominently than tags with smaller priority values. This value must be between 0 and [MAX_TIMELINE_PRIORITY](#constants). |
    | start_offset_seconds | float | The time offset in seconds to apply to the start of the event. Negative times indicate an event that happened in the past. |
    | possible_clip | [TimelineEventClipPriority enum](#timelineeventclippriority) | Allows the game to describe events that should be suggested to the user as possible video clips. |

    Use this to mark the start of an event (A) on the Timeline that takes some amount of time to complete. The duration of the event is determined by a matching call to [endRangeTimelineEvent](#endrangetimelineevent). If the game wants to cancel an event in progress, they can do that with a call to [removeTimelineEvent](#removetimelineevent).

	The event in progress can be updated any number of times with [updateRangeTimelineEvent](#updaterangetimelineevent).

	The game can nominate an event as being suitable for a clip by passing [TIMELINE_EVENT_CLIP_PRIORITY_STANDARD](#timelineeventclippriority) or [TIMELINE_EVENT_CLIP_PRIORITY_FEATURED](#timelineeventclippriority) to **possible_clip**. Players can make clips of their own at any point, but this lets the game suggest some options to Steam to make that process easier for players.

	!!! returns "Returns: uint64_t"
        Returns a timeline event handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#StartRangeTimelineEvent){ .md-button .md-button--doc_classes target="_blank" }

### updateRangeTimelineEvent

!!! function "updateRangeTimelineEvent( `uint64_t` this_event, `string` title, `string` description, `string` icon, `uint32_t` priority, `TimelineEventClipPriority` possible_clip = TIMELINE_EVENT_CLIP_PRIORITY_NONE )"
	| Argument | Type | Notes |
    | -------- | ---- | ----- |
    | timeline_event_handle | uint64_t | Handle of the event to check for recordings. |
    | title | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | description | string | Title-provided localized string in the language returned by [getSteamUILanguage](utils.md#getsteamuilanguage). |
    | icon | string | The name of the icon to show at the timeline at this point. This can be one of the icons uploaded through the Steamworks partner site for your title, or one of the provided icons that start with steam_. [The Steam Timelines overview includes a list of available icons.](https://partner.steamgames.com/doc/features/timeline#icons){ target="_blank" } |
    | priority | uint32_t | Provide the priority to use when the UI is deciding which icons to display. Tags with larger priority values will be displayed more prominently than tags with smaller priority values. This value must be between 0 and [MAX_TIMELINE_PRIORITY](#constants). |
    | possible_clip | [TimelineEventClipPriority enum](#timelineeventclippriority) | Allows the game to describe events that should be suggested to the user as possible video clips. |

	Use this to update the details of an event that was started with [startRangeTimelineEvent](#startrangetimelineevent).

	!!! returns "Returns: void"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#UpdateRangeTimelineEvent){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### timeline_event_recording_exists

!!! function "timeline_event_recording_exists"
	Called when asking if recordings exist for an event handle.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| event_id | uint64_t | The handle of the event that was asked about.
    	| recording_exists | bool | This is true if recording exists for the requested event handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#SteamTimelineEventRecordingExists_t){ .md-button .md-button--doc_classes target="_blank" }

### timeline_game_phase_recording_exists

!!! function "timeline_game_phase_recording_exists"
	Called when asking if recordings exist for a game phase ID.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
    	| phase_id | string | The phase ID that this result corresponds with.
    	| recording_ms | uint64_t | The phase ID that this result corresponds with.
    	| longest_clips_ms | uint64_t | The total length of the longest clip in this phase in milliseconds.
    	| clip_count | uint32_t | The number of clips that include video from this phase.
    	| screenshot_count | uint32_t | The number of screenshots the user has from this phase.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamTimeline#SteamTimelineGamePhaseRecordingExists_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Details
---- | -------- | ----- | -------
MAX_PHASE_ID_LENGTH | k_cchMaxPhaseIDLength | 64 | -
MAX_TIMELINE_PRIORITY | k_unMaxTimelinePriority | 1000 | -
MAX_TIMELINE_EVENT_DURATION | k_flMaxTimelineEventDuration | 600.f | -
TIMELINE_PRIORITY_KEEP_CURRENT_VALUE | k_unTimelinePriority_KeepCurrentValue | 1000000 | Use with [updateRangeTimelineEvent enum](#updaterangetimelineevent) to not change the priority.

{==
## :material-numeric: Enums
==}

### TimelineGameMode

Controls the color of the timeline bar segments. The value names listed here map to a multiplayer game, where the user starts a game (in menus), then joins a multiplayer session that first has a character selection lobby then finally the multiplayer session starts. However, you can also map these values to any type of game. In a single player game where you visit towns & dungeons, you could set TIMELINE_GAME_MODE_MENUS when the player is in a town buying items, TIMELINE_GAME_MODE_STAGING for when a dungeon is loading and TIMELINE_GAME_MODE_PLAYING for when inside the dungeon fighting monsters.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
TIMELINE_GAME_MODE_INVALID | k_ETimelineGameMode_Invalid | 0 | -
TIMELINE_GAME_MODE_PLAYING | k_ETimelineGameMode_Playing | 1 | -
TIMELINE_GAME_MODE_STAGING | k_ETimelineGameMode_Staging | 2 | -
TIMELINE_GAME_MODE_MENUS | k_ETimelineGameMode_Menus | 3 | -
TIMELINE_GAME_MODE_LOADING_SCREEN | k_ETimelineGameMode_LoadingScreen | 4 | -
TIMELINE_GAME_MODE_MAX | k_ETimelineGameMode_Max | - | One past the last valid value.

### TimelineEventClipPriority

Used where Featured events will be offered before Standard events.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
TIMELINE_EVENT_CLIP_PRIORITY_INVALID | k_ETimelineEventClipPriority_Invalid | 0 | -
TIMELINE_EVENT_CLIP_PRIORITY_NONE | k_ETimelineEventClipPriority_None | 1 | -
TIMELINE_EVENT_CLIP_PRIORITY_STANDARD | k_ETimelineEventClipPriority_Standard | 2 | -
TIMELINE_EVENT_CLIP_PRIORITY_FEATURED | k_ETimelineEventClipPriority_Featured | 3 | -