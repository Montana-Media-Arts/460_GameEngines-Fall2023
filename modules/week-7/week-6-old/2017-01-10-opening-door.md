---
title: Opening a Door
module: 6
jotted: true
---

# Opening a Door

In the 2D Game Kit, Events allow us to create actions. We’ll use Events to trigger a door opening when the player steps on a pressure pad.

Let’s start by adding in a Door:

1. In the Project Window, go to Prefabs > Interactables
2. Find the Door Prefab and drag it into the Scene View. Place it somewhere where it blocks the player’s path.

Now let’s add the pressure pad into the Scene:

1. In the Project window, go to Prefabs > Interactables and find the PressurePad Prefab
2. Drag it onto the ground in front of the Door

Note: If the player seems to be blocked by the pressure pad instead of stepping on it, lower it slightly. The player can easily get stuck against the collider.

Some events are already defined; for example, when stepped on, the PressurePad plays a sound and lights up. Now let’s connect the PressurePad to the Door.

1. In the Hierarchy window, select the PressurePad
2. In the Inspector, find the Pressure Pad component
3. In the On Pressed list, click the + at the bottom right to add a new event
4. Drag the Door GameObject from the Hierarchy window to the None(Object) field in the event
5. In the No Function dropdown, find Animator > Play(string)

In the text box that appeared under the dropdown, enter the text DoorOpening

Press Play and use the movement keys (A and D) to run onto the Pressure Pad. The door should open!

All the Animation Clips are stored in the Project window, in Art > Animations > Animation Clips. To play different Animators in an Event, you must match the name (string) of the Animation Clip exactly.

If you wanted to shoot a switch instead of placing a pressure pad, you can do that by using the same steps, but selecting the ResusableSwitch in the Prefabs folder instead. Give it a try!