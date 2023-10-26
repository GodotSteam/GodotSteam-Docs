# Tutorials - Initializing Steam

In this tutorial, we will cover the basic initialization of Steamworks in your game; as well as getting callbacks globally. [Check out the common issues tutorial if](common_issues.md) you have issues getting things to work too.

Please note, this tutorial is only valid for the module and GDExtension versions of GodotSteam; the GDNative version will already have these functions present.

---

## Preparation

Before we go any further, it is recommended that you enable logging in your project if you have not done so. This will help both you, and us, debug any issues you might run into down the road.

Of course, if you have a custom logging system, don't worry about this.

To enable logging in the Godot editor, go to: **Projects > Project Settings > Debug > File Logging** and check **Enable File Logging**. This will start placing logs in your project's user data folder. Where are these, you might ask?  [Check out the official Godot Engine documentation to find the locations.](https://docs.godotengine.org/en/stable/tutorials/io/data_paths.html?highlight=user%20data){ target="_blank" }

![Enable Logging](../assets/images/tutorial-initializing-logging.png){ loading=lazy }

---

## Initialize Steam

In my personal projects, I usually create an auto-load GDscript called **global.gd** which is added as a singleton.

Thanks to user **B0TLANNER**, we will use a much cleaner method of letting Steam know what game we're running.  It is important that you set these two enviroment variables so they run when your game boots up.

````
func _init() -> void:
	# Set your game's Steam app ID here
	OS.set_environment("SteamAppId", str(480))
	OS.set_environment("SteamGameId", str(480))
````

We can also just put our game's app ID in a variable and pass it along there.

````
var STEAM_APP_ID: int = 480


func _init() -> void:
	# Set your game's Steam app ID here
	OS.set_environment("SteamAppId", str(STEAM_APP_ID))
	OS.set_environment("SteamGameId", str(STEAM_APP_ID))
````

The older, alternative method is creating a `steam_appid.txt` file and placing it with the editor or exported game but this is much better.

I then create a function called ***_initialize_Steam()*** and add the code below. This is then called from the ***_ready()*** function in my **global.gd**:

````
func _ready() -> void:
	_initialize_Steam()


func _initialize_Steam() -> void:
	var INIT: Dictionary = Steam.steamInitEx()
	print("Did Steam initialize?: "+str(INIT))
````

By default, ***Steam.steamInitEx()*** will query Steamworks for the local user's current statistics and send this data back as a callback (signal). You can pass a boolean (false) to the function to prevent this behavior: `Steam.steamInitEx(false)`.

***Steam.steamInitEx()*** will always send back a dictionary with two keys / values:

- verbal - The verbose, text version of status
- status
	- 0 - Successfully initialized
    - 1 - Some other failure
	- 2 - We cannot connect to Steam, steam probably isn't running
	- 3 - Steam client appears to be out of date
---

## Checking For Errors

The returned dictionary from ***Steam.steamInitEx()*** can be printed and ignored. However, there are certain conditions where you might not know why the game crashed at boot or does something unexpected; especially in development. For these cases we will check if Steamworks was actually initialized and to stop the game if anything is amiss, we do this:

````
func _initialize_Steam() -> void:
	var INIT: Dictionary = Steam.steamInitEx()
	print("Did Steam initialize?: "+str(INIT))

	if INIT['status'] > 0:
		print("Failed to initialize Steam. "+str(INIT['verbal'])+" Shutting down...")
		get_tree().quit()
````

This code will obviously shut down the game if Steam does not initialize and returns a status of anything except 0.  You may just want to capture the failure data and continue on, though the Steamworks functionality won't quite work.

Most times, in development, getting a failure will probably be caused by a missing API file (steam_api.dll, libsteam_api.so, libsteam_api.dylib) or not setting the game's app ID as an environment variable.  Also not having the `steam_appid.txt` file with your game's app ID in it; if you aren't using the environment variable.

---

## Getting More Data

You can check for a few additional things at boot like:

````
var IS_ONLINE: bool = Steam.loggedOn()
var STEAM_ID: int = Steam.getSteamID()
var IS_OWNED: bool = Steam.isSubscribed()
````

This will check if Steam is online, get the current user's Steam ID64, and check if the current user owns the game. You can also have the game turn itself off if the current user does not own the game by simply doing this:

````
if IS_OWNED == false:
	print("User does not own this game")
	get_tree().quit()
````

Please note that this behavior might cause problems from people using Family Share, Free Weekends, or other methods of trying the game out. There are other functions to check for those conditions which you might want to consider.

There are other things you may want to do during a boot-up process after Steamworks is initialized, like getting current achievements or statistics, but we'll cover that in another tutorial.

---

## Callbacks

Last, but not least, anywhere you want or need to retrieve callbacks from Steam, you'll need to add this:

````
func _process(_delta: float) -> void:
	Steam.run_callbacks()
````

I highly suggest, much like the initialization process, you put this ***_process()*** function with the ***Steam.run_callbacks()*** in a global (singleton) script so it is always checking for callbacks. Though, if you want, you can put it in any ***_process()*** function in any given script that might be using callback information.

---

## Altogether Now

Putting it together should give us something like this:

````
extends Node

# Steam variables
var IS_OWNED: bool = false
var IS_ONLINE: bool = false
var STEAM_APP_ID: int = 480
var STEAM_ID: int = 0


func _init() -> void:
	# Set your game's Steam app ID here
	OS.set_environment("SteamAppId", str(STEAM_APP_ID))
	OS.set_environment("SteamGameId", str(STEAM_APP_ID))


func _ready() -> void:
	_initialize_Steam()


func _process(_delta: float) -> void:
	Steam.run_callbacks()


func _initialize_Steam() -> void:
	var INIT: Dictionary = Steam.steamInitEx()
	print("Did Steam initialize?: "+str(INIT))

	if INIT['status'] > 0:
		print("Failed to initialize Steam. "+str(INIT['verbal'])+" Shutting down...")
		get_tree().quit()

	IS_ONLINE = Steam.loggedOn()
	STEAM_ID = Steam.getSteamID()
	IS_OWNED = Steam.isSubscribed()

	# Check if account owns the game
	if IS_OWNED == false:
		logging.error("User does not own this game")
		get_tree().quit()
````

---

This covers the initialization and basic set-up.

[To see this tutorial in action, check out our GodotSteam Example Project on GitHub.](https://github.com/CoaguCo-Industries/GodotSteam-Example-Project){ target="_blank" } There you can get a full view of the code used which can serve as a starting point for you to branch out from.
