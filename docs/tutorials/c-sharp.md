# Tutorials - C-Sharp

Every so often we get someone who asks how to use GodotSteam with C# so I figured I would write this up. First and foremost, I have never used C# so my understanding of it is prety much null and void.

{==
## Where's The Mono Build?
==}

To the point, GodotSteam does not have a C# version currently. I tried once or twice to compile a version with Mono and it failed; I have yet to attempt this again.

That doesn't mean it isn't on the to-do list but there is no timeline for when this will happen. However, anyone and everyone is welcome to make that work and submit a pull request for it!

{==
## Workarounds
==}

Another user from the Discord, **jolexxa**, mentions this advice:

"My current recommendation is to use GodotSteam as a GDExtension and write 'glue' code in GDScript that can be called from C# to interface with Steam. Definitely a little overhead to doing that but it shouldn't be much worse that interfacing with the engine normally from C#."

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
