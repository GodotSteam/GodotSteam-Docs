---
title: Introduction
description: An introduction to GodotSteam and the differences between the two major types of tools.
icon: material/file-document
---

# Getting Started

GodotSteam allows for easy integration of [Valve's Steamworks API](https://partner.steamgames.com){ target="\_blank" } into your [Godot Engine](https://godotengine.org){ target="\_blank" } game either by downloading our pre-compiled editors / template bundles from Github or installing the GDExtension / GDNative plugin-ins from [Godot's Asset Library](https://godotengine.org/asset-library/asset?user=Gramps){ target="\_blank" }.

While you do not need to sign up with Valve to test out the Steamworks API, technically you are required to do so to gain access to the API files.  Also, without your own app ID, you are very limited in what you can test.  For instance, you cannot use the Inventory or Workshop features, anything that requires Steamworks back-end settings, and any lobbies you create get lost in a sea of SpaceWar demo lobby results.

{==
## :material-comment-question: Pre-Compiled Or Plug-In?
==}

The pre-compiled editor / template bundles are zipped up with everything you need to start working. Plug-ins are not much different except they use the regular version of Godot Engine.  However, depending on which version you are using, there may be some additional differences.

### Godot 4

With Godot 4 projects, there are not a whole lot of differences between the pre-compiled editor / template bundles or the GDExtension plug-ins.  Thanks to improvements with the plug-in system, they are pretty much on par with each other:

__Similar__

- Both have access to in-editor documentation
- Both have the Steam object globally by default
- Both behave exactly the same using the same code-base underneath

__Different__

- Pre-compiled requires its specific templates for exporting games, plug-in uses Godot's standard templates
- Plug-in requires additional shared library files to function after exporting, but they are provided
- Debugging plug-in failures can be a bit more complicated due to occasional downstream issues

---

### Godot 3

With Godot 3 projects, the differences are more obvious as the plug-in system had just been introduced and is not as refined.  People still enjoy the ease of just downloading the plug-in directly into the editor but there are some drawbacks:

__Similar__

- Large majority of the Steam functionality is identical

__Different__

- Pre-compiled requires its specific templates for exporting games, plug-in uses Godot's standard templates
- Plug-in does not have access to in-editor documentation
- Plug-in must instance the Steam object
- Plug-in has no access to Steam constants or enums
- [Plug-in has issues with Rich Presence on Windows and other minor quirks](../issues/common_issues.md/#gdnative)

---

### In Most Cases

The majority of users seem to prefer the plug-ins due to the ease of just installing them through the Godot editor they are already using.  Downloading the pre-compiled editor / template bundles are just as easy as getting Godot Engine itself though.

Gramps often suggests using the pre-compiled editor / template bundles as they are the most stable and battle-tested versions; especially if you are using Godot 3.  Though with Godot 4, you should not run into any real problems with the GDExtension plug-in.

Regardless of which option you pick, we **strongly suggest** you do not mix GodotSteam's pre-compiled editors / templates with the plug-ins; this can lead to unexpected results.