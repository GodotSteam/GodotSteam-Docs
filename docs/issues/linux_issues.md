---
title: Linux Issues
description: A list of common Linux issues you may run into.
icon: simple/linux
---

# Issues - Linux Platform

There are issues some users might run into on Linux when using Godot and GodotSteam.

{==
## GLES 2 / Mesa / Overlay Crash
==}

There is apparently an issue with games using GLES 2 and systems running Mesa 20.3.5 through 21.2.5 that causes the Steam overlay to not work or the game to crash. As of now there is no solution to fix this, except to use GLES 3 instead or a Mesa version outside that small window.

{==
## Libsteam_api.so Issues
==}

You may run into the following error:

```
error while loading shared libraries: libsteam_api.so: cannot open shared object file: No such file or directory
```

This means you forgot to place `libsteam_api.so` next to your executable.
Remember to include it along with your shipped game, as described in the [Export and Shipping tutorial](../tutorials/exporting_shipping.md).

{==
## Steam on Flatpak
==}

The Flatpak version of Steam will not work with GodotSteam in testing (through the editor) as it will be unable to find the running process correctly.  This may or may not affect the Steam version of Godot and downloading the plug-in version through the Asset Library; I have not yet tested this setup.  We did, however, get an updated piece of information to fix this situation though, you can symlink Steam out of Flatpak like so:

```
~/.steam to ~/.var/app/com.valvesoftware.Steam/.steam
```

If you are using something like Fedora Atomic, you can install Distrobox to work around this.