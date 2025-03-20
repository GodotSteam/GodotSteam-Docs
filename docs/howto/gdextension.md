---
title: How To Compile GDExtension From Scratch
description: Complete instructions on how to compile GDExtension from source code.
icon: material/book-cog
---

# Compiling The GodotSteam GDExtension From Scratch

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
### :simple-godotengine: Get GodotSteam GDExtension
==}

This is a little different from the modules as we get GodotSteam's source first and _then_ Godot's.

#### Cloning the Source

You can clone the source into a folder called ***godotsteam_gdextension*** like so:

```shell
git clone -b gdextension https://github.com/GodotSteam/GodotSteam.git godotsteam_gdextension
```

Cloning the repo should also pull the sub-module for Godot CPP, so you can [skip to Get the Steamworks SDK.](#get-the-steamworks-sdk)

However, you may want to change the version of Godot CPP, depending on your target version. If so, [read the next section Get Godot CPP](#get-godot-cpp).

#### Downloading the Source

Alternatively, you can [download the GDExtension source from our repository](https://github.com/GodotSteam/GodotSteam){ target="\_blank" } then unpack it into a folder named ***godotsteam_extension***.  You will need to pull the Godot CPP source yourself if you use this method.

{==
### :simple-godotengine: Get Godot CPP
==}

#### Cloning the Source

If you changed your target version or manually downloaded the GDExtension source, you can head into the ***godotsteam_gdextension*** folder and clone the latest Godot CPP source in a folder called ***godot-cpp*** like so:

```shell
git clone https://github.com/godotengine/godot-cpp.git -b 4.4 godot-cpp
```

You may need to change the given tag(s) above from whatever it is to whatever the current version or whatever version you need.

#### Downloading the Source

Alternatively, you can download and unpack the [Godot CPP source from their Github](https://github.com/godotengine/godot-cpp){ target="\_blank" } in a folder called ***godot-cpp***.

#### Compile the Bindings

In either case, you will want to head into the **godot-cpp** folder and compile the bindings for your platform. Make sure your slashes are OS appropriate:

```shell
  scons platform=<your platform> target=template_release
  scons platform=<your platform> target=template_debug
```

{==
### :simple-steam: Get the Steamworks SDK
==}

Download the [Steamworks SDK from Valve's partners site](https://partner.steamgames.com){ target="\_blank" }. This requires a Steam developer account.

Move the ***public*** and ***redistributable_bin*** folders from the unzipped Steamworks SDK into the ***godotsteam_gdextension/godotsteam/sdk/*** so you have the following layout:

```shell
godotsteam_gdextension/
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
godotsteam_gdextension
│─ bin/
├─ godot-cpp/
│  ├─ bin/*
│  ├─ cmake/*
│  ├─ gdextension/*
│  ├─ gen/*
│  ├─ include/*
│  ├─ misc/*
│  ├─ src/*
│  ├─ test/*
│  ├─ tools/*
│  ├─ binding_generator.py
│  └─ SConstruct
├─ godotsteam/
│  ├─ doc_classes/*
│  ├─ sdk/
│  │  ├─ public/steam/*
│  │  └─ redistributable_bin/*
│  ├─ api.json
│  ├─ godotsteam.h
│  ├─ godotsteam.cpp
│  ├─ gdootsteam_constants.h
│  ├─ register_types.cpp
│  ├─ register_types.h
│  └─ godotsteam.gdextension
└─ SConstruct
```
You probably will not have the ***bin/*** folder in the root so go ahead and create that.

{==
### :octicons-terminal-16: Compiling Time
==}

Just run the following command for your operating system from the ***godotsteam_gdextension*** folder root:

=== "Windows PowerShell"

	```shell
	scons platform=windows target=template_release
	scons platform=windows target=template_debug
	```

=== "Linux"

	```shell
	scons platform=linuxbsd target=template_release
	scons platform=linuxbsd target=template_debug
	```

=== "macOS"

	```shell
	scons platform=macos target=template_release
	scons platform=macos target=template_debug
	```

The Visual Studio instructions carried over from ***GDNative*** _probably do not_ work anymore so they were removed. If anyone would like to contribute new ones, please feel free to do so!

{==
### :fontawesome-solid-box: All Together Now
==}

When compiling is finished, create a brand new folder somewhere called ***addons*** and, inside that, create a folder called ***godotsteam***.

Now copy the whole ***win64/***, ***linuxbsd/***, or ***osx/*** folder(s) from within the ***godotsteam_gdextension/bin/*** folder into ***addons/godotsteam/***. We will want to rename that ***linuxbsd*** folder to ***linux64*** or ***linux32*** depending on what you created.

You will also want to copy the matching Steamworks API file(s) from ***godotsteam_gdextension/godotsteam/sdk/redistributable_bin/*** and put them in with the corresponding platform's folder.

This all sound a little confusing? It should look a little something like this:
  
=== "Windows 64-bit"

	```shell
	addons
	└─ godotsteam
	   └─ win64
		  │─ libgodotsteam.windows.template_debug.x86_64.dll
		  │─ libgodotsteam.windows.template_release.x86_64.dll
		  └─ steam_api64.dll
	```

=== "Windows 32-bit"

	```shell
	addons
	└─ godotsteam
	   └─ win32
		  │─ libgodotsteam.windows.template_debug.x86_32.dll
		  │─ libgodotsteam.windows.template_release.x86_32.dll
		  └─ steam_api.dll
	```

=== "Linux 64-bit"

	```shell
	addons
	└─ godotsteam
	   └─ linux64
		  │─ libgodotsteam.linuxbsd.template_debug.x86_64.so
		  │─ libgodotsteam.linuxbsd.template_release.x86_64.so
		  └─ libsteam_api.so
	```

=== "Linux 32-bit"

	```shell
	addons
	└─ godotsteam
	   └─ linux32
		  │─ libgodotsteam.linuxbsd.template_debug.x86_32.so
		  │─ libgodotsteam.linuxbsd.template_release.x86_32.so
		  └─ libsteam_api.so
	```

=== "Mac"

	```shell
	addons
	└─ godotsteam
	   └─ osx
		  │─ libgodotsteam.debug.framework
		  │  │─ libgodotsteam.macos.template_debug.universal
		  │  └─ libsteam_api.dylib
		  │─ libgodotsteam.framework
		  │  │─ libgodotsteam.macos.template_release.universal
		  │  └─ libgodotsteam.framework/libsteam_api.dylib
		  └─ libsteam_api.dylib
	```

Lastly, copy the **godotsteam.gdextension** file from the base of your ***godotsteam_gdextensioni*** folder and place it into the **addons/godotsteam/** folder.

This can now be added to any project you have; just pop it into the root of the project and go!

#### Renaming Notes

Typically we rename the produced shared library files by removing the ***template_*** prefixes and operating system naming like ***windows***, ***linuxbsd***, or ***macos*** as those are implied through their parent folders. Just in case you like a tidier plug-in.

{==
### :octicons-thumbsup-16: Good to Go
==}

This GDExtension **does not** have to be enabled as it is **always present** if the addon is there.

From here you should have access to all of the Steamworks SDK functions and callbacks.

You should be able to [read the Steamworks API documentation](https://partner.steamgames.com/doc/){ target="\_blank" } to see what all is available and cross-reference with GodotSteam's documentation.

Feel free to check out our tutorials if you want to learn some basics or just start tinkering!