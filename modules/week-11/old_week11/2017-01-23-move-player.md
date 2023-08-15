---
title: Move Player
module: 11
jotted: false
---

# Movement

## Rigidbody

Now that you've created a player object and a game environment, it's time to think about how the player GameObject should move and behave. 

It needs to be able to roll around on the game area, bump into walls but stay on the ground and not fly off into space, and collide with collectable GameObjects to pick them up. 

These things require physics. 

To use physics, the GameObject needs a Rigidbody component. 

To attach this, you need to, in the Hierarchy, select the player GameObject. 

Next, in the Inspector, select Add Component then search for Rigidbody, and add it to the sphere. 

You can collapse or expand the component view by clicking on the component's title bar. 

Whenever you expand a component like this, it will also toggle the relevant gizmos for that component in the Scene view. 

Okay, now that the player GameObject can interact with the physics system, it's time to apply some forces to it. 


## Input Package

Now it's time to get the player object moving under your control. 

To do this, you need to apply keyboard input to the player GameObject, which will act as forces to move the sphere in the scene. 

You can do this using the Input System Package and a script attached the player GameObject. 

Unity Packages are used to add specific enhanced functionality to your Unity Project. 

To install the Input System Package, in the top menu go to Window > Package Manager. 
You can use the Package Manager window to install, remove or update packages for your project. 

Select All packages to reveal all of the currently available Unity Packages. 

Find Input System, then select Install to add the package to your project. 

You might see a dialog window, asking you to enable the native platform backends for the Unity Input System. 

If you see this, select Yes to fully enable the new system. 

And then save your changes to the scene. 

That's it, you've now got the package that you need to make keyboard inputs. 

If you are creating this game for Windows, there is one more thing you need to do, to use the Input System Package for your game. 

In the top menu, go to File, Build Settings. 

You can also use the hotkeys Shift + Control or Command + B, this will open up the Build Settings window. 

In the PC, Max and Linux Standalone settings change the Architecture to x86_64. 

Close the Build Settings window. 

You'll find out more about this when you finished making your game. 

Now you're ready to continue. 

## Player Input

Now you've added the Input System package to your project, you can use the components that come with it. 

Let's add the Player Input to the sphere. 

First, in the Hierarchy, select the player GameObject. 

In the Inspector, select Add Component. 

Then search for and add Player Input. 

This will help you get the player movement for your game set up quickly. 

Next, you need to create an Input Action asset for the Player Input to use. 

Player Input checks to see if a given action has happened and triggers a notification which can be used by a script to trigger different things in a game. 

To define the actions that trigger a notification, you need to create an Input Action asset and link certain keys or buttons to an action name. 

Luckily, the Input System has a default template which will be perfect for this game. 

Go to the Inspector window. 

In the Player Input component, select Create Actions. 

Let's create a new folder called Input and save the Input Action asset there. 

The player sphere still won't move just yet but you will be able to access the information this component provides about control devices, such as the player's keyboard, using a script to control the player object's movement. 

## Create Script

Before you create the script to control the player objects movement, let's make a folder to organize script assets. 

In the Project window, click on the Create menu and choose create Folder. 
Rename this folder Scripts. 

Now you're ready to create a new C# script. 

You'll be using C# as the programming language as this is what Unity supports. 
To create a new script, you have some choices. You can go to the top menu and select Assets > Create, to create a new C# script. 

You can also use the Create menu in the Project window. 

But what might be most efficient in this case is to select the player GameObject in the Hierarchy and then use the Add Component button in the Inspector. 

The Add Component menu contains the option new script. You can select this to create and attach a new script in one step. Name the new script PlayerController. 

Select Create and Add or press the enter key to confirm your selection. 

Unity will create, compile, and attach the script to the selected GameObject. 

In this case, the sphere called Player. 

Creating a script directly on a GameObject creates that script asset on the root or top level of our project files shown in the Project window. 

To keep things organized, move the player controller asset into the Scripts directory you created. 

Next, select the script in the Project window. 

You'll be able to see a preview of the script asset in the Inspector but you can't edit this text. 

Let's open the script fully. 

There are two main ways to do this. 

You can double click on the script asset in the Project window or you can use the open button in the Inspector when the script is selected in the Project window. 

Each of these options will open the script in your preferred script editor. 

This is Visual Studio by default. 

Don't worry if this takes a little bit of time to load, especially if this is the first time you've opened your script editor. 

## Movement Script

Okay, let's get started with your first script. 

You'll see that there is some code already there. 

This is a template to make it easier to get started writing Unity scripts. 

A function is a way to group code under one name. 

There are two functions included in the script template, Start and Update. 
You can use the function name as a shorthand, or call the function instead of writing the same code every time. 

The code in Start is called when your game starts, and the code in Update is called once per frame of your game. 

You won't actually need the Update function in this script, so remove that code for now. 
Before you begin writing code, let's think for a moment. 

What do you need to do with the script? 

The script needs to check every frame for player input and then apply that to the player GameObject every frame as movement. 

Where will it check for the movement? 

The Input System has a method you can use to get the data from this input. 

Where will it apply the movement? 

There are two things you can use to apply this, Update and Fixed Update. 
Update is called before rendering a frame, and this is where most of your code will go. 
Fixed Update, on the other hand, is called just before performing any physics calculations, and this is where your physics code will go. 

You will be moving the ball by applying forces to its Rigidbody. 

This is physics, so you will put this part of the code in Fixed Update. 

Okay, let's get started. 

Because you are using the Input System, you need to add its namespace to the script. 
Namespaces are a collection of classes and other data types which can be imported at the start of your scripts. 

There are already three namespaces used in the template, System.Collections, System.Collections.Generic, and UnityEngine. 

There are lots of different namespaces. 

The additional one you need for this script is the InputSystem namespace. 

Add a new line below the first three namespaces. 

Write using UnityEngine .InputSystem; 

This will enable you to access the code and functions in the InputSystem namespace in this script. 

The PlayerInput component will notify the PlayerController script of action happening by calling functions with pre-defined names within your scripts. 

For example, to create the Roll-a-Ball game, you need to be notified whenever the move action happens. 

The predefined function for the changes in movement controls when pressing WASD or moving a joystick on a controller is called OnMove. 

In that function, the computer will read the value of the input, for example, up, down, left, or right, and then use that information to move the ball using code in the Update function, which you'll write later. 

Let's write an OnMove function. 

The first thing you need to do is write a function declaration, which tells the computer to create a function. 

Below the start function but before the final curly brace, add a new line. 

Write void OnMove, open and closed brackets. 

Void means that this function won't return any values. 

Next, add an open curly brace, leave a line, and then add a closed curly brace. 

The space inside the braces is called the function body, and this is where you add instructions for the computer to complete. 

These instructions are specifically for the function OnMove. 

Each function has its own set of curly braces. 

Okay, what's next? 

The PlayerInput component will be sending data of type input value to your script, so you need to add this to the function's input parameters. 

These are variables which will be used to store and reference data for the function. 

Inside the parenthesis, in your function call, add **InputValue movementValue**.

movementValue is the name of the variable you will use within the function. 

InputValue is the type of variable. 

Variables can have different data types, which means they store different types of data. 

The movement of the Roll-a-Ball sphere will be captured in two directions, up and down and left and right. 

In other words, the MovementValue variable will capture and store the data from the X direction and the Y direction of input devices. 

This kind of data can be stored as a Vector2 variable. 

## Input Data

In the last video, you wrote a declaration for the OnMove function. 

Next, let's use the method Get to get to movement input data from the sphere and store it as a Vector2 variable. 

In the space inside the OnMove function, add the line, 

```csharp
Vector2 movementVector = movementValue.get < Vector2 >() ; 
```

This code takes or gets the Vector2 data from the movement value and stores it in a Vector2 variable you are creating called movementVector. 

The player GameObject uses a Rigidbody and interacts with a physics engine. 

Next, you need to use the variable you just created to add or apply forces to the Rigidbody and move the player GameObject in the scene. 

To do this, your Player Controller script will need to access the Rigidbody component and add force to the player GameObject. 

First, let's create a variable to hold the reference in the script. 

Above the Start function, add the following code, private Rigidbody rb; 

This will create a private variable of the type Rigidbody and call that variable rb. 

This will hold a reference to the Rigidbody you need to access. 

The variable is private and not public, because you don't need this variable to be accessible from the Inspector or from other scripts right now. 

Next, inside the Start function, write rb = GetComponent &lt;Rigidbody&gt;() ; 

This sets the value of the variable rb by getting a reference to the Rigidbody component attached to the player sphere GameObject. 

There is definitely a rigidbody component attached to the GameObject because you added that component earlier. 

All of the code in the Start function is called on the first frame that the script is active. 

This is often the very first frame of the game. 

So the player will be able to move the sphere straight away. 

Now you need to set up the Fixed Update function so you can call add force on the Rigidbody stored in the variable rb. 

First, create a new function called FixedUpdate below the Start function. 

And just like that function, the type should be void because it will perform a task and not return any values, and there should be no parameters in the parentheses. 

You'll learn more about the uses of the void type with functions as you continue your programming learning journey. 

Remember to add a set of curly braces beneath the function declaration. 

This is where you'll add your code. 

Excellent. 

## Force

Next, let's start to write the code for adding force. 

In the function body, add the code rb.AddForce, inside open and closed brackets, type movementVector and then end that line with a semicolon. 

You'll probably see in your script editor that there is an error on this line. 

There are two reasons for this. 

Firstly, the variable you are trying to use is a Vector2 and you need to give this method a vector3 variable. 

Vector3 variables store data across three axes, x, y, and z. 

These values will determine the direction of the force you add to the ball. 

If you head back to the editor and look at the global gizmo, you can see the arrows also indicate this. 

To resolve this issue, you need to create two new variables for the individual input directions. 

Underneath the Rigidbody variable you created, add two more variables called movementX and movementY. 

These should be private like the previous variable, float type, which is short for floating point or decimal point value. 

These give you more precision than a whole number or integer value. 

You can then reference these values inside of OnMove and assign these values of movement vectors X and Y. 

In OnMove, add the following two lines of code. 

movementX equals movementVector.X; 
movementY equals movementVector.Y; 

Excellent. 

The second reason you were getting an error is because the movement vector variable was created inside of OnMove. 

That means it's what's called nonexistent in the current context, as it was inside another method. 

To fix this, you can combine the movement floats you just created inside of FixedUpdate to create a vector3 variable. 

This will take zero as the y value because the ball needs to move along the x and z axis in 3D space. 

In the FixedUpdate function, add a new line at the top of the function and write Vector3 movement equals new Vector3, open brackets, movementX, 0.0f, movementY close brackets and then a semicolon. 

The f after the value signifies that this is a float value. 

Next, use the new Vector3 variable, movement, to Add Force to the Rigidbody of the player sphere. 

Revise the second line of code in FixedUpdate to rb.AddForce ; 

Let's save the script and then return to the Unity editor to give it a try. 

Use the Play button to enter play mode and test. 

It works! But the player is moving really slowly. 

Leave Play mode to stop testing. 

In the next video, you'll adjust the script to fix this.

## Fix Speed

Okay, you've almost finished setting up the player movement, but the speed isn't quite right. 

Let's go back to your script editor to fix that. 

First, let's add a speed variable to the script, so that you can control the player movement from inside the Inspector. 

Go to the start of the script, where you have declared other variables. 

Add a public float variable called speed, to the start of the script with a starting value of zero. 

To do this, write public float speed = 0; 

Next, back in Fixed Update, multiply the force you are adding to the sphere's Rigidbody, by this strength variable. 

To do this, revise your line of code to, rb.AddForce ; 

Then save your script, and return to the editor. 

Because you made the speed variable public, you can find it in the Inspector on the Player Controller script component. 

Let's try changing the variable's value to ten, to see if the speed is fast enough to improve the player experience. 

Enter play mode, and test the revised movement. 

Great. 

This looks like a good speed for the player, and the value is now exposed, so you can adjust it easily from the Unity editor if you need to. 

Exit play mode, and then save your changes to the scene. 

Congratulations. 

The player can now move the sphere. 

In the next section of this project, you'll explore how to set up the camera for your game.

PlayerController

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

}
```