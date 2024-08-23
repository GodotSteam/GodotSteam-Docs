# Skillet Workshop Editor
This editor lets you upload and edit Steam Workshop items, like cosmetics and skins, for [our open-source game Skillet.](https://store.steampowered.com/app/3013040/Skillet/){ target="\_blank" }

Just like the game, the editor is open-source which means you can submit new patches and features if you want.  You can also reskin or alter the code then ship an editor along with your game or put the functionality in your game.

Skillet Workshop Editor is available on Steam with the full game. It can also be manually installed by running `steam://install/3143450`

Tutorials on how the editor works are coming soon!

{==
## Making Workshop Items
==}

[You can find templates for new skins here.](https://github.com/GodotSteam/Skillet/tree/assets/templates){ target="\_blank" } Follow the layout to match your skins to what the game expects:

- For bodies, each state has two frame that it animates between for the "wiggly" effect.
- Faces only have one frame, with the exception of Dash.
- Cosmetics are like bodies in that they have different states and two frames each.

Items can be officially added to the game but we aren't ready for that just yet!

{==
## Current Build
==}

[You can see the full history of changes here.](../../changelog/skillet_editor.md){ target="\_blank" }

**Version 1.0.1**

- Changed: export settings for various platforms
- Fixed: issues with web links not opening