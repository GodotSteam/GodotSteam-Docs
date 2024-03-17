# Common Issues

What could possibly go wrong? Well, we will cover some of those common pitfalls people find themselves in. This should help you troubleshoot some of the basics for when things don't seem to be working.

{==
## steam_appid.txt.txt
==}

If you aren't using the environment variables to set what game you're running, then you're probably using the older method of seatting your game's app ID in the `steam_appid.txt` file. For Windows users, when creating the `steam_appid.txt` file you need to watch out for the dreaded `.txt.txt` extension. If you have extensions hidden in the file explorer, this is more likely to happen since you can't see that extra `.txt`. If you do have them hidden, make sure to skip adding `.txt` to the end.

{==
## Error 79 When Initializing
==}

This can have quite a few causes but a common one is not having your depots or packages set up correctly. Check out [the packages page in the official Steamworks SDK documentation](https://partner.steamgames.com/doc/store/application/packages){ target="\_blank" } for more on how to do it.

{==
## Parse Error
==}

Some people get `Parse Error: The identifier Steam isn't declared in the current scope`. If you get this error, one of the following is the cause:

- Either you're not using a pre-compiled editor.
- You didn't actually include GodotSteam in your build when compiling.
- You're using a non-GodotSteam template when exporting from a GodotSteam-enabled editor.

{==
## Achievements Not Working
==}

Sometimes your brand new achievements don't seem to be triggering. One cause can be that you didn't publish them in the Steamworks back-end. Once they are added into Steam's system, you'll need to publish the changes to be able to work with them.

Some users have also found that getting or setting achievements doesn't work at all until the player's current stats have been retrieved. GodotSteam should do this by default when you initialize Steamworks; unless, that is, you passed `false` to either `steamInit()` or `steamInitEx()`.  If so, just call `requestCurrentStats()` or `requestUserStats()`.

{==
## Steam Overlay and Forward+ / Vulkan
==}

Every so often folks will find that Steam overlay does not work or flickers when running their project from the Godot 4.x editor or a standalone Forward+ build outside of Steam. This has been the case since the Godot 4 alpha builds. Steam overlay _should_ work fine when using Compability / OpenGL mode.

The Steam overlay will render correctly once a Forward+ build is run from the Steam client itself, as Steam injects the overlay during boot. When run from Steam as a Non-Steam app, the overlay will display correctly, but won't be tied to the app ID, so achievements and game artwork won't show up inside the overlay.

### Driver Updates

Certain driver versions on GPUs will allow Steam overlay to work with Forward+ mode. If the overlay isn't working, you may want to try changing version numbers.

A user also reported that with NVidia driver version 546.33, in Forward+ mode, that the game stuttered / jittered a bit when Steam was initialized. However, the stuttering went away when switching to Compatibility mode.

### Other Solutions

There is also a nice video about getting overlay to work without messing with drivers, thanks to FinePointCGI:

[ :simple-youtube: 'How To Fix Your Steam Overlay' by FinePointCGI](https://www.youtube.com/watch?v=VCwNxfYZ8Cw&t=6725s){ .md-button .md-button--resource target="\_blank" }

{==
## Leaderboard Names
==}

I haven't actually confirmed this yet, but it seems that a dash in the leaderboard name will cause it to fail. For example, ***this-leaderboard*** will probably not work but ***this_leaderboard*** will.

Also remember that leaderboards need to be published to be functional.

{==
## Using the Module and Plug-in
==}

On occasion folks will download the pre-compiled editor (module version) and then install the plug-in from the Godot Asset Library. This will result in a few weird errors as you will probably be making duplicate calls.

### Using Plug-In First

If you installed the plug-in and want to switch to the pre-compiled module, or your own compiled version; you'll need to completely remove the plug-in from you project's folder and in the settings.

If using Godot 4 and the GodotSteam GDExtension, check in the .godot folder for any traces of the extension in your extension_list.cfg and remove it.

### Using Module First

If you are coming from the pre-compiled modules versions and want to switch to the plug-ins, make sure you remove the Steam shared library file (steam_api64.dll, libsteam_api.so, or libsteam_api.dylib) from either the root of your project or sitting next to your editor executable.

In some cases, this will prevent the plug-in from loading correctly.

{==
## Steam on Flatpak
==}

The Flatpak version of Steam will not work with GodotSteam in testing (through the editor) as it will be unable to find the running process correctly.  This may or may not affect the Steam version of Godot and downloading the plug-in version through the Asset Library; I have not yet tested this setup.

At the time of writing, we have not found a way to get these two talking.  However, your shipped game will work fine.  Also, if anyone knows how to solve this, please let us know!

If you are using something like Fedora Atomic, you can install Distrobox to work around this.

{==
## GDNative Quirks
==}

GDNative is the odd-man-out in GodotSteam as it has some extra "got ya's" that need to be listed.

### No Enums

There is no way to bind the enums so they are not present.

### Rich Presence

Rich presence will work fine on all operating systems _except_ Windows.  It will occasionally make the value the key on random calls.

### Getting Lobbies

For whatever reason, the get lobby callback would not return a single array like everything else so the callback will send an array of lobbies and the total count.