---
title: How To Compile GDNative From Scratch
description: Complete instructions on how to compile GDNative from source code.
icon: material/book-cog
---


For those of you who are comfortable compiling or want to give it a shot, here are some steps to follow.

{==
### :fontawesome-solid-toolbox: Set Up Tools
==}

Follow Godot's documentation to setup the required tools for your operating system.

<div class="button-grid" markdown>

[:material-microsoft-windows: Windows](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_windows.html){ .md-button .md-button--primary target="\_blank" }
[:material-linux: Linux](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_linuxbsd.html){ .md-button .md-button--primary target="\_blank" }
[:material-apple: Mac](https://docs.godotengine.org/en/latest/contributing/development/compiling/compiling_for_macos.html){ .md-button .md-button--primary target="\_blank" }

</div>

{==
### :simple-godotengine: Get GodotSteam GDNative
==}

This is a little different from the modules as we get GodotSteam's source first and _then_ Godot's.

#### Cloning the Source

You can clone the source into a folder called ***godotsteam_gdnative*** like so:

```shell
git clone -b gdnative https://github.com/GodotSteam/GodotSteam.git godotsteam_gdnative
```

Cloning the repo should also pull the sub-module for Godot CPP, so you can [skip to Get the Steamworks SDK.](#get-the-steamworks-sdk)

However, you may want to change the version of Godot CPP, depending on your target version. If so, [read the next section Get Godot CPP](#get-godot-cpp).

#### Downloading the Source

Alternatively, you can [download the GDNative source from our repository](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } then unpack it into a folder named ***godotsteam_gdnative***.  You will need to pull the Godot CPP source yourself if you use this method.

{==
### :simple-godotengine: Get Godot CPP
==}

#### Cloning the Source

If you changed your target version or manually downloaded the GDNative source, you can head into the ***godotsteam_gdnative*** folder and clone the latest Godot CPP source in a folder called ***godot-cpp*** like so:

```shell
git clone https://github.com/godotengine/godot-cpp.git -b godot-3.5.1-stable godot-cpp
```

You may need to change the given tag(s) above from whatever it is to whatever the current version or whatever version you need.

#### Downloading the Source

Alternatively, you can download and unpack the [Godot CPP source from their Github](https://github.com/godotengine/godot-cpp){ target="\_blank" } in a folder called ***godot-cpp***.

#### Compile the Bindings

In either case, you will want to head into the **godot-cpp** folder and compile the bindings for your platform. Make sure your slashes are OS appropriate:

```shell
scons platform=<your platform> generate_bindings=yes target=release
```

{==
### :simple-steam: Get the Steamworks SDK
==}

Download the [Steamworks SDK from Valve's partners site](https://partner.steamgames.com){ target="\_blank" }. This requires a Steam developer account.

Move the ***public*** and ***redistributable_bin*** folders from the unzipped Steamworks SDK into the ***godotsteam_gdnative/godotsteam/sdk/*** so you have the following layout:

```shell
godotsteam_gdnative/
└─ godotsteam/
   └─ sdk/
	  ├─ public/*
	  └─ redistributable_bin/*
```

{==
### :material-file-tree: Double-Checking Folder / File Structure
==}

Before we start compiling, let us make sure everything is in place. Your ***godotsteam_gdextension*** directory should look something like this:

```shell
godotsteam_gdnative
│─ bin/
├─ godot-cpp/
│  ├─ bin/*
│  ├─ godot-headers/*
│  ├─ include/*
│  ├─ misc/*
│  ├─ src/*
│  ├─ test/*
│  ├─ binding_generator.py
│  └─ SConstruct
├─ godotsteam/
│  ├─ sdk/
│  │  ├─ public/steam/*
│  │  └─ redistributable_bin/*
│  ├─ godotsteam.h
│  ├─ godotsteam.cpp
│  ├─ gdootsteam_constants.h
│  └─ init.cpp
├─ VisualC/*
└─ SConstruct
```
You probably will not have the ***bin/*** folder in the root so go ahead and create that.

{==
### :octicons-terminal-16: Compiling Time
==}

Just run the following command for your operating system from the ***godotsteam_gdextension*** folder root:

=== "Windows Powershell / VS Prompt"

	```shell
	scons platform=windows production=yes target=release
	```

=== "Windows VS"
	Follow these steps for Visual Studio (big thanks to **willnationsdev**):

	- [x] Create a new Visual Studio project.
	- [x] Name it **GDNative** and make sure it DOES NOT create a directory.
		- Uncheck the box here.
	- [x] Select the **GDNative** folder we were working in.
	- [x] Choose Win32 Desktop Wizard template.
	- [x] Select options for both a dynamic library (.dll) and an empty project.
	- [x] Things should look like this:
	  ```shell
	  GDNative -godot-cpp -godot_headers -lib -GDNative --.vs --GDNative.sln --GDNative.vcxproj --GDNative.vcsproj.filters -src
	  ```
	- [x] Make sure you have a debug, x64 configuration for the solution.
		- The options are located in the toolbar at the top left.
	- [x] Go to "Project > GDNative Properties" to open the project properties.
	- [x] Ensure you are on the x64 Debug configurations at the top and make these changes:
		- VC++ Directories > Include Directories. Add 'GDNative\godot-cpp\include', 'GDNative\godot-cpp\include\core', and 'GDNative\godot-cpp\godot-headers' to the list.
		- VC++ Directories > Library Directories. Add 'GDNative\godotsteam'.
		- VC++ Directories > Source Directories. Add 'GDNative\godotsteam'.
		- C/C++ > Linker > System. Subsystem = "Console (/SUBSYSTEM:CONSOLE)"
		- C/C++ > Linker > Input. Add "godot-cpp.windows.64.lib" (without quotes) to the Additional Dependencies parameter.
	- [x] Click on Apply and then Save.
	- [x] Now build the solution.

=== "Linux"

	```shell
	scons platform=linux production=yes target=release
	```

=== "Mac"

	```shell
	scons platform=osx production=yes target=release
	```

{==
### :fontawesome-solid-box: All Together Now
==}

When compiling is finished, create a brand new folder somewhere called ***addons*** and, inside that, create a folder called ***godotsteam***.

Now copy the whole ***win64/***, ***linuxbsd/***, or ***osx/*** folder(s) from within the ***godotsteam_gdnative/bin/*** folder into ***addons/godotsteam/***. We will want to rename that ***linuxbsd*** folder to ***x11***.

You will also want to copy the matching Steamworks API file(s) from ***godotsteam_gdnative/godotsteam/sdk/redistributable_bin/*** and put them in with the corresponding platform's folder.

This all sound a little confusing? It should look a little something like this:
  
=== "Windows"

	```shell
	addons
	└─ godotsteam
	   └─ win64
		  │─ godotsteam.dll
		  └─ steam_api64.dll
	```

=== "Linux"

	```shell
	addons
	└─ godotsteam
	   └─ x11
		  │─ libgodotsteam.so
		  └─ libsteam_api.so
	```

=== "Mac"

	```shell
	addons
	└─ godotsteam
	   └─ osx
		  │─ libgodotsteam.dylib
		  └─ libsteam_api.dylib
	```

#### Making godotsteam.gdnlib

In a text editor, create a file called ***godotsteam.gdnlib*** and place the following inside this file then save it in the **addons/godotsteam/** folder:

```shell
[general]
singleton=false
load_once=true
symbol_prefix="godot_"
reloadable=true

[entry]
(read below)

[dependencies]
(read below)
```

Replace (read below) with the following, based on platform. You can also use all three if you have the right files.

=== "Using Windows"

	```shell
	[entry]
	Windows.64="res://addons/godotsteam/win64/godotsteam.dll"

	[dependencies]
	Windows.64=[ "res://addons/godotsteam/win64/steam_api64.dll" ]
	```

=== "Using Linux"

	```shell
	[entry]
	X11.64="res://addons/godotsteam/x11/libgodotsteam.so"

	[dependencies]
	X11.64=[ "res://addons/godotsteam/x11/libsteam_api.so" ]
	```

=== "Using OSX"

	```shell
	[entry]
	OSX.64="res://addons/godotsteam/osx/libgodotsteam.dylib"

	[dependencies]
	OSX.64=[ "res://addons/godotsteam/osx/libsteam_api.dylib" ]
	```
 
To double-check this worked, place the ***addons*** folder in a Godot project then open the ***.gdnlib*** file in the ***Inspector***. It will have the correct data in the GUI editor that pops up in the bottom panel.

#### Making godotsteam.gdns

In a text editor, create a file called ***godotsteam.gdns*** and place the following inside this file then save it in the ***addons/godotsteam/*** folder:

```shell
[gd_resource type="NativeScript" load_steps=2 format=2]

[ext_resource path="res://addons/godotsteam/godotsteam.gdnlib" type="GDNativeLibrary" id=1]

[resource]

resource_name = "godotsteam"
class_name = "Steam"
library = ExtResource( 1 )
script_class_name = "Steam"
```

#### Making steam.gd

In a text editor, create a file called ***steam.gd*** and place the following inside this file then save it in the ***addons/godotsteam/*** folder:

```gdscript
extends Node

onready var Steam: Object = preload("res://addons/godotsteam/godotsteam.gdns").new()
var steam_app_id: int = 480   # or your game's app ID


func _init() -> void:
  OS.set_environment("SteamAppId", str(steam_app_id))
  OS.set_environment("SteamGameId", str(steam_app_id))

func _ready():
  var initialize_response: Dictionary = Steam.steamInitEx()
  print(initialize_response)

```

After placing the ***addons/*** folder into your project, you will want to navigate to ***Project > Project Settings*** in the editor and click on ***Autoload*** then add your ***steam.gd*** as a singleton, with the name ***Steam***.

{==
### :octicons-thumbsup-16: Good to Go
==}

GodotSteam GDNative will behave a little differently than the modules or GDExtension since it has to be loaded in and passed to an object in the ***steam.gd*** file.

While you can call functions from ***Steam*** like you would normally with the ***GodotSteam modules***, they have to be added to your **steam.gd** script like this:

```gdscript
name = Steam.getPersonaName()
country = Steam.getIPCountry()
running = Steam.isSteamRunning()

func setAchievement(achieve: String) ->v void:
  Steam.setAchievement( achieve )
```

If you call Steamworks function outside of that script, however they must be prefaced with another ***Steam.*** like this:

```
name = Steam.Steam.getPersonaName()
country = Steam.Steam.getIPCountry()
running = Steam.Steam.isSteamRunning()

func setAchievement(achieve: String) -> void:
  Steam.Steam.setAchievement( achieve )
```

It may be less verbose and messy to write your functions in the ***steam.gd*** file then just reference them in other scripts.

From here you should have access to all of the Steamworks SDK functions and callbacks.

You should be able to [read the Steamworks API documentation](https://partner.steamgames.com/doc/){ target="\_blank" } to see what all is available and cross-reference with GodotSteam's documentation.

Feel free to check out our tutorials if you want to learn some basics or just start tinkering!

#### Final Notes

GDNative on Windows has some odd glitch with ***setRichPresence()*** where sometimes the key is sent as the value; this bug does not exist in the Linux or OSX versions of GodotSteam GDNative nor in any versions of the GodotSteam module nor any versions of the GodotSteam GDExtension. In this case it is deemed unfixable.
