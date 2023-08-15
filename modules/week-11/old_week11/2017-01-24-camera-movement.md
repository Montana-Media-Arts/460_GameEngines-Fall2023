---
title: Camera Movement
module: 11
jotted: false
---

# Camera

## Camera Position

So right now, the camera doesn't move and from its current position can't see a lot. 

To change this, you need to tie the camera to the player GameObject. 

First, set the position of the camera. 

Lift it up by 10 units, tilt it down by 45 degrees. 

This is a typical third person setup with the camera as a child of the player GameObject. 

This works well for a lot of games because the camera GameObject will inherit the transform changes of the player GameObject. 

When the player moves in the game, the camera will move with them. 

However, for this game, that setup creates a problem. 

When the player rotates, the camera rotates as well. 

Because the ball is rolling, the camera is also going to inherit this motion. 

Let's look at this in play mode. 

Hold down the up arrow to move. 

The camera is a child of the player sphere so even though the camera is not moving at all relative to the player's game object, you can see that the player game object is rotating wildly and the camera's point of view rotates with it. 

Okay, let's exit play mode. 

Let's think about how to resolve this. 

The player GameObject in your game, a sphere, is rotating on all three axes, not just one. 

In a more typical third person game camera setup, the camera as a child of the player GameObject will always be in the position relative to its immediate parent. 

This position will be the parent's position in the game modified or offset by any value in the child's transform. 

In this game, that approach won't work so let's detach the camera. 

The new offset value will be the difference between the player game object and the camera. 

To do this, you need to associate the camera with the player GameObject with a script rather than as a child of the GameObject. 

## Camera Controller

Okay, let's start by creating the new script to control the camera. 

With the Main Camera GameObject selected in the Hierarchy, select Add Component In the Inspector. 

Call your new script, CameraController. 

If you use the Inspector to create the script, it will be placed in the root or top level of the project folders. 

Move it to your scripts Folder. 

Open the new script for editing. 

This script needs two variables. 

A public GameObject reference to the player GameObject. 

And a private Vector3 variable to hold the offset value. 

Inside the first curly brace, add the following line. public GameObject player; 

This will reference the player GameObject's position. 

On a new line underneath, add another variable declaration. private Vector3 offset; 

This will store the offset value. 

The offset value variable is private because you're going to set the value in the script. 

Now that you've declared the variable, let's calculate it. 

To do this, you're going to take the current transform position of the camera GameObject and subtract the transform position of the player GameObject to find the difference between the two. 

The Start function is the best place for this code. 

It needs to be calculated immediately when the game starts, but it only needs to be calculated once. 

In the Start function, on a new line, after the first curly brace, let's write the equation like this. offset = transform.position minus player.transform.position; 

This will set off set equal to the camera transform position minus the player GameObjects transform position. 

Next, you're going to use that to set the camera GameObjects transform position. 

This needs to happen in every frame. 

So the best place for that code is the Update function. 

Inside the first curly brace, write transform.position equals player.transform.position + offset; 

Now when a player moves the sphere with controls on the keyboard, the frame before displaying the camera, the camera GameObject is moved into a new position, aligned with the player GameObject before the frame is displayed. 

This is just like what would happen if it were a child of that object, except it's only matching the position and not the rotation of the sphere. 

However, Update is not actually the best place for this code. 

It is true that Update runs every frame and in Update, each frame, you can track the position of the player GameObject and set the position of the camera. 

However, you don't control which order all of the Update functions happen. 

That means that the Update could run before other scripts or Unity systems that will change the player position, like the physics system. 

So, what's the solution? 

Well, for follow cameras and tasks, like gathering last known states, it's best to use Late Update. 

Late Update runs every frame just like Update, but will run after all of the other updates are done. 

Add Late in front of the Update method to change this. 

Okay, brilliant. 

Now, you know that the camera position won't be set until the player has moved for that frame. 

Let's save the script and return to Unity to test it out.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class CameraController : MonoBehaviour
{
    public GameObject player;

    private Vector3 offset;

    // Start is called before the first frame update
    void Start()
    {
        offset = transform.position - player.transform.position;
    }

    // Update is called once per frame
    void LateUpdate()
    {
        transform.position = player.transform.position + offset;
    }
}

```