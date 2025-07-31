---
title: Leaderboards
description: A guide on setting up leaderboards for your game.
icon: material/controller
---

# Tutorials - Leaderboards
:material-badge-account-horizontal: _By Gramps_

---

This tutorial will cover setting up leaderboards for your games. I will add more to this tutorial at a later date with other functions but this should cover all the basics for working with leaderboards.

??? guide "Relevant GodotSteam classes and functions"
	* [User Stats class](../classes/user_stats.md)
		* [findLeaderboard()](../classes/user_stats.md#findleaderboard)
		* [uploadLeaderboardScore()](../classes/user_stats.md#uploadleaderboardscore)
		* [setLeaderboardDetailsMax()](../classes/user_stats.md#setleaderboarddetailsmax)
		* [downloadLeaderboardEntries()](../classes/user_stats.md#downloadleaderboardentries)
		* [downloadLeaderboardEntriesForUsers()](../classes/user_stats.md#downloadleaderboardentriesforusers)

!!! warning "Note"
	You may want to [double-check our Initialization tutorial](initializing.md) to set up initialization and callbacks functionality if you haven't done so already.

{==
## Set Up
==}

First, let's get our signals set up.

=== "Godot 2.x, 3.x"

	```gdscript
	Steam.connect("leaderboard_find_result", self, "_on_leaderboard_find_result")
	Steam.connect("leaderboard_score_uploaded", self, "_on_leaderboard_score_uploaded")
	Steam.connect("leaderboard_scores_downloaded", self, "_on_leaderboard_scores_downloaded")
	```

=== "Godot 4.x"

	```
	Steam.leaderboard_find_result.connect(_on_leaderboard_find_result)
	Steam.leaderboard_score_uploaded.connect(_on_leaderboard_score_uploaded)
	Steam.leaderboard_scores_downloaded.connect(_on_leaderboard_scores_downloaded)
	```

We'll go over each signal and related function in order. First, you'll need to pass your leaderboard's Steamworks back-end name to the `findLeaderboard()` function like so:

```gdscript
Steam.findLeaderboard( your_leaderboard_name )
```

!!! warning "Notes"
	When using `findOrCreateLeader()`, as opposed to `findLeaderboard()`, make sure not to set the sort method or display types as none. Avoid these enums: LEADERBOARD_SORT_METHOD_NONE and LEADERBOARD_DISPLAY_TYPE_NONE.

Once Steam finds your leaderboard it will pass back the handle to the `leaderboard_find_result` callback. The `_on_leaderboard_find_result()` function that it is connected to it should look something like this:

```gdscript
func _on_leaderboard_find_result(handle: int, found: int) -> void:
	if found == 1:
		leaderboard_handle = handle
		print("Leaderboard handle found: %s" % leaderboard_handle)
	else:
		print("No handle was found")
```

Once you have this handle you can use all the additional functions. **Please note** you do not need to save the leaderboard handle since it is stored internally. However, you will only be able to work with one leaderboard at a time unless you store them locally in a variable. I would keep a dictionary of handles locally like:

```gdscript
var leaderboard_handles: Dictionary = {
	"top_score": handle1,
	"most_kills": handle2,
	"most_games": handlde3
	}
```

This way you can call whatever handle you need when updating leaderboards quickly. Otherwise you have to query each leaderboard again with `findLeaderboard()` then wait for the callback then upload the new score. If you aren't updating leaderboards frequently or that many of them, then using the internally stored handle will probably work fine.

### Getting Leaderboards In A Loop

Some users have wanted to pull all leaderboard handles from the get-go.  While it is possible, it definitely feels like it wasn't intended to work that way.  However, these bits of code should help grab all the leaderboard handles.  This example relies on having your handles stored in a dictionary with the API name as the key as shown above.

```gdscript
func get_handles_in_loop() -> void:
	for this_leaderboard in leaderboard_handles.keys():
		Steam.findLeaderboard(this_leaderboard)
		await Steam.leaderboard_find_result


func _on_leaderboard_find_result(this_handle: int, was_found: int) -> void:
	if was_found != 1:
		print("Leaderboard could not be found: %s" % was_found)
		return

	var api_name: String = Steam.getLeaderboardName(this_handle)
	print("Leaderboard %s handle was found: %s" % [api_name, this_handle])

	leaderboard_handles[api_name] = this_handle

```

Originally, I had a timer for the **await** line `await get_tree().create_timer(0.5).timeout` which also worked pretty good.  I did find that if you set the timeout to 0.25, it was too fast and some of the leaderboards would not be found.  0.5 was a sweet-spot that felt instant and reliably worked during testing.

Big thanks to **TriMay** for the bit about getting the leaderboard name in the callback so we can assign the handle to the right leaderboard in our dictionary.

{==
## Uploading Scores
==}

Before we can download scores, we need to upload them. The function itself is pretty simple:

```gdscript
Steam.uploadLeaderboardScore( score, keep_best, details, handle )
```

The first argument is, obviously, the score. The second is if you want the score to update regardless of whether it is better than the current score for the user. The third are details which must be integers; they essentially can be anything but [here is what Valve says about it.](https://partner.steamgames.com/doc/api/ISteamUserStats#UploadLeaderboardScore){ target="\_blank" }  The fourth is the leaderboard handle we are updating. You do not have to pass the handle though, if you want to use the internally stored one.

!!! info "Notes"
	If you want the score to be something like milliseconds, as one community member did, you will want to multiply that value by 1000 and submit it as an int.

Once you pass a score to Steam, you should receive a callback from `leaderboard_score_uploaded`. This will trigger our `_on_leaderboard_score_uploaded()` function:

```gdscript
func _on_leaderboard_score_uploaded(success: int, this_handle: int, this_score: Dictionary) -> void:
	if success == 1:
		print("Successfully uploaded scores!")
		# Add additional logic to use other variables passed back
	else:
		print("Failed to upload scores!")
```		

For the most part you are just looking for a success of 1 to tell that it worked. However, you may with to use the additional variables passed back by the signal for logic in your game. They are contained in the dictionary called `this_score` which contains these keys:

- **score:** the new score as it stands
- **score_changed:** if the score was changed (0 if false, 1 if true)
- **new_rank:** the new global rank of this player
- **prev_rank:** the previous rank of this player

### Passing Extra Details

If you want to pass extra details, here are some neat hints from ***sepTN***:

> You can add small data as detail, for example embedding the character name (as a string) into that leaderboard entry. If you try to put detail that has a bigger size than possible, it will simply ignore it.  To retrieve it, you need to process it again because Steam will spit out arrays (PackedInt32Array).

Here is some code that was shared:

=== "Godot 2.x / 3.x"

	```
	# Godot 2 and 3 have no equivalent for to_int32_array I am aware of. Any corrections welcome!
	#
	Steam.uploadLeaderboardScore(score, keep_best, var2bytes(details), handle)
	```

=== "Godot 4.x"

	```
	Steam.uploadLeaderboardScore(score, keep_best, var_to_bytes(details).to_int32_array(), handle)
	```

When you download these scores and need to get our score details back to something readable, make sure you reverse this with `bytes_to_var()` or `bytes2var()` on Godot 4 and Godot 3 respectively.

{==
## Downloading Scores
==}

### Setting Max Details Or Not

Naturally you'll want to display leaderboard scores to the player. But before we pull any leaderboard entries, we need to set the maximum amount of details each one contains by setting the `setLeaderboardDetailsMax()` function up:

```gdscript
var details_max: int = Steam.setLeaderboardDetailsMax( value )
print("Max details: %s" % details_max)
```
In GodotSteam versions 4.6 or older, by default, the value is set to 0 but you may want to change it to match the number of details you upload with scores from the previous section. If you do not save any details with the scores you can safely ignore this part and move on to just requesting leaderboard entries.

In GodotSteam 4.6.1 and higher, the default value for max leaderboard details is 256 or the Steam maximum. This change makes it so you don't have to call `setLeaderboardDetailsMax()` first anymore; only if you want to change the max or turn off details.

### Getting Scores

In most cases you'll want to use `downloadLeaderboardEntries()`, but you can also use `downloadLeaderboardEntriesForUsers()` by passing an array of users' Steam IDs to it. Both will respond with the same callback, but `downloadLeaderboardEntriesForUsers()` does not allow as much control over what you can request:

```gdscript
Steam.downloadLeaderboardEntries( 1, 10, Steam.LEADERBOARD_DATA_REQUEST_GLOBAL, leaderboard_handle )

Steam.downloadLeaderboardEntriesForUsers( user_array, leaderboard_handle )
```

Just like uploading, downloading scores does not require a leaderboard handle to be included if you are using the internally stored one. You will noticed, as I mentioned above, that `downloadLeaderboardEntriesForUsers()` only takes an array of users' Steam IDs as it's other argument whereas `downloadLeaderboardEntries()` has quite a few others; which we will cover right now.

The first argument the index of entries you are starting with; this is usually 1 for your first request. The second argument is the last index to retrieve; there isn't a listed cap to this number but make it something you can display easily like 10 or so. The third argument is the type of data request; [you can read more details about it in the SDK documentation](https://partner.steamgames.com/doc/api/ISteamUserStats#ELeaderboardDataRequest){ target="\_blank" }. For a quick overview:

Leaderboard Data Request Enums 				| Values	| Descriptions
--- 										| --- 		| ---
LEADERBOARD_DATA_REQUEST_GLOBAL				| 0			| Used for a sequential range by leaderboard rank.
LEADERBOARD_DATA_REQUEST_GLOBAL_AROUND_USER	| 1			| Used to get entries relative to the user's entry. You may want to use negatives for the start to get entries before the user's.
LEADERBOARD_DATA_REQUEST_FRIENDS			| 2			| Used to get all entries for friends for the current user. Start and end arguments are ignored.
LEADERBOARD_DATA_REQUEST_USERS				| 3			| Used internally by Steam, **do not use this**.

After you request leaderboard entries, you should receive a `leaderboard_scores_downloaded` callback which will trigger our `_on_leaderboard_scores_downloaded()` function. That function should look similar to this:

```gdscript
func _on_leaderboard_scores_downloaded(message: String, this_leaderboard_handle: int, result: Array) -> void:
	print("Scores downloaded message: %s" % message)

	# Save this for later leaderboard interactions, if you want
	var leaderboard_handle: int = this_leaderboard_handle

	# Add logic to display results
	for this_result in result:
		# Use each entry that is returned
```

The message is just a basic message to inform you to the status the download; whether successful or not and why. The second piece sent back are the results as an array. Each entry in the array is actually a dictionary like so:

- **score:** this user's score
- **steam_id:** this user's Steam ID; you can use this to get their avatar, name, etc.
- **global_rank:** obviously their global ranking for this leaderboard
- **ugc_handle:** handle for any UGC that is attached to this entry
- **details:** any details you stored with this entry for later use

{==
## Uploading UGC to Leaderboard
==}

You can use this method if you want to attach replays or any other data to leadearboard entry.
So first you will need to setup signals

=== "Godot 2.x, 3.x"

	```gdscript
	Steam.connect("file_share_result", self, "_on_file_share_result")
	Steam.connect("file_share_result", self, "on_file_write_async_complete")
	Steam.connect("leaderboard_ugc_set", self, "on_leaderboard_ugc_set")
	```

=== "Godot 4.x"

	```
	Steam.file_share_result.connect(_on_file_share_result)
	Steam.file_write_async_complete.connect(_on_file_write_async_complete)
	Steam.leaderboard_ugc_set.connect(_on_leaderboard_ugc_set)
	```
	
### Creating a file we want to share

Created file is stored in PackedByteArray format.
Frist we need to create a file with content that we want to share.

```gdscript
var data: PackedByteArray
Steam.fileWriteAsync( "file_name", data, data.size() )
```

!!! warning "File location"
	File will be created in location `...\Steam\userdata\<localid?>\<appid>\remote`,


Once it is created you can mark the file to be shared.

```gdscript
func on_file_write_async_complete(result):
	print(result)
	if result == 1:
		Steam.fileShare("file_name")
	elese:
		pass #Handle errors here
```

### Understanding FileShare method

Method is used to mark existing files to be shared that are present in remote local directory.

```gdscript
Steam.fileShare("file_name")
```

!!! warning "File location"
	Files you want to share must exist in remote location `...\Steam\userdata\<localid?>\<appid>\remote`. Providing absolute path will not work.
	The function is only marking files to be shared; otherwise, if they do not exist, you will get error.
	
```gdscript
func on_file_share_result( result: int, handle, name: String ):
	if result == 1:
		Steam.attachLeaderboardUGC( handle, LEADERBOARD_HANDLE )
	elese:
		pass #Handle errors here
```

When you successfully set your file and uploaded it now you can attach it to leaderboard.

!!! warning "Notes"
	You must call `findLeaderboard()` or `findOrCreateLeaderboard()` to get a leaderboard handle prior to calling this function. You can store it for example as `LEADERBOARD_HANDLE`.

```gdscript
func on_leaderboard_ugc_set( handle:int, result:String ) -> void:
	if result == 1:
		print( "Hurray!" )
	elese:
		pass #Handle errors here
```

Once attached you can eiter leave file or delete it as it should be avalible even if deleted through leaderboard.

{==
## Possible Oddities
==}

A user in our Discord noted that sometimes `downloadLeaderboardEntriesForUsers()` would trigger a callback but have zero entries. Oddly, they reported that creating a second leaderboard then deleting the first one would fix this.  While I don't understand why this would be the case, in the event you come across this, perhaps try this solution!

{==
## :material-content-save-settings: Additional Resources
==}

### Video Tutorials

Prefer video tutorials? Feast your eyes and ears!

[ :simple-youtube: 'How To Build Leaderboards Out' by FinePointCGI](https://www.youtube.com/watch?v=VCwNxfYZ8Cw&t=3394s){ .md-button .md-button--resource target="\_blank" }

[ :simple-youtube: 'Godot 4 Steam Leaderboards' by Gwizz](https://www.youtube.com/watch?v=51qre_hodZI){ .md-button .md-button--resource target="\_blank" }

### Example Project

[Later this year you can see this tutorial in action with more in-depth information by checking out our upcoming free-to-play game Skillet on GitHub.](https://github.com/GodotSteam/Skillet){ target="\_blank" } There you will be able to view of the code used which can serve as a starting point for you to branch out from.