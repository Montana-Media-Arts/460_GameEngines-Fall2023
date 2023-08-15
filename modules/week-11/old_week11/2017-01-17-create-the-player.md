---
title: Create the Player
module: 11
jotted: false
---

# Player

1. Now that you've created a playing field for your game, you're ready to create a **player GameObject**. 

2. For this game, that will be a **Unity Sphere Primitive**. 

3. To create the sphere, in the **Hierarchy**, select the **Create** menu, and then select **Sphere**. 

**Rename** the sphere **Player**. 

Reset the Transform to make sure it's at the origin point of the scene, 0,0,0. 

Either select Edit, Frame Selected in the top menu, or while the cursor is over the Scene view, press the F key on your keyboard. 

This will focus the Scene view camera on your new sphere GameObject. 

Do you see how the sphere is buried in the plane? 

This is because both GameObjects are in the exact same location in the scene. 

The origin point, or 0,0,0 on the x, y, and z axis. 

Now you need to move the sphere up until it rests on the plane so the player can use it as a playing field. 

All Unity primitive objects, cubes, spheres, capsules, have a standard size. 

They are either one by one by one or one by two by one Unity units of measurement. 

Unity units can be anything you want for your game. 

Centimeters, inches, et cetera, but default convention used by most people and by the Unity Lighting and Physics Systems is one unit is equal to one meter. 

If you raise the player object up by half a unit along the y-axis, it will be perfectly on top of the plane. 

To do this, in the Inspector, find the Transform component. 

Set the position y-value to 0.5. 

