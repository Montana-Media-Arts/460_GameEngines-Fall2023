---
title: Create Collectibles
module: 11
jotted: false
---

# Collectibles

## Create Object

Let's get started creating objects for the player to collect. 

In the Hierarchy, select Add, 3D Object, Cube. 

Rename the cube PickUp. 

In the transform component, use the vertical ellipsis to reset the GameObject's transform to origin. 

Press F to focus the scene view camera on the pickup cube. 

The player GameObject is currently overlapping the new cube so select the move tool and drag pickup out of the way while you work on its functionality. 

The cube is currently buried in the plane, just like the player sphere was when you started this project. 

Remember that? 

The cube is also a regular shape, one by one by one. 

So let's lift it up by half a unit so that it rests on top of the plane. 

In the transform component for the pickup GameObject, set the position y-value to 0.5. 

This cube will be the collectible object in the game. 

To be effective, it needs to attract the attention of the player so let's make the cube visually interesting. 

How about making it smaller? 

In the transform component, set each of the scale axis values to 0.5. 

This will give it the effect of floating above the play area. 

That will help identify this object as special. 

But perhaps that's not enough. 

Let's tilt it too. 

Set each of the rotation axis values to 45. 

The cube also needs to stand out more against the background walls and the player GameObject. 

Let's change its color by creating another material. 

In the Project window, find and select your first material, background. 

In the top menu, go to Edit > Duplicate. 

Rename the new material PickUp. 

With the pickup material still selected in the Project window, use the color picker in the Inspector to change its base map color property. 

Let's make it yellow and use the RGB values 255, 200, and zero. 

Now you can change the color of the pickup GameObject by changing its material on the Mesh Renderer component or by dragging the material from the Project window onto the cube in the Scene view. 

Now it's starting to look more like a pickup, but it's still very static. 

One thing that attracts attention of a user is movement. 

## Rotate Object

Okay, let's get that cube rotating. 

As you've done before, select the PickUp GameObject in the hierarchy and then select Add Component, then, New script in the Inspector. 

Call the new script Rotator, then, select Create and Add to confirm. 

In the Project window, move the Rotator script to your scripts folder to keep things organized, and then, open the script to edit it. 

Once it's opened in your script editor, remove the Start function from the template. 

You won't be using forces to rotate the cube, so you can use Update rather than using the Fixed Update function. 

So the rest is fine. 

Now, before you begin, let's think for a moment about what this script needs to do. 

It needs to make the PickUp GameObject spin while the game is active. 

You have already set up the PickUp GameObject's transform rotation property to 45, 45 and 45 for the X, Y and Z axes. 

But these values don't change by themselves, and for the cube to spin, these values need to change every frame. 

To do this, you need to write a script that rotates the GameObject's transform. 

There are two main ways to affect the transform of a GameObject. 

These are Translate and Rotate. 

Translate moves the GameObject by its transform. 

Rotate rotates the GameObject by its transform. 

It has two possible initial parameters, one using a Vector3 variable and the other using three float values for X, Y and Z. 

The simplest option is the Vector3 parameter, which is all that's needed for this game. 

After the opening curly brace of the update function, add the following line of code. transform .Rotate, ( new Vector3, open brackets again, 15 comma 30 comma 45 then two closed brackets, semicolon. 

Make sure that transform is a lowercase T, because you're referring to the component not the variable type. 

This action also needs to be smooth and frame rate independent. 

To do this, multiply the Vector3 value by time.deltaTime, before the closing parenthesis. 
deltaTime is perfect for ensuring actions happen smoothly, because deltaTime is a float representing the difference in seconds since the last frame update occurred. 

It will dynamically change its value based on the length of a frame. 

Okay, that's all you need. 

Save this script and return to Unity. 

Let's enter play mode and test the script. 

Excellent, the PickUp GameObject rotates. 

Exit play mode when you're done testing. 

Remember to save your Unity project frequently. 

Now's a great time to do that. 

Now you've made a beautifully rotating collective object, but there's only one. 

In the next video, you'll learn how to turn the PickUp GameObject into a prefab and add lots of collectable objects to the play area.

Rotator Script

```csharp
using UnityEngine;
using System.Collections;

public class Rotator : MonoBehaviour {

	// Before rendering each frame..
	void Update () 
	{
		// Rotate the game object that this script is attached to by 15 in the X axis,
		// 30 in the Y axis and 45 in the Z axis, multiplied by deltaTime in order to make it per second
		// rather than per frame.
		transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
	}
}
```

## Pickup Prefab

Now you've created one rotating pickup collectible, it's time to place them around the play area. 

There's one vital step needed to do this. 

You need to make the pickup GameObject into a prefab. 

A prefab is an asset that contains a template, or a blueprint, of a GameObject or GameObject's family. 

Once a GameObject is turned into a prefab, you can use the prefab in any scene in your current project. 

By turning the pickup GameObject into a prefab, you will be able to make changes to a single instance of the prefab or to the prefab asset itself. 

And all of the pickup GameObjects in the game will be updated with those changes. 

So let's get started. 

First, go into the project window and check that you're at the root, or top level, of the project. 

Make sure that no other item or directory is highlighted. Select Create > Folder, and rename this folder Prefabs. 

Now, select the pickup GameObject in the Hierarchy and drag it into the Prefabs folder. 

When you drag an item from the Hierarchy into the Project window, Unity creates a new prefab asset containing a template, or a blueprint, of the GameObject, or GameObject family. 

Your GameObject will turn blue in the Hierarchy when it becomes a prefab instance. 

Select the arrow to the right of the pickup prefab in the Hierarchy to open the Prefab Edit mode. 

From this mode, you can use the arrow at the top of the Hierarchy to return to a normal scene view. 

Now you've turned the pickup GameObject into a prefab. 

## Add more collectibles

Okay, let's fill the play area with rotating pickup objects. 
Before you start, let's get things organized. 

In the Hierarchy, create a new GameObject and call it pickup parent. 

In the Inspector, right-click the Transform component title and select Reset to reset pickup parent to origin. 

Then, in the Hierarchy, drag the pickup GameObject onto pickup parent. 

Next, make sure that you have the child pickup GameObject selected in Hierarchy, not the parent. 

Select the gizmo in the upper right of the scene view to switch to a top down view. 

Now use your scroll wheel to zoom out a little so you can see the entire play area. 

Okay, let's start placing pickup objects. 

Move the first pickup GameObject somewhere you like. 

Next, let's duplicate the GameObject. 

Make sure it's selected, and then go to Edit > Duplicate. 

You can use the hot keys control-D for Windows or command-D for Mac OS. 

Now place the second instance of the prefab. 

Using the hot keys, create some more pickup GameObjects and place them around the play area. 

About 12 is a good number for this game. 

To make this easier, you can move parallel to the ground or XZ plane by selecting the XZ plane at the center of the gizmo. 

Okay, that's 12. 

At the moment, the collectible objects are present in the game. 

But the player can't actually collect them. 

That functionality is the next thing you're going to add.

What if you want to generate them at run time? (Instatiate)