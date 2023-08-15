---
title: Damage with Objects
module: 6
jotted: false
---

# Damage with Objects

Damaging with Objects

To do this, we’ll go through the steps of dropping a box on a Spitter to kill him. Sorry, little guy!

Start by drawing out a level where Ellen is higher up, and the Spitter is on a platform underneath a drop, like this:

1. Go to Prefabs > Enemies and drag a Spitter into the Scene View
2. Place him on the lower portion of your level, close to the cliff-face
3. With Spitter selected, locate the Enemy Behaviour Script
4. Reduce the View Distance number so that Spitter does not shoot you immediately while you test this gameplay

1. In the Project Window go to Prefabs > Interactables, and click and drag the PushableBox Prefab into the Scene View
2. Select the PushableBox in the Hierarchy window, and, in the Inspector click Add Component
3. In the Search Box, type Damage
4. Click on Damager to add it to the PushableBox
5. The Damager Component tells any GameObject that has a Damageable Component on it (like a Spitter or a Chomper) to give it damage. There is more in-depth information on this system in the Components Documentation.

The Damager is represented by a green collision box as shown above. This is the area which causes damage. This is not covering the PushableBox right now, so when we push the box onto the Spitter, it will not damage him.

Let’s move this box so that it’s roughly the size and position of the PushableBox. There are two ways to do this:

Select and drag the green dots on the edges of the green collision box to be over the PushableBox

In the Inspector locate the Damager Component

Adjust the Offset and Size to position and size the collision box. The easiest way to do this is to left-click on the words and drag left and right to scrub through values.

Lastly, we need to make sure the damage is given to the right GameObjects. We separate objects into Layers in the Editor so that they can easily be found and seperated:

1. Select the PushableBox
2. In the Inspector, find the Damager Component
3. On the HittableLayers dropdown select the Enemy Layer

The PushableBox will now cause damage to anything on the Enemy Layer, like our Spitter.
Experiment with causing damage to other enemies, and even Ellen herself!
