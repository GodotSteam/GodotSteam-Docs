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
Unless you choose to turn it off by passing `false` to the `steamInit()` or `steamInitEX()` functions; it will automatically request the local user's current statistics and achievements. I generally pass false to prevent this.

To retrieve the data from the resulting callback, you need to connect the `current_stats_received` callback to a function like so:

=== "Godot 2.x, 3.x"
	```gdscript
	Steam.connect("current_stats_received", self, "_on_steam_stats_ready", [], CONNECT_ONESHOT)
	```
=== "Godot 4.x"
	```gdscript
	Steam.current_stats_received.connect(_on_steam_stats_ready)
	```

You'll notice that `CONNECT_ONESHOT` is passed along to prevent this from firing more than once. This is because that signal is sent any time stats are updated or received and will run the `_on_steam_stats_ready()` again.

In my use-cases this is not desirable, but it may be in yours. If you don't mind the `_on_steam_stats_ready()` firing each time, depending on your function's logic, feel free to omit that part like so:

=== "Godot 2.x, 3.x"
	```gdscript
	Steam.connect("current_stats_received", self, "_on_steam_stats_ready")
	```
=== "Godot 4.x"
	```gdscript
	Steam.current_stats_received.connect(_on_steam_stats_ready)
	```

If you do keep the `CONNECT_ONESHOT` as I do, I suggest calling for Steam stat updates with `requestUserStats()` and pass with it the user's Steam ID. This function will work with any user: local or remote. You'll also want to connect it's signal in a similar manner:

=== "Godot 2.x, 3.x"
	```gdscript
	Steam.connect("user_stats_received", self, "_on_steam_stats_ready")
	```
=== "Godot 4.x"
	```gdscript
	Steam.user_stats_received.connect(_on_steam_stats_ready)
	```

It can be connected to the same function as `requestCurrentStats()` as they send the same data back. For our example, here is the `_on_steam_stats_ready()` function we listed in the connected signals:

```gdscript
func _on_steam_stats_ready(game: int, result: int, user: int) -> void:
	print("This game's ID: %s" % game)
	print("Call result: %s" % result)
	print("This user's Steam ID: %s" % user)
```

{==
## Working With The Data
==}

In this function you can check if the result is what you expect (ideally it is 1), see if the given stats are for the current player, and check that the game's ID matches. Also you can now pass the achievements and stats to local variables or functions. I will often pass the achievement to a function to parse them correctly as they send back a BOOL for retrieval and a BOOL for earned or not.

```gdscript
var achievements: Dictionary = {"achieve1":false, "achieve2":false, "achieve3":false}

func _on_steam_stats_ready(game: int, result: int, user: int) -> void:
	print("This game's ID: %s" % game)
	print("Call result: %s" % result)
	print("This user's Steam ID: %s" % user)

	# Get achievements and pass them to variables
	get_achievement("acheive1")
	get_achievement("acheive2")
	get_achievement("acheive3")

	# Get statistics (int) and pass them to variables
	var highscore: int = Steam.getStatInt("HighScore")
	var health: int = Steam.getStatInt("Health")
	var money: int = Steam.getStatInt("Money")

# Process achievements
func get_achievement(value: String) -> void:
	var this_achievement: Dictionary = Steam.getAchievement(value)

	# Achievement exists
	if this_achievement['ret']:

		# Achievement is unlocked
		if this_achievement['achieved']:
			achievements[value] = true

		# Achievement is locked
		else:
			achievements[value] = false

	# Achievement does not exist
	else:
		achievements[value] = false
```

{==
## Setting Achievements
==}

Setting the achievements and statistics is pretty simple too. We'll start with achievements. You need to tell Steam the achievement is unlocked and then store it so 'pops':

```gdscript
Steam.setAchievement("achieve1")
Steam.storeStats()
```

If you don't call `storeStats()` the achievement pop-up won't trigger but the achievement should be recorded. However, you will still have to call `storeStats()` at some point to upload them. I generally make a generic function to house this process then call it when needed:

```gdscript
func _fire_Steam_Achievement(value: String) -> void:
	# Set the achievement to an in-game variable
	achievements[value] = true

	# Pass the value to Steam then fire it
	Steam.setAchievement(value)
	Steam.storeStats()
```

When that last `storeStats()` is called the achievement will "pop" visually for the user and update their Steam client.

{==
## Setting Statistics
==}

Statistics follow a pretty similar process; both int and float based ones. Set them like so:

```gdscript
Steam.setStatInt("stat1", value)
Steam.setStatFloat("stat2", value)
Steam.storeStats()
```

When that last `storeStats()` is called the stats will update on Steam's servers.

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

[Later this year you can see this tutorial in action with more in-depth information by checking out our upcoming free-to-play game Skillet on GitHub.](https://github.com/GodotSteam/Skillet){ target="\_blank" } There you will be able to view of the code used which can serve as a starting point for you to branch out from.