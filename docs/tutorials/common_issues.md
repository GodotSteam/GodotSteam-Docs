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

Every so often folks will find that Steam overlay does not work when running their project from the Godot 4.x editor. This has been the case since the Godot 4 alpha builds. Steam overlay _should_ work fine when using Compability / OpenGL mode; also the Steam overlay will work when the game is exported and run from the Steam client itself, as Steam injects the overlay during boot.

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

On occasion folks will download the pre-compiled editor (module version) and then install the plug-in from the Godot Asset Library. This will result in a few weird errors, most of which will prevent anything from working. If you do this, you'll need to pick one or the other.

If you stick with the pre-compiles, which is recommended, you'll need to completely remove the plug-in to get things working again.
