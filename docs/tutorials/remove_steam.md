# Tutorials - Removing Steam Easily

A lot of folks like to ship on other platforms like Playstation, XBox, Switch, Itch.io, etc. Removing deeply embedded Steamworks stuff can be a pain and some have opted to keep separate repositories for their game's Steam version. However, there is an alternative to this: programmatically ignoring the Steamworks bits. Below are some examples shared by users in our Discord.

---
## How We Do It

So the example I'll be using for this tutorial is based on solution #2 submitted by Rutger. I am actively using this in my current project.

We will create two variable to hold our platform and the Steamworks object then modify the usual `initialize_steam()` function to look for the Steam singleton before trying to do anything:

```gdscript
var this_platform: String = "steam"
var steam_api: Object = null

# Initialize Steam
func initialize_steam() -> void:
	if Engine.has_singleton("Steam"):
		this_platform = "steam"
		steam_api = Engine.get_singleton("Steam")
		
		var initialized: Dictionary = steam_api.steamInitEx(false)

		print("[STEAM] Did Steam initialize?: %s" % initialized))

		# In case it does fail, let's find out why and null the steam_api object
		if initialized['status'] > 0:
			print("Failed to initialize Steam, disabling all Steamworks functionality: %s" % initialized)
			steam_api = null
 
		# Is the user online?
		steam_id = steam_api.getSteamID()
		steam_name = steam_api.getPersonaName()
 
	else:
		this_platform = "itch"  # Could be anything else really like a console, etc.
		steam_id = 0
		steam_name = "You"
```

Now we can use the `steam_api` object to call our Steam functions elsewhere in the game and, without Steamworks present, it won't crash the game due to the missing singleton call.

I also create a helper function to quickly check if anything Steam-wise can be used, like so:

```gdscript
func is_steam_enabled() -> bool:
	if this_platform == "steam" and steam_api != null:
		return true
	return false
```

Since it is in my global script, it can be called anywhere I need to use Steam functions. If this function doesn't return true, then I just have my code ignore related Steam portions like so:

```gdscript
func fire_steam_achievement(value: int) -> void:
	if not achievements[value]:
		achievements[value] = true
		
		# Pass variable to Steam and fire it off
		var this_achievement: String = "ACHIEVE"+str(value)
		
		# Now, if Steam exists and is enabled, use the Steamworks functions
		if is_steam_enabled():
			var was_set: bool = steam_api.setAchievement(ACHIEVE)
			print("[STEAM] Firing achievement %s, success: %s" % [this_achievement, was_set])
			
			var was_stored: bool = steam_api.storeStats()
			print("[STEAM] Statistics stored: %s" % was_stored)
```

So when you need to export for non-Steam platforms, you can just use vanilla Godot templates and you don't have to worry about anything else!  No need for different versions of your game at all.

Below we have the two submitted user suggestions on how to pull this off; the example above, as mnetioned, was based on solution #2 but do also check out solution 1 if that makes more sense for you.

---
## Solution 1: Multiple Files

Albey shared some scripts for his solution in GDScript and there are three separate files. The `SteamHandler.gd` file which swaps out which version of the game this is:

````gdscript
extends Node

var interface: SteamIntegrationBlank

func _ready() -> void:
	if OS.has_feature("Steam"):
		interface = load("res://Entities/Autoloads/Steam/SteamIntegration.gd").new()
	else:
		interface = load("res://Entities/Autoloads/Steam/SteamIntegrationBlank.gd").new()

	interface.initialise_steam()
	if interface.status != interface.STATUS_OK:
		get_tree().quit()
````

The `SteamIntegrationBlank.gd` file which handles what happens when Steam is not present:

````gdscript
extends Reference
class_name SteamIntegrationBlank

const STATUS_OK = 1
const STATUS_STEAM_NOT_RUNNING = 20
var status := STATUS_OK

func initialise_steam() -> void:
	pass
````

And finally the `SteamIntegration.gd` file for when Steam is present:

````gdscript
extends SteamIntegrationBlank

const APP_ID = ***

func initialise_steam() -> void:
	if Steam.restartAppIfNecessary(APP_ID):
		status = STATUS_STEAM_NOT_RUNNING
		return

	var init: Dictionary = Steam.steamInit()
	status = init['status']
````

---
## Solution 2: Check For Singleton

[Rutger from Roost Games (maker of Cat Cafe Manager)](https://catcafemanager.com){ target="\_blank" } shared a tidbit about it: "If anyone is wondering how to do that, since I had to find out through the Switch port, I have a `platform` global as a wrapper for any platform specific stuff, it just does this in the `_ready()`". His example code is as following:

````gdscript
if Engine.has_singleton("Steam"):
	self.platform = "steam"
	self.Steam = Engine.get_singleton("Steam")
````

---

Hopefully these examples give you some ideas on how to remove Steamworks features from your game without having to make a bunch of different builds and repositories.
