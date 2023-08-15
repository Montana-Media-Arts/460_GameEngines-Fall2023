---
title: Game Boundaries
module: 10
jotted: true
---

# Game Boundaries
When building a level you may find that in testing you can easily escape from your level and get outside where you want a player to be able to go. This can’t always be fixed by adding more geometry, we can add invisible walls to prevent players from being able to escape.
We can do this by adding a Collider on an empty GameObject and adding it to our scene;
At the top of the Hierarchy go to Create > Create Empty
In the Inspector Rename this to GameBoundary
In the Inspector click Add Component
In the search box type in Box Collider

Click on Box Collider to add it to the GameBoundary GameObject you just created
Now let’s locate this object in your Scene;
Select GameBoundary in the Hierarchy
Hover the mouse over the Scene View
Press F on the Keyboard to Frame Select the Object.

The Box Collider is visible by a green outline. Position, rotate and scale this object to any problem areas in your level.
We need to ensure that this Collider will stop Ellen from being able to pass through it. We have set up a specific Layer for this, with GameBoundary selected;
In the Inspector click on the Layer drop down menu
Left Click on Environment to set it to that Layer.

We have placed ours in an area where the player can easily jump up and escape, don’t forget to test your game boundary by pressing Play.

