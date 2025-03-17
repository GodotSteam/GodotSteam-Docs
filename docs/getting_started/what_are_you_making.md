---
title: What Are You Actually Making?
description: A look at the different options available depending on your project.
icon: material/tools
---

# What Are You Actually Making?

Depending on the type of project you are working, you will probably want to use a different version of GodotSteam.  Below we will talk about the different versions and how they best fit your goals.

{==
## :material-gamepad-variant: Singleplayer or Steam Networking
==}

The standard GodotSteam version will be what you want if you are just making a singleplayer game. It will also work if you are making a multiplayer game that uses one of the following:

- Steam's networking classes by themselves
- Godot's ENet networking
- Godot's MultiplayerPeer nodes in Godot 4.x
- Mixed Steam / Godot MultiplayerPeer through [ExpressoBits' SteamMultiplayerPeer GDExtension](https://github.com/expressobits/steam-multiplayer-peer){ target="\_blank" }.

<div class="grid cards triple-grid" markdown>
	
- :simple-godotengine:{ .lg .middle } __Godot 4.x__

	---
	
	<span class="badge-group">
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">4.4</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.61</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .steam title="GodotSteam" }</span>
			<span class="sep">|</span>
			<span class="value">4.13</span>
			<span class="sep">]</span>
		</span>
	</span>

	[:octicons-arrow-right-24: Download Pre-Compiled Bundle](https://github.com/GodotSteam/GodotSteam/releases/tag/v4.13){ target="\_blank" }

	[:octicons-arrow-right-24: Download GDExtension](https://godotengine.org/asset-library/asset/2445){ target="\_blank" }

- :simple-godotengine:{ .lg .middle } __Godot 3.x__

	---
	
	<span class="badge-group">
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">3.6</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.61</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .steam title="GodotSteam" }</span>
			<span class="sep">|</span>
			<span class="value">3.28</span>
			<span class="sep">]</span>
		</span>
	</span>

	[:octicons-arrow-right-24: Download Pre-Compiled Bundle](https://github.com/GodotSteam/GodotSteam/releases/tag/v3.28){ target="\_blank" }

	[:octicons-arrow-right-24: Download GDNative (Deprecated)](https://godotengine.org/asset-library/asset/1045){ target="\_blank" }

</div>

{==
## :fontawesome-solid-people-group: Multiplayer Using Godot's MultiplayerPeer Nodes
==}

The GodotSteam MultiplayerPeer version allows you to use Godot 4's multiplayer nodes, like synchronizers and spawners, with Steam's networking classes.  They can still be used in singleplayer games but is a bit extra overhead.

As we mentioned earlier, you can also use [ExpressoBits' SteamMultiplayerPeer GDExtension](https://github.com/expressobits/steam-multiplayer-peer){ target="\_blank" } too.  It is a completely separate project from GodotSteam and uses Steam's Networking Sockets class.  You may want to read up about it on their Github repository.

<div class="grid cards triple-grid" markdown>

- :simple-godotengine:{ .lg .middle } __GodotSteam MultiplayerPeer__

	---
	
	<span class="badge-group">
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">4.4</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.61</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .steam title="GodotSteam MultiplayerPeer" }</span>
			<span class="sep">|</span>
			<span class="value">4.13</span>
			<span class="sep">]</span>
		</span>
	</span>

	[:octicons-arrow-right-24: Download Pre-Compiled Bundle](https://github.com/GodotSteam/MultiPlayerPeer/releases/tag/v4.13-mp){ target="\_blank" }

- :simple-godotengine:{ .lg .middle } __ExpressoBits SteamMultiplayerPeer__

	---

	<span class="badge-group">
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">4.4</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:material-network:{ .steam title="ExpressoBits SMP" }</span>
			<span class="sep">|</span>
			<span class="value">0.2.2</span>
			<span class="sep">]</span>
		</span>
	</span>

	[ :octicons-arrow-right-24: Download GDExtension](https://godotengine.org/asset-library/asset/2258){ target="\_blank" }

</div>

{==
## :octicons-server-24: Master or Dedicated Servers
==}

The GodotSteam Server version is good if you want to build a centralized master server or dedicated servers that your players can run themselves for others to connect to.
  
<div class="grid cards triple-grid" markdown>

- :simple-godotengine:{ .lg .middle } __Godot 4.x__

	---
	
	<span class="badge-group">
		<span class="badge badge-normal tag-godot">
		<span class="sep">[</span>
		<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
		<span class="sep">|</span>
		<span class="value">4.4</span>
		<span class="sep">]</span>
	</span>
	<span class="badge badge-normal tag-steam">
		<span class="sep">[</span>
		<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
		<span class="sep">|</span>
		<span class="value">1.61</span>
		<span class="sep">]</span>
	</span>
	<span class="badge badge-normal tag-steam">
		<span class="sep">[</span>
		<span class="title">:simple-godotengine:{ .steam title="GodotSteam Server" }</span>
		<span class="sep">|</span>
		<span class="value">4.5.2</span>
		<span class="sep">]</span>
	</span>

	[:octicons-arrow-right-24: Download Pre-Compiled Bundle](https://github.com/GodotSteam/GodotSteam-Server/releases/tag/v4.5.2){ target="\_blank" }

	[:octicons-arrow-right-24: Download GDExtension](https://godotengine.org/asset-library/asset/2218){ target="\_blank" }

- :simple-godotengine:{ .lg .middle } __Godot 3.x__

	---
	
	<span class="badge-group">
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">3.6</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.61</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:octicons-server-24:{ .steam title="GodotSteam" }</span>
			<span class="sep">|</span>
			<span class="value">3.5.1</span>
			<span class="sep">]</span>
		</span>
	</span>

	[:octicons-arrow-right-24: Download Pre-Compiled Bundle](https://github.com/GodotSteam/GodotSteam-Server/releases/tag/v3.5.1){ target="\_blank" }

	[:octicons-arrow-right-24: Download GDNative (Deprecated)](https://godotengine.org/asset-library/asset/2222){ target="\_blank" }
</div>

{==
## :simple-dotnet: C#, In General
==}

You are making a singleplayer or multiplayer game with only Steam's networking classes or also with Godot's multiplayer nodes in C#. You also might be checking things out with C#.

Sadly, there are currently no .NET-enabled versions of GodotSteam yet. However, you can use LauraWebdev's C# bindings with any version of standard version of GodotSteam.

If you would like to help resolve this issue, we are trying to create Github Action processes to reliably build pre-compiles and templates. [Feel free to check out our existing workflows to edit in C# versions by pull request.](https://github.com/GodotSteam/GodotSteam/){ target="\_blank" }

  ---

<div class="grid cards triple-grid" markdown>

- :fontawesome-brands-github:{ .lg .middle } __LauraWebdev__

	---

	C# Bindings for GodotSteam GDExtension.

	[:octicons-arrow-right-24: View Their Project](https://github.com/LauraWebdev/GodotSteam_CSharpBindings){ target="\_blank" }

- :octicons-tools-24:{ .lg .middle } __ChickenSoft__

	---

	Open source tools for Godot and C#.

 	[:octicons-arrow-right-24: View Their Website](https://chickensoft.games/){ target="\_blank" }
</div>