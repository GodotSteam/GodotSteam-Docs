# How-To Compile Multiplayer Peer

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
### :simple-godotengine: Get Godot
==}

Regardless of which option you pick, make sure it is for Godot version 4.0 or higher.

#### Cloning the Source

You can clone the latest Godot source in a folder called ***godot*** like so:

```shell
git clone https://github.com/godotengine/godot.git -b 4.2.1-stable godot
```
You may need to change the tag from ***4.2.1-stable*** to whatever the current version is or whatever version you need.

#### Downloading the Source

Alternatively, you can download and unpack the [Godot source from their Github](https://github.com/godotengine/godot){ target="\_blank" } into a folder called ***godot***.

{==
### :simple-godotengine: Get GodotSteam
==}

#### Cloning the Source

Head into the ***godot/modules/*** folder and clone the source like so:

```shell
git clone -b godot4 https://github.com/CoaguCo-Industries/GodotSteam.git godotsteam
```

#### Downloading the Source

Alternatively, you can [download the godot4 branch from our repository](https://github.com/CoaguCo-Industries/GodotSteam/tree/godot4){ target="\_blank" } then unpack it into a folder named ***godotsteam*** inside your Godot source ***godot/modules/*** folder.

{==
### :simple-godotengine: Get GodotSteam Multiplayer Peer
==}

Head into the ***godot/modules*** folder and clone the source like so:

```shell
git clone -b multiplayer-peer https://github.com/CoaguCo-Industries/GodotSteam.git godotsteam_multiplayer
```
Alternatively, you can [download the multiplayer-peer branch from our repository](https://github.com/CoaguCo-Industries/GodotSteam){ target="\_blank" } then unpack it into a folder called ***godotsteam_multiplayer*** inside your Godot source ***godot/modules/*** folder.

{==
### :simple-steam: Get the Steamworks SDK
==}

Download the [Steamworks SDK from Valve's partners site](https://partner.steamgames.com){ target="\_blank" }. This requires a Steam developer account.

Move the ***public*** and ***redistributable_bin*** folders from the unzipped Steamworks SDK into the ***godot/modules/godotsteam/sdk/*** so you have the following layout:

```shell
godot/
└─ modules/
   └─ godotsteam/
      └─ sdk/
         ├─ public/*
         └─ redistributable_bin/*
```

{==
### :material-file-tree: Double-Checking Folder / File Structure
==}

Before we start compiling, let us make sure everything is in place. Your ***godot*** directory should look something like this:

```shell
godot/
└─ modules/
   ├─ godotsteam/
   │  ├─ doc_classes/
   │  ├─ sdk/
   │  │  ├─ public/*
   │  │  └─ redistributable_bin/*
   │  ├─ config.py
   │  ├─ godotsteam.cpp
   │  ├─ godotsteam.h
   │  ├─ godotsteam_constants.h
   │  ├─ register_types.cpp
   │  ├─ register_types.h
   │  └─ SCsub
   └─ godotsteam_multiplayer/
      ├─ config.py
      ├─ register_types.cpp
      ├─ register_types.h
      ├─ SCsub
      ├─ steam_id.cpp
      ├─ steam_id.h
      ├─ steam_multiplayer_peer.cpp
      └─ steam_multiplayer_peer.h
```

You can also just put the ***godotsteam*** and ***godotsteam_multiplayer*** directories where ever you like and just point to them in SCONS when compiling: 

```shell
custom_modules='../path/to/dir/godotsteam, ../path/to/dir/godotsteam_multiplayer'
```

{==
### :octicons-terminal-16: Compiling Time
==}

To compile editors, use this SCONS line:

```shell
scons platform=<your platform> tools=yes target=editor
```

To compile debug templates, use this SCONS line:

```shell
scons platform=<your platform> tools=no target=template_debug
```

To compile release templates, use this SCONS line:

```shell
scons platform=<your platform> tools=no target=template_release
```

#### A Linux Note

If using Ubuntu 16.10 or higher and having issues with PIE security in GCC, use ```LINKFLAGS='-no-pie'``` to get an executable instead of a shared library.

{==
### :fontawesome-solid-box: All Together Now
==}

When the compiling is finished, make sure to copy the Steam shared library, for your operating system, from ***godot/modules/godotsteam/sdk/redistributable_bin/*** folder to the newly-compiled Godot binary location.

By default, it should be in the ***godot/bin/*** folder. You can move them to a new location as long as you keep the two files together.

!!! warning "Missing Shared Library"
    
    A lack of the **Steam API .dll/.so/.dylib** for your respective OS will cause the editor or game to fail and crash when testing or running the game _outside_ of the Steam client.

#### A General Note

Some people report putting the Steam API file inside their project folder fixes some issues.

#### A Mac Note

For MacOS, the ***libsteam_api.dylib*** _must_ be in the ***Content/MacOS/*** folder in your application zip or the game will crash.

#### A Linux Note

For Linux, you may have to load the overlay library for Steam overlay to work:

```shell
export LD_PRELOAD=~/.local/share/Steam/ubuntu12_32/gameoverlayrenderer.so;~/.local/share/Steam/ubuntu12_64/gameoverlayrenderer.so
  
or 
  
export LD_PRELOAD=~/.local/share/Steam/ubuntu12_32/gameoverlayrenderer.so;
export LD_PRELOAD=~/.local/share/Steam/ubuntu12_64/gameoverlayrenderer.so;
```

This can be done in an .sh file that runs these before running your executable. This issue may not arise for all users and can also just be done by the user in a terminal separately. You can [read more about it in our Linux Caveats doc](../tutorials/linux_caveats.md).

{==
### :octicons-thumbsup-16: Good to Go
==}

From here you should have access to all of the Steamworks SDK functions and callbacks, as well as use Godot's RPC, MultiplayerSynchronizer node, and MultiplayerSpawner node.

You should be able to [read the Steamworks API documentation](https://partner.steamgames.com/doc/){ target="\_blank" } to see what all is available and cross-reference with GodotSteam's documentation.

Feel free to check out our tutorials if you want to learn some basics or just start tinkering!