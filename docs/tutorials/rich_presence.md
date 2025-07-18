---
title: Rich Presence
description: A guide on setting rich presence for your game.
icon: material/message-draw
---

# Tutorials - Rich Presence
:material-badge-account-horizontal: _By Gramps_

---

This short tutorial is all about rich presence for your game; specifically the game's enhanced rich presence. You have probably seen a friend in-game, in your friend list, that has a secondary text string with some information about the game. Usually something about the level they are on, a lobby, number of players, etc. Well, that's what this tutorial is all about.

[You can read more about enhanced rich presence in the Steamworks documentation](https://partner.steamgames.com/doc/features/enhancedrichpresence){ target="\_blank" }.

??? guide "Relevant GodotSteam classes and functions"
	* [Friends class](../classes/friends.md)
		* [setRichPresence()](../classes/friends.md#setrichpresence)

!!! warning "Note"
	You may want to [double-check our Initialization tutorial](initializing.md) to set up initialization and callbacks functionality if you haven't done so already.

{==
## :material-file-document-check: Preparations
==}

First you will need to set up your localization file in the Steamworks back-end. Obviously without this step the rich presence text does not really work as it has nothing to reference. You will need to set up your text file like this and save it as `.vdf` file:

```gdscript
"lang" {
	"Language" "english"
	"Tokens" {
		"#something1"   "Rich presence string"
		"#something2"   "Another string"
		"#something_with_input"	"{#something%something_number%}: %score%"
	}
}
```

Make sure you tab separate the strings in the tokens bracket. Also make sure you publish the changes after your file is uploaded _before_ you try to test it. The changes must be live in Steamworks for it to appear.

You can put various languages in their own nests so these show up for users how have their Steam client language set specifically. The first string in each token is what you'll end up using in your game's code.

{==
## Suggested Code
==}

I have a global function, in my `global.gd` that can be called anywhere in my project and handles this. It is written something like this:

```gdscript
func set_rich_presence(token: String) -> void:
	# Set the token
	var setting_presence = Steam.setRichPresence("steam_display", token)

	# Debug it
	print("Setting rich presence to "+str(token)+": "+str(setting_presence))
```

And in whatever scene you want to set the token:

```gdscript
global.set_rich_presence("#something1")
```

And now Steam will set the text in your friend's list to what you have set it as in the token list in the Steamworks back-end. You can also hover over your own profile picture to see the in-game text; for testing purposes.

There are additional tags you can set which are [covered in detail in the SDK documentation here.](https://partner.steamgames.com/doc/api/ISteamFriends#SetRichPresence){ target="\_blank" }

{==
## Weird Bug In GDNative
==}

A weird bug exists in the **Windows version of GDNative**, both in GodotSteam and Samsfacee's plug-in. Sometimes the `setRichPresence()` function will send the key as the value. It isn't consistent but happens enough to be noticeable and a pain.

Please note this bug *does not exist* in the pre-compiled module versions nor GDExtension version or the GDNative versions for Linux or OSX.

This behavior definitely seems to be a GDNative problem so we can't really fix it on our and an issue has been submitted to the Godot GitHub page.

Thankfully, **Furcifer** has shared some code that should help with this!

```gdscript
var iteration = 0
var richPresenceKeyValue = []
var updatingRichPresence = false

func callNextFrame(methodName, arguments = []):
	get_tree().connect("idle_frame", self, methodName, arguments, CONNECT_ONESHOT)

func updateRichPresence():
	iteration = 0
	richPresenceKeyValue.clear()
	
	addRichPresence("numwins", String(Game.getNumWins()))
	addRichPresence("steam_display", "#status_Ingame")

	if not updatingRichPresence:
		updatingRichPresence = true
		setPresenceDelayed()

func addRichPresence(key, value):
	richPresenceKeyValue.push_back([key, value])
	Steam.setRichPresence(key, value)

func setPresenceDelayed():
	var remaining = []
	for tuple in richPresenceKeyValue:
		var success = forceSetProperty(tuple[0], tuple[1])
		if not success:
			remaining.push_back(tuple)
	
	richPresenceKeyValue = remaining
	
	if not richPresenceKeyValue.empty():
		iteration += 1
		recallSetPresenceDelayed() # recalls this function after a frame
	else:
		updatingRichPresence = false

func recallSetPresenceDelayed():
	Util.callNextFrame(self, "setPresenceDelayed")

func forceSetProperty(key : String, value : String) -> bool:
	for i in 10:
		var setval = Steam.getFriendRichPresence(STEAM_ID, key)
		if setval == key:
			#print("could not set key ", key, " - iteration: ", iteration, "/",i)
			Steam.setRichPresence(key, value)
		else:
			return true
	
	return false
```

{==
## :material-content-save-settings: Additional Resources
==}

### Video Tutorials

Prefer video tutorials? Feast your eyes and ears!

[ :simple-youtube: 'Setting Up Rich Presence' by FinePointCGI](https://www.youtube.com/watch?v=VCwNxfYZ8Cw&t=4762s){ .md-button .md-button--resource target="\_blank" }

### Example Project

[Later this year you can see this tutorial in action with more in-depth information by checking out our upcoming free-to-play game Skillet on GitHub.](https://github.com/GodotSteam/Skillet){ target="\_blank" } There you will be able to view of the code used which can serve as a starting point for you to branch out from.