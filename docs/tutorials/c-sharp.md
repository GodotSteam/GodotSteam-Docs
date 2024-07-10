# Tutorials - C-Sharp

Every so often we get someone who asks how to use GodotSteam with C# so I figured I would write this up. First and foremost, I have never used C# so my understanding of it is prety much null and void.

{==
## Where's The Mono Build?
==}

To the point, GodotSteam does not have a C# version currently. I am in the process of adding said builds to our Github Actions list so we can start producing them with every update!

{==
## Workarounds
==}

Thankfully, for those of you using the GDExtension version of the project, there is a lovely project that will help!  Thanks to **LauraWebdev**, we have this cool plug-in: [GodtoSteam C# Bindings](https://github.com/LauraWebdev/GodotSteam_CSharpBindings){ target="_blank" }

While I am still trying to get up to speed on all this C# business, you can read more about how it works on Github. There are even some excellent pieces of example code!

### SteamMultiplayerPeer

If you are using [SteamMultiplayerPeer](https://github.com/expressobits/steam-multiplayer-peer) for networking you can use a C# wrapper. It's currently not merged but there is a [PR](https://github.com/expressobits/steam-multiplayer-peer/pull/21/commits/9ed16cdc27fcd21c9cd28dbe652c55f79b1b3a82). You can put that script anyway in your Godot project and it will work with the rest of the C# bindings above.

{==
## Other Resources
==}

The awesome folks at Chickensoft are all about C#, visit them here or at their Discord:

[:simple-firefox: Chickensoft Website](https://chickensoft.games/){ .md-button .md-button--resource target="\_blank" }

[:simple-discord: Chickensoft Discord](https://discord.gg/MjA6HUzzAE){ .md-button .md-button--resource target="\_blank" }

More often than not, I will refer C# users to one of two wonderful libraries for Steamworks and C#:

[:simple-steam: Facepunch.Steamworks by the makers of Rust and Garry's Mod](https://wiki.facepunch.com/steamworks){ .md-button .md-button--resource target="\_blank" }

[:simple-steam: Steamworks.NET by the super-nice Riley Labrecque](https://steamworks.github.io){ .md-button .md-button--resource target="\_blank" }

I have no idea how you actually integrate these with Godot but some members of our Discord do. I will try reaching out to them at some point for additions to this tutorial.
