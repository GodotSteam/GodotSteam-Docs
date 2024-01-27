# Tutorials - Achievement Icons

This quick tutorial will cover to how get achievement icons from Steam's servers. It was made because there are some extra steps folks need to use to render the images and it may not be very clear otherwise.

??? guide "Relevant GodotSteam classes and functions"
	* [User Stats class](../classes/user_stats.md)
		* [getAchievementIcon()](../classes/user_stats.md#getachievementicon)
		* [requestCurrentStats()](../classes/user_stats.md#requestcurrentstats)
	* [Utils class](../classes/utils.md)
		* [getImageRGBA()](../classes/utils.md#getimagergba)
		* [getImageSize()](../classes/utils.md#getimagesize)

{==
## Prerequisites
==}

Before you can get achievement icons you first need to have retrieved the user's stats from Steam. This is done by default when initializing Steam, but can optionally be disabled. This tutorial assumes that you have already received the user stats. See [requestCurrentStats()](../classes/user_stats.md#requestcurrentstats) for more information if needed.

{==
## Getting the Handle and Buffer
==}

First you will need to request the icon handle by using the achievement's API name you set in the Steamworks back-end:

```gdscript
var icon_handle: int = Steam.getAchievementIcon("ACHIEVEMENT_NAME")
```

This handle is an ID used to get information about the image's size and a byte array (buffer) of the pixel data:

```gdscript
var icon_size: Dictionary = Steam.getImageSize(icon_handle)
var icon_buffer: Dictionary = Steam.getImageRGBA(icon_handle)
```

Note that the `getImageRGBA()` function can be somewhat expensive so it is recommended to only call this once per image handle and cache the result if you will need to access the image data multiple times.

{==
## Creating the Image
==}

This buffer is a dictionary that contains two keys: `success` and `buffer` which contains the actual image data for our icon. However, as it is simply binary data, we need to load it into an Image so that Godot can use it as a Texture. The format of the data in the buffer is RGBA8 when we receive it from Steam, so we need to tell Godot how to make sense of it by specifying it as such:

=== "Godot 2.x, 3.x"

	```gdscript
	var icon_image: Image = Image.new()
	icon_image.create_from_data(icon_size.width, icon_size.height, false, Image.FORMAT_RGBA8, icon_buffer["buffer"])
	```

=== "Godot 4.x"

	```gdscript
	var icon_image: Image = Image.create_from_data(icon_size.width, icon_size.height, false, Image.FORMAT_RGBA8, icon_buffer["buffer"])
	```

The size of the image is determined by the images you uploaded when configuring achievements on the Steamworks back-end. Valve recommends larger (256x256) images. If you want to display the image at a different size, say 64x64, you can optionally resize it now. Check the [Godot documentation](https://docs.godotengine.org/en/stable/classes/class_image.html#class-image-method-resize) on `Image.resize()` for which interpolation mode best suits your needs.

```gdscript
icon_image.resize(64, 64, Image.INTERPOLATE_LANCZOS)
```

Now that all the pixels are set in the right places, we can create the actual texture which will be displayed:

=== "Godot 2.x, 3.x"

	```gdscript
	var icon_texture: ImageTexture = ImageTexture.new()
	icon_texture.create_from_image(icon_image)
	```

=== "Godot 4.x"

	```gdscript
	var icon_texture: ImageTexture = ImageTexture.create_from_image(icon_image)
	```

Finally we can display the icon. Using the previous `icon_texture`, we can place this icon on our waiting Sprite or TextureRect, etc.

```gdscript
$Sprite.texture = icon_texture
```
{==
## All-In-One
==}

Our complete example should look something like this:

=== "Godot 2.x, 3.x"

	```gdscript
	# Get the image's handle
	var icon_handle: int = Steam.getAchievementIcon("ACH_WIN_ONE_GAME")

	# Get the image data
	var icon_size: Dictionary = Steam.getImageSize(icon_handle)
	var icon_buffer: Dictionary = Steam.getImageRGBA(icon_handle)

	# Create the image for loading
	var icon_image: Image = Image.new()
	icon_image.create_from_data(icon_size.width, icon_size.height, false, Image.FORMAT_RGBA8, icon_buffer["buffer"])

	# Create a texture from the image
	var icon_texture: ImageTexture = ImageTexture.new()
	icon_texture.create_from_image(icon_image)

	# Display the texture on a sprite node
	$Sprite.texture = icon_texture
	```

=== "Godot 4.x"

	```gdscript
	# Get the image's handle
	var icon_handle: int = Steam.getAchievementIcon("ACH_WIN_ONE_GAME")

	# Get the image data
	var icon_size: Dictionary = Steam.getImageSize(icon_handle)
	var icon_buffer: Dictionary = Steam.getImageRGBA(icon_handle)

	# Create the image for loading
	var icon_image: Image = Image.create_from_data(icon_size.width, icon_size.height, false, Image.FORMAT_RGBA8, icon_buffer["buffer"])

	# Create a texture from the image
	var icon_texture: ImageTexture = ImageTexture.create_from_image(icon_image)
	
	# Display the texture on a sprite node
	$Sprite.texture = icon_texture
	```

And that's how you display an achievement icon.

{==
## Extra Resources
==}

### Example Project

To see this tutorial in action, [check out our GodotSteam Example Project on GitHub](https://github.com/CoaguCo-Industries/GodotSteam-Example-Project){ target="\_blank" }. There you can get a full view of the code used which can serve as a starting point for you to branch out from.
