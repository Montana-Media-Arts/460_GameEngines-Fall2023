---
title: Moving Platforms
module: 10
jotted: true
---

# A Deeper Look at Moving Platforms
We will take a look at adding various gameplay elements using the Kit. We have set up a demo level to show these, you can use your own scene you have built to follow along or use a new scene.
If you have used the 2D Game Kit, some of these techniques may be familiar although we have adjusted some elements to simplify workflow.
Moving platforms can be used to get your player from one place to another, let’s add one to our scene;
Navigate to the Project Window
Go to Assets > 3D GameKit > Prefabs > Interactables

The Interactables folder is where you can find premade gameplay elements created for this Kit.
Left Click and drag the MovingPlatform Prefab into the Scene View OR the Hierarchy
Note: if you move this into the Hierarchy don’t forget to use Frame Select to find it (Keyboard Shortcut - F)
You can also position, rotate and scale the object through the Inspector in the Transform Component

Tip: Hovering your mouse over X, Y or Z and Left Click drag left or right will increase and decrease these values respectively.
Position the whole object where you would like the platform to start from.
We have positioned ours above the Acid pool we created earlier.

Now let’s take a look at making the platform and setting its path, to change where the platform will move to.
Gizmos
The moving platform comes with 3 gizmos attached to allow you to easily alter the motion of the platform. In the screen, you can see that it allows you to set the Transform Position to place it in the world (1), Starting position (2) and Ending Position (4) with the distance (3) between presented on the dashed translation line.

In the Inspector window, find the Simple Translator (Script) and check the box for Activate to enable the movement of the MovingPlatform.

If you’ve moved the Start location during this process before checking Activate, you will notice that the Moving Platform snaps to the Start location as opposed to the initial position set in Transform. This is to ensure the platform always starts from the right place and translates according to behaviours set in the Simple Translator Script.
Tip: Holding the Ctrl (Cmd) key when moving a gizmo snaps it to the grid and increments movements, this can really help in getting straight paths.
Ellen needs to traverse the environment and avoid the acid pool, therefore the End Gizmo needs to be adjusted or it will just do the default Translate on the Z Axis which is not very helpful to the player.
To see the current translation motion of the Moving Platform in the Editor, you can move the slider for Preview Position in a range from 0 to 1. You will see in the Scene window that the platform moves to where it would be in the game at that stage of the translation.
This is a very useful feature to help you see any potential overlaps with other GameObjects in the scene or preview the behaviour.
Duration
To alter the speed of the sequence from Start to End, you can change the Duration value which, is set to 5 by default and is measured in Seconds. Making this value smaller will result in a faster sequence and larger results in a slower translation.
Setup Moving Platforms
Click on the Y handle (Green) on the End Gizmo, Press Ctrl (Cmd) to snap, then drag it down to your desired height.

In this example, we want it to be a straight translation downward. Drag the End Gizmo’s Z handle (Blue) and Snap (Ctrl/Cmd) drag the Gizmo to below the Start Point.

If you move the Preview Position slider now, the Moving Platform should move towards the new End Point position.
The last step for this example is to set the Loop Type to Ping Pong which is currently set to Once. Changing this will repeat the sequence from Start to End and back again.

Loop Type
Once - Linear - moves from Start to End and stops upon reaching End
The Once loop type is most commonly used when you only require the player to be transported from the Start to the End.
Ping Pong - Two Way Loop - moves from Start to End and then End to Start giving a continuous motion loop.
The Ping Pong loop type is most commonly used when the route the platform provides for the player should be continuously available.
Repeat - One Way Loop - moves from Start to End then returns to Start to repeat.
The Repeat loop type is most commonly used for single direction sequences where the platforms would spawn in from Start and move to End. This can be seen in Level 2’s acid pit of the 3D Game Kit (3D Game Kit > Scenes >Gameplay).

Game Commands with Pressure Pads
Game Commands enable GameObjects in your scene to communicate with other GameObjects by sending and receiving commands. This helps you design complex behaviours and scripts.
Game Commands are based around Events taking place and vary per object in the 3D Game Kit. In this example we will look at setting up a PressurePad Prefab that will activate the MovingPlatform Prefab added in the MovingPlatforms tutorial using Game Commands.
To prepare the MovingPlatform for interactions from the PressurePad, disable the Activate property.

Linking Interactable Objects

Go to the Project view and navigate to 3DGamekit > Prefabs > Interactables .
Drag and drop a PressurePad from the Project window into the Scene view near the MovingPlatform that you are going to couple the PressurePad with.

In the Inspector, the component that you will be editing is Send On Trigger Enter script.

This Script will allow you to hook into other GameObjects by using the Interactive Object selection, which links the Game Command Receiver to the relevant GameObject in the Hierarchy.
In this example, we will link to the MovingPlatform already in the Hierarchy view.
To do this, click the Selection button for Interactive Object, select MovingPlatform and set Interaction Type to Activate.

You should see a link line with directional arrow heads in the Scene view from the PressurePad to the MovingPlatform. This visualises the relationship between the two GameObjects and shows the flow of Sender to Receiver for Game Commands.

Play your game and walk on the One Shot PressurePad (it should stay pressed) to activate the MovingPlatform and begin the looping sequence.

