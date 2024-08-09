<link rel="stylesheet" href="assets/stylesheets/intro.css" />

# GodotSteam Documentation

Welcome!  If you find any mistakes or have some additions to make, [please check out our documentation repository here.](https://github.com/GodotSteam/GodotSteam-Docs){ target="\_blank" }

{==
## Where Do I Start?
==}


### In Most Cases

Most users will just want to [install the GDExtension or GDNative plug-in to quickly use the standard GodotSteam.](https://godotengine.org/asset-library/asset/2445){ target="\_blank" }

**To get started**:

- [x] Open your Godot editor
- [x] Click on the 'Assets' tab
- [x] Search for 'GodotSteam'
- [x] If using Godot 4.x, install the GDExtension 4.1 / 4.2 plug-in
- [ ] If using Godot 3.x, install the GDNative plug-in

You may need to restart your editor but the _plug-in does not need to be enabled_ manually.  Check by calling the **Steam** class.

**However, you might be trying to make something else.** If so, keep reading to find out about other versions of GodotSteam.

---

### What Are You Making?

Depending on what your end goals are, you will probably want to use a different version of GodotSteam. See which of the following best fits your project.

<div class="grid full cards" markdown>

- 	:material-gamepad-variant:{ .godotsteam .lg .middle } __Singleplayer or Steam Networking__

	You are making a singleplayer game or a multiplayer game that solely uses Steam's networking classes. You also might be just checking things out. You will want the standard GodotSteam version.
	
	---

	<span class="badge-group">
		<span class="badge badge-normal version">Godot 4</span>
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">4.2.2</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.60</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .steam title="GodotSteam" }</span>
			<span class="sep">|</span>
			<span class="value">4.9</span>
			<span class="sep">]</span>
		</span>
	</span>

	[ :fontawesome-brands-github: Pre-compiled Editor and Templates](https://github.com/GodotSteam/GodotSteam/releases/tag/v4.9){ .md-button .md-button--downloads target="\_blank" }

	[:octicons-plug-24: GDExtension | Godot Asset Library](https://godotengine.org/asset-library/asset/2445){ .md-button .md-button--downloads target="\_blank" }
 
 	---

 	<span class="badge-group">
 		<span class="badge badge-normal version">Godot 3</span>
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">3.5.3</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.60</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .steam title="GodotSteam" }</span>
			<span class="sep">|</span>
			<span class="value">3.26</span>
			<span class="sep">]</span>
		</span>
	</span>

	[ :fontawesome-brands-github: Pre-compiled Editors and Templates](https://github.com/GodotSteam/GodotSteam/releases/tag/v3.26){ .md-button .md-button--downloads target="\_blank" }

	[ :octicons-plug-24: GDNative | Godot Asset Library](https://godotengine.org/asset-library/asset/1045){ .md-button .md-button--downloads target="\_blank" }

- 	:fontawesome-solid-people-group:{ .godotsteam .lg .middle } __Multiplayer Using Godot's Nodes__

	You are making a multiplayer game using Godot 4's multiplayer nodes like synchronizer and spawner but you want to use Steam's networking with it. Sounds like you want the MultiplayerPeer version.

	---

	<span class="badge-group">
		<span class="badge badge-normal version">Godot 4</span>
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">4.2.2</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.60</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .steam title="GodotSteam MultiplayerPeer" }</span>
			<span class="sep">|</span>
			<span class="value">4.9</span>
			<span class="sep">]</span>
		</span>
	</span>

	[ :fontawesome-solid-people-group: MultiplayerPeer Editor and Templates](https://github.com/GodotSteam/GodotSteam/releases/tag/v4.9-mp){ .md-button .md-button--downloads target="\_blank" }

	---

	[<h3>ExpressoBit's SteamMultiplayerPeer</h3>](https://github.com/expressobits/steam-multiplayer-peer){ target="\_blank" }

	A separate project using Steam Sockets networking class with Godot's multiplayer nodes and available as a GDExtension.

	[ :octicons-plug-24: Expresso SteamMultiplayerPeer GDExtension](https://godotengine.org/asset-library/asset/2258){ .md-button .md-button--downloads target="\_blank" }

-	:simple-dotnet:{ .godotsteam .lg .middle } __C# In General__

	You are making a singleplayer or multiplayer game with only Steam's networking classes or also with Godot's multiplayer nodes in C#. You also might be checking things out with C#.

	Sadly, there are currently no .NET-enabled versions of GodotSteam yet. However, you can use LauraWebdev's C# bindings with any version of standard version of GodotSteam.

	If you would like to help resolve this issue, we are trying to create Github Action processes to reliably build pre-compiles and templates. [Feel free to check out our existing workflows to edit in C# versions by pull request.](https://github.com/GodotSteam/GodotSteam/){ target="\_blank" }

	---

	[ :fontawesome-brands-github: GodotSteam C# Bindings From LauraWebdev](https://github.com/LauraWebdev/GodotSteam_CSharpBindings){ .md-button .md-button--downloads target="\_blank" }

	[ :octicons-tools-24: ChickenSoft For C# Tools and Tips](https://chickensoft.games/){ .md-button .md-button--downloads target="\_blank" }

- 	:octicons-server-24:{ .godotsteam .lg .middle } __Master or Dedicated Server__
	
	You are making a master server that runs somewhere and players connected to it or a dedicated server that players can run themselves and other players connect to that. In that case, you will want to use the GodotSteam Server builds.

	---

	<span class="badge-group">
		<span class="badge badge-normal version">Godot 4</span>
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">4.2.2</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.60</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .steam title="GodotSteam" }</span>
			<span class="sep">|</span>
			<span class="value">4.9</span>
			<span class="sep">]</span>
		</span>
	</span>

	[ :octicons-server-24: Server Pre-Compiled Editors and Templates](https://github.com/GodotSteam/GodotSteam-Server/releases/tag/v4.3){ .md-button .md-button--downloads target="\_blank" }

	[ :octicons-plug-24: Server GDExtension](https://godotengine.org/asset-library/asset/2218){ .md-button .md-button--downloads target="\_blank" }

	---

	<span class="badge-group">
		<span class="badge badge-normal version">Godot 3</span>
		<span class="badge badge-normal tag-godot">
			<span class="sep">[</span>
			<span class="title">:simple-godotengine:{ .godotengine title="Godot Engine" }</span>
			<span class="sep">|</span>
			<span class="value">3.5.3</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:simple-steam:{ .steam title="Steamworks SDK" }</span>
			<span class="sep">|</span>
			<span class="value">1.59</span>
			<span class="sep">]</span>
		</span>
		<span class="badge badge-normal tag-steam">
			<span class="sep">[</span>
			<span class="title">:octicons-server-24:{ .steam title="GodotSteam" }</span>
			<span class="sep">|</span>
			<span class="value">3.3</span>
			<span class="sep">]</span>
		</span>
	</span>

	[ :octicons-server-24: Server Pre-compiled Editors and Templates](https://github.com/GodotSteam/GodotSteam-Server/releases/tag/v3.3){ .md-button .md-button--downloads target="\_blank" }

	[ :octicons-plug-24: Server GDNative | Godot Asset Library](https://godotengine.org/asset-library/asset/2222){ .md-button .md-button--downloads target="\_blank" }
</div>

---

### Differences Between Pre-Compiles and Plug-ings

For the most part, the pre-compiles and plug-ins are functionally the same. One major difference is the plug-ins have no in-editor documentation.

GDNative, however, has no access to Steam constants or enums, an issue with Rich Presence on Windows, others which are documented in the [Common Issues section.](issues/common_issues.md/#gdnative)

[Previous versions of all branches can be found on Github.](https://github.com/GodotSteam/GodotSteam/releases){ target="\_blank" }

Pre-compiled editors and template zips are self-contained and have everything you need to work with GodotSteam. The GDNative and GDExtension plug-ins require [an official version of Godot Engine.](https://godotengine.org){ target="\_blank" }

{==
## Compile It Yourself
==}

Roll up your sleeves and build your own versions of GodotSteam. More instructions are available on each branch's readme page or up above [under the ***Compiling*** tab.](howto/modules.md)

<div class="grid full cards" markdown>

- 	:simple-godotengine:{ .godotsteam .lg .middle } __Godot 4.x Branches__

	---

	[:fontawesome-solid-code-branch: Module](https://github.com/GodotSteam/GodotSteam/tree/godot4){ .md-button .md-button--downloads target="\_blank" }

	[:octicons-plug-24: GDExtension Plug-in](https://github.com/GodotSteam/GodotSteam/tree/gdextension){ .md-button .md-button--downloads target="\_blank" }

	[:fontawesome-solid-people-group: MultiplayerPeer Module](https://github.com/GodotSteam/GodotSteam/tree/multiplayer-peer){ .md-button .md-button--downloads target="\_blank" }


- 	:simple-godotengine:{ .godotsteam .lg .middle } __Server 4.x Branches__

	---

	[:fontawesome-solid-code-branch: Server Module](https://github.com/GodotSteam/GodotSteam-Server/tree/godot4){ .md-button .md-button--downloads target="\_blank" }

	[:octicons-plug-24: Server GDExtension](https://github.com/GodotSteam/GodotSteam-Server/tree/gdextension){ .md-button .md-button--downloads target="\_blank" }

- 	:simple-godotengine:{ .godotsteam .lg .middle } __Godot 3.x Branches__

	---

	[:fontawesome-solid-code-branch: Module](https://github.com/GodotSteam/GodotSteam/tree/godot3){ .md-button .md-button--downloads target="\_blank" }

	[:octicons-plug-24: GDNative Plug-in](https://github.com/GodotSteam/GodotSteam/tree/gdnative){ .md-button .md-button--downloads target="\_blank" }

- 	:simple-godotengine:{ .godotsteam .lg .middle } __Server 3.x Branches__

	---

	[:fontawesome-solid-code-branch: Server Module](https://github.com/GodotSteam/GodotSteam-Server/tree/godot3){ .md-button .md-button--downloads target="\_blank" }

	[::octicons-plug-24: GDNative Plug-in](https://github.com/GodotSteam/GodotSteam-Server/tree/gdnative){ .md-button .md-button--downloads target="\_blank" }

</div>

### Requirements

If you are compiling the module, GDNative, or GDExtension version yourself, there are a few things you'll need to start working regardless of which flavor you decide to try out.

<div class="grid full cards" markdown>

- :simple-steam: __Steamworks SDK__
	
	While we suggest the latest version of Valve's Steamworks SDK 1.59 or newer, the older versions work well too.

	---

	[:octicons-file-zip-24: Log In and Download The SDK](https://partner.steamgames.com/){ .md-button .md-button--downloads target="\_blank" }

- :simple-godotengine: __Godot Engine Source__

	You will need the version that matches your GodotSteam version. Use the tags section in Github.

	---

	[:fontawesome-solid-code-branch: Grab the Engine Source](https://github.com/godotengine/godot){ .md-button .md-button--downloads target="\_blank" }

</div>

Just make sure there are no compatibility breaks between the SDK and GodotSteam; usually noted in the readme's.

{==
## Quick Start
==}

<div class="grid full cards" markdown>

- :material-ray-start: __Tutorials__

	There currently is a wide selection of tutorials available with more in the works. Feel free to submit corrections or new ones  So far there are topics such as:

	---

	[:fontawesome-solid-book-bookmark: Initializing Steam](tutorials/initializing.md){ .md-button .md-button--downloads }

	[:fontawesome-solid-book-bookmark: Lobbies](tutorials/lobbies.md){ .md-button .md-button--downloads }

	[:fontawesome-solid-book-bookmark: Stats and Achievements](tutorials/stats_achievements.md){ .md-button .md-button--downloads }

	[:fontawesome-solid-arrow-right: See More Tutorials](tutorials/achievement_icons.md){ .md-button .md-button--downloads }

- :fontawesome-solid-book-bookmark: __External Resources__

	A list of things related to Godot and Steam like video and text tutorials, linked to other tools and plug-ins, and other Steam API projects.

	---

	[:fontawesome-solid-book-bookmark: External Resources](tutorials/external.md){ .md-button .md-button--downloads }

- :fontawesome-solid-book: __Compiling How-To Guides__

	If you want to build GdotoSteam from scratch, use one of these compiling how-to guides to continue on:

	---

	[:fontawesome-solid-book: Module How-To](howto/modules.md){ .md-button .md-button--downloads }

	[:fontawesome-solid-book: Multiplayer Peer How-To](howto/multiplayer_peer.md){ .md-button .md-button--downloads }

	[:fontawesome-solid-book: GDNative How-To](howto/gdnative.md){ .md-button .md-button--downloads }

	[:fontawesome-solid-book: GDExtension How-To](howto/gdextension.md){ .md-button .md-button--downloads }

	[:fontawesome-solid-book: Server How-To](howto/server.md){ .md-button .md-button--downloads }

- :material-folder-open: __GodotSteam Example Project__

	A working example of some GodotSteam features based on current tutorials.

	---

	=== "Godot 3.x"
			
		[:fontawesome-solid-code-branch: Example 3.x Project Source](https://github.com/GodotSteam/GodotSteam-Example-Project/tree/godot3){ .md-button .md-button--downloads target="\_blank" }

		[:simple-godotengine: Godot Asset Library](https://godotengine.org/asset-library/asset/1956){ .md-button .md-button--downloads target="\_blank" }

		[:fontawesome-solid-file-zipper: GitHub Drop-in ZIP](https://github.com/GodotSteam/GodotSteam-Example-Project/zipball/godot3){ .md-button .md-button--downloads target="\_blank" }

	=== "Godot 4.x"
		
		[:fontawesome-solid-code-branch: Example 4.x Project Source](https://github.com/GodotSteam/GodotSteam-Example-Project/tree/godot4){ .md-button .md-button--downloads target="\_blank" }
		
		[:simple-godotengine: Godot Asset Library](https://godotengine.org/asset-library/asset/1959){ .md-button .md-button--downloads target="\_blank" }
		
		[:fontawesome-solid-file-zipper: GitHub Drop-in ZIP](https://github.com/GodotSteam/GodotSteam-Example-Project/zipball/godot4){ .md-button .md-button--downloads target="\_blank" }

</div>

{==
## Have A Game Using GodotSteam?
==}

Finally got your Steam store page up? Whether you are about to release your game, already did, or are just tinkering away at it, you have your game added to the list of Games Using GodotSteam section.

<div class="grid full cards" markdown>

- :fontawesome-solid-gamepad: __E-Mail Your Game__

	Please include your Steam store page and up to five additional links like social platforms, Discord invite links, development website, devlog, or anything relevant to your game or studio.

	---

	[:material-email: Get To Typing](mailto:games@godotsteam.com){ .md-button .md-button--downloads target="\_blank" }

- :fontawesome-brands-github: __PR Submit Your Game__
	
	You can create your own entry in our documentation by submitting a pull request on Github. Make sure not to add any more than six additional links. You can view other entries to see how the submission should be formatted.

	---

	[:octicons-git-pull-request-24: Create A Pull Request](https://github.com/GodotSteam/GodotSteam-Docs/pulls){ .md-button .md-button--downloads target="\_blank" }

</div>

{==
## Need Support?
==}

<div class="grid full cards" markdown>

- :fontawesome-brands-github: __Github Issues__

	These should only be used for actual bugs in the project. Please direct questions to either an e-mail or Discord.
	
	--- 

	[:fontawesome-brands-github: Create An Issue](https://github.com/GodotSteam/GodotSteam/issues){ .md-button .md-button--downloads target="\_blank" }


-  :fontawesome-solid-paper-plane: __By E-Mail__

	Send me an e-mail with as many details as you can about your issue. Unless you are just saying hi.

	---

	[:fontawesome-solid-paper-plane: Send A Support E-Mail](mailto:support@godotsteam.com){ .md-button .md-button--downloads target="\_blank" }

- :material-account-group: __Community Support__
	
	A bunch of really smart folks can usually help out with issues; especially things like MultiplayerPeer.

	---

	[:fontawesome-brands-discord: Chat With Us On Discord](https://discord.gg/SJRSq6K){ .md-button .md-button--downloads target="\_blank" }

</div>


{==
## Contributing and Donating
==}

Want to help out? There are a few ways and all of them get you listed in our contributors section. Yes, even for tiny corrections.


<div class="grid full cards" markdown>

 - :octicons-git-pull-request-24: __Pull Requests__

 	A huge help is the contributions of fixes and additions through pull-requests on GitHub. 

 	---

 	[:material-github: All GodotSteam Repos](https://github.com/GodotSteam){ .md-button .md-button--downloads target="\_blank" }

 	[:material-list-box: Check Out Our To-Do List](https://github.com/orgs/GodotSteam/projects/3/views/1){ .md-button .md-button--downloads target="\_blank" }

 - :material-heart: __Donations__

 	You can provide donations for the project through Github Sponsors. One-time or reoccuring donors get a variety of perks like access to our sponsors-only repo, special Discord roles, and my undying love.

 	---

	[:material-heart: GitHub Sponsors](https://github.com/sponsors/Gramps){ .md-button .md-button--downloads target="\_blank" }

</div>