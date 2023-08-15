---
title: World Design
module: 7
jotted: true
---

# World Design

### Introduction

Again, some of this is review (I hope!)
So far, you’ve seen that to display something on screen you need to add a Sprite renderer to a GameObject. Unity will then render it where the GameObject is. 
This is great for single moving objects like your character, but the world in which the character is moving can be very big — drawing the full world would be a lot of work. For games where you’ll want to change the world based on gameplay tests, you may need to redraw assets a lot.

**Tilemaps** can be used to fix this problem! A Tilemap makes the world a grid in which you can set a different Sprite in each grid cell. By using Sprites that can be visually connected together, you can create a bigger image that is easy to change directly inside the Editor.

### Create a Tilemap
Let’s start by creating a Tilemap:

1. In the Hierarchy window, right-click on an empty space. 
2. Select 2D Object > Tilemap from the context menu.

This will create two GameObjects in your Hierarchy window:

1. **Grid:** A Grid that is, just as the name implies, a grid in your Scene that can be used to place GameObjects evenly inside its cells.
2. **Tilemap:** A Tilemap, which is a child GameObject of the grid. The Tilemap is composed of Tiles — for the purpose of this tutorial, think of these like special Sprites.

### Create a New Tile

The Tilemap doesn’t directly use Sprites, it uses Tiles. To create a new Tile:

1. In the Project window, go to Assets > Art.
2. Right-click within the folder and select Create > Folder. Call your new folder "Tiles".
3. Double-click Tiles to open the folder.
4. Right-click within the folder and select Create > Tile. In the dialog box, call this Tile “FirstTile” and save it.
5. In the Project window, select FirstTile.
6. In the Inspector, you can see the properties of the Tile Asset. These include a slot for a Sprite, which the Tile will draw:

### Assign a Sprite to FirstTile

To assign a Sprite to your Tile:

1. Just as you did with Ruby.png, save this Tile inside your Sprites folder:
2. Make sure that the FirstTile Tile is still selected in the Project window.
3. In the Inspector, click the circle select button to the right of the Sprite property. This will open a dialog window, where all the Sprites of your project appear. Select the one you just saved in the Project.

Alternatively, you can drag and drop the Sprite you just saved from the Project folder to the Sprite property slot in the Inspector.

Now that you have the Tile ready, you will need a way to tell the Tilemap which cell of the grid contains that specific Tile. To do this, you’ll need to paint the Tilemap.

### Add FirstTile to your Palette

To define which Tile is rendered in which cell of the Tilemap, Unity uses a Palette. Your Tilemap is a Canvas, and Tiles are your colors. You can put the Tiles on the Palette, so that you can pick them and apply them to the Tilemap.

To add FirstTile to the Palette:

1. Go to Window > 2D > Tile palette. This will open the Tile Palette window:
 
The center of the window is empty — that’s because you don’t have a Palette yet. 

2. Select Create New Palette. A window will appear so that you can set up the Palette.

Give the new Palette a name, like GamePalette, and then click Create. 

3. Save the new Palette in the Tile folder (which you created earlier in this tutorial).
4. Drag your new Tile (FirstTile) from the Project window to the centre of the new Palette, where it tells you to.
5. Your Tile is now displayed in the grid of the Palette. Click on the Tile to select it.
6. From the toolbar at the top of the Tile Palette, select the Brush Tool. 

Now you can paint on the Grid inside your Scene view:

But… horror! Your Tiles aren’t connected! There are huge gaps between them! Why is that?

## Fit your Tile Sprite to the Grid

Let’s investigate what’s wrong:

1. In the Hierarchy window, select the Grid GameObject. In the Inspector, find the Cell Size property. You’ll see that x and y are both set to 1:

That means each cell is 1 unit in width and 1 unit in height.

2. In the Project window, select the Tile Sprite. The Inspector will show its Import Settings. You’ll see that the Pixels Per Unit property is set to 100.

"Pixels per Unit" tells Unity how to size the Sprite, by defining how many pixels should fit inside 1 unit. In this case, it's 100 pixels per unit. 

3. At the bottom of the Inspector, look at the size of the Sprite. You’ll see that it’s only 64 pixels in width and height.

Because it’s smaller than 100 pixels, it will be smaller than 1 unit, so it won't fill the grid cell that has 1 unit sides.

4. Change the Pixel Per Unit value to 64. This tells Unity to fit the 64 pixels of that Sprite inside 1 unit in the Scene. Since your Sprite is 64 pixels, it will then perfectly fill 1 unit. 
5. Click Apply at the bottom of the Inspector after changing the Pixel Per Unit (PPU) value. All your Sprites should now perfectly fill the grid!

### What is a Tileset?

You now know:

1. What a Tile is
2. How it relates to Sprites and Tilemaps
3. How to paint a Tilemap using the Tile Palette.

But building an interesting world requires lots of different Tiles. Remember the example grid in the introduction at the beginning of this tutorial? It uses 9 different Tiles. Importing all those as tiny image files can be cumbersome and time consuming.

To make it easier to create Tiles, and for technical optimisation, Sprites used for Tiles usually come as a single image file called a Tileset. 

For example, the following image contains nine Tiles on a 3x3 grid which Unity can then split into 9 different Sprites in the Import Settings:

### Adjust a Tileset

The Tutorial Resources you downloaded at the beginning of this Project contains all the Tilesets you need for the game. 

Let's adjust one of these:

1. In the Project window, go to Art > Sprites > Environment. All files with names starting with “Floor” are Tilesets. 
2. Select the Tileset called FloorBricksToGrassCorner. Just as you did in the previous tutorial, click on the small arrow to see which Sprite was created from the image.
 
At the moment, there is a single Sprite that is the full image. Instead, you need to tell Unity that this image actually contains nine different Sprites. 

3. In the Inspector, change the Sprite Mode from Single to Multiple using the drop-down menu.
4. Change the Pixel Per Unit value from 100 to 64 (because these Tiles are 64 pixels wide, just like the previous example. 
5. Click the Apply button to apply your changes.

### Adjust the Tileset’s Sprite Settings

Now you need to adjust which part of the image is imported as a Sprite. You could select this manually nine times, but that’s quite tedious — there’s a simpler way to do this.

To split the image into 9 Sprites:

1. In the **Inspector**, click the **Sprite Editor** button. This opens a window where you can tweak which part of the image is imported as a Sprite.
2. In the menu bar at the top of the window, click **Slice**. A window will appear where you can specify how to slice up the image.
3. Set the **Type field** to **Grid by Cell Count** using the drop-down menu.
4. Set the Column & Row values (both C and R) to 3.
5. Click **Slice**. A grid will appear on your image; each cell of that grid is a separate Sprite.
6. In the top right corner of the window menu bar, click Apply.  This will save the new way your image is spliced into different Sprites
7. Close the window. If you try to close the window without clicking Apply, a warning dialog box will display to ask whether you want to apply your changes.
Now if you look at your Project window and click on the small arrow to see the Sprites created from this image, there will be nine of them!

### Assign your Sprites to Tiles
You could manually create all your Tiles and assign those Sprites one by one, like you did before. But Unity also offers a shortcut for that!

To assign your Sprites to Tiles:

1. Drag and drop the whole image from the Project window into the Palette window.
2. When you release the mouse button a dialog box will appear, asking you to select the folder where your Tiles will be saved. Choose the Tile folder.

The Tiles are now created and part of your Palette! You will be able to select them and paint them, just like you did with the previous one. 

### Adjust the Other Tilesets

Before you begin to paint your world:

1. Repeat the process in the previous three steps for all Assets with filenames beginning with "Floor" (which are all Tilesets of 3x3 Sprites). 
2. Now you’re ready to paint an interesting world to your heart’s content! 

### Paint your Tilemap

1. The Tile Palette toolbar 
2. There are two useful tools you can use to paint with the Tilemap Palette: the Tile Palette Toolbar and other shortcuts.

#### Toolbar

There are a number of useful tools within this toolbar. 

#### Keyboard and mouse shortcuts

There are also a few useful shortcuts which will help you to paint your Tilemap:
1. Alt + left button drag — pan
2. Wheel button drag — pan
3. Rotate the wheel button — zoom in or out


### Paint Your Tilemap
 
You're ready to paint your world! 

For now, you don’t need to paint too far outside the white rectangle in the Scene view. That rectangle shows the area your camera sees, which is what you can see in your Game view. In a later tutorial, you’ll learn how to move the camera.

**TIP:** Remember, the Fill Box Tool can help you paint your world quickly:

When you’ve finished painting your Tilemap:

1. Save your changes to the Scene.
2. Click Play to enter Play mode and admire your character moving through that world you’ve painted!

If you see your character, great! If not, don't despair! 

This is because the Tilemap and the character have the same depth (the z coordinate of their position), which is set to 0. If Unity draws your character after it has drawn the Tilemap, you will see Ruby because she will appear on top of Tilemap. But if Ruby is drawn first, drawing the Tilemap will erase her. 

To avoid the Tilemap erasing Ruby, you could change the z position. But for a purely 2D game, it's better to avoid touching the depth. 

To make Ruby appear, you need to tell Unity which order in which it needs to draw things. Unity needs to draw the Tilemap first, so that Ruby appears on top of background. 

### Change the Order in Layer for your Tilemap

To change the order in which Unity draws the Tilemap and your character:

1. In the Hierarchy, select the Tilemap GameObject.
2. In the Inspector, find the Tilemap Renderer component.
3. Find the Order in Layer field. This defines the order in which GameObjects in the same layer are drawn, with higher numbers going last. In this case, all your GameObjects are in the Sorting Layer default. 
4. Set the Order in Layer field to -10. This will ensure that the Tilemap is drawn before any GameObject. Since your character is at Order in Layer 0 (the default), it will now be drawn after the Tilemap and reappear.
5. Save your changes to the Scene. 

### Summary
In this section, you’ve learned how to:
1. Paint on Tilemap with Tiles
2. Create Tiles from Sprites
3. Create multiple Sprites in a single image file

In the next section, you will add more GameObjects to your world. You'll also talk more Prefabs!