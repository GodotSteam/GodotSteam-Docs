# Contributing Code Or Docs To GodotSteam

<div class="link-grid" markdown>

[:octicons-code-square-16: Main Project](https://github.com/GodotSteam/GodotSteam/){ .md-button .md-button--primary }
[:material-server: Server](https://github.com/GodotSteam/GodotSteam-Server/){ .md-button .md-button--primary }
[:fontawesome-solid-book: Documentation](https://github.com/GodotSteam/GodotSteam-Docs/){ .md-button .md-button--primary }
[:material-file-document: Examples](https://github.com/GodotSteam/GodotSteam-Examples/){ .md-button .md-button--primary }

</div>

{==
### Modifying Code
==}

There is a convention we try to keep, which seems like a hodge-podge of styles:

- [x] Use camel-case for all function names. We mostly follow Valve's lead with this instead of Godot's, but we have discussed changing it.
- [x] Use snake-case for all signal names, variables, and arguments.
- [x] Make sure your code compiles before submitting a pull-request. Run a quick:

	=== "Godot 2.x, 3.x"

		```shell
		scons platform=[your platform] production=yes tools=yes target=release_debug
		```
	
	=== "Godot 4.x"

		```shell
		scons platform=[your platform] tools=yes target=editor
		```

We'll figure the rest out later. So far that's it!

{==
## Modifying Examples
==}

Add new examples or edit the existing ones. 

Please try to keep most of the standard Godot conventions, use snake_case, double linebreak between functions, etc.

Don't shy away from explaining things in the comments. Since these are meant for people to learn adn figure out how Steamworks functions, definitely pack in any information you think necessary.

{==
## Modifying Docs
==}

Feel free to make any edits you think are needed, really. CSS changes, new tutorials, extra links, fixes for functions or signals, or even adding your own game to the ["Games Using GodotSteam" list](/games/games).

Try to make any new or edited content fit the same styles as the other pages.  But if you have an eye for design and think of something better, please share that too.

---

Thank you for helping make the project more useful for the community!