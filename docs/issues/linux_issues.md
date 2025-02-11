---
title: Linux Issues
description: A list of common Linux issues you may run into.
icon: simple/linux
---

# Issues - Linux Platform

There are issues some users might run into on Linux when using Godot and GodotSteam.

{==
## :material-black-mesa: GLES 2 / Mesa / Overlay Crash
==}

There is apparently an issue with games using GLES 2 and systems running Mesa 20.3.5 through 21.2.5 that causes the Steam overlay to not work or the game to crash. As of now there is no solution to fix this, except to use GLES 3 instead or a Mesa version outside that small window.

{==
## :material-api: Libsteam_api.so Issues
==}

You may run into the following error:

```
error while loading shared libraries: libsteam_api.so: cannot open shared object file: No such file or directory
```

This means you forgot to place `libsteam_api.so` next to your executable.
Remember to include it along with your shipped game, as described in the [Export and Shipping tutorial](../tutorials/exporting_shipping.md).

{==
## :simple-nixos: NixOS Setup
==}

NixOS seemed to be problematic for some users but thankfully **GreenAnts** shared this awesome series of instructions to get it all working.  [You can read his original write-up on Github Discussions.](https://github.com/GodotSteam/GodotSteam/discussions/814){ target="\_blank" }

!!! warning Notes
	TLDR: Add nix-ld and the accompanied library "pkgs.stdenv.cc.cc"

### Step 1
This is likely something you have already done, but ensure you have Godot "installed".

Find the code that reads **environment.systemPackages** and add **godot_4** so that it looks something like this:

```
environment.systemPackages = with pkgs; [
    ### multiple packages should be listed here
    godot_4
  ];
```

### Step 2
Go to **/etc/nixos/configuration.nix** and add the following lines:

```
programs.nix-ld.enable = true;
  programs.nix-ld.libraries = [
    pkgs.stdenv.cc.cc
    #pkgs.stdenv.cc.cc.lib
  ];
```

Just to be extra clear, the code should be added within the second set of { }. Example:

```
{ config, pkgs, ... }:
{
### Lots of other code is here ###
programs.nix-ld.enable = true;
  programs.nix-ld.libraries = [
    pkgs.stdenv.cc.cc
    #pkgs.stdenv.cc.cc.lib
  ];
}
```

There are two lines: **pkgs.stdenv.cc.cc** and **#pkgs.stdenv.cc.cc.lib**.  One is commented out and the other is not. If you run into issues, you can try the other one instead.

### Step 3
Go to the console and rebuild by typing `sudo nixos-rebuild switch`

### Step 4
After rebuild is complete, you must run Godot through the terminal with the following command:

```
export LD_LIBRARY_PATH=$NIX_LD_LIBRARY_PATH; godot4
```

Note that **godot_4** is what the package name is; however, we use **godot4** to execute the software from the terminal.

Reasoning for what the export command is doing:

> Afaik, export sets name=value pairs as environment variables, and would think that you are trying to unset the environment variable named godot4
>
> - Phush (from nixOS Discord server)

Additionally, to prevent the need to type the command every time you need to run godot, you could save the command into a shell script, and run that script:

```
bash
#!/usr/bin/env bash
export LD_LIBRARY_PATH=$NIX_LD_LIBRARY_PATH
godot4
```

To do this, copy the above code and paste it into a document. Lets say we save the file as **launch_godot** - then navigate to the folder (or possibly you could add it to the path?) and type `bash launch_godot` (or whatever other file name).

{==
## :simple-flatpak: Steam on Flatpak
==}

The Flatpak version of Steam will not work with GodotSteam in testing (through the editor) as it will be unable to find the running process correctly.  This may or may not affect the Steam version of Godot and downloading the plug-in version through the Asset Library; I have not yet tested this setup.  We did, however, get an updated piece of information to fix this situation though, you can symlink Steam out of Flatpak like so:

```
~/.steam to ~/.var/app/com.valvesoftware.Steam/.steam
```

If you are using something like Fedora Atomic, you can install Distrobox to work around this.