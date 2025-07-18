---
title: Statistics and Achievements
description: A guide on using statistics and achievements in your game.
icon: material/trophy-award
---

# Tutorials - Stats And Achievements
:material-badge-account-horizontal: _By Gramps_

---

At some point you may want to save statistics to Steam's database and / or use their achievement system. You will want to [read up about these in Steam's documentation](https://partner.steamgames.com/doc/features/achievements){ target="\_blank" } as I won't be covering the basics on how to set it up in the Steamworks back-end.

??? guide "Relevant GodotSteam classes and functions"
	* [User Stats class](../classes/user_stats.md)
		* [requestCurrentStats()](../classes/user_stats.md#requestcurrentstats)
		* [getAchievement()](../classes/user_stats.md#getachievement)
		* [setAchievement()](../classes/user_stats.md#setachievement)
		* [setStatFloat()](../classes/user_stats.md#setstatfloat)
		* [setStatInt()](../classes/user_stats.md#setstatint)
		* [storeStats()](../classes/user_stats.md#storestats)

{==
## :material-file-document-check: Preparations
==}

First thing, you'll want to set up your achievements and statistics in the Steamworks back-end. Most importantly, you want to publish these changes live. If not, they will not be exposed to the Steamworks API and you will get errors when trying to retrieve or set them. Once they have been published you can continue on with this tutorial.

{==
## Getting Steam Statistics And Achievements
==}

When starting up your game, in most cases, you'll want to pull all related achievements and statistics from Steam's servers for the local user. There are a few ways to handle connecting the callback for statistics / achievements data received from Steam's servers.
Unless you choose to turn it off by passing **false** to the **steamInit()** or **steamInitEX()** functions; it will automatically request the local user's current statistics and achievements. I generally pass false to prevent this.

To retrieve the data from the resulting callback, you need to connect the **current_stats_received** callback to a function like so:

=== "Godot 2.x, 3.x"
	```gdscript
	Steam.connect("current_stats_received", self, "_on_steam_stats_ready", [], CONNECT_ONESHOT)
	```
=== "Godot 4.x"
	```gdscript
	Steam.current_stats_received.connect(_on_steam_stats_ready)
	```

You'll notice that **CONNECT_ONESHOT** is passed along to prevent this from firing more than once. This is because that signal is sent any time stats are updated or received and will run the **\_on_steam_stats_ready()** again.

In my use-cases this is not desirable, but it may be in yours. If you don't mind the **\_on_steam_stats_ready()** firing each time, depending on your function's logic, feel free to omit that part like so:

=== "Godot 2.x, 3.x"
	```gdscript
	Steam.connect("current_stats_received", self, "_on_steam_stats_ready")
	```
=== "Godot 4.x"
	```gdscript
	Steam.current_stats_received.connect(_on_steam_stats_ready)
	```

If you do keep the **CONNECT_ONESHOT** as I do, I suggest calling for Steam stat updates with **requestUserStats()** and pass with it the user's Steam ID. This function will work with any user: local or remote. You'll also want to connect it's signal in a similar manner:

=== "Godot 2.x, 3.x"
	```gdscript
	Steam.connect("user_stats_received", self, "_on_steam_stats_ready")
	```
=== "Godot 4.x"
	```gdscript
	Steam.user_stats_received.connect(_on_steam_stats_ready)
	```

It can be connected to the same function as **requestCurrentStats()** as they send the same data back. For our example, here is the **\_on_steam_stats_ready()** function we listed in the connected signals:

```gdscript
func _on_steam_stats_ready(this_game: int, this_result: int, this_user: int) -> void:
	print("Received local player stats and achievements from Steam: %s / %s /%s" % [this_user, this_result, this_game])

	# These will check against the data we pulled in the initialization tutorial
	if this_user != steam_id:
		print("These stats belong to %s instead, aborting Steam stat and achievement loading" % this_user)
		return

	if this_game != app_id:
		print("Stats are for a different app ID: %s" % this_game)
		return

	if this_result != Steam.RESULT_OK:
		print("Failed to get stats and achievements from Steam: %s" % this_result)
		return
```

{==
## Working With The Data
==}

In this function you can check if the result is what you expect (ideally it is 1), see if the given stats are for the current player, and check that the game's ID matches. Also you can now pass the achievements and stats to local variables or functions. I will often pass the achievement to a function to parse them correctly as they send back a BOOL for retrieval and a BOOL for earned or not.

```gdscript
var achievements: Dictionary = { "achieve1": false, "achieve2": false, "achieve3": false }
var statistics: Dictionary = { "highscore": 0, "health": 0, "money": 0 }

func _on_steam_stats_ready(this_game: int, this_result: int, this_user: int) -> void:
	print("Received local player stats and achievements from Steam: %s / %s /%s" % [this_user, this_result, this game])

	# These will check against the data we pulled in the initialization tutorial
	if this_user != steam_id:
		print("These stats belong to %s instead, aborting Steam stat and achievement loading" % this_user)
		return

	if this_game != app_id:
		print("Stats are for a different app ID: %s" % this_game)
		return

	if this_result != Steam.RESULT_OK:
		print("Failed to get stats and achievements from Steam: %s" % this_result)
		return

	load_steam_stats()
	load_steam_achievements()


# Process statistics
func load_steam_stats() -> void:
	for this_stat in statistics.keys():
		var steam_stat: int = Steam.getStatInt(this_stat)

		# The set_statistic function below in the Setting Statistics section
		if statistics[this_stat] > steam_stat:
			print("Stat mismatch; local value is higher (%s), replacing Steam value (%s)" % [statistics[this_stat], steam_stat])
			set_statistic(this_stat, statistics[this_stat])

		elif statistics[this_stat] < steam_stat:
			print("Stat mismatch; local value is lower (%s), replacing with Steam value (%s)" % [statistics[this_stat], steam_stat]))
			set_statistic(this_stat, steam_stat)

		else:
			print("Steam stat matches local file: %s" % this_stat)

	print("Steam statistics loaded")


# Process achievements
func load_steam_achievements() -> void:
	for this_achievement in achievements.keys():
		var steam_achievement: Dictionary = Steam.getAchievement(this_achievement)

		# The set_achievement function is below in the Setting Achievements section
		if not steam_achievement['ret']:
			print("Steam does not have this achievement, defaulting to local value: achieve%s" % this_achievement)
			continue

		if achievements[this_achievement] == steam_achievement['achieved']:
			print("Steam achievements match local file, skipping: %s" % this_achievement)
			continue

		set_achievement(this_achievement)

	print("Steam achievements loaded")
```

{==
## Setting Achievements
==}

Setting the achievements and statistics is pretty simple too. We'll start with achievements. You need to tell Steam the achievement is unlocked and then store it so 'pops':

```gdscript
Steam.setAchievement("achieve1")
Steam.storeStats()
```

If you don't call **storeStats()** the achievement pop-up won't trigger but the achievement should be recorded. However, you will still have to call **storeStats()** at some point to upload them. I generally make a generic function to house this process then call it when needed:

```gdscript
func set_achievement(thiss_achievement: String) -> void:
	if not achievements.has(this_achievement):
		print("This achievement does not exist locally: %s" % this_achievement)
		return
	achievements[this_achievement] = true

	if not Steam.setAchievement(this_achievement):
		print("Failed to set achievement: %s" % this_achievement)
		return

	print("Set acheivement: %s" % this_achievement)

	# Pass the value to Steam then fire it
	if not Steam.storeStats():
		print("Failed to store data on Steam, should be stored locally")
		return

	print("Data successfully sent to Steam")
```

When that last **storeStats()** is called the achievement will "pop" visually for the user and update their Steam client.

{==
## Setting Statistics
==}

Statistics follow a pretty similar process; both int and float based ones. Set them like so:

```gdscript
func set_statistic(this_stat: String, new_value: int = 0) -> void:
	if not Settings.statistics.has(this_stat):
		print("This statistic does not exist locally: %s" % this_stat)
		return
	Settings.statistics[this_stat] = new_value

	if not Steam.setStatInt(this_stat, new_value):
		print("Failed to set stat %s to: %s" % [this_stat, new_value])
		return

	print("Set statistics %s succesfully: %s" % [this_stat, new_value])


	# Pass the value to Steam then fire it
	if not Steam.storeStats():
		print("Failed to store data on Steam, should be stored locally")
		return

	print("Data successfully sent to Steam")
```

When that last **storeStats()** is called the stats will update on Steam's servers.

{==
## :material-content-save-settings: Additional Resources
==}

### Video Tutorials

Prefer video tutorials? Feast your eyes and ears!

[ :simple-youtube: 'Godot + Steam tutorial' by BluePhoenixGames](https://www.youtube.com/watch?v=J0GrG-AffCI&t=571s){ .md-button .md-button--resource target="\_blank" }

[ :simple-youtube: 'How Achievements Are Done' by FinePointCGI](https://www.youtube.com/watch?v=VCwNxfYZ8Cw&t=938s){ .md-button .md-button--resource target="\_blank" }

[ :simple-youtube: 'Let's Talk About Statistics' by FinePointCGI](https://www.youtube.com/watch?v=VCwNxfYZ8Cw&t=1504s){ .md-button .md-button--resource target="\_blank" }

[ :simple-youtube: 'Godot 4 Steam Achievements' by Gwizz](https://www.youtube.com/watch?v=dg6fSBe5EEE){ .md-button .md-button--resource target="\_blank" }

### Example Project

[You can view this tutorial in action with more in-depth information by checking out our upcoming free-to-play game Skillet on GitHub.](https://github.com/GodotSteam/Skillet/blob/master/src/autoload/steamworks.gd){ target="\_blank" } This link will take you to the direct file where this tutorial comes from but feel free to explore the rest of the project too!
