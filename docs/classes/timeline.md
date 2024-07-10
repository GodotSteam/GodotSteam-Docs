# Timeline

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### addTimelineEvent

!!! function "addTimelineEvent( ```string``` icon, ```string``` title, ```string``` description, ```int32_t``` priority, ```float``` start_offset, ```float``` duration, ```int``` possible_clip )"
	Use this to mark an event on the Timeline. The event can be instantaneous or take some amount of time to complete, depending on the value passed in duration.

	**Returns:** void

### clearTimelineStateDescription

!!! function "clearTimelineStateDescription( ```float``` time_delta )"
	Removes the description set for the specific clip.

	**Returns:** void

### setTimelineGameMode

!!! function "setTimelineGameMode( ```int``` mode )"
	Changes the color of the timeline bar. See TimelineGameMode comments for how to use each value.

	**Returns:** void

### setTimelineStateDescription

!!! function "setTimelineStateDescription( ```string``` description, ```float``` time_delta )"
	Sets a description for the current game state in the timeline. These help the user to find specific moments in the timeline when saving clips. Setting a new state description replaces any previous description.

	**Returns:** void

{==
## :material-numeric: Enums
==}

### TimelineGameMode

Enumerator | Value
---------- | -----
TIMELINE_GAME_MODE_INVALID | 0
TIMELINE_GAME_MODE_PLAYING | 1
TIMELINE_GAME_MODE_STAGING | 2
TIMELINE_GAME_MODE_MENUS | 3
TIMELINE_GAME_MODE_LOADING_SCREEN | 4
TIMELINE_GAME_MODE_MAX

### TimelineEventClipPriority

Enumerator | Value
---------- | -----
TIMELINE_EVENT_CLIP_PRIORITY_INVALID | 0
TIMELINE_EVENT_CLIP_PRIORITY_NONE | 1
TIMELINE_EVENT_CLIP_PRIORITY_STANDARD | 2
TIMELINE_EVENT_CLIP_PRIORITY_FEATURED | 3