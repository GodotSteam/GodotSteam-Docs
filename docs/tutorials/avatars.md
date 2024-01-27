# Tutorials - Avatars

You may want to get the current player's avatar, or someone else's, and display it in your game. This tutorial will walk you through the basics of avatar retrieval and passing it to a sprite so you can use it.

??? guide "Relevant GodotSteam classes and functions"
	* [Friends class](../classes/friends.md)
		* [getPlayerAvatar](../classes/friends.md#getplayeravatar)

{==
## Request the Avatar
==}

The default Steamworks functions to get player avatars requires multiple steps. However, GodotSteam has a unique function which makes getting the avatar data incredibly easy. Just use the following:

````gdscript
Steam.getPlayerAvatar()
````

This function has optional parameters. By default it will get medium-sized (64x64) avatar data for the current player. But you may pass it either `Steam.AVATAR_SMALL` (32x32), `Steam.AVATAR_MEDIUM` (64x64), or `Steam.AVATAR_LARGE` (128x128 or larger) to get different sizes; the numerical counterparts to these enums (1, 2, or 3) will also work. Additionally you can pass along a Steam ID64 to get avatar data for a specific user.

{==
## Retrieve and Create the Image
==}

To retrieve the avatar data buffer, you'll need to hook the signal for the callback:

=== "Godot 2.x, 3.x"

	```gdscript
	Steam.connect("avatar_loaded", self, "_on_loaded_avatar")
	```

=== "Godot 4.x"

	```gdscript
	Steam.avatar_loaded.connect(_on_loaded_avatar)
	```

This will return the user's Steam ID, the avatar's size, and the data buffer for rendering the image. If you have read the [Achievement Icons tutorial](achievement_icons.md), this should all look pretty familiar. Our `_on_loaded_avatar` function will look something like this:

=== "Godot 2.x, 3.x"

	```gdscript
	func _on_loaded_avatar(user_id: int, avatar_size: int, avatar_buffer: PoolByteArray) -> void:
		print("Avatar for user: %s" % user_id)
		print("Size: %s" % avatar_size)

		# Create the image for loading
		avatar_image = Image.new()
		avatar_image.create_from_data(avatar_size, avatar_size, false, Image.FORMAT_RGBA8, avatar_buffer)
		
		# Optionally resize the image if it is too large
		if avatar_size > 128:
			avatar_image.resize(128, 128, Image.INTERPOLATE_LANCZOS)

		# Apply the image to a texture
		var avatar_texture: ImageTexture = ImageTexture.new()
		avatar_texture.create_from_image(avatar_image)

		# Set the texture to a Sprite, TextureRect, etc.
		$Sprite.set_texture(avatar_texture)
	```

=== "Godot 4.x"

	```gdscript
	func _on_loaded_avatar(user_id: int, avatar_size: int, avatar_buffer: PackedByteArray) -> void:
		print("Avatar for user: %s" % user_id)
		print("Size: %s" % avatar_size)

		# Create the image and texture for loading
		var avatar_image: Image = Image.create_from_data(avatar_size, avatar_size, false, Image.FORMAT_RGBA8, avatar_buffer)

		# Optionally resize the image if it is too large
		if avatar_size > 128:
			avatar_image.resize(128, 128, Image.INTERPOLATE_LANCZOS)

		# Apply the image to a texture
		var avatar_texture: ImageTexture = ImageTexture.create_from_image(avatar_image)

		# Set the texture to a Sprite, TextureRect, etc.
		$Sprite.set_texture(avatar_texture)
	```

Naturally that texture could be applied elsewhere, depending on where you place this function.

That covers the basics of getting player avatars.

{==
## Additional Resources
==}

### Video Tutorials

Prefer video tutorials? Feast your eyes and ears!

[:simple-youtube:'Getting Your Avatar Picture and Processing It' by FinePointCGI](https://www.youtube.com/watch?v=VCwNxfYZ8Cw&t=731s){ .md-button .md-button--resource target="\_blank" }

[:simple-youtube:'How Do We Load Our Friends Avatar' by FinePointCGI](https://www.youtube.com/watch?v=VCwNxfYZ8Cw&t=6301s){ .md-button .md-button--resource target="\_blank" }

### Example Project

To see this tutorial in action, [check out our GodotSteam Example Project on GitHub](https://github.com/CoaguCo-Industries/GodotSteam-Example-Project){ target="\_blank" }. There you can get a full view of the code used which can serve as a starting point for you to branch out from.
