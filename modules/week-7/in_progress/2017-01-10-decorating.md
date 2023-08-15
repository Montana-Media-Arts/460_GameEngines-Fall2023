---
title: Decorating
module: 7
jotted: true
---

# Decorating

## Add Decorations

First, add some decorations to the world that you’ve created:  

1. In the Project window, go to Assets > Art > Sprites > Environment. Select the MetalCube Sprite.
2. Drag MetalCube to the Hierarchy window to add it to your Scene.
3. Use the Move Tool (shortcut: w) to place MetalCube where you’d like it in the Scene.
4. Press Ctrl + S (Windows/Linux) or Cmd + S (macOS) to save your changes.
5. Click Play to enter Play Mode, and move your character around the cube!

At the moment, Ruby goes through the metal cube without colliding with it. She is also either always on top of the cube or behind it, depending on which GameObject was drawn first.

You'll add collisions in the next tutorial — for now, let’s fix the GameObject ordering problem. 


## How can you fix the ordering problem?

You previously fixed an ordering issue with the Tilemap by adjusting its Order in Layer, but that’s not appropriate here: you don’t always want one GameObject on top of another. You need to “fake” the perspective. Instinctively, players expect the character to draw first when it is in front of the cube and last when it is behind the cube.

In more technical terms, what you need to do is instruct Unity to draw GameObjects depending on their y coordinate (remember, y is the vertical axis and x is the horizontal one). 

A GameObject that is lower on the screen (with a small y coordinate) should be drawn after a GameObject higher on the screen (with a larger y coordinate). This will enable it to appear on top. 


### Change the Graphics Settings

To instruct Unity to draw GameObjects depending on their y coordinate:
1. Go to Edit > Project Settings.
2. In the left-hand category menu, click Graphics.
3. In the Camera Settings, find the Transparency Sort Mode field. This decides the order in which Sprites are drawn. Change this from Default to Custom Axis, using the drop-down menu.
4. Add the following Transparency Sort Axis coordinates: 

x = 0
y = 1
z = 0

This tells Unity to draw Sprites based on their position on the y-axis.

5. Close the Project Settings window and save your changes.
6. Press Play to enter Play Mode and test your changes. Now your character should be drawn behind the box when it’s higher than it, and in front of the box when it’s lower.

This is a start, but it’s not perfect — the moment where Ruby is drawn behind the box instead of in front looks like it is happening too early. To fix this, you’ll need to adjust the Sprite Renderer component.

### Adjusting the Sprite Settings

Let’s look at Ruby’s Sprite Renderer:

1. In the Hierarchy, select the Ruby GameObject.
2. In the Inspector, find its Sprite Renderer component.
3. Find the Sprite Sort Point field. This is currently set to Center, which means that it uses the center point of the Sprite to decide whether it should be in front or behind another GameObject.
4. Change the Sprite Sort Point to Pivot.

### Adjust a Single Sprite Pivot

A Pivot is a special point that you can manually define, which acts as the "anchor" for the Sprite. If you rotate the Sprite, it will rotate around that point. It is also the point used to place the Sprite. This means that if a GameObject is at (0,0) and the Pivot is set to the head of the character, the head will be drawn at (0,0). If the pivot is on the feet, the feet will be drawn at (0,0).

You need to change the Pivots of both the Ruby and MetalCube Sprites, so they are placed where you want them.

To change a single Sprite Pivot:

1. In the Project window, go to Assets > Art > Sprites > Environment. Select the MetalCube Sprite.
2. In the Inspector, find the Pivot field. Set this to Bottom using the drop-down menu.
3. Click the Apply button at the bottom of the Inspector.

This places the Pivot at the bottom of the Sprite, instead of the middle where it was before.

### Change Pivots Using the Sprite Editor

The process you just followed only works for a single Sprite — you wouldn’t be able to change the Pivot on a Tileset, for example.

The other way to change a Pivot is to use the Sprite Editor:

1. In the Project window, go to Assets > Art > Sprites. Select the Ruby Sprite.
2. In the Inspector, click the Sprite Editor button. This will open the Sprite Editor you used in the previous tutorial. 
3. Click on the image, to display the Sprite border. In the center, you should see a little blue circle. This is the current position of the Pivot.

The Sprite Editor allows you to drag and drop that blue circle wherever you want in the Sprite.

Alternatively, in the grey Sprite window you can:

Set in the Pivot drop down to Center.

Set the Pivot to Custom and set the Custom Pivot manually.
4. Set the Custom Pivot to 0.5 in x and 0 in y. Since the Pivot Unit Mode is set to Normalized, 0 is minimum (so left for x and bottom to y), 1 is maximum (right for x, top for y), and 0.5 is middle.
5. Click Apply to save your changes.
6. Click Play to enter Play mode and try the game. Everything should display properly now!

### What is a Prefab?

You've now changed a lot of settings, and you would need to do that for every new metal box Sprite we put in the Scene. 

You could avoid doing that by copying and pasting the metal box in the Hierarchy, to duplicate it in your Scene. But that would cause two problems:

If you realise that you want to make a change (for example, reverting to use the center to sort), you would need to change that setting for each individual GameObject you have duplicated. That could really add up!

If you want to reuse the box with those settings in another Scene, you would need to reopen the Scene that contains it to copy it every time. 

To make it easy to change settings across multiple GameObjects, and to avoid the above issues, Unity uses Prefabs. A Prefab makes a GameObject with all its components and settings into an Asset, just like your Sprites are. 

### Create a Prefab

To create a new Prefab:

1. In the Project window, go to the top level folder (Assets).
2. Create a new folder and call it “Prefabs”.
3. Drag the MetalBox GameObject from the Hierarchy to the new Prefab folder.
4. Now, just like a Sprite, you can drag it from the Project window into either the Hierarchy or Scene view to create a new GameObject with the exact same settings.
You can use this process to create Prefabs to reuse GameObjects in any Scene in your Project. Now, what about the settings? 

### Adjust Prefab settings

In the Hierarchy, you’ll see that the MetalBox GameObjects are now blue, with a blue cube next to them and a little arrow to the right:

That means those GameObjects come from Prefabs, and that they are “linked” to that Prefab — any changes you make to the Prefab will be applied to them.
Let’s start by changing the color of the Prefab:

1. In the Project window, double-click the MetalBox Prefab. Alternatively, select the Prefab and click Open Prefab in the Inspector. You will see that the Scene view and Hierarchy now only contains the metal box. This is Prefab Mode. 
2. In the Inspector, find the Color field in the SpriteRenderer component. Click on it and change it to something else, like red.
3. Now you should see that at the top right of the Scene view, the save button isn’t greyed out anymore. That means you have changed your Prefab and need to save the changes, so click the Save button.

**Note:**  The Auto Save button is turned on by default. This will ensure you automatically save any changes performed and if that functionality isn't enabled you have to manually save as instructed above. 
 
4. In the Scene view, click the Scenes breadcrumb to return back to the Scene you were editing. Alternatively, you can click the arrow in the Hierarchy to take you back along the breadcrumb. 
5. Admire the fact that all the boxes in the Scene are now red!
This is how easy it is to use Prefabs! You will be using them a lot, because they allow you to go back and change something on a GameObject without having to manually change all the instances of that GameObjects placed in a Scene. 

In the next section, for example, you will add collision to the box Prefab so that any box in the Scene collides with the character.

### Creative Time

Now you can add your own decorations to the game! Inside the folder Art > Sprites > Environment you’ll find lots of items, such as houses, trees, and a DrainCover.

Import them like you did the box. Remember:

1. Move the Pivot to the bottom.
2. Adjust the PPU value to scale them as you want.
3. Create a Prefab from the GameObject, and then use that Prefab to place other instances in the Scene.
4. You can experiment with the Order in Layer settings of those items too, as you may want some items to always appear behind the character, such as the drain cover. 

### Summary
You've now learned how to change the drawing order of your GameObjects and how to create Prefabs from those GameObjects. 

Prefabs are a very important concept in Unity and you will be using them a lot from now on. Feel free to play around with creating and modifying some in the Scene before moving on to the next tutorial!

In the next section, you will learn how to stop your character from moving through a box or walking on water as using collision and the Physics System.