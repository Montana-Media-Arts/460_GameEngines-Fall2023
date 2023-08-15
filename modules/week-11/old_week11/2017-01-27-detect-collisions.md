---
title: Detect Collisions
module: 11
jotted: false
---

# Collisions

## Disable OnTrigger

Okay, what's next? 

Well, your players need to be able to collect the pickup GameObjects when the player GameObject collides with them. 

For this to happen, the game needs to detect the collisions between the player GameObject and the pickup GameObjects and use this information to trigger a new behavior. 

It's really important to test the collisions carefully. 

The pickup objects, the player's sphere, the ground plane, and the walls, all have colliders. 

Without testing, the player might be able to collect and disable the wrong objects in the scene. 

For example, the ground plane. 

Let's get started. 

In the Project window, go to the Scripts folder and open the Player Controller script for editing. 

Before you begin writing the script, let's think about what the code needs to do. 

It needs to detect and test collisions on the player's sphere collider. 

To do this, you're going to use the function OnTriggerEnter. 

Imagine that you're playing a platform game, and you jump up to collect a perfect arch of coins, but bounce off every first one and fall back to the ground. 

That's not very elegant. 

The OnTrigger function will detect the contact between the player GameObject and the pickup GameObjects without actually creating a physical collision. 

Let's start with the function declaration and parameters. 

Beneath the FixedUpdate function write void OnTriggerEnter open brackets Collider other closed brackets. 

Next, remember to add a set of curly braces to contain the function body. 

OnTriggerEnter will be called by Unity when the player GameObject first touches a trigger collider. 

It will be given a reference to the trigger collider that has been touched. 

This is the collider called other. 

This reference gives you a way to identify the colliders that the sphere hits. 

You could destroy the GameObject that the trigger collider is attached to. 

However, in case you want to add additional functionality to the collectable objects later on, in this project, you will only deactivate the GameObject rather than destroy it. 

So how can you deactivate the correct object, and in this case, the pickup GameObjects using a script. 

Let's start to write the first line of code inside the function body for OnTriggerEnter. 

You can address the other colliders GameObject through other.gameObject. 

To disable it, you need to use the method SetActive. 

This method accepts a boolean value inside the parentheses. 

That is true or false. 

Use false to disable the GameObject. 

The full line should be other.gameObject .SetActive semi colon. 

This code will disable GameObjects correctly. 

But you don't have a way to filter which GameObjects this sphere can disable. 

## Add a Tag

Now, you've written code to deactivate GameObjects that they player GameObject collides with, but this should only apply to the pickup GameObjects. 

The ground or walls disappearing would not be good. 

To do this, you're going to use the built-in Unity Tag System. 

Tags allow you to identify a GameObject by comparing the tag value to a string. 

The first thing we need to do is set up the tag value for the pickup objects back in the Unity Editor. 

In the Project window, go to the Prefabs folder and select the pickup prefab. 

You can also use the arrow next to one of the prefab instances in the Hierarchy. 

This opens prefab edit mode. 

In the tag list at the top of the Inspector, there's a few pre-made tags, but there's nothing called pickup. 

Select Add Tag. 

This brings up the tags and layers panel with a list that's currently empty. 

To create a new custom tag, select the Add button to add a new row to the tag list. 

In the new empty element, which will be tag zero, type PickUp. 

This is case sensitive so be careful. 

It needs to be exactly the same string that you use in the script. 

You can close the tags and layers panel when you're done. 

Now you need to apply that tag to the prefab asset. Select the tag dropdown again. 

You should see that your new tag is now available in the list. 

Select this tag from the list. 

The asset is now tagged PickUp. 

Use the arrow to leave prefab edit mode, and with the power of prefab, all the instances are also now tagged PickUp as well. 

Great! 

Save your changes in the Unity Editor before you return to the script. 

## Conditionals

Now that you've created a pickup tag, you can use it to make sure the sphere only disables the correct GameObject. 

Let's do that using an if statement. 

This means that if a certain condition is met, the script will behave in a specified way. 

To create the if statement, you're going to use CompareTag. 

CompareTag can compare the tag of any GameObject to a string value. 

At the top of the function body for OnTriggerEnter, write, if( other.gameObject .CompareTag( "PickUp" and then close that statement with two closed brackets. 

This is the if statement. 

Be careful here. 

Comparison is case sensitive. 

Make sure to write the tag exactly as you did it in the Unity Editor. 

Next, add a set of curly braces and cut and paste the line that reads, other.gameObject.SetActive ; other.gameObject.SetActive ; other.gameObject.SetActive ; 

Now this code will be called every time the sphere GameObject touches a trigger collider. 

It will receive a reference for the collider that the sphere has hit, and then check its tag. 

If it is a collectible object, the script will call SetActive which will deactivate the GameObject. 

Let's save this script and then return to the editor and enter play mode to test the game. 

So the target is set correctly, but the sphere is still bouncing off the pickup cubes just like we were bouncing off the walls. 

## Pickup Colliders as Triggers

So you created tags and used them to differentiate the pickup GameObjects. 

But when you tested the game the sphere didn't disable them on collision. 

This is because the pickup GameObjects are still using normal colliders and not triggers. 

Remember, colliders will stop physics objects from overlapping with them. 

Which is perfect for a wall, for example. 

But triggers will just notify you if something is overlapping without stopping it. 

You can fix this issue by making the colliders into triggers or trigger colliders. 

When you turn a collider into a trigger you can detect the contact with the trigger through on trigger event messages. 

When a collider is a trigger you can do some clever things. 

You could place a trigger in the middle of a doorway in say, an adventure game. 

When the player enters the trigger the mini-map could update and a message could be displayed to tell them you have discovered a new room. 

Or every time the player walks around a particular corner, spiders might drop from the ceiling because they have walked through a trigger. 

You have already use on trigger enter in your code, rather than on collision enter. 

Now you just need to make sure the collider is set to be a trigger. 

Let's get started. 

First, check that you're not in play mode. 

Changes made in this mode won't be saved. 

Next, go to the prefabs folder in the Project window. 

And select the pickup prefab. 

Now find its Box Collider component in the Inspector. 

And enable the Is Trigger checkbox. 

Now through the power of prefabs all the pickup GameObjects have trigger colliders. 

Let's save the scene and enter play mode to test it. 

Okay, as the players sphere enters the trigger the pickup GameObjects disappear. 

Excellent! 

Everything looks great, so you can exit play mode. 

You're almost done on this section of the project. 

## Add RigidBody

There's one more tweak you can make to the collectible cubes, adding a Rigidbody component. 

Any GameObject with a collider and a Rigidbody is considered dynamic, which means Unity won't treat it as static or stationary. 

You don't want the pickup GameObject to be considered static as it will take Unity longer each frame to calculate the physics and it would do this each frame because the cubes are rotating. 

The solution is to add a Rigidbody component to the pickup objects. 

In the Project window, go to the Prefabs folder and select the pickup prefab. 

In the Inspector, select Add Component and add a Rigidbody. 

In the Inspector, select Add Component and add a Rigidbody. 

Save your changes, then test in play mode and ah, the cubes all fall through the floor. 

The force of gravity is pulling them down and as they are triggers, they don't collide with the floor. 

If you find the Rigidbody component in the Inspector, you'll see that you could just disable the use gravity checkbox which could prevent the cubes from being pulled downwards. 

However, this is only a partial solution. 

If you do this, even though the cubes would not respond to gravity, they would still respond to physics' forces. 

There is a better solution, enable is kinematic. 

This sets the Rigidbody component as a kinematic Rigidbody. 

A kinematic Rigidbody will not react to physics' forces and it can be animated and moved by its transform. 

This is great for everything from objects with colliders like elevators and moving platforms to objects with triggers like your collectible cubes that need to be animated or moved by their transform. 

Let's save the scene again and enter play mode to test. 

Now the cubes do what they did before but more efficiently. 

Excellent! 

So to recap, static colliders shouldn't move like walls and floors. 

Dynamic colliders can move and have a Rigidbody attached. 

Standard Rigidbodies are moved using physics' forces. 

Kinematic Rigidbodies are moved using their transform. 

In the next section of this project, you'll count the collected objects and make a simple UI to display the player's score and messages.

PlayerController script

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerController : MonoBehaviour
{
    public float speed = 0;

    private Rigidbody rb;

    private float movementX;
    private float movementY;

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    private void OnMove(InputValue movementValue)
    {
        Vector2 movementVector = movementValue.Get<Vector2>();

        movementX = movementVector.x;
        movementY = movementVector.y;
    }

    private void FixedUpdate()
    {
        Vector3 movement = new Vector3(movementX, 0.0f, movementY);

        rb.AddForce(movement * speed);
    }

    private void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.CompareTag("PickUp")) 
        {
            other.gameObject.SetActive(false);
        }
    }
}
```