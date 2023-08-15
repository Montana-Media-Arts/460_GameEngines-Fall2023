---
title: Add Moving Platform
module: 6
jotted: false
---

# Add Moving Platform


Before we begin manipulating GameObjects in the Scene View, make sure that you are not in paint mode. To do this, close the Tile Palette or select the Move Tool at the top left of the editor (you can also select the Move Tool by pressing W on your keyboard).

Now let’s add a moving platform to our level;

1. Navigate to the Project Window
2. Go to Prefabs > Interactables
3. Left click and drag MovingPlatform into the Scene View
4. Make sure the MovingPlatform is selected in the Hierarchy
5. Press W to use the Move Tool
6. Use the Move Tool on the MovingPlatform to put it wherever you want in your level.
7. When you select the MovingPlatform, a red dotted line appears, with a Move Tool gizmo at the end of it (the red arrow). This indicates the path the platform will take when the game is playing.

Use the Move Tool gizmo at the end of the red dotted line to indicate the path you would like the platform to take.

To preview the path the MovingPlatform will take, select the MovingPlatform and follow these steps:

1. Navigate to the Inspector
2. In the Moving Platform Component, find the Preview Position slider
3. Click and drag to scrub and see where the MovingPlatform will go.
4. Let’s make the path of this platform a little more complex by making it loop in a square. We do this by adding Nodes.
5. Nodes are additional navigation points for the Moving Platform Component.

In the Inspector locate the Moving Platform Component

1. Click Add Node twice
This adds an additional two red dotted lines and gizmos. Use the gizmos to position your extra Nodes into a square shape.

You’ll notice that there isn’t a path going from the last node back to beginning. If we press Play now, it will stop at the last point, go backwards on the path, and start again.
1. You can change the settings of the platform to make it form a loop

In the Moving Platform Component find the Looping option

1. Click the dropdown that says BACK_FORTH
2. Select Loop

This forms a loop in the gizmos, and the platform takes this path.

Note: The preview slider does not preview a complete loop, so you need to press Play to see it fully in action.

Every Component we have provided in the Kit has many options to customise the way they work. For more information on what every option does in the Moving Platform Component, check the Components Documentation and find the Moving Platform section.
