---
title: Rock Painter
module: 10
jotted: true
---

# Rock and Vegetation Painter

To further spruce up your scene, we have created a Rock Painter and a Vegetation Painter. These tools allow you to place vegetation and rocks with varying sizes and rotations while keeping them aligned with the surface you are painting on.
To use these tools;
In the Hierarchy locate VegetationPainter
Click on the arrow to expand its children

Click on GroundCover
Hover the mouse over the ground in the Scene View
Left click to place some grass/lilly pads where you desire.

You can change the type of object being placed to do this;
Navigate to the Inspector
In the Instance Painter component there are pictures of each prefab in the painter.

The selected prefab will be grey
Click on the white highlighted box to select that Prefab to paint with.
Controls for the painter can be found at the top of the Instance Painter Component, some controls to get your started:
Left Click : Paint
Ctrl (Cmd) + Left Click : Delete
Alt + Scroll : Increase Brush Size
Space : Randomize positions and rotation
Any object you paint on will be stored as a Child of the Parent. For example;
Navigate to the Hierarchy
Expand GroundCover

Every object of that type from the paint will be stored here as a child. You can also click on these to individually edit and place them.
The process is the same to paint VegetationSmall, VegetationMedium & VegetationLarge

Painting rocks is done in the same way;
In the Hierarchy find RockPainter
Click on the arrow to expand its children
Click on RocksSmall
Hover your mouse over the desired area to place rocks and Click to place.

These tools help you to quickly add variation in the way you decorate your level. These can be found and placed the traditional way of dragging in prefabs from the Prefabs folder.
Go to Project Window
Go to Assets > 3DGameKit > Prefabs > Environment
Here you can find folders for Rocks, VegetationSmall, VegetationMedium, VegetationLarge.

You can click and drag any of these into the Scene View to place them in your level. Please note that position, rotations and size will not be randomised when dragging objects in this way.
Continue to place objects in your scene. Don’t forget to occasionally test your level by pressing Play to see how things are looking in the game.
We’ve created a small environment to show you game mechanics in the following chapters. Have fun and get creative with your level building!




