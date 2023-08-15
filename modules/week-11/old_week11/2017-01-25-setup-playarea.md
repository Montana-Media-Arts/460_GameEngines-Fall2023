---
title: Setup Play Area
module: 11
jotted: false
---

# Play Area

## Create a wall

Okay, let's set up the play field. 

The play field for your game will be very simple. 

You will place walls around the edges to keep the player GameObject from falling off. 

And you'll create and place a set of collectible objects for the player to pick up. 

First, let's get organized. 

In the Hierarchy, create a new empty GameObject, reset its transform, and then rename it Walls. 

This will be the parent GameObject for all of the wall GameObjects. 

Now you can build some walls. 

Let's start by creating a new cube to be the first wall. 

In the Hierarchy, select Create > 3D Object > Cube. 

Rename the cube GameObject, West Wall. 

In the Inspector, right click the Transform component title and select Reset. 

This resets the component to its default values. 

Now drag the West Wall GameObject onto the Walls GameObject and release. 

This makes West Wall the child of Walls. 

Let's focus the scene view camera to the West Wall object. 

To do this, press the F key while the cursor is over the scene view, or select Edit > Frame Selected. 

Next, you need to change the size of the cube to fit one side of the play area. 

In the Inspector, change the cube's Transform scale x value to 0.5, y value to two, and z value to 20.5. 

You can push the wall into place using the Translate tool, or you could enter a transform position value in the Inspector. 

Let's use the Inspector. 

Set the transform's position x value to minus 10. 

This places the wall neatly to the edge of the play area. 

Now just one more thing. 

Let's create a new material to change the color of the wall. 

In the Project window, go to the Materials folder and select Create > Material. 

Rename this new material Walls. 

In the Inspector, select the base map's color field to open a color picker. 

Set the RGB values to 79, 79, and 79. Next, set the metallic to zero and change the smoothness to 0.25 from matte finish. 

Then, drag the material from the Project window onto West Wall in the scene view. 

Great, now you've done one wall. 

## Finish Walls

To create the next wall, you could start with another new cube, but then you'd have to rescale the new cube before placing it. 

Your first wall is already a perfect size. 

Right-click the West Wall GameObject and select Duplicate. 

Rename the new GameObject East Wall. 

To place the wall, just remove the negative sign on the position x-value of the transform component. 

Then it will move into place on the east side of your game area. 

Now let's duplicate the East Wall and rename the duplicate North Wall. 

Reset the North Wall's position x-value to zero so it moves to the center of the play area. 

You now have two choices. 

You can either rotate the wall by 90 degrees around the y-axis or as this is a cuboid, you can scale the wall using 20.5 in the x-axis and 0.5 in the z-axis scale values. 

Now the GameObject is scaled correctly for its orientation as the North Wall. 

Awesome! 

You can drag the wall into place by hand or set the north wall's transform z-axis position to 10 to place it. 

Next, duplicate North Wall and rename it South Wall. 

Set the transform z-axis position to minus 10 and it will move into place too. 

First, save your changes. 

Then let's enter play mode and test. 

Fantastic! 

The walls are exactly where they need to be and working properly as boundaries of the play area. 

Remember, it's important to test early and often to catch errors as soon as possible. 

Let's exit play mode now. 

The walls are colliding with the player sphere because cube primitives come with a Box Collider component by default. 

This interacts with GameObjects with Rigidbody and Collider components. 

If you are encountering issues with wall collisions, you may have accidentally enabled the Is Trigger property on the collider component. 

This means the collider will be used to trigger events via script and won't be used for collisions. 

To fix this, disable the Is Trigger property checkbox. 

You've now completed the playing area for your roll-a-ball game. 

In the next part of this project, you'll create the pickup objects for the player to collect.

