---
title: Friends
description: A class reference for Friends.
icon: material/heart
---

# Friends

Interface to both friends list data and general information about users as well as interact with the [Steam Overlay](https://partner.steamgames.com/doc/features/overlay){ target="\_blank" }.

!!! info "Only available in the main [GodotSteam branches](https://github.com/GodotSteam/GodotSteam){ target="\_blank" }"

{==
## :material-function-variant: Functions
==}

### activateGameOverlay

!!! function "activateGameOverlay( `string` type )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | type | string | The dialog to open. |

	Activates the overlay with optional dialog to open the following:

	* friends
	* community
	* players
	* settings
	* officialgamegroup
	* stats
	* achievements
	* lobbyinvite
	* chatroomgroup/<group name>

	!!! returns "Returns: void"

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#ActivateGameOverlay){ .md-button .md-button--doc_classes target="_blank" }

### activateGameOverlayInviteDialog

!!! function "activateGameOverlayInviteDialog( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the lobby that selected users will be invited to. |

	Activates game overlay to open the invite dialog. Invitations will be sent for the provided lobby. 

	!!! returns "Returns: void"

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#ActivateGameOverlayInviteDialog){ .md-button .md-button--doc_classes target="_blank" }

### activateGameOverlayInviteDialogConnectString

!!! function "activateGameOverlayInviteDialogConnectString( `string` connect_string )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | connect_string | string | The connection string for your game or lobby. |

	Activates the game overlay to open an invite dialog that will send the provided Rich Presence connect string to selected friends. 

	!!! returns "Returns: void"

### activateGameOverlayRemotePlayTogetherInviteDialog

!!! function "activateGameOverlayRemotePlayTogetherInviteDialog( `uint64_t` lobby_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | lobby_id | uint64_t | The lobby for the Remote Play invite. |

    Activates game overlay to open the Remote Play Together invite dialog. Invitations will be sent for Remote Play Together.

	Currently unclear if this is a Matchmaking class lobby ID though.

	!!! returns "Returns: void"

	---
	[ :material-tag-plus: Added GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### activateGameOverlayToStore

!!! function "activateGameOverlayToStore( `uint32_t` app_id = 0 )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | app_id | uint32_t | The app ID to show the store page of. |

	Activates the overlay with the application/game Steam store page. Using [APP_ID_INVALID](main.md#constants) brings the user to the front page of the Steam store.

	!!! returns "Returns: void"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#ActivateGameOverlayToStore){ .md-button .md-button--doc_classes target="_blank" }

### activateGameOverlayToUser

!!! function "activateGameOverlayToUser( `string` type, `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | type | string | The dialog to open. |
	| steam_id | uint64_t | The Steam ID of the context to open this dialog to. |

	Activates the overlay to the following:

	* "steamid" - Opens the overlay web browser to the specified user or groups profile.
	* "chat" - Opens a chat window to the specified user, or joins the group chat.
	* "jointrade" - Opens a window to a Steam Trading session that was started with the ISteamEconomy/StartTrade Web API.
	* "stats" - Opens the overlay web browser to the specified user's stats.
	* "achievements" - Opens the overlay web browser to the specified user's achievements.
	* "friendadd" - Opens the overlay in minimal mode prompting the user to add the target user as a friend.
	* "friendremove" - Opens the overlay in minimal mode prompting the user to remove the target friend.
	* "friendrequestaccept" - Opens the overlay in minimal mode prompting the user to accept an incoming friend invite.
	* "friendrequestignore" - Opens the overlay in minimal mode prompting the user to ignore an incoming friend invite.

	!!! returns "Returns: void"

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#ActivateGameOverlayToUser){ .md-button .md-button--doc_classes target="_blank" }

### activateGameOverlayToWebPage

!!! function "activateGameOverlayToWebPage( `string` url, `OverlayToWebPageMode` webpage_mode = OVERLAY_TO_WEB_PAGE_MODE_DEFAULT)"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | url | string | The webpage to open. |
	|webpage_mode | [OverlayToWebPageMode enum](#overlaytowebpagemode) | Mode for the web page. Defaults to OVERLAY_TO_WEB_PAGE_MODE_DEFAULT |

	Activates the overlay with specified web address. Full address with protocol type is required, e.g. http://www.steamgames.com/.

	!!! returns "Returns: void"

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#ActivateGameOverlayToWebPage){ .md-button .md-button--doc_classes target="_blank" }

### clearRichPresence

!!! function "clearRichPresence( )"
	Clear the game information in Steam; used in 'View Game Info' section of Friends list.

	!!! returns "Returns: void"

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#ClearRichPresence){ .md-button .md-button--doc_classes target="_blank" }

### closeClanChatWindowInSteam

!!! function "closeClanChatWindowInSteam( `uint64_t` chat_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | chat_id | uint64_t | The Steam ID of the Steam group chat room to close. |

	Closes the specified Steam group chat room in the Steam UI. 

	!!! returns "Returns: bool"
		Returns true if the user successfully left the Steam group chat room; otherwise, false if the user is not in the provided Steam group chat room.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#DownloadClanActivityCounts){ .md-button .md-button--doc_classes target="_blank" }

### downloadClanActivityCounts

!!! function "downloadClanActivityCounts( `PackedInt64Array` clan_id_array )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id_array | PackedInt64Array | A list of steam groups to get the updated data for. |

	Refresh the Steam Group activity data or get the data from groups other than one that the current user is a member. After receiving the callback you can then use [getClanActivityCounts](#getclanactivitycounts) to get the up to date user counts.

	!!! returns "Returns: void"
 
	!!! trigger "Triggers"
		[clan_activity_downloaded](#clan_activity_downloaded) callback

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#DownloadClanActivityCounts){ .md-button .md-button--doc_classes target="_blank" }

### enumerateFollowingList

!!! function "enumerateFollowingList( `uint32_t` start_index )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | start_index | uint32_t | The index to start receiving followers from. This should be 0 on the initial call. |

	Gets the list of users that the current user is following. You can be following people that are not your friends. Following allows you to receive updates when the person does things like post a new piece of content to the Steam Workshop.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[enumerate_following_list](#enumerate_following_list) callback

	!!! info "Notes"
		This returns up to [ENUMERATED_FOLLOWERS_MAX](#constants) / 50 users at once. If the current user is following more than that, you will need to call this repeatedly, with **start_index** set to the total number of followers that you have received so far. I.E. If you have received 50 followers, and the user is following 105, you will need to call this again with **start_index** = 50 to get the next 50, and then again with **start_index** = 100 to get the remaining 5 users.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#EnumerateFollowingList){ .md-button .md-button--doc_classes target="_blank" }

### getChatMemberByIndex

!!! function "getChatMemberByIndex( `uint64_t` clan_id, `int` user )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | This MUST be the same source used in the previous call to [getClanChatMemberCount](#getclanchatmembercount). |

	Gets the Steam ID at the given index in a Steam group chat. 

	You must call [getClanChatMemberCount](#getclanchatmembercount) before calling this.

	!!! returns "Returns: uint64_t"
		Invalid **user** index will return [STEAM_ID_NIL](main.md#constants).

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetChatMemberByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getClanActivityCounts

!!! function "getClanActivityCounts( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam group to get the activity of. |

	Gets the most recent information we have about what the users in a Steam Group are doing. This can only retrieve data that the local client knows about. To refresh the data or get data from a group other than one that the current user is a member of you must call [downloadClanActivityCounts](#downloadclanactivitycounts).

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| clan | uint64_t | The Steam group we are getting thea activity of.
		| online | int | Returns the number of members that are online.
		| ingame | int | Returns the number members that are in game (excluding those with their status set to offline).
		| chatting | int | Returns the number of members in the group chat room.

		This may return an empty dictionary if the Steam ID is invalid or the local client does not have info about the Steam group.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanActivityCounts){ .md-button .md-button--doc_classes target="_blank" }

### getClanByIndex

!!! function "getClanByIndex( `int` clan_index )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_index | int | An index between 0 and [getClanCount](#getclancount). |

	Gets the Steam group's Steam ID at the given index. 

	You must call [getClanCount](#getclancount) before calling this.

	!!! returns "Returns: uint64_t"
		Invalid **user** index will return [STEAM_ID_NIL](main.md#constants).

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getClanChatMemberCount

!!! function "getClanChatMemberCount( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam group to get the chat count of. |

	Get the number of users in a Steam group chat. The current user must be in a lobby to retrieve the Steam IDs of other users in that lobby. This is used for iteration, after calling this then [getChatMemberByIndex](#getchatmemberbyindex) can be used to get the Steam ID of each person in the chat.

	Large steam groups cannot be iterated by the local user.

	!!! returns "Returns: int"
		May return 0 if the **clan_id** provided is invalid or if the local user doesn't have the data available.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanChatMemberCount){ .md-button .md-button--doc_classes target="_blank" }

### getClanCount

!!! function "getClanCount( )"
	Gets the number of Steam groups that the current user is a member of. This is used for iteration, after calling this then [getClanByIndex](#getclanbyindex) can be used to get the Steam ID of each Steam group.

	!!! returns "Returns: int"
		The number of Steam groups that the user is a member of.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanCount){ .md-button .md-button--doc_classes target="_blank" }

### getClanName

!!! function "getClanName( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam group to get the name of. |

	Gets the display name for the specified Steam group; if the local client knows about it.

	!!! returns "Returns: string"
		The Steam group's name in UTF-8 format. Returns an empty string if the provided **clan_id** is invalid or the user does not know about the group.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanName){ .md-button .md-button--doc_classes target="_blank" }

### getClanOfficerByIndex

!!! function "getClanOfficerByIndex( `uint64_t` clan_id, `int` officer_index )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | This must be the same steam group used in the previous call to [getClanOfficerCount](#getclanofficercount). |
    | officer_index | int | An index between 0 and [getClanOfficerCount](#getclanofficercount). |

	Gets the Steam ID of the officer at the given index in a Steam group.

	You must call [getClanOfficerCount](#getclanofficercount) before calling this.

	!!! returns "Returns: uint64_t"
		Will return [STEAM_ID_NIL](main.md#constants) if the **clan_id** or **officer_index** are invalid.
 
	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanOfficerByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getClanOfficerCount

!!! function "getClanOfficerCount( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam group to get the officer count of. |

	Gets the number of officers (administrators and moderators) in a specified Steam group. This also includes the owner of the Steam group. This is used for iteration, after calling this then [getClanOfficerByIndex](#getclanofficerbyindex) can be used to get the Steam ID of each officer.

	You must call [requestClanOfficerList](#requestclanofficerlist) before this to get the required data.

	!!! returns "Returns: int"
		The number of officers in the Steam group. Returns 0 if **clan_id** is invalid or if [requestClanOfficerList](#requestclanofficerlist) has not been called for it.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanOfficerCount){ .md-button .md-button--doc_classes target="_blank" }

### getClanOwner

!!! function "getClanOwner( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam ID of the Steam group to get the owner for. |

	Get the steam ID of the clan owner.

	!!! returns "Returns: uint64_t"
		Will return [STEAM_ID_NIL](main.md#constants) if the **clan_id** is invalid or if [requestClanOfficerList](#requestclanofficerlist) has not been called for it.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanOwner){ .md-button .md-button--doc_classes target="_blank" }

### getClanTag

!!! function "getClanTag( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam group to get the tag of. |

	Gets the unique tag (abbreviation) for the specified Steam group; If the local client knows about it. The Steam group abbreviation is a unique way for people to identify the group and is limited to 12 characters. In some games this will appear next to the name of group members. 

	!!! returns "Returns: string"
		The Steam groups tag in UTF-8 format. Returns an empty string if the provided **clan_id** is invalid or the user does not know about the group.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetClanTag){ .md-button .md-button--doc_classes target="_blank" }

### getCoplayFriend

!!! function "getCoplayFriend( `int` friend_index )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
	| friend_index | int | An index between 0 and [getCoplayFriendCount](#getcoplayfriendcount). |

	Gets the Steam ID of the recently played with user at the given index.

	!!! returns "Returns: uint64_t"
		Invalid indices return [STEAM_ID_NIL](main.md#constants).

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetCoplayFriend){ .md-button .md-button--doc_classes target="_blank" }

### getCoplayFriendCount

!!! function "getCoplayFriendCount( )"
	Gets the number of players that the current users has recently played with, across all games. This is used for iteration, after calling this then [getCoplayFriend](#getcoplayfriend) can be used to get the Steam ID of each player. These players are have been set with previous calls to [setPlayedWith](#setplayedwith).

	!!! returns "Returns: int"
		The number of users that the current user has recently played with.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetCoplayFriendCount){ .md-button .md-button--doc_classes target="_blank" }

### getFollowerCount

!!! function "getFollowerCount( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The user to get the follower count for. |

	Gets the number of users following the specified user. 

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[get_follower_count](#get_follower_count) callback

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFollowerCount){ .md-button .md-button--doc_classes target="_blank" }

### getFriendByIndex

!!! function "getFriendByIndex( `int` friend_number, `FriendFlags` friend_flags )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_number | int | An index between 0 and [getFriendCount](#getfriendcount).
    | friend_flags | [FriendFlags enum](#friendflags) | A combined union (binary "or") of one or more [FriendFlags](#friendflags). This must be the same value as used in the previous call to [getFriendCount](#getfriendcount).

	Gets the Steam ID of the user at the given index.

	See the [FriendFlags enum](#friendflags) for possible values to pass in.

	You must call [getFriendCount](#getfriendcount), passing in the same **friend_flags** value, before calling this.

	!!! returns "Returns: int"
		Invalid indices return [STEAM_ID_NIL](main.md#constants).

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getFriendCoplayGame

!!! function "getFriendCoplayGame( `uint64_t` friend_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t | The Steam ID of the user on the recently-played-with list to get the game played. |

	Gets the app ID of the game that user played with someone on their recently-played-with list. 

	!!! returns "Returns: uint32_t"
		If the **friend_id** is not in the recently-played-with list, this will return [STEAM_ID_NIL](main.md#constants).
 
 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendCoplayGame){ .md-button .md-button--doc_classes target="_blank" }
		

### getFriendCoplayTime

!!! function "getFriendCoplayTime( `uint64_t` friend_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t | The Steam ID of the user on the recently-played-with list to get the timestamp for. |

	Gets the timestamp of when the user played with someone on their recently-played-with list. The time is provided in Unix epoch format (seconds since Jan 1st 1970). 

	!!! returns "Returns: int"
		The time is provided in Unix epoch format (seconds since Jan 1st 1970).  If the **friend_id** is not in the recently-played-with list, this will return 0.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendCoplayTime){ .md-button .md-button--doc_classes target="_blank" }

### getFriendCount

!!! function "getFriendCount( `FriendFlags` friend_flags = FRIEND_FLAG_ALL )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_flags | [FriendFlags enum](#friendflags) | A combined union (binary "or") of one or more [FriendFlags](#friendflags). Defaults to [FRIEND_FLAG_ALL](#friendflags).

	Gets the number of users the client knows about who meet a specified criteria. (Friends, blocked, users on the same server, etc). This can be used to iterate over all of the users by calling [getFriendByIndex](#getfriendbyindex) to get the Steam IDs of each user.
	
	See the [FriendFlags enum](#friendflags) for possible values to pass in.

	!!! returns "Returns: int"
		The number of users that meet the specified criteria. Returns -1 if the current user is not logged on.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendCount){ .md-button .md-button--doc_classes target="_blank" }

### getFriendCountFromSource

!!! function "getFriendCountFromSource( `uint64_t` source_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | source_id | uint64_t | The Steam group, chat room, lobby or game server to get the user count of. |

	Get the number of users in a source (Steam group, chat room, lobby, or game server). This is used for iteration, after calling this then [getFriendFromSourceByIndex](#getfriendfromsourcebyindex) can be used to get the Steam ID of each person in the source.

	Large Steam groups cannot be iterated by the local user.

	If you're getting the number of lobby members then you should use [getNumLobbyMembers](matchmaking.md#getnumlobbymembers) instead.

	!!! returns "Returns: int"
		Returns 0 if the Steam ID provided is invalid or if the local user doesn't have the data available.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendCountFromSource){ .md-button .md-button--doc_classes target="_blank" }

### getFriendFromSourceByIndex

!!! function "getFriendFromSourceByIndex( `uint64_t` source_id, `int` friend_number )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | source_id | uint64_t | This **must** be the same source used in the previous call to [getFriendCountFromSource](#getfriendcountfromsource). |
    | friend_index | int | An index between 0 and [getFriendCountFromSource](#getfriendcountfromsource). |

	Gets the Steam ID at the given index from a source (Steam group, chat room, lobby, or game server).

	You must call [getFriendCountFromSource](#getfriendcountfromsource) before calling this.

	!!! returns "Returns: uint64_t"
		Invalid indices return [STEAM_ID_NIL](main.md#constants).

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendFromSourceByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getFriendGamePlayed

!!! function "getFriendGamePlayed( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the other user. |

    Checks if the specified friend is in a game, and gets info about the game if they are.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| id | int | The game ID that the friend is playing.
		| ip | string | The IP of the server the friend is playing on.
		| game_port | uint16 | The port of the server the friend is playing on.
		| query_port | uint16 | The query port of the server the friend is playing on.
		| lobby | uint64_t | The Steam ID of the lobby the friend is in.

		The dictionary will be empty if the friend is offline or not playing a game.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendGamePlayed){ .md-button .md-button--doc_classes target="_blank" }

### getFriendMessage

!!! function "getFriendMessage( `uint64_t` friend_id, `int` message )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t | The Steam ID of the friend that sent this message. |
    | message | int | The index of the message. This should be the **message_index** field of [connected_friend_chat_message](#connected_friend_chat_message). |

	Gets the data from a Steam friends message. This should only ever be called in response to a [connected_friend_chat_message](#connected_friend_chat_message) callback.

	!!! returns "Returns: dictionary"
		Contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| ret | int | Size of **text**; will be 0 and sets **type** to [CHAT_ENTRY_TYPE_INVALID](main.md#chatentrytype) if the current user is chat-restricted, if the provided **friend_id** is not a friend, or if the index provided in **message** is invalid.
		| text | string | The chat message.
		| type | [ChatEntryType enum](main.md#chatentrytype) | The type of chat entry that was received.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendMessage){ .md-button .md-button--doc_classes target="_blank" }
	[ :material-tag-remove: Removed GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### getFriendPersonaName

!!! function "getFriendPersonaName( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the other user. |

    Gets the specified user's persona (display) name. This will only be known to the current user if the other user is in their friends list, on the same game server, in a chat room or lobby, or in a small Steam group with the local user. To get the persona name of the current user use [getPersonaName](#getpersonaname).

    Upon on first joining a lobby, chat room, or game server the current user will not known the name of the other users automatically; that information will arrive asynchronously via [persona_state_change](#persona_state_change) callbacks.

	!!! returns "Returns: string"
		The current user's persona name in UTF-8 format. Returns an empty string or "[unknown]" if the Steam ID is invalid or not known to the caller.

	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendPersonaName){ .md-button .md-button--doc_classes target="_blank" }

### getFriendPersonaNameHistory

!!! function "getFriendPersonaNameHistory( `uint64_t` steam_id, `int` name_history )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the other user. |
    | name_history | int | The index of the history to receive. 0 is their current persona name, 1 is their most recent before they changed it, etc. |

    Gets one of the previous display names for the specified user. This only works for display names that the current user has seen on the local computer.

	!!! returns "Returns: string"
		The players old persona name at the given index. Returns an empty string when there are no further items in the history.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendPersonaNameHistory){ .md-button .md-button--doc_classes target="_blank" }

### getFriendPersonaState

!!! function "getFriendPersonaState( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the user to get the state of. |

    Gets the current status of the specified user. This will only be known to the current user if the other user is in their friends list, on the same game server, in a chat room or lobby, or in a small Steam group with the local user. To get the state of the current user use [getPersonaState](#getpersonastate).

	!!! returns "Returns: int / [PersonaState enum](#personastate)"
		The friend state of the specified user.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendPersonaState){ .md-button .md-button--doc_classes target="_blank" }

### getFriendRelationship

!!! function "getFriendRelationship( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the other user. |

	Gets a relationship to a specified user.

	!!! returns "Returns: [FriendRelationship enum](#friendrelationship)
		How the users know each other.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendRelationship){ .md-button .md-button--doc_classes target="_blank" }

### getFriendRichPresence

!!! function "getFriendRichPresence( `uint64_t` friend_id, `string` key )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t | The friend to get the Rich Presence value for. |
    | key | string | The Rich Presence key to request. |

	Get a Rich Presence value from a specified friend (typically only used for debugging). 

	!!! returns "Returns: string"
		Returns an empty string if the specified key is not set.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendRichPresence){ .md-button .md-button--doc_classes target="_blank" }

### getFriendRichPresenceKeyCount

!!! function "getFriendRichPresenceKeyCount( `uint64_t` friend_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t | The Steam ID of the user to get the Rich Presence key count of. |

    Gets the number of Rich Presence keys that are set on the specified user. This is used for iteration, after calling this then [getFriendRichPresenceKeyByIndex](#getfriendrichpresencekeybyindex) to get the rich presence keys. This is typically only ever used for debugging purposes.

	!!! returns "Returns: int"
		Returns 0 if there is no Rich Presence information for the specified user.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendRichPresenceKeyCount){ .md-button .md-button--doc_classes target="_blank" }

### getFriendRichPresenceKeyByIndex

!!! function "getFriendRichPresenceKeyByIndex( `uint64_t` friend_id, `int` key_index )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t |This should be the same user provided to the previous call to [getFriendRichPresenceKeyCount](#getfriendrichpresencekeycount). |
    | key_index | int | An index between 0 and [getFriendRichPresenceKeyCount](#getfriendrichpresencekeycount). |

	Get the Rich Presence key at the given index.

	!!! returns "Returns: string"
		Returns an empty string if the index is invalid or the specified user has no Rich Presence data available.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendRichPresenceKeyByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getFriendsGroupCount

!!! function "getFriendsGroupCount( )"
	Gets the number of friends groups (tags) the user has created. This is used for iteration, after calling this then [getFriendsGroupIDByIndex](#getfriendsgroupidbyindex) can be used to get the ID of each friend group. This is not to be confused with Steam groups. Those can be obtained with [getClanCount](#getclancount).

	!!! returns "Returns: int"
		The number of friends groups the current user has.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendsGroupMembersCount){ .md-button .md-button--doc_classes target="_blank" }

### getFriendsGroupIDByIndex

!!! function "`int16` getFriendsGroupIDByIndex( `int16` friend_group )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_group | int16 | An index between 0 and [getFriendsGroupCount](#getfriendsgroupcount). |

	Gets the friends group ID for the given index.

	You must call [getFriendsGroupCount](#getfriendsgroupcount) before calling this.

	!!! returns "Returns: int16"
		Invalid indices return [INVALID_FRIEND_GROUP_ID](#constants)

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendsGroupIDByIndex){ .md-button .md-button--doc_classes target="_blank" }

### getFriendsGroupMembersCount

!!! function "getFriendsGroupMembersCount( `int16` friend_group )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_group | int16 | The friends group ID to get the number of friends in. |

	Gets the number of friends in a given friends group. This should be called before getting the list of friends with [getFriendsGroupMembersList](#getfriendsgroupmemberslist).

	!!! returns "Returns: int"
		The number of friends in the specified friends group.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendsGroupMembersCount){ .md-button .md-button--doc_classes target="_blank" }

### getFriendsGroupMembersList

!!! function "`array` getFriendsGroupMembersList( `int16` friend_group, `int` member_count )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_group | int16 | The friends group ID to get the members list of. |
    | member_count | int | This should be the value returned by [getFriendsGroupMembersCount](#getfriendsgroupmemberscount). |

	Gets the number of friends in the given friends group. If fewer friends exist than requested those positions' Steam IDs will be invalid.

	You must call [getFriendsGroupMembersCount](#getfriendsgroupmemberscount) before calling this to set up the **member_count** argument.

	!!! returns "Returns: array"
		Contains a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| friend Steam IDs | uint64_t | -
 
 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendsGroupMembersList){ .md-button .md-button--doc_classes target="_blank" }

### getFriendsGroupName

!!! function "getFriendsGroupName( `int16` friend_group )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_group | int16 | The friends group ID to get the name of. |

	Gets the name for the given friends group. 

	!!! returns "Returns: string"
		The friend groups name in UTF-8 format. Returns an empty string if the group ID is invalid.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendsGroupName){ .md-button .md-button--doc_classes target="_blank" }

### getFriendSteamLevel

!!! function "getFriendSteamLevel( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the other user. |

	Gets the Steam level of the specified user. You can use the local users Steam ID to get their level.

	!!! returns "Returns: int"
		The Steam level if it's available.

		If the Steam level is not immediately available for the specified user then this returns 0 and queues it to be downloaded from the Steam servers. When it gets downloaded a [persona_state_change](#persona_state_change) callback will be posted with **flags** including [PERSONA_CHANGE_STEAM_LEVEL](#constants).

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetFriendSteamLevel){ .md-button .md-button--doc_classes target="_blank" }

### getLargeFriendAvatar

!!! function "getLargeFriendAvatar( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam user to get the avatar for. |

	Gets a handle to the large (128x128px) avatar for the specified user. You can pass in [getSteamID](user.md#getsteamid) to get the current user's avatar.

	It is possible for the size to be larger than 128x128 if the user uploaded a larger image to their profile.

	This only works for users that the local user knows about. They will automatically know about their friends, people on leaderboards they've requested, or people in the same source as them (Steam group, chat room, lobby, or game server). If they don't know about them then you must call [requestUserInformation](#requestuserinformation) to cache the avatar locally.

	!!! returns "Returns: int"
		A Steam image handle which is used with [getImageSize](utils.md#getimagesize) and [getImageRGBA](utils.md#getimagergba).  Returns 0 if no avatar is set for the user.  Returns -1 if the avatar image data has not been loaded yet and requests that it gets download.  In this case wait for a [avatar_loaded](#avatar_loaded) or [avatar_image_loaded](#avatar_image_loaded) callback and then call this again.

	!!! trigger "Triggers"
		* [avatar_loaded](#avatar_loaded) callback
		* [avatar_image_loaded](#avatar_image_loaded) callback

	!!! info "Notes"
		Consider using [getPlayerAvatar](#getplayeravatar) instead because it simplifies the process of getting avatar data.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetLargeFriendAvatar){ .md-button .md-button--doc_classes target="_blank" }

### getMediumFriendAvatar

!!! function "getMediumFriendAvatar( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam user to get the avatar for. |

	Gets a handle to the medium (64x64px) avatar for the specified user. You can pass in [getSteamID](user.md#getsteamid) to get the current user's avatar.

	This only works for users that the local user knows about. They will automatically know about their friends, people on leaderboards they've requested, or people in the same source as them (Steam group, chat room, lobby, or game server). If they don't know about them then you must call [requestUserInformation](#requestuserinformation) to cache the avatar locally.

	!!! returns "Returns: int"
		A Steam image handle which is used with [getImageSize](utils.md#getimagesize) and [getImageRGBA](utils.md#getimagergba).  Returns 0 if no avatar is set for the user.  Returns -1 if the avatar image data has not been loaded yet and requests that it gets download.  In this case wait for a [avatar_loaded](#avatar_loaded) or [avatar_image_loaded](#avatar_image_loaded) callback and then call this again.

	!!! info "Notes"
		Consider using [getPlayerAvatar](#getplayeravatar) instead because it simplifies the process of getting avatar data.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetMediumFriendAvatar){ .md-button .md-button--doc_classes target="_blank" }

### getNumChatsWithUnreadPriorityMessages

!!! function "getNumChatsWithUnreadPriorityMessages( )"
	Return the number of chats (friends or chat rooms) with unread messages. A "priority" message is one that would generate some sort of toast or notification, and depends on user settings.

	You can register for [unread_chat_messages_changed](#unread_chat_messages_changed) callbacks to know when this has potentially changed.

	!!! returns "Returns: int"
		The number of chats (friends or chat rooms) with unread messages.

	---
	[ :material-tag-plus: Added GodotSteam 4.16](../changelog/godot4.md/#version-416){ .md-button .md-button--changes target="_blank" }

### getPersonaName

!!! function "getPersonaName( )"
	This is stored in UTF-8 format. Gets the current user's persona (display) name. This is the same name that is displayed the users community profile page. To get the persona name of other users use [getFriendPersonaName](#getfriendpersonaname).

	!!! returns "Returns: string"
		The current user's persona name in UTF-8 format. Guaranteed to not be empty.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetPersonaName){ .md-button .md-button--doc_classes target="_blank" }

### getPersonaState

!!! function "getPersonaState( )"
	Gets the friend status of the current user. To get the state of other users use [getFriendPersonaState](#getfriendpersonastate).

	!!! returns "Returns: [PersonaState enum](#personastate)"
		The friend state of the current user. (Online, Offline, In-Game, etc).

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetPersonaState){ .md-button .md-button--doc_classes target="_blank" }

### getPlayerAvatar

!!! function "getPlayerAvatar( `int` size = 2, `uint64_t` steam_id = 0 )"
	Get a player's avatar. This is the preferred method of getting avatars as it shortcuts the various avatar functions in Steamworks to reduce the number of steps required.

	Size can be passed as the [AvatarSizes enum](#avatarsizes) or an integer:

	* 1 (small)
	* 2 (medium)
	* 3 (large)

	If no **steam_id** is passed in, it will get the current user's avatar.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[avatar_loaded](#avatar_loaded) callback

	!!! info "Notes"
		This is a unique function to GodotSteam. See the [Avatars tutorial](../tutorials/avatars.md) for more information.

### getPlayerNickname

!!! function "getPlayerNickname( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the other user. |

	Returns nickname the current user has set for the specified player. Returns NULL if the no nickname has been set for that player. 

	!!! returns "Returns: string"
		Returns empty if the no nickname has been set for that user.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetPlayerNickname){ .md-button .md-button--doc_classes target="_blank" }

### getProfileItemPropertyString

!!! function "getProfileItemPropertyString( `uint64_t` steam_id, `CommunityProfileItemType` item_type, `CommunityProfileItemProperty` item_property )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The user that you had already retrieved equipped items for. |
    | item_type | [CommunityProfileItemType enum](#communityprofileitemtype) | Type of item you are retrieving the property for. |
    | item_property | [CommunityProfileItemProperty enum](#communityprofileitemproperty) | The string property you want to retrieve. |

    Returns a string property for a user's equipped profile item.

	!!! returns "Returns: string"

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetProfileItemPropertyString){ .md-button .md-button--doc_classes target="_blank" }

### getProfileItemPropertyInt

!!! function "getProfileItemPropertyInt( `uint64_t` steam_id, `CommunityProfileItemType` item_type, `CommunityProfileItemProperty` item_property )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The user that you had already retrieved equipped items for. |
    | item_type | [CommunityProfileItemType enum](#communityprofileitemtype) | Type of item you are retrieving the property for. |
    | item_property | [CommunityProfileItemProperty enum](#communityprofileitemproperty) | The string property you want to retrieve. |

    Returns an unsigned integer property for a user's equipped profile item.

	!!! returns "Returns: uint32_t"

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetProfileItemPropertyUint){ .md-button .md-button--doc_classes target="_blank" }

### getRecentPlayers

!!! function "getRecentPlayers( )"
	Get list of players user has recently played game with.

	!!! returns "Returns: array"
		Contains a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| player | dictionary | Information about the recent player.

		**player** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| id | uint64_t | The Steam ID of the recent player.
		| name | string | The Steam username of the recent player.
		| time | int | The timestamp of when the user played with someone on their recently-played-with list.
		| status | [PersonaState enum](#personastate) | The persona state of the recent player.

	!!! info "Notes""
		This is a unique function to GodotSteam.

### getSmallFriendAvatar

!!! function "getSmallFriendAvatar( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam user to get the avatar for. |

    Gets a handle to the small (32x32px) avatar for the specified user. You can pass in [getSteamID](user.md#getsteamid) to get the current user's avatar.

    This only works for users that the local user knows about. They will automatically know about their friends, people on leaderboards they've requested, or people in the same source as them (Steam group, chat room, lobby, or game server). If they don't know about them then you must call [requestUserInformation](#requestuserinformation) to cache the avatar locally.

	!!! returns "Returns: int"
		A Steam image handle which is used with [getImageSize](utils.md#getimagesize) and [getImageRGBA](utils.md#getimagergba).

	!!! info "Notes"
		Consider using [getPlayerAvatar](#getplayeravatar) instead because it simplifies the process of getting avatar data.

 	---
 	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GetSmallFriendAvatar){ .md-button .md-button--doc_classes target="_blank" }

### getUserFriendsGroups

!!! function "getUserFriendsGroups( )"
	Get list of friends groups (tags) the user has created. This is not to be confused with Steam groups.

	!!! returns "Returns: array"
		Contains a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| tags | dictionary | Information about the friends groups (tags).

		**tags** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| id | int16 | The friends group ID.
		| name | string | The name for the given friends group.
		| members | int | The number of members in the friends group.

### getUserRestrictions

!!! function "getUserRestrictions( )"
	If current user is chat restricted, they can't send or receive any text or voice chat messages. The user can't see custom avatars. But the user can be online and send or receive game invites. A chat restricted user cannot add friends or join any groups. Restricted users can still be online and send/receive game invites.

	!!! returns "Returns: uint32"
		It should be one of the UserRestriction enums.

	---
	[ :material-tag-remove: Removed GodotSteam 4.14](../changelog/godot4.md/#version-414){ .md-button .md-button--changes target="_blank" }

### getUserSteamFriends

!!! function "getUserSteamFriends( )"
	Get a list of user's Steam friends; a mix of different Steamworks API friend functions.

	!!! returns "Returns: array"
		Contains a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| friends | dictionary | Information about the friend.

		**friends** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| id | uint64_t | The Steam ID of the friend.
		| name | string | The Steam username of the friend.
		| status | [PersonaState enum](#personastate) | The persona state of the friend.

	!!! info "Notes"
		This is a unique function to GodotSteam.

### getUserSteamGroups

!!! function "`array` getUserSteamGroups( )"
	Get list of user's Steam groups; a mix of different Steamworks API group functions.

	!!! returns "Returns: array"
		Contains a list of:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| groups | dictionary | Information about the group.

		**groups** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| id | uint64_t | The Steam group's Steam ID.
		| name | string | The Steam group's name in UTF-8 format.
		| tag | string | The Steam groups tag in UTF-8 format.

	!!! info "Notes"
		This is a unique function to GodotSteam.

### hasEquippedProfileItem

!!! function "hasEquippedProfileItem( `uint64_t` steam_id, `CommunityProfileItemType` item_type )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The user that you had already retrieved equipped items for. |
    | item_type | [CommunityProfileItemType enum](#communityprofileitemtype) | Type of item you want to see is equipped or not. |

    After calling [requestEquippedProfileItems](#requestequippedprofileitems), you can use this function to check if the user has a type of profile item equipped or not.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[equipped_profile_items](#equipped_profile_items) call result

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#BHasEquippedProfileItem){ .md-button .md-button--doc_classes target="_blank" }

### hasFriend

!!! function "hasFriend( `uint64_t` steam_id, `FriendFlags` friend_flags )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the other user. |
    | friend_flags | [FriendFlags enum](#friendflags) | A combined union (binary "or") of one or more [FriendFlags](#friendflags).

	!!! returns "Returns: bool"
		Returns true if the specified user meets any of the criteria specified in **friend_flags**; otherwise, false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#HasFriend){ .md-button .md-button--doc_classes target="_blank" }

### inviteUserToGame

!!! function "inviteUserToGame( `uint64_t` friend_id, `string` connect_string )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t | The Steam ID of the friend to invite. |
    | connect_string | string | A string that lets the friend know how to join the game (I.E. the game server IP). This can not be longer than specified in [MAX_RICH_PRESENCE_VALUE_LENTH](#constants) (256). |

    Invites a friend or clan member to the current game using a special invite string. If the target user accepts the invite then the **connect_string** gets added to the command-line when launching the game.

	!!! returns "Returns: bool"
		Returns true if the invite was successfully sent; otherwise, false under the following conditions:

		* The Steam ID provided to **friend_id** was invalid.
		* The Steam ID provided to **friend_id** is not a friend or does not share the same Steam Group as the current user.
		* The value provided to **connect_string** was too long.

	!!! trigger "Triggers"
		[join_game_requested](#join_game_requested) callback if the game is already running for that user.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#InviteUserToGame){ .md-button .md-button--doc_classes target="_blank" }

### isClanChatAdmin

!!! function "isClanChatAdmin( `uint64_t` chat_id, `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | chat_id | uint64_t | The Steam ID of the Steam group chat room. |
    | steam_id | uint64_t | The Steam ID of the user to check the admin status of. |

	Checks if a user in the Steam group chat room is an admin. 

	!!! returns "Returns: bool"
		Returns true if the specified user is an admin; otherwise, false if:

		* The user is not an admin.
		* The current user is not in the chat room specified.
		* The specified user is not in the chat room.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#IsClanChatAdmin){ .md-button .md-button--doc_classes target="_blank" }

### isClanPublic

!!! function "isClanPublic( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam ID of the Steam group. |

    Checks if the Steam group is public.

	!!! returns "Returns: bool"
		Returns true if the specified group is public; otherwise, false if the specified group is not public.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#IsClanPublic){ .md-button .md-button--doc_classes target="_blank" }

### isClanOfficialGameGroup

!!! function "isClanOfficialGameGroup( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam ID of the Steam group. |

    Checks if the Steam group is an official game group/community hub. 

	!!! returns "Returns: bool"
		Returns true if the specified group is an official game group/community hub; otherwise, false if the specified group is not an official game group / community hub.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#IsClanOfficialGameGroup){ .md-button .md-button--doc_classes target="_blank" }

### isClanChatWindowOpenInSteam

!!! function "isClanChatWindowOpenInSteam( `uint64_t` chat_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | chat_id | uint64_t | The Steam ID of the Steam group chat room to check. |

	Checks if the Steam Group chat room is open in the Steam UI.

	!!! returns "Returns: bool" 
		Returns true if the specified Steam group chat room is opened; otherwise, false.  This also returns false if the specified Steam group chat room is unknown.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#IsClanChatWindowOpenInSteam){ .md-button .md-button--doc_classes target="_blank" }

### isFollowing

!!! function "isFollowing( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID to check if we are following. |

	Checks if the current user is following the specified user.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[is_following](#is_following) callback

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#IsFollowing){ .md-button .md-button--doc_classes target="_blank" }

### isUserInSource

!!! function "isUserInSource( `uint64_t` steam_id, `uint64_t` source_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The user to check if they are in the source. |
    | source_id | uint64_t | The source to check for the user. |

	Checks if a specified user is in a source (Steam group, chat room, lobby, or game server).

	!!! returns "Returns: bool"
		Returns true if the local user can see that **steam_id** is a member or in **source_id**; otherwise, false.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#IsUserInSource){ .md-button .md-button--doc_classes target="_blank" }

### joinClanChatRoom

!!! function "joinClanChatRoom( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam ID of the Steam group to join. |

	Allows the user to join Steam group (clan) chats right within the game. The behavior is somewhat complicated, because the user may or may not be already in the group chat from outside the game or in the overlay. You can use [activateGameOverlayToUser](#activategameoverlaytouser) to open the in-game overlay version of the chat.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[join_clan_chat_complete](#join_clan_chat_complete) call result

		May additionally trigger the following callbacks:

		* [connected_chat_join](#connected_chat_join)
		* [connected_clan_chat_message](#connected_clan_chat_message)

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#JoinClanChatRoom){ .md-button .md-button--doc_classes target="_blank" }

### leaveClanChatRoom

!!! function "leaveClanChatRoom( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam ID of the Steam group to leave. |

	Leaves a Steam group chat that the user has previously entered with [joinClanChatRoom](#joinclanchatroom).

	!!! returns "Returns: bool"
		Returns true if user is in the specified chat room; otherwise, false.

	!!! trigger "Triggers"
		[connected_chat_leave](#connected_chat_leave) callback

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#LeaveClanChatRoom){ .md-button .md-button--doc_classes target="_blank" }

### openClanChatWindowInSteam

!!! function "openClanChatWindowInSteam( `uint64_t` chat_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | chat_id | uint64_t | The Steam ID of the Steam group chat room to open. |

	Opens the specified Steam group chat room in the Steam UI.

	!!! returns "Returns: bool"
		Returns true if the user successfully entered the Steam group chat room; otherwise, false in one of the following situations:

		* The provided Steam group chat room does not exist or the user does not have access to join it.
	    * The current user is currently rate limited.
	    * The current user is chat restricted.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#OpenClanChatWindowInSteam){ .md-button .md-button--doc_classes target="_blank" }

### registerProtocolInOverlayBrowser

!!! function "registerProtocolInOverlayBrowser( `string` protocol )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | protocol | string | The navigations to block. |

    Call this before calling [activateGameOverlayToWebPage](#activategameoverlaytowebpage) to have the Steam Overlay Browser block navigations to your specified protocol (scheme) URIs and instead dispatch a [overlay_browser_protocol](#overlay_browser_protocol) callback to your game. [activateGameOverlayToWebPage](#activategameoverlaytowebpage) must have been called with [OVERLAY_TO_WEB_PAGE_MODE_MODAL](#overlaytowebpagemode).

	!!! returns "Returns: bool"

	!!! info "Notes"
		While this function is in the SDK, it is not listed in the Steamworks docs.

### replyToFriendMessage

!!! function "replyToFriendMessage( `uint64_t` steam_id, `string` message )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The Steam ID of the friend to send the message to. |
    | message | string | The UTF-8 formatted message to send. |

	Sends a message to a Steam friend. Can possibly fail if the user is rate limited or chat restricted.

	!!! returns "Returns: bool"
		Returns true if the message was successfully sent; otherwise, false if the current user is rate limited or chat restricted.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#ReplyToFriendMessage){ .md-button .md-button--doc_classes target="_blank" }

### requestClanOfficerList

!!! function "requestClanOfficerList( `uint64_t` clan_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | clan_id | uint64_t | The Steam group to get the officers list for. |

	Requests information about a Steam group officers (administrators and moderators). You can only ask about Steam groups that a user is a member of. This won't download avatars for the officers automatically. If no avatar image is available for an officer, then call [requestUserInformation](#requestuserinformation) to download the avatar.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[request_clan_officer_list](#request_clan_officer_list) callback

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#RequestClanOfficerList){ .md-button .md-button--doc_classes target="_blank" }

### requestEquippedProfileItems

!!! function "requestEquippedProfileItems( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The user that you want to retrieve equipped items for. |

	Requests the list of equipped Steam Community profile items for the given user from Steam.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[equipped_profile_items](#equipped_profile_items) call result

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#RequestEquippedProfileItems){ .md-button .md-button--doc_classes target="_blank" }

### requestFriendRichPresence

!!! function "requestFriendRichPresence( `uint64_t` friend_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | friend_id | uint64_t | The Steam ID of the user to request the rich presence of. |

	Requests Rich Presence data from a specific user. This is used to get the Rich Presence information from a user that is not a friend of the current user, like someone in the same lobby or game server. This function is rate limited, if you call this too frequently for a particular user then it will just immediately post a callback without requesting new data from the server.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		[friend_rich_presence_update](#friend_rich_presence_update) callback

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#RequestFriendRichPresence){ .md-button .md-button--doc_classes target="_blank" }

### requestUserInformation

!!! function "requestUserInformation( `uint64_t` steam_id, `bool` require_name_only )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The user to request the information of. |
    | require_name_only | bool | Retrieve the Persona name only (true)? Or both the name and the avatar (false)? |

	Requests the persona name and the avatar of a specified user. If **require_name_only** is set, then the avatar of a user isn't downloaded.

	It's a lot slower to download avatars and churns the local cache, so if you don't need avatars, set require_name_only to true.

	If this returns true, it means that data is being requested and a [persona_state_change](#persona_state_change) callback will be posted when it's retrieved. If this returns false, it means that we already have all the details about that user and functions can be called immediately.

	!!! returns "Returns: bool"
		Returns true means that the data has being requested, and a [persona_state_change](#persona_state_change) callback will be posted when it's retrieved; otherwise, false means that we already have all the details about that user and functions that require this information can be used immediately.

	!!! trigger "Triggers"
		[persona_state_change](#persona_state_change) callback

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#RequestUserInformation){ .md-button .md-button--doc_classes target="_blank" }

### sendClanChatMessage

!!! function "sendClanChatMessage( `uint64_t` chat_id, `string` text )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | chat_id | uint64_t | The Steam ID of the group chat to send the message to. |
    | text | string | The UTF-8 formatted message to send. This can be up to 2048 characters long. |

	Sends a message to a Steam group chat room.

	!!! returns "Returns: bool"
		Returns rue if the message was successfully sent; otherwise, false under one of the following circumstances:

		* The current user is not in the specified group chat.
		* The current user is not connected to Steam.
		* The current user is rate limited.
		* The current user is chat restricted.
		* The message in pchText exceeds 2048 characters.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#SendClanChatMessage){ .md-button .md-button--doc_classes target="_blank" }

### setInGameVoiceSpeaking

!!! function "setInGameVoiceSpeaking( `uint64_t` steam_id, `bool` speaking )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | User to set voice speaking for; allegedly unused. |
    | speaking | bool | Did the user start speaking in game (true) or stopped speaking in game (false)? |

	User is in a game pressing the talk button (will suppress the microphone for all voice comms from the Steam friends UI). 

	Let Steam know that the user is currently using voice chat in game. This will suppress the microphone for all voice communication in the Steam UI.

	!!! returns "Returns: void"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#SetInGameVoiceSpeaking){ .md-button .md-button--doc_classes target="_blank" }

### setListenForFriendsMessages

!!! function "setListenForFriendsMessages( `bool` intercept_enabled )"
	Listens for Steam friends chat messages. You can then show these chats inline in the game. For example with a Blizzard style chat message system or the chat system in Dota 2.

	After enabling this you will receive [connected_friend_chat_message](#connected_friend_chat_message) callbacks when ever the user receives a chat message. You can get the actual message data from this callback with [getFriendMessage](#getfriendmessage). You can send messages with [replyToFriendMessage](#replytofriendmessage).

	!!! returns "Returns: bool"
		Always returns true.

	!!! trigger "Triggers"
		[connected_friend_chat_message](#connected_friend_chat_message) callback

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#SetListenForFriendsMessages){ .md-button .md-button--doc_classes target="_blank" }

### setPersonaName

!!! function "setPersonaName( `string` name )"
	Sets the current user's persona name, stores it on the server and publishes the changes to all friends who are online. Changes take place locally immediately, and a [persona_state_change](#persona_state_change) callback is posted, presuming success. If the name change fails to happen on the server, then an additional [persona_state_change](#persona_state_change) callback will be posted to change the name back, in addition to the final result available in the call result.

	!!! returns "Returns: void"

	!!! trigger "Triggers"
		name_changed callback

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#SetPersonaName){ .md-button .md-button--doc_classes target="_blank" }
	[ :material-tag-remove: Removed GodotSteam 4.14](../changelog/godot4.md/#version-414){ .md-button .md-button--changes target="_blank" }

### setPlayedWith

!!! function "setPlayedWith( `uint64_t` steam_id )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | steam_id | uint64_t | The other user that we have played with. |

	Set player as 'played With' for the game.  You can view the players you have recently played with [here on the Steam community](http://steamcommunity.com/my/friends/coplay/){ target="\_blank" } and in the [Steam Overlay.](https://partner.steamgames.com/doc/features/overlay){ target="\_blank" }

	The current user must be in game with the other player for the association to work.

	!!! returns "Returns: void"

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#SetPlayedWith){ .md-button .md-button--doc_classes target="_blank" }

### setRichPresence

!!! function "setRichPresence( `string` key, `string` value )"
	| Parameter | Type | Notes |
    | -------- | ---- | ----- |
    | key | string | The rich presence 'key' to set. This can not be longer than specified in MAX_RICH_PRESENCE_KEY_LENGTH (64). |
    | value | string | The rich presence 'value' to associate with pchKey. This can not be longer than specified in MAX_RICH_PRESENCE_VALUE_LENTH (256). If this is set to an empty string ("") then the key is removed if it's set. |

    Sets a Rich Presence key/value for the current user that is automatically shared to all friends playing the same game. Each user can have up to 20 keys set as defined by [MAX_RICH_PRESENCE_KEYS](#constants). There are two special keys used for viewing/joining games:

	* **status** - A UTF-8 string that will show up in the 'view game info' dialog in the Steam friends list.
	* **connect** - A UTF-8 string that contains the command-line for how a friend can connect to a game. This enables the 'join game' button in the 'view game info' dialog, in the steam friends list right click menu, and on the players Steam community profile. Be sure your app implements [getLaunchCommandLine](apps.md#getlaunchcommandline) so you can disable the popup warning when launched via a command line.

	There are three additional special keys used by the new Steam Chat:

	* **steam_display** - Names a rich presence localization token that will be displayed in the viewing user's selected language in the Steam client UI. See Rich Presence Localization for more info, including a link to a page for testing this rich presence data. If steam_display is not set to a valid localization tag, then rich presence will not be displayed in the Steam client.
	* **steam_player_group** - When set, indicates to the Steam client that the player is a member of a particular group. Players in the same group may be organized together in various places in the Steam UI. This string could identify a party, a server, or whatever grouping is relevant for your game. The string itself is not displayed to users.
	* **steam_player_group_size** - When set, indicates the total number of players in the steam_player_group. The Steam client may use this number to display additional information about a group when all of the members are not part of a user's friends list. (For example, "Bob, Pete, and 4 more".)

	You can clear all of the keys for the current user with [clearRichPresence](#clearrichpresence). To get rich presence keys for friends use [getFriendRichPresence](#getfriendrichpresence).

	!!! returns "Returns: bool"
		Returns true if the rich presence was set successfully; otherwise, false under the following conditions:

		* **key** was longer than [MAX_RICH_PRESENCE_KEY_LENGTH (64)](#constants) or had a length of 0.
		* **value** was longer than [MAX_RICH_PRESENCE_KEY_LENGTH (256)](#constants).
		* The user has reached the maximum amount of rich presence keys as defined by [MAX_RICH_PRESENCE_KEYS (30)](#constants).

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#SetRichPresence){ .md-button .md-button--doc_classes target="_blank" }

{==
## :material-signal: Signals
==}

These callbacks require you to [setup one of the three callback methods to receive them.](https://godotsteam.com/tutorials/initializing/#callbacks)

### avatar_loaded
			
!!! function "avatar_loaded"
	Called when a large avatar is loaded if you have tried requesting it when it was unavailable.

	Emits signal in response to function [getLargeFriendAvatar](#getlargefriendavatar), [getMediumFriendAvatar](#getmediumfriendavatar), or [getSmallFriendAvatar](#getsmallfriendavatar).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| avatar_id | uint64_t | Steam ID the avatar has been loaded for.
		| width | int | Width of the loaded image.
		| data | PackedByteArray | The actual image data to use.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#AvatarImageLoaded_t){ .md-button .md-button--doc_classes target="_blank" }

### avatar_image_loaded

!!! function "avatar_image_loaded"
	Called when an avatar is requested; sends back the raw Steamworks callback data compared to [avatar_loaded](#avatar_loaded).

	Emits signal in response to function [getLargeFriendAvatar](#getlargefriendavatar), [getMediumFriendAvatar](#getmediumfriendavatar), or [getSmallFriendAvatar](#getsmallfriendavatar).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| avatar_id | uint64_t | Steam ID the avatar has been loaded for.
		| avatar_handle | uint32_t | The image handle of the now loaded image.
		| width | uint32_t | Width of the loaded image.
		| height | uint32_t | Height of the loaded image.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#AvatarImageLoaded_t){ .md-button .md-button--doc_classes target="_blank" }

### change_server_requested

!!! function "change_server_requested"	
	This callback is made when joining a game. If the user is attempting to join a lobby, then the callback [join_requested] will be made.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | server | string | Server address, examples: "127.0.0.1:27015", "tf2.valvesoftware.com".
		| password | string |  Server password, if any.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GameServerChangeRequested_t){ .md-button .md-button--doc_classes target="_blank" }

### clan_activity_downloaded

!!! function "clan_activity_downloaded"
	Called when a Steam group activity has received.

	Emits signal in response to function [downloadClanActivityCounts](#downloadclanactivitycounts).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | online | int | Returns the number of members that are online.
        | in_game | int | Returns the number members that are in game (excluding those with their status set to offline).
        | chatting | int | Returns the number of members in the group chat room.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#DownloadClanActivityCountsResult_t){ .md-button .md-button--doc_classes target="_blank" }

### connected_chat_join

!!! function "connected_chat_join"
	Called when a user has joined a Steam group chat that the we are in.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | chat_id | uint64_t | The Steam ID of the chat that a user has joined.
		| steam_id | uint64_t | The Steam ID of the user that has joined the chat.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GameConnectedChatJoin_t){ .md-button .md-button--doc_classes target="_blank" }

### connected_chat_leave

!!! function "connected_chat_leave"
	Called when a user has left a Steam group chat that the we are in.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | chat_id | uint64_t | The Steam ID of the chat that a user has left.
		| steam_id | uint64_t | The Steam ID of the user that has left the chat.
		| kicked | bool | 	Was the user kicked by an officer (true), or not (false)?
		| dropped | bool | Was the user's connection to Steam dropped (true), or did they leave via other means (false)?

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GameConnectedChatLeave_t){ .md-button .md-button--doc_classes target="_blank" }

### connected_clan_chat_message

!!! function "connected_clan_chat_message"
	Called when a chat message has been received in a Steam group chat that we are in.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | clan_chat_id | uint64_t | The Steam ID of the chat that the message was received in.
        | message_index | int | The index of the message.
        | message_text | string | The actual chat message.
        | type | [ChatEntryType enum](main.md#chatentrytype) | The type of chat entry that was received.
        | chatter | uint64_t | The Steam ID of the user that sent the message.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GameConnectedClanChatMsg_t){ .md-button .md-button--doc_classes target="_blank" }

### connected_friend_chat_message

!!! function "connected_friend_chat_message"
	Called when chat message has been received from a friend.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | steam_id | uint64_t | The Steam ID of the friend that sent the message.
        | message_index | int | The index of the message.
        | message_text | string | The actual chat message.
        | type | [ChatEntryType enum](main.md#chatentrytype) | The type of chat entry that was received.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GameConnectedFriendChatMsg_t){ .md-button .md-button--doc_classes target="_blank" }

### enumerate_following_list

!!! function "enumerate_following_list"
	Returns the result of [enumerateFollowingList](#enumeratefollowinglist).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
        | result | [Result enum](main.md#result) | The result of the operation.
		| following | PackedInt64Array | The list of users that we are following; their Steam ID's.
		| returned_following | int32 | The number of users we are following returned in **following**.
		| total_following | int32 | The total number of people we are following. If this is greater than **returned_following**, then you should make a subsequent call to [enumerateFollowingList](#enumeratefollowinglist) with **returned_following** as the index to get the next portion of followers.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#FriendsEnumerateFollowingList_t){ .md-button .md-button--doc_classes target="_blank" }

### equipped_profile_items

!!! function "equipped_profile_items"
	Call result from [requestEquippedProfileItems](#requestequippedprofileitems). Also sent as a callback.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | The result of the operation.
		| steam_id | uint64_t | The Steam ID of the profile that was checked.
		| profile_data | dictionary | A collection of profile information.

		**profile_data** contains the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| avatar_animated | bool | Whether or not the profile has an animated avatar equipped.
		| avatar_frame | bool | Whether or not the profile has an avatar frame equipped.
		| profile_modifier | bool | Whether or not the profile has a modifier equipped.
		| profile_background | bool | Whether or not the profile has a background equipped.
		| profile_mini_background | bool | Whether or not the profile has a mini-background equipped.
		| from_cache | bool | Whether or not the results were from the local cache.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#EquippedProfileItems_t){ .md-button .md-button--doc_classes target="_blank" }

### equipped_profile_items_changed

!!! function "equipped_profile_items_changed"
	Callback for when a user's equipped Steam Commuity profile items have changed. This can be for the current user or their friends.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| steam_id | uint64_t | The Steam ID of the profile that changed.

	!!! info "Notes"
		GodotSteam is using the callback version.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#EquippedProfileItemsChanged_t){ .md-button .md-button--doc_classes target="_blank" }

### friend_rich_presence_update

!!! function "friend_rich_presence_update"
	Called when rich presence data has been updated for a user, this can happen automatically when friends in the same game update their rich presence, or after a call to [requestFriendRichPresence](#requestfriendrichpresence).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| steam_id | uint64_t | Friend whose rich presence has changed.
		| app_id | uint32_t | The app ID of the game; should always be the current game.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#FriendRichPresenceUpdate_t){ .md-button .md-button--doc_classes target="_blank" }

### get_follower_count

!!! function "get_follower_count"
	Returns the result of [getFollowerCount](#getfollowercount).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | The result of the operation.
		| steam_id | uint64_t | The Steam ID of the user we requested the follower count for.
		| count | int | 	The number of followers the user has.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#FriendsGetFollowerCount_t){ .md-button .md-button--doc_classes target="_blank" }

### is_following

!!! function "is_following"
	Returns the result of [isFollowing](#isfollowing).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| result | [Result enum](main.md#result) | The result of the operation.
		| steam_id | uint64_t | The Steam ID that was checked.
		| following | bool | Are we following the user?

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#FriendsIsFollowing_t){ .md-button .md-button--doc_classes target="_blank" }

### join_clan_chat_complete

!!! function "join_clan_chat_complete"
	Posted when the user has attempted to join a Steam group chat via [joinClanChatRoom](#joinclanchatroom).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| chat_id | uint64_t | The Steam ID of the chat that the user has joined.
		| response | [ChatRoomEnterResponse enum](main.md#chatroomenterresponse) | The result of the operation.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#JoinClanChatRoomCompletionResult_t){ .md-button .md-button--doc_classes target="_blank" }

### join_game_requested

!!! function "join_game_requested"
	Called when the user tries to join a game from their friends list or after a user accepts an invite by a friend with [inviteUserToGame](#inviteusertogame).

	Emits signal in response to receiving a Steam invite.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| user | uint64_t | The friend they joined through. This will be invalid if not directly via a friend.
		| connect | string | The value associated with the "connect" Rich Presence key.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GameRichPresenceJoinRequested_t){ .md-button .md-button--doc_classes target="_blank" }

### join_requested

!!! function "join_requested"
	Called when the user tries to join a lobby from their friends list or after a user accepts an invite by a friend with [inviteUserToGame](#inviteusertogame).

	Emits signal in response to receiving a Steam invite.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| lobby | uint64_t | The Steam ID of the lobby to connect to.
		| steam_id | uint64_t | The friend they joined through. This will be invalid if not directly via a friend.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GameLobbyJoinRequested_t){ .md-button .md-button--doc_classes target="_blank" }

### overlay_browser_protocol

!!! function "overlay_browser_protocol"
	Dispatched when an overlay browser instance is navigated to a protocol/scheme registered by [registerProtocolInOverlayBrowser](#registerprotocolinoverlaybrowser).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| uri | string | The blocked navigation URI which was sent back to the game.

### overlay_toggled

!!! function "overlay_toggled"
	Posted when the Steam Overlay activates or deactivates. The game can use this to be pause or resume single player games.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| toggled | bool | True if it's just been activated, false otherwise.
		| user_initiated | bool | True if the user asked for the overlay to be activated/deactivated.
		| app_id | int | The app ID of the game; should always be the current game.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#GameOverlayActivated_t){ .md-button .md-button--doc_classes target="_blank" }

### persona_state_change

!!! function "persona_state_change"
	This is called when a user has some kind of change.

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| steam_id | uint64_t | Steam ID of the user who changed.
		| flags | int | A bit-wise union of [PersonaChange enum](#personachange).

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#PersonaStateChange_t){ .md-button .md-button--doc_classes target="_blank" }

### request_clan_officer_list

!!! function "request_clan_officer_list"
	Marks the return of a request officer list call.

	Emits signal in response to function [requestClanOfficerList](#requestclanofficerlist).

	!!! returns "Returns"
		| Key | Type | Notes |
        | --- | ---- | ----- |
		| success | bool | Was the call successful? If it wasn't this may indicate a temporary loss of connection to Steam. If this returns true, this does not necessarily mean that all of the info for this Steam group has been downloaded.
		| officers_list | array | A list of officer dictionaries.

		**officer** dictionaries contain the following keys:

		| Key | Type | Notes |
        | --- | ---- | ----- |
		| id | uint64_t | The Steam ID of the officer.
		| name | string | The Steam username of the officer.

	---
	[:fontawesome-brands-steam: Read more in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/api/ISteamFriends#ClanOfficerListResponse_t){ .md-button .md-button--doc_classes target="_blank" }

### unread_chat_messages_changed

!!! function "unread_chat_messages_changed"
	Invoked when the status of unread messages changes

	!!! returns "Returns"
		 Nothing.

{==
## :material-infinity: Constants
==}

Name | SDK Name | Value | Notes
---- | -------- | ----- | -----
CHAT_METADATA_MAX | k_cubChatMetadataMax | 8192 | Size limit on chat room or member metadata.
ENUMERATED_FOLLOWERS_MAX | k_cEnumerateFollowersMax | 50 | -
FRIEND_GAME_INFO_QUERY_PORT_ERROR | k_usFriendGameInfoQueryPort_Error | 0xFFFE | We were unable to get the query port for this server.  Was #define QUERY_PORT_ERROR in older versions of Steamworks SDK.
FRIEND_GAME_INFO_QUERY_PORT_NOT_INITIALIZED | k_usFriendGameInfoQueryPort_NotInitialized | 0xFFFF | We haven't asked the GS for this query port's actual value yet.  Was #define QUERY_PORT_NOT_INITIALIZED in older versions of Steamworks SDK.
FRIENDS_GROUP_LIMIT | k_cFriendsGroupLimit | 100 | Maximum number of groups a single user is allowed.
INVALID_FRIEND_GROUP_ID | k_FriendsGroupID_Invalid | -1 | Invalid friends group identifier constant.
MAX_FRIENDS_GROUP_NAME | k_cchMaxFriendsGroupName | 64 | Maximum length of friend group name (not including terminating null).
MAX_RICH_PRESENCE_KEY_LENGTH | k_cchMaxRichPresenceKeyLength | 64 | Size limits on Rich Presence data.
MAX_RICH_PRESENCE_KEYS | k_cchMaxRichPresenceKeys | 30 | -
MAX_RICH_PRESENCE_KEY_LENGTH | k_cchMaxRichPresenceValueLength | 256 | -
PERSONA_NAME_MAX_UTF8 | k_cchPersonaNameMax | 128 | Maximum number of characters in a user's name. Two flavors; one for UTF-8 and one for UTF-16. The UTF-8 version has to be very generous to accomodate characters that get large when encoded in UTF-8.
PERSONA_NAME_MAX_UTF16 | k_cwchPersonaNameMax | 32 | -

{==
## :material-numeric: Enums
==}

### AvatarSizes

GodotSteam-specific enums for avatar sizes.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
AVATAR_SMALL | - | 1 | Small avatar.
AVATAR_MEDIUM | - | 2 | Medium avatar.
AVATAR_LARGE | - | 3 | Large avatar.

### CommunityProfileItemType

See [getProfileItemPropertyString](#getprofileitempropertystring) and [getProfileItemPropertyInt](#getprofileitempropertyint).

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
PROFILE_ITEM_TYPE_ANIMATED_AVATAR | k_ECommunityProfileItemType_AnimatedAvatar | 0 | -
PROFILE_ITEM_TYPE_AVATAR_FRAME | k_ECommunityProfileItemType_AvatarFrame | 1 | -
PROFILE_ITEM_TYPE_PROFILE_MODIFIER | k_ECommunityProfileItemType_ProfileModifier | 2 | -
PROFILE_ITEM_TYPE_PROFILE_BACKGROUND | k_ECommunityProfileItemType_ProfileBackground | 3 | -
PROFILE_ITEM_TYPE_MINI_PROFILE_BACKGROUND | k_ECommunityProfileItemType_MiniProfileBackground | 4 | -

### CommunityProfileItemProperty

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
PROFILE_ITEM_PROPERTY_IMAGE_SMALL | k_ECommunityProfileItemProperty_ImageSmall | 0 | String.
PROFILE_ITEM_PROPERTY_IMAGE_LARGE | k_ECommunityProfileItemProperty_ImageLarge | 1 | String.
PROFILE_ITEM_PROPERTY_INTERNAL_NAME | k_ECommunityProfileItemProperty_InternalName | 2 | String.
PROFILE_ITEM_PROPERTY_TITLE | k_ECommunityProfileItemProperty_Title | 3 | String.
PROFILE_ITEM_PROPERTY_DESCRIPTION | k_ECommunityProfileItemProperty_Description | 4 | String.
PROFILE_ITEM_PROPERTY_APP_ID | k_ECommunityProfileItemProperty_AppID | 5 | uint32_t.
PROFILE_ITEM_PROPERTY_TYPE_ID | k_ECommunityProfileItemProperty_TypeID | 6 | uint32_t.
PROFILE_ITEM_PROPERTY_CLASS | k_ECommunityProfileItemProperty_Class | 7 | uint32_t.
PROFILE_ITEM_PROPERTY_MOVIE_WEBM | k_ECommunityProfileItemProperty_MovieWebM | 8 | String.
PROFILE_ITEM_PROPERTY_MOVIE_MP4 | k_ECommunityProfileItemProperty_MovieMP4 | 9 | String.
PROFILE_ITEM_PROPERTY_MOVIE_WEBM_SMALL | k_ECommunityProfileItemProperty_MovieWebMSmall | 10 | String.
PROFILE_ITEM_PROPERTY_MOVIE_MP4_SMALL | k_ECommunityProfileItemProperty_MovieMP4Small | 11 | String.

### FriendFlags

Flags for enumerating friends list, or quickly checking a the relationship between users.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
FRIEND_FLAG_NONE | k_EFriendFlagNone | 0X00 | -
FRIEND_FLAG_BLOCKED | k_EFriendFlagBlocked | 0X01 | -
FRIEND_FLAG_FRIENDSHIP_REQUESTED | k_EFriendFlagFriendshipRequested | 0X02 | -
FRIEND_FLAG_IMMEDIATE | k_EFriendFlagImmediate | 0X04 | "Regular" friend.
FRIEND_FLAG_CLAN_MEMBER | k_EFriendFlagClanMember | 0X08 | -
FRIEND_FLAG_ON_GAME_SERVER | k_EFriendFlagOnGameServer | 0X10 | -
FRIEND_FLAG_REQUESTING_FRIENDSHIP | k_EFriendFlagRequestingFriendship | 0X80 | -
FRIEND_FLAG_REQUESTING_INFO | k_EFriendFlagRequestingInfo | 0X100 | -
FRIEND_FLAG_IGNORED | k_EFriendFlagIgnored | 0X200 | -
FRIEND_FLAG_IGNORED_FRIEND | k_EFriendFlagIgnoredFriend | 0X400 | -
FRIEND_FLAG_CHAT_MEMBER | k_EFriendFlagChatMember | 0X1000 | -
FRIEND_FLAG_ALL | k_EFriendFlagAll | 0XFFFF | -

### FriendRelationship

Set of relationships to other users.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
FRIEND_RELATION_NONE | k_EFriendRelationshipNone | 0 | -
FRIEND_RELATION_BLOCKED | k_EFriendRelationshipBlocked | 1 | This doesn't get stored; the user has just done an Ignore on an friendship invite.
FRIEND_RELATION_REQUEST_RECIPIENT | k_EFriendRelationshipRequestRecipient | 2 | -
FRIEND_RELATION_FRIEND | k_EFriendRelationshipFriend | 3 | -
FRIEND_RELATION_REQUEST_INITIATOR | k_EFriendRelationshipRequestInitiator | 4 | -
FRIEND_RELATION_IGNORED | k_EFriendRelationshipIgnored | 5 | This is stored; the user has explicit blocked this other user from comments/chat/etc.
FRIEND_RELATION_IGNORED_FRIEND | k_EFriendRelationshipIgnoredFriend | 6 | -
FRIEND_RELATION_SUGGESTED | k_EFriendRelationshipSuggested_DEPRECATED | 7 | Was used by the original implementation of the facebook linking feature, but now unused.
FRIEND_RELATION_MAX | k_EFriendRelationshipMax | 8 | Keep this updated.

### OverlayToStoreFlag

These values are passed as parameters to the store.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
OVERLAY_TO_STORE_FLAG_NONE | k_EOverlayToStoreFlag_None | 0 | -
OVERLAY_TO_STORE_FLAG_ADD_TO_CART | k_EOverlayToStoreFlag_AddToCart | 1 | -
OVERLAY_TO_STORE_FLAG_AND_TO_CART_AND_SHOW | k_EOverlayToStoreFlag_AddToCartAndShow | 2 | -

### OverlayToWebPageMode

Tells Steam where to place the browser window inside the overlay.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
OVERLAY_TO_WEB_PAGE_MODE_DEFAULT | k_EActivateGameOverlayToWebPageMode_Default | 0 | Browser will open next to all other windows that the user has open in the overlay. The window will remain open, even if the user closes then re-opens the overlay.
OVERLAY_TO_WEB_PAGE_MODE_MODAL | k_EActivateGameOverlayToWebPageMode_Modal | 1 | Browser will be opened in a special overlay configuration which hides all other windows that the user has open in the overlay. When the user closes the overlay, the browser window will also close. When the user closes the browser window, the overlay will automatically close.

### PersonaChange

Used in [persona_state_change](#persona_state_change) **flags** value to describe what's changed about a user these flags describe what the client has learned has changed recently, so on startup you'll see a name, avatar, and relationship change for every friend.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
PERSONA_CHANGE_NAME | k_EPersonaChangeName | 0X0001 | -
PERSONA_CHANGE_STATUS | k_EPersonaChangeStatus | 0X0002 | -
PERSONA_CHANGE_COME_ONLINE | k_EPersonaChangeComeOnline | 0X0004 | -
PERSONA_CHANGE_GONE_OFFLINE | k_EPersonaChangeGoneOffline | 0X0008 | -
PERSONA_CHANGE_GAME_PLAYED | k_EPersonaChangeGamePlayed | 0X0010 | -
PERSONA_CHANGE_GAME_SERVER | k_EPersonaChangeGameServer | 0X0020 | -
PERSONA_CHANGE_AVATAR | k_EPersonaChangeAvatar | 0X0040 | -
PERSONA_CHANGE_JOINED_SOURCE | k_EPersonaChangeJoinedSource | 0X0080 | -
PERSONA_CHANGE_LEFT_SOURCE | k_EPersonaChangeLeftSource | 0X0100 | -
PERSONA_CHANGE_RELATIONSHIP_CHANGED | k_EPersonaChangeRelationshipChanged | 0X0200 | -
PERSONA_CHANGE_NAME_FIRST_SET | k_EPersonaChangeNameFirstSet | 0X0400 | -
PERSONA_CHANGE_FACEBOOK_INFO | k_EPersonaChangeBroadcast | 0X0800 | -
PERSONA_CHANGE_NICKNAME | k_EPersonaChangeNickname | 0X1000 | -
PERSONA_CHANGE_STEAM_LEVEL | k_EPersonaChangeSteamLevel | 0X2000 | -
PERSONA_CHANGE_RICH_PRESENCE | k_EPersonaChangeRichPresence | 0x4000 | -

### PersonaState

List of states a friend can be in.

Enumerator | SDK Name | Value | Notes
---------- | -------- | ----- | -----
PERSONA_STATE_OFFLINE | k_EPersonaStateOffline | 0 | Friend is not currently logged on.
PERSONA_STATE_ONLINE | k_EPersonaStateOnline | 1 | Friend is logged on.
PERSONA_STATE_BUSY | k_EPersonaStateBusy | 2 | User is on, but busy.
PERSONA_STATE_AWAY | k_EPersonaStateAway | 3 | Auto-away feature.
PERSONA_STATE_SNOOZE | k_EPersonaStateSnooze | 4 | Auto-away for a long time.
PERSONA_STATE_LOOKING_TO_TRADE | k_EPersonaStateLookingToTrade | 5 | Online, trading.
PERSONA_STATE_LOOKING_TO_PLAY | k_EPersonaStateLookingToPlay | 6 | Online, wanting to play.
PERSONA_STATE_INVISIBLE | k_EPersonaStateInvisible | 7 | Online, but appears offline to friends.  This status is never published to clients.
PERSONA_STATE_MAX | k_EPersonaStateMax | - | -