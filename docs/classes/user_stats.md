---
title: User Stats
description: A class reference for User Statistics.
icon: material/clipboard-edit
---

# User Stats

Provides functions for accessing and submitting stats, achievements, and leaderboards.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### attachLeaderboardUGC

!!! function "attachLeaderboardUGC( `uint64_t` ugc_handle, `uint64_t` this_leaderboard = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | ugc_handle | uint64_t | Handle to a piece of user generated content that was shared using [fileShare.](remote_storage.md#fileshare) |
    | this_leaderboard | uint64_t | A leaderboard handle obtained from [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard). Defaults to 0. |

	If no this_leaderboard is passed, then the function will use the last internally-stored handle.

	Attaches a piece of user generated content the current user's entry on a leaderboard.

	This content could be a replay of the user achieving the score or a ghost to race against. The attached handle will be available when the entry is retrieved and can be accessed by other users using the [leaderboard_scores_downloaded](#leaderboard_scores_downloaded) callback. To create and download user generated content see the documentation for the Steam Workshop.

	Once attached, the content will be available even if the underlying Cloud file is changed or deleted by the user.

	You must call [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard) to get a leaderboard handle prior to calling this function.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[leaderboard_ugc_set](#leaderboard_ugc_set) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#AttachLeaderboardUGC){ .md-button .md-button--doc_classes target="_blank" }

### clearAchievement

!!! function "clearAchievement( `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to reset; uses the name set in the Steamworks back-end. |

	Resets the unlock status of an achievement.

	This is primarily only ever used for testing.

	This call only modifies Steam's in-memory state so it is quite cheap. To send the unlock status to the server and to trigger the Steam overlay notification you must call [storeStats](#storestats).

	!!! returns "Returns: bool"
		Returns true upon success if the specific achievement API name exists in the App Admin on the Steamworks website and changes are published; otherwise, false.

	!!! info "Notes"
		If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#ClearAchievement){ .md-button .md-button--doc_classes target="_blank" }

### downloadLeaderboardEntries

!!! function "downloadLeaderboardEntries( `int` start, `int` end, `LeaderboardDataRequest` type = LEADERBOARD_DATA_REQUEST_GLOBAL, `uint64_t` this_leaderboard = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | start | int | The index to start downloading entries relative to LeaderboardDataRequest. |
    | end | int | The last index to retrieve entries for relative to LeaderboardDataRequest. |
    | type | [LeaderboardDataRequest enum](#leaderboarddatarequest) | The type of data request to make. Defaults to LEADERBOARD_DATA_REQUEST_GLOBAL. |
    | this_leaderboard | uint64_t | A leaderboard handle obtained from [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard). Defaults to 0. |

	If no leaderboard_handle is passed, then the function will use the last internally-stored handle.

	Fetches a series of leaderboard entries for a specified leaderboard.

	You can ask for more entries than exist, then this will return as many as do exist.

	If you want to download entries for an arbitrary set of users, such as all of the users on a server then you can use [downloadLeaderboardEntriesForUsers](#downloadleaderboardentriesforusers) which takes an array of Steam IDs.

	You must call [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard) to get a leaderboard handle prior to calling this function.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[leaderboard_scores_downloaded](#leaderboard_scores_downloaded) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#DownloadLeaderboardEntriesForUsers){ .md-button .md-button--doc_classes target="_blank" }
	

### downloadLeaderboardEntriesForUsers

!!! function "downloadLeaderboardEntriesForUsers( `array` users_id, `uint64_t` this_leaderboard = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | users_id | array | An array of Steam IDs to get the leaderboard entries for. |
    | this_leaderboard | uint64_t | A leaderboard handle obtained from [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard). Defaults to 0. | 

    If no leaderboard_handle is passed, then the function will use the last internally-stored handle.

	Fetches leaderboard entries for an arbitrary set of users on a specified leaderboard.

	A maximum of 100 users can be downloaded at a time, with only one outstanding call at a time. If a user doesn't have an entry on the specified leaderboard, they won't be included in the result.

	If you want to download entries based on their ranking or friends of the current user then you should use [downloadLeaderboardEntries](#downloadleaderboardentries).

	You must call [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard) to get a leaderboard handle prior to calling this function.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[leaderboard_scores_downloaded](#leaderboard_scores_downloaded) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#DownloadLeaderboardEntries){ .md-button .md-button--doc_classes target="_blank" }

### findLeaderboard

!!! function "findLeaderboard( `string` leaderboard_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | leaderboard_name | string | The name of the leaderboard to find. Must not be longer than LEADERBOARD_NAME_MAX / 128. |

	Gets a leaderboard by name.

	You must call either this or [findOrCreateLeaderboard](#findorcreateleaderboard) to obtain the leaderboard handle which is valid for the game session for each leaderboard you wish to access prior to calling any other Leaderboard functions.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[leaderboard_find_result](#leaderboard_find_result) call result

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#FindLeaderboard){ .md-button .md-button--doc_classes target="_blank" }

### findOrCreateLeaderboard

!!! function "findOrCreateLeaderboard( `string` leaderboard_name, `LeaderboardSortMethod` sort_method, `LeaderboardDisplayType` display_type )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | leaderboard_name | string | The name of the leaderboard to find or create. Must not be longer than LEADERBOARD_NAME_MAX / 128. |
    | sort_method | [LeaderboardSortMethod enum](#leaderboardsortmethod) | The sort order of the new leaderboard if it's created. |
    | display_type | [LeaderboardDisplayType enum](#leaderboarddisplaytype) | The display type (used by the Steam Community web site) of the new leaderboard if it's created. |

    Gets a leaderboard by name, it will create it if it's not yet created.

	You must call either this or [findLeaderboard](#findleaderboard) to obtain the leaderboard handle which is valid for the game session for each leaderboard you wish to access prior to calling any other Leaderboard functions.

	Leaderboards created with this function will not automatically show up in the Steam Community. You must manually set the Community Name field in the App Admin panel of the Steamworks website. As such it's generally recommended to prefer creating the leaderboards in the App Admin panel on the Steamworks website and using [findLeaderboard](#findleaderboard) unless you're expected to have a large amount of dynamically created leaderboards.

	You should never pass 0 for sort_method or 0 for display_type as this is undefined behavior.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[leaderboard_find_result](#leaderboard_find_result) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#FindOrCreateLeaderboard){ .md-button .md-button--doc_classes target="_blank" }

### getAchievement

!!! function "getAchievement( `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to obtain the status of; uses the name set in the Steamworks back-end. |

    Gets the unlock status of the Achievement.

	The equivalent function for other users is [getUserAchievement](#getuserachievement).

	!!! returns "Returns: dictionary"
		Contains the following keys:

		* ret (bool) - Returns true if the API name of the specified achievement exists in the App Admin on the Steamworks site and the changes are published; otherwise, false.
		* achieved (bool)

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetAchievement){ .md-button .md-button--doc_classes target="_blank" }

### getAchievementAchievedPercent

!!! function "getAchievementAchievedPercent( `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to obtain the status of; uses the name set in the Steamworks back-end. |

    Returns the percentage of users who have unlocked the specified achievement.

	You must have called [requestGlobalAchievementPercentages](#requestglobalachievementpercentages) and it needs to return successfully via its callback prior to calling this.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| ret | bool | Returns true upon success; otherwise, false if [requestGlobalAchievementPercentages](#requestglobalachievementpercentages) has not been called or if the specified API names does not exist in the global achievement percentages.
		| percent | float | The percentage of people that have unlocked this achievement from 0 to 100.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetAchievementAchievedPercent){ .md-button .md-button--doc_classes target="_blank" }

### getAchievementAndUnlockTime

!!! function "getAchievementAndUnlockTime( `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to obtain the status of; uses the name set in the Steamworks back-end. |

    Gets the achievement status, and the time it was unlocked if unlocked.

	If the return value is true, but the unlock time is zero, that means it was unlocked before Steam began tracking achievement unlock times (December 2009). The time is provided in Unix epoch format, seconds since January 1, 1970 UTC.

	The equivalent function for other users is [getUserAchievementAndUnlockTime](#getuserachievementandunlocktime).

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
		| --- | ---- | ----- |
		| retrieve | bool | Returns true if the API name of the specified achievement exists in the App Admin on the Steamworks site and the changes are published; otherwise, false.
		| achieved | bool | Returns whether the current user has unlocked the achievement.
		| unlocked | uint32_t | Returns the time that the achievement was unlocked; if **achieved** is true.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetAchievementAndUnlockTime){ .md-button .md-button--doc_classes target="_blank" }

### getAchievementDisplayAttribute

!!! function "getAchievementDisplayAttribute( `string` achievement_name, `string` key )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to obtain the attribute of; uses the name set in the Steamworks back-end. |
    | key | string | The key to get a value for. |

    Get general attributes for an achievement. Currently provides: Name, Description, and Hidden status.

	This receives the value from a dictionary/map keyvalue store, so you must provide one of the following keys:

	* name - to retrive the localized achievement name in UTF8
	* desc - to retrive the localized achievement description in UTF8
	* hidden - for retrieving if an achievement is hidden. Returns "0" when not hidden, "1" when hidden

	This localization is provided based on the games language if it's set, otherwise it checks if a localization is avilable for the users Steam UI Language. If that fails too, then it falls back to english.

	!!! returns "Returns: string"
		Returns a value as a string upon success if all the following conditions are met; otherwise, it returns an empty string:

		* The specified achievement exists in the App Admin on the Steamworks site and the changes are published.
		* The specified **key** is valid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetAchievementDisplayAttribute){ .md-button .md-button--doc_classes target="_blank" }

### getAchievementIcon

!!! function "getAchievementIcon( `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to obtain the icon of; uses the name set in the Steamworks back-end. |

    Gets the icon for an achievement.

	The image is returned as a handle to be used with [getImageRGBA](utils.md#getimagergba) to get the actual image data.

	!!! returns "Returns: int"
		Returns a handle to be used with [getImageRGBA](utils.md#getimagergba). It will return an invalid handle of 0 under the following conditions:

		* The specified achievement does not exists in the App Admin on the Steamworks site or the changes are not published.
		* Steam is still fetching image data from the server.  This will trigger a [user_achievement_icon_fetched](#user_achievement_icon_fetched) callback which will notify you when the image data is ready and provide you with a new handle. If the **icon_handle** in the callback is still 0, then there is no image set for the specified achievement.

	!!! info "Notes"
		See the [Achievement Icons tutorial](../tutorials/achievement_icons.md) for an example.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetAchievementIcon){ .md-button .md-button--doc_classes target="_blank" }

### getAchievementName

!!! function "getAchievementName( `uint32_t` achievement_index )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_index | uint32_t | Index of the achievement. |

    Gets the 'API name' for an achievement index between 0 and [getNumAchievements](#getnumachievements).

	This function must be used in conjunction with [getNumAchievements](#getnumachievements) to loop over the list of achievements.

	In general games should not need these functions as they should have the list of achievements compiled into them.

	!!! returns "Returns: string

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetAchievementName){ .md-button .md-button--doc_classes target="_blank" }

### getAchievementProgressLimitsFloat

!!! function "getAchievementProgressLimitsFloat( `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to obtain the progress limits of; uses the name set in the Steamworks back-end. |

    For achievements that have related progress stats, use this to query what the bounds of that progress are. You may want this info to selectively call [indicateAchievementProgress](#indicateachievementprogress) when appropriate milestones of progress have been made, to show a progress notification to the user.

	!!! returns "Returns: dictionary
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| min | float | The minimum value for the achievement progress.
		| max | float | The maximum value for the achievement progress; when the achievement should trigger.

### getAchievementProgressLimitsInt

!!! function "getAchievementProgressLimitsInt( `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to obtain the progress limits of; uses the name set in the Steamworks back-end. |

    For achievements that have related progress stats, use this to query what the bounds of that progress are. You may want this info to selectively call [indicateAchievementProgress](#indicateachievementprogress) when appropriate milestones of progress have been made, to show a progress notification to the user.

	!!! returns "Returns: dictionary
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| min | int32 | The minimum value for the achievement progress.
		| max | int32 | The maximum value for the achievement progress; when the achievement should trigger.

### getGlobalStatFloat

!!! function "getGlobalStatFloat( `string` stat_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to obtain the global value of; uses the name set in the Steamworks back-end. |

    Gets the lifetime totals for an aggregated stat.

	You must have called [requestGlobalStats](#requestglobalstats) and it needs to return successfully via its callback prior to calling this.

	!!! returns "Returns: float"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetGlobalStat){ .md-button .md-button--doc_classes target="_blank" }

### getGlobalStatInt

!!! function "getGlobalStatInt( `string` stat_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to obtain the global value of; uses the name set in the Steamworks back-end. |

    Gets the lifetime totals for an aggregated stat.

	You must have called [requestGlobalStats](#requestglobalstats) and it needs to return successfully via its callback prior to calling this.

	!!! returns "Returns: uint64_t"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetGlobalStat){ .md-button .md-button--doc_classes target="_blank" }

### getGlobalStatFloatHistory

!!! function "getGlobalStatFloatHistory( `string` stat_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to obtain the global history of; uses the name set in the Steamworks back-end. |

    Gets the daily history for an aggregated stat.

	!!! returns "Returns: PackedFloat64Array (4.x) / PoolRealArray (3.x)"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetGlobalStatHistory){ .md-button .md-button--doc_classes target="_blank" }

### getGlobalStatIntHistory

!!! function "getGlobalStatIntHistory( `string` stat_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to obtain the global history of; uses the name set in the Steamworks back-end. |

    Gets the daily history for an aggregated stat.

	!!! returns "Returns: PackedInt64Array (4.x) / PoolIntArray (3.x)"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetGlobalStatHistory){ .md-button .md-button--doc_classes target="_blank" }	

### getLeaderboardDisplayType

!!! function "getLeaderboardDisplayType( `uint64_t` leaderboard_handle = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | leaderboard_handle | uint64_t | A leaderboard handle obtained from [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard). Defaults to 0. |

    If no leaderboard_handle is passed, then the function will use the last internally-stored handle.

	Get the display type of a leaderboard handle.

	!!! returns "Returns: int / [LeaderboardDisplayType enum](#leaderboarddisplaytype)"
		This will return [LEADERBOARD_DISPLAY_TYPE_NONE / 0](#leaderboarddisplaytype) if the leaderboard handle is invalid.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetLeaderboardDisplayType){ .md-button .md-button--doc_classes target="_blank" }

### getLeaderboardEntries

!!! function "getLeaderboardEntries( )"
	Get the currently used leaderboard entries.

	!!! returns "Returns: array"

	!!! info "Notes"
		This is a GodotSteam specific function.  [Was replaced with get_leaderboard_entries() in Main class as a set-get.](main.md#get_leaderboard_entries)

	---
	[ :material-tag-remove: Removed GodotSteam 4.11](../changelog/godot4.md/#version-411){ .md-button .md-button--changes target="_blank" }

### getLeaderboardEntryCount

!!! function "getLeaderboardEntryCount( `uint64_t` this_leaderboard = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | this_leaderboard | uint64_t | A leaderboard handle obtained from [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard). Defaults to 0. |

    If no leaderboard_handle is passed, then the function will use the last internally-stored handle.

	Returns the total number of entries in a leaderboard.

	This is cached on a per leaderboard basis upon the first call to [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard) and is refreshed on each successful call to [downloadLeaderboardEntries](#downloadleaderboardentries), [downloadLeaderboardEntriesForUsers](#downloadleaderboardentriesforusers), and [uploadLeaderboardScore](#uploadleaderboardscore).

	!!! returns "Returns: int"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetLeaderboardEntryCount){ .md-button .md-button--doc_classes target="_blank" }

### getLeaderboardName

!!! function "getLeaderboardName( `uint64_t` this_leaderboard = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | this_leaderboard | uint64_t | A leaderboard handle obtained from [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard). Defaults to 0. |

    If no leaderboard_handle is passed, then the function will use the last internally-stored handle.

	!!! returns "Returns: string"
		Returns the API name of a leaderboard handle.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetLeaderboardName){ .md-button .md-button--doc_classes target="_blank" }

### getLeaderboardSortMethod

!!! function "getLeaderboardSortMethod( `uint64_t` this_leaderboard = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | this_leaderboard | uint64_t | A leaderboard handle obtained from [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard). Defaults to 0. |

    If no leaderboard_handle is passed, then the function will use the last internally-stored handle.

	Get the sort order of a leaderboard handle. If no thisLeaderboard handle is passed, then the function will use the last stored internal handle.

	!!! returns "Returns: int / [LeaderboardSortMethod enum](#leaderboardsortmethod)"

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetLeaderboardSortMethod){ .md-button .md-button--doc_classes target="_blank" }

### getMostAchievedAchievementInfo

!!! function "getMostAchievedAchievementInfo( )"
	Gets the info on the most achieved achievement for the game.

	You must have called [requestGlobalAchievementPercentages](#requestglobalachievementpercentages) and it needs to return successfully via its callback prior to calling this.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| iterator | int | Returns -1 if[requestGlobalAchievementPercentages](#requestglobalachievementpercentages) has not been called or if there are no global achievement percentages for this app ID. If the call lis successful, it is an iterator which should be used with [getNextMostAchievedAchievementInfo](#getnextmostachievedachievementinfo).
		| name | string | The API name of the achievement.
		| percent | float | The percentage of people that have unlocked this achievement from 0 to 100.
		| achieved | bool | Whether the current user has unlocked this achievement.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetMostAchievedAchievementInfo){ .md-button .md-button--doc_classes target="_blank" }

### getNextMostAchievedAchievementInfo

!!! function "getNextMostAchievedAchievementInfo( `int` iterator )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | iterator | int | The index for getting achievement information. |

    Gets the info on the next most achieved achievement for the game.

	You must have called [requestGlobalAchievementPercentages](#requestglobalachievementpercentages) and it needs to return successfully via its callback prior to calling this.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| iterator | int | Returns -1 if[requestGlobalAchievementPercentages](#requestglobalachievementpercentages) has not been called or if there are no global achievement percentages for this app ID. If the call lis successful, it is an iterator which should be used with subsequent calls to this function.
		| name | string | The API name of the achievement.
		| percent | float | The percentage of people that have unlocked this achievement from 0 to 100.
		| achieved | bool | Whether the current user has unlocked this achievement.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetNextMostAchievedAchievementInfo){ .md-button .md-button--doc_classes target="_blank" }

### getNumAchievements

!!! function "getNumAchievements( )"
	Get the number of achievements defined in the App Admin panel of the Steamworks website.

	This is used for iterating through all of the achievements with [getAchievementName](#getachievementname).

	In general games should not need these functions because they should have a list of existing achievements compiled into them.

	!!! returns "Returns: uint32_t"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetNumAchievements){ .md-button .md-button--doc_classes target="_blank" }

### getNumberOfCurrentPlayers

!!! function "getNumberOfCurrentPlayers( )"
	Asynchronously retrieves the total number of players currently playing the current game. Both online and in offline mode.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[number_of_current_players](#number_of_current_players) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetNumberOfCurrentPlayers){ .md-button .md-button--doc_classes target="_blank" }

### getStatFloat

!!! function "getStatFloat( `string` stat_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to obtain the value of; uses the name set in the Steamworks back-end. |

    Gets the current value of the a stat for the current user.

	To receive stats for other users use [getUserStatFloat](#getuserstatfloat).

	!!! returns "Returns: float"

	!!! info "Notes"
		If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetStat){ .md-button .md-button--doc_classes target="_blank" }

### getStatInt

!!! function "getStatInt( `string` stat_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to obtain the value of; uses the name set in the Steamworks back-end. |

    Gets the current value of the a stat for the current user.

	To receive stats for other users use [getUserStatInt](#getuserstatint).

	!!! returns "Returns: int"

	**Notes:** If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetStat){ .md-button .md-button--doc_classes target="_blank" }


### getUserAchievement

!!! function "getUserAchievement( `uint64_t` steam_id, `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user you want to get the achievement for. |
    | achievement_name | string | The achievement you want to get the status of; uses the name set in the Steamworks back-end. |

    Gets the unlock status of the Achievement.

	The equivalent function for the local user is [getAchievement](#getachievement).

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
        | steam_id | uint64_t | The Steam ID of the user this was called for.
		| retrieved | bool | Returns true if successfully retrieved the status; otherwise, false if the API name does not exist in the App Admin on the Steamworks site or the changes aren't published.
		| name | string | The API name of the achievement.
		| achieved | bool | Returns the unlock status of the achievement.

	---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetUserAchievement){ .md-button .md-button--doc_classes target="_blank" }

### getUserAchievementAndUnlockTime

!!! function "getUserAchievementAndUnlockTime( `uint64_t` steam_id, `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user you want to get the achievement for. |
    | achievement_name | string | The achievement you want to get the status of; uses the name set in the Steamworks back-end. |

    Gets the achievement status, and the time it was unlocked if unlocked.

	If the return value is true, but the unlock time is zero, that means it was unlocked before Steam began tracking achievement unlock times (December 2009). The time is provided in Unix epoch format, seconds since January 1, 1970 UTC.

	The equivalent function for the local user is [getAchievementAndUnlockTime](#getachievementandunlocktime).

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| retrieved | bool | Returns true upon success; otherwise, false if the API name of the specified achievement does not exist in the App Admin on the Steamworks site or the changes are not published.
		| name | string | The API name of the achievement.
		| achieved | bool | Returns whether the current user has unlocked the achievement.
		| unlocked | uint32_t | Returns the time that the achievement was unlocked; if **achieved** is true.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetUserAchievementAndUnlockTime){ .md-button .md-button--doc_classes target="_blank" }

### getUserStatFloat

!!! function "getUserStatFloat( `uint64_t` steam_id, `string` stat_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user you want to get the statistic for. |
    | stat_name | string | The statistic you want to obtain the value of; uses the name set in the Steamworks back-end. |

    Gets the current value of the a stat for the specified user.

	The equivalent function for the local user is [getStatFloat](#getstatfloat).

	!!! returns "Returns: float"
		Returns the float value upon success; otherwise, it may return 0.0 if the specified stat does not exist in the App Admin on the Steamworks site or the changes are not published.

	!!! info "Notes"
		If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetUserStat){ .md-button .md-button--doc_classes target="_blank" }

### getUserStatInt

!!! function "getUserStatInt( `uint64_t` steam_id, `string` stat_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user you want to get the statistic for. |
    | stat_name | string | The statistic you want to obtain the value of; uses the name set in the Steamworks back-end. |

    Gets the current value of the a stat for the specified user.

	The equivalent function for the local user is [getStatInt](#getstatint).

	!!! returns "Returns: int"
		Returns the integer value upon success; otherwise, it may return 0 if the specified stat does not exist in the App Admin on the Steamworks site or the changes are not published.

	!!! info "Notes"
		If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GetUserStat){ .md-button .md-button--doc_classes target="_blank" }

### indicateAchievementProgress

!!! function "indicateAchievementProgress( `string` achievement_name, `int` current_progress, `int` max_progress )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to show the progress of; uses the name set in the Steamworks back-end. |
    | current_progress | int | The current progress. |
    | max_progress | int | The progress required to unlock the achievement. |

	Shows the user a pop-up notification with the current progress of an achievement.

	Calling this function will not set the progress or unlock the achievement, the game must do that manually by calling [setStatInt](#setstatint) or [setStatFloat](#setstatfloat).

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false if:

		* The specified achievement does not exist in the App Admin on the Steamworks site or the changes are not published.
		* The specified achievement is already unlocked.
		* The **current_progress** equals **max_progress**.

	!!! trigger "Triggers"
		* [user_stats_stored](#user_stats_stored) callback
		* [user_achievement_stored](#user_achievement_stored) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#IndicateAchievementProgress){ .md-button .md-button--doc_classes target="_blank" }

### requestCurrentStats

!!! function "requestCurrentStats( )"
	Asynchronously request the user's current stats and achievements from the server.

	You must always call this first to get the initial status of stats and achievements. Only after the resulting callback comes back can you start calling the rest of the stats and achievement functions for the current user.

	The equivalent function for other users is [requestUserStats](#requestuserstats).

	!!! returns "Returns: bool"
		Only returns false if there is no user logged in.

	!!! trigger "Triggers"
		[current_stats_received](#current_stats_received) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#RequestCurrentStats){ .md-button .md-button--doc_classes target="_blank" }
	[ :material-tag-remove: Removed GodotSteam 4.12](../changelog/godot4.md/#version-412){ .md-button .md-button--changes target="_blank" }

### requestGlobalAchievementPercentages

!!! function "requestGlobalAchievementPercentages( )"
	Asynchronously fetches global stats data, which is available for stats marked as "aggregated" in the App Admin panel of the Steamworks website.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[global_achievement_percentages_ready](#global_achievement_percentages_ready) callback

	!!! info "Notes" If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

	This function may not respond with actual data until the game is released on Steam.

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#RequestGlobalAchievementPercentages){ .md-button .md-button--doc_classes target="_blank" }

### requestGlobalStats

!!! function "requestGlobalStats( `int` history_days )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | history_days | int | How many days of day-by-day history to retrieve in addition to the overall totals. The limit is 60. |

	Asynchronously fetches global stats data, which is available for stats marked as "aggregated" in the App Admin panel of the Steamworks website.

	The limit is 60.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[global_stats_received](#global_stats_received) callback

	**Notes:** If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#RequestGlobalStats){ .md-button .md-button--doc_classes target="_blank" }

### requestUserStats

!!! function "requestUserStats( `uint64_t` steam_id )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user you want to get statistics and achievements for. |

    Asynchronously downloads stats and achievements for the specified user from the server.

	These stats are not automatically updated; you'll need to call this function again to refresh any data that may have change.

	To keep from using too much memory, an least recently used cache (LRU) is maintained and other user's stats will occasionally be unloaded. When this happens a [user_stats_unloaded](#user_stats_unloaded) callback is sent. After receiving this callback the user's stats will be unavailable until this function is called again.

	The equivalent function for the local user is [requestCurrentStats](#requestcurrentstats).

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[user_stats_received](#user_stats_received) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#RequestUserStats){ .md-button .md-button--doc_classes target="_blank" }

### resetAllStats

!!! function "resetAllStats( `bool` achievements_too = true )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievements_too | bool | Whether this will reset the user's achievements also. Defaults to true.

    Resets the current users stats and, optionally achievements.

	This automatically calls [storeStats](#storestats) to persist the changes to the server. This should typically only be used for testing purposes during development. Ensure that you sync up your stats with the new default values provided by Steam after calling this by calling [requestCurrentStats](#requestcurrentstats).

	!!! returns "Returns: bool"

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#ResetAllStats){ .md-button .md-button--doc_classes target="_blank" }

### setAchievement

!!! function "setAchievement( `string` achievement_name )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | achievement_name | string | The achievement you want to unlock; uses the name set in the Steamworks back-end. |

	Unlocks an achievement.

	You can unlock an achievement multiple times so you don't need to worry about only setting achievements that aren't already set. This call only modifies Steam's in-memory state so it is quite cheap. To send the unlock status to the server and to trigger the Steam overlay notification you must call [storeStats](#storestats).

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false if the specified achievement API name does not exist in the App Admin on the Steamworks site or the changes are not published.

	!!! info "Notes"
		If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#SetAchievement){ .md-button .md-button--doc_classes target="_blank" }

### setLeaderboardDetailsMax

!!! function "setLeaderboardDetailsMax( `int` max )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | max | int | The maximum number of details to return. |

   	Set the maximum number of details to return for leaderboard entries.

	The current leaderboard details max value.  This can only go as high as [LEADERBOARD_DETAILS_MAX](#constants) / 256.

	!!! returns "Returns: int"

	!!! info "Notes"
		This is a GodotSteam specific function.  [Was replaced with set_leaderboard_details_max() in Main class as a set-get.](main.md#set_leaderboard_details_max)

	---
	[ :material-tag-remove: Removed GodotSteam 4.11](../changelog/godot4.md/#version-411){ .md-button .md-button--changes target="_blank" }

### setStatFloat

!!! function "setStatFloat( `string` stat_name, `float` value )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to set the value for; uses the name set in the Steamworks back-end. |
    | value | float | The new value of the stat. This must be an absolute value, it will not increment or decrement for you. |

    Sets / updates the float value of a given stat for the current user.

	This call only modifies Steam's in-memory state and is very cheap. Doing so allows Steam to persist the changes even in the event of a game crash or unexpected shutdown. To submit the stats to the server you must call [storeStats](#storestats).

	If this is returning false and everything appears correct, then check to ensure that your changes in the App Admin panel of the Steamworks website are published.

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false if the specified stat does not exist in the App Admin on the Steamworks site or the changes are not published.

	!!! info "Notes"
		If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#SetStat){ .md-button .md-button--doc_classes target="_blank" }

### setStatInt

!!! function "setStatInt( `string` name, `int32` value )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to set the value for; uses the name set in the Steamworks back-end. |
    | value | float | The new value of the stat. This must be an absolute value, it will not increment or decrement for you. |

    Sets / updates the integer value of a given stat for the current user.

	This call only modifies Steam's in-memory state and is very cheap. Doing so allows Steam to persist the changes even in the event of a game crash or unexpected shutdown. To submit the stats to the server you must call [storeStats](#storestats).

	If this is returning false and everything appears correct, then check to ensure that your changes in the App Admin panel of the Steamworks website are published.

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false if the specified stat does not exist in the App Admin on the Steamworks site or the changes are not published.

	!!! info "Notes"
		If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#SetStat){ .md-button .md-button--doc_classes target="_blank" }

### storeStats

!!! function "storeStats( )"
	Send the changed stats and achievements data to the server for permanent storage.

	If this fails then nothing is sent to the server. It's advisable to keep trying until the call is successful.

	This call can be rate limited. Call frequency should be on the order of minutes, rather than seconds. You should only be calling this during major state changes such as the end of a round, the map changing, or the user leaving a server. This call is required to display the achievement unlock notification dialog though, so if you have called [setAchievement](#setachievement) then it's advisable to call this soon after that.

	If you have stats or achievements that you have saved locally but haven't uploaded with this function when your application process ends then this function will automatically be called.

	You can find additional debug information written to the `%steam_install%\logs\stats_log.txt` file.

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false if the current game does not have stats associated with it in the Steamworks partner backend or those stats are not published.

	!!! trigger "Triggers"
		* [user_stats_stored](#user_stats_stored) callback
		* [user_achievement_stored](#user_achievement_stored) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#StoreStats){ .md-button .md-button--doc_classes target="_blank" }

### updateAvgRateStat

!!! function "updateAvgRateStat( `string` stat_name, `float` this_session, `double` session_length )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | stat_name | string | The statistic you want to set the value for; uses the name set in the Steamworks back-end. |
    | this_session | float | The value accumulation since the last call to this function. |
    | session_length | double | The amount of time in seconds since the last call to this function. |

    Updates an AVGRATE stat with new values.

	This call only modifies Steam's in-memory state and is very cheap. Doing so allows Steam to persist the changes even in the event of a game crash or unexpected shutdown. To submit the stats to the server you must call [storeStats](#storestats).

	If this is returning false and everything appears correct, then check to ensure that your changes in the App Admin panel of the Steamworks website are published.

	!!! returns "Returns: bool"
		Returns true upon success; otherwise, false if:

		* The specified stat does not exist in the App Addmin on the Steamworks site.
		* The changes are not published.
		* The stat is not set as AVGRATE in the Setamworks partner backend.

	!!! info "Notes"
		If using earlier than SDK 1.60, you must have called [requestCurrentStats](#requestcurrentstats) and it needs to return successfully via its callback prior to calling this!

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#UpdateAvgRateStat){ .md-button .md-button--doc_classes target="_blank" }

### uploadLeaderboardScore

!!! function "uploadLeaderboardScore( `int32` score, `bool` keep_best = false, `PackedInt32Array` details = [], `uint64_t` this_leaderboard = 0 )"
	| :material-variable: Argument | Type | Notes |
    | -------- | ---- | ----- |
    | score | int32 | Do you want to force the score to change or keep the previous score if it was better? |
    | keep_best | bool | The score to upload. Defaults to false.
    | details | PackedInt32Array | Optional: Array containing the details surrounding the unlocking of this score. Defaults to empty array. |
    | this_leaderboard | uint64_t | A leaderboard handle obtained from [findLeaderboard](#findleaderboard) or [findOrCreateLeaderboard](#findorcreateleaderboard). Defaults to 0. |

    If this_leaderboard is not passed, then the function will use the last internally-stored handle.

	Uploads a user score to a specified leaderboard.

	Details are optional game-defined information which outlines how the user got that score. For example if it's a racing style time based leaderboard you could store the timestamps when the player hits each checkpoint. If you have collectibles along the way you could use bit fields as booleans to store the items the player picked up in the playthrough.

	Uploading scores to Steam is rate limited to 10 uploads per 10 minutes and you may only have one outstanding call to this function at a time.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[leaderboard_score_uploaded](#leaderboard_score_uploaded) callback

    ---
    [:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#UploadLeaderboardScore){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### current_stats_received

!!! function "current_stats_received"
	Called when the latest stats and achievements for the local user have been received from the server; in response to function [requestCurrentStats](#requestcurrentstats).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| game | uint64_t | The game ID that these stats are for; should match the app ID.
		| result | uint32_t | Returns whether the call was successful or not; if the user has no stats, this will be set to [RESULT_FAIL](main.md#result).
		| user | uint64_t | The user whose stats were retrieved.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#UserStatsReceived_t){ .md-button .md-button--doc_classes target="_blank" }

### global_achievement_percentages_ready

!!! function "global_achievement_percentages_ready"
	Called when the global achievement percentages have been received from the server.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| game | uint64_t | Game ID which these achievement percentages are for.
		| result | [Result enum](main.md#result) | Result of the request. May get [RESULT_FAIL](main.md#result) if the remote call fails or there are no global achievement percentages for the current app ID.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GlobalAchievementPercentagesReady_t){ .md-button .md-button--doc_classes target="_blank" }

### global_stats_received

!!! function "global_stats_received"
	Called when the global stats have been received from the server.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| game_id | uint64_t | Game ID which these global stats are for.
		| result | [Result enum](main.md#result) | The result of the request. May be [RESULT_FAIL](main.md#result) if the remote call fails.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#GlobalStatsReceived_t){ .md-button .md-button--doc_classes target="_blank" }

### leaderboard_find_result

!!! function "leaderboard_find_result"
	Result when finding a leaderboard.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| leaderboard_handle | uint64_t | Handle to the leaderboard that was searched for. 0 if no leaderboard was found.
		| found | uint8_t | Was the leaderboard found? 1 if it was, 0 if it wasn't.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#LeaderboardFindResult_t){ .md-button .md-button--doc_classes target="_blank" }

### leaderboard_scores_downloaded

!!! function "leaderboard_scores_downloaded"
	Called when scores for a leaderboard have been downloaded and are ready to be retrieved.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| message | string | The message about the successful call or not.
		| this_handle | uint64_t | Handle to the leaderboard that these entries belong to.
		| leaderboard_entries_array | array | An array of downloaded leaderboard entries as dictionaries.

		The **leaderboard_entries_array** contains a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| entry_dict | dictionary | The leaderboard entry for this call.

		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| score | int | The raw score as set in the leaderboard.
		| steam_id | uint64_t | The Steam ID of the user who this entry belongs to.
		| global_rank | int | The global rank of this entry ranging from [1..N], where N is the number of users with an entry in the leaderboard.
		| ugc_handle | uint64_t | The handle for the UGC attached to this entry. Will be [UGC_HANDLE_INVALID](remote_storage.md#constants) if there is none.
		| details | PackedInt32Array | An array of details for this entry.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#LeaderboardScoresDownloaded_t){ .md-button .md-button--doc_classes target="_blank" }

### leaderboard_score_uploaded

!!! function "leaderboard_score_uploaded"
	Result indicating that a leaderboard score has been uploaded.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| success | uint8 | Was the call successful? Returns 1 if so, 0 on failure. Failure may happen if the details sent exceeds [LEADERBOARD_DETAILS_MAX](user_stats.md#constants) or the leaderboard is set to "Trusted" so it can only accepts scores frmo the Web API.
		| this_handle | uint64_t | Handle to the leaderboard that this score was uploaded to.
		| this_score | dictionary | The dictionary of score data for the entry uploaded.

		The **this_score** dictionary contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| score | int32 | The score that was attempted to set.
		| score_changed | uint8 | True if the score on the leaderboard changed otherwise false if the existing score was better.
		| global_rank_new | int | The new global rank of the user on this leaderboard.
		| global_rank_prev | int | The previous global rank of the user on this leaderboard; 0 if the user had no existing entry in the leaderboard.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#LeaderboardScoreUploaded_t){ .md-button .md-button--doc_classes target="_blank" }

### leaderboard_ugc_set

!!! function "leaderboard_ugc_set"
	Result indicating that user generated content has been attached to one of the current user's leaderboard entries.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| leaderboard_handle | uint64_t | Handle to the leaderboard that the UGC was attached to.
		| result | [Result enum](main.md#result) | The result of the operation. [RESULT_TIMEOUT](main.md#result) if the upload took too long; the UGC was not submitted. [RESULT_INVALID_PARAM](main.md#result) if the handle passed was invalid; the UGC was not submitted.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#LeaderboardUGCSet_t){ .md-button .md-button--doc_classes target="_blank" }

### number_of_current_players

!!! function "number_of_current_players"
	Gets the current number of players for the current app ID.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | success | uint8 | Was the call successful? Returns 1 if it was; otherwise, 0 on failure.
		| players | int32 | Number of players currently playing.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#NumberOfCurrentPlayers_t){ .md-button .md-button--doc_classes target="_blank" }

### user_achievement_icon_fetched

!!! function "user_achievement_icon_fetched"
	Result of an achievement icon that has been fetched.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| game_id | int | The game ID this achievement is for.
		| achievement_name | string | The name of the achievement that this callback is for.
		| achieved | bool | Returns whether the icon for the achieved (true) or unachieved (false) version.
		| icon_handle | int | Handle to the image, which can be used with [getImageRGBA](utils.md#getimagergba) to get the image data. 0 means no image is set for the achievement.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#UserAchievementIconFetched_t){ .md-button .md-button--doc_classes target="_blank" }

### user_achievement_stored

!!! function "user_achievement_stored"
	Result of a request to store the achievements on the server, or an "indicate progress" call. If both current progress and max progress are zero, that means the achievement has been fully unlocked.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| game_id | uint64_t | Game ID that this achievement is for.
		| group_achieve | bool | Unused.
		| name | string | Name of the achievement.
		| current_progress | uint32_t | Current progress towards the achievement.
		| max_progress | uint32_t | The total amount of progress required to unlock.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#UserAchievementStored_t){ .md-button .md-button--doc_classes target="_blank" }

### user_stats_received

!!! function "user_stats_received"
	Called when the latest stats and achievements for a specific user (including the local user) have been received from the server.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| game_id | uint64_t | Game ID that these stats are for.
		| result | uint32_t | Returns whether the call was successful or not. If the user has no stats, this will be set to [RESULT_FAIL](main.md#result)
		| user | uint64_t | The user whose stats were retrieved.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#UserStatsReceived_t){ .md-button .md-button--doc_classes target="_blank" }

### user_stats_stored

!!! function "user_stats_stored"
	Result of a request to store the user stats.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| game_id | uint64_t | Game ID that these stats are for.
		| result | [Result enum](main.md#result) | Returns whether the call was successful or not.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#UserStatsStored_t){ .md-button .md-button--doc_classes target="_blank" }

### user_stats_unloaded

!!! function "user_stats_unloaded"
	Callback indicating that a user's stats have been unloaded. Call [requestUserStats](#requestuserstats) again before accessing stats for this user.
	
	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| user | uint64_t | User whose stats have been unloaded.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#UserStatsUnloaded_t){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Notes
---- | -------- | ----- | -----
LEADERBOARD_DETAILS_MAX | k_cLeaderboardDetailsMax | 64 | Maximum number of bytes for a leaderboard name (UTF-8 encoded).
LEADERBOARD_NAME_MAX | k_cchLeaderboardNameMax | 128 | Maximum number of bytes for stat and achievement names (UTF-8 encoded).
STAT_NAME_MAX | k_cchStatNameMax | 128 | Maximum number of details that you can store for a single leaderboard entry.

{==
## :material-numeric: Enums
==}

### LeaderboardDataRequest

Type of data request when downloading leaderboard entries.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LEADERBOARD_DATA_REQUEST_GLOBAL | k_ELeaderboardDataRequestGlobal | 0 | -
LEADERBOARD_DATA_REQUEST_GLOBAL_AROUND_USER | k_ELeaderboardDataRequestGlobalAroundUser | 1 | -
LEADERBOARD_DATA_REQUEST_FRIENDS | k_ELeaderboardDataRequestFriends | 2 | -
LEADERBOARD_DATA_REQUEST_USERS | k_ELeaderboardDataRequestUsers | 3 | -

### LeaderboardDisplayType

The display type (used by the Steam Community web site) for a leaderboard.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LEADERBOARD_DISPLAY_TYPE_NONE | k_ELeaderboardDisplayTypeNone | 0 | -
LEADERBOARD_DISPLAY_TYPE_NUMERIC | k_ELeaderboardDisplayTypeNumeric | 1 | Simple numerical score.
LEADERBOARD_DISPLAY_TYPE_TIME_SECONDS | k_ELeaderboardDisplayTypeTimeSeconds | 2 | The score represents a time, in seconds.
LEADERBOARD_DISPLAY_TYPE_TIME_MILLISECONDS | k_ELeaderboardDisplayTypeTimeMilliSeconds | 3 | The score represents a time, in milliseconds,

### LeaderboardSortMethod

The sort order of a leaderboard.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LEADERBOARD_SORT_METHOD_NONE | k_ELeaderboardSortMethodNone | 0 | -
LEADERBOARD_SORT_METHOD_ASCENDING | k_ELeaderboardSortMethodAscending | 1 | -
LEADERBOARD_SORT_METHOD_DESCENDING | k_ELeaderboardSortMethodDescending | 2 | -

### LeaderboardUploadScoreMethod

How to handle uploaded scores.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
LEADERBOARD_UPLOAD_SCORE_METHOD | k_ELeaderboardUploadScoreMethodNone | 0 | -
LEADERBOARD_UPLOAD_SCORE_METHOD_KEEP_BEST | k_ELeaderboardUploadScoreMethodKeepBest | 1 | Leaderboard will keep user's best score.
LEADERBOARD_UPLOAD_SCORE_METHOD_FORCE_UPDATE | k_ELeaderboardUploadScoreMethodForceUpdate | 2 | Leaderboard will always replace score with specified.