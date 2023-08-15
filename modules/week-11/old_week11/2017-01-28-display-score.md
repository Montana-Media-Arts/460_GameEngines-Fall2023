---
title: Display Score
module: 11
jotted: false
---

# Score

## Store Value

In this section of the project, you'll explore counting the collected cubes, displaying text, and ending the game with a message. 

First things first, the collectibles. 

To count these, you'll need a tool to store the value of the counted collectibles and another to add the value as we collect and count them. 

Let's add this functionality to the Player Controller script. 

Select the Player GameObject in the hierarchy and open the Player Controller script for editing. 

First, let's declare a private variable to hold the count. 

This should be an integer variable written as int because the count will be a whole number. 

The player won't be collecting partial objects. 

Underneath the Rigidbody variable declaration, write private int count; 

Naming the variable count makes it easier to understand its role in context. 

The game needs to start with a Count value of zero. 

Each time the player picks up a new cube, it then needs to increase by one. 

As the Count variable is private, you won't be able to set its initial value in the Inspector. 

You'll need to do that in this script. 

There are several ways you can set the starting value of Count. 

But for this game, let's do it in the Start function. 

In Start, write count equals zero to set the initial Count value to zero, followed by a semicolon. 

Next, you need to increase the Count variable when the player collects a cube. 

These cubes are collected or disabled to make them disappear in the OnTrigger if the other GameObject has the tag PickUp. 

That means the OnTriggerEnter function is where you need to add the counting code. 

After setting the other GamesObject's active state to false, add another line of code to add one to the variable count, then save it back to that same variable. 

This is basic form of incrementing a value. 

To do this, write count equals count plus one. 

Then add a semicolon. 

There are other ways to add Count up or increment the value when coding for Unity, but this one is the easiest to explain visually. 

Now save this script and return to the Unity Editor. 

In the next video, you'll create and configure a UI text element so that you can display 
the incremental Count value to the player.

PlayerController script

```csharp
using UnityEngine;

// Include the namespace required to use Unity UI and Input System
using UnityEngine.InputSystem;
using TMPro;

public class PlayerControllerNew : MonoBehaviour {
	
	// Create public variables for player speed, and for the Text UI game objects
	public float speed;
	public TextMeshProUGUI countText;
	public GameObject winTextObject;

        private float movementX;
        private float movementY;

	private Rigidbody rb;
	private int count;

	// At the start of the game..
	void Start ()
	{
		// Assign the Rigidbody component to our private rb variable
		rb = GetComponent<Rigidbody>();

		// Set the count to zero 
		count = 0;

		SetCountText ();

                // Set the text property of the Win Text UI to an empty string, making the 'You Win' (game over message) blank
                winTextObject.SetActive(false);
	}

	void FixedUpdate ()
	{
		// Create a Vector3 variable, and assign X and Z to feature the horizontal and vertical float variables above
		Vector3 movement = new Vector3 (movementX, 0.0f, movementY);

		rb.AddForce (movement * speed);
	}

	void OnTriggerEnter(Collider other) 
	{
		// ..and if the GameObject you intersect has the tag 'Pick Up' assigned to it..
		if (other.gameObject.CompareTag ("Pick Up"))
		{
			other.gameObject.SetActive (false);

			// Add one to the score variable 'count'
			count = count + 1;

			// Run the 'SetCountText()' function (see below)
			SetCountText ();
		}
	}

        void OnMove(InputValue value)
        {
        	Vector2 v = value.Get<Vector2>();

        	movementX = v.x;
        	movementY = v.y;
        }

        void SetCountText()
	{
		countText.text = "Count: " + count.ToString();

		if (count >= 12) 
		{
                    // Set the text value of your 'winText'
                    winTextObject.SetActive(true);
		}
	}
}

```

## Create UI

Now you can store and increment the players collectible cube count but there's currently no way to display it. 

To do this you're going to use two systems built into Unity, the User Interface or UI system and TextMesh Pro. 

Let's get started by creating a new TextMesh Pro text element in the Unity editor. 

First, in the Hierarchy select the Add button and go to UI > Text - TextMesh Pro. 

A dialogue window will open to install TextMesh Pro essentials. 

Select Import TMP essentials. 

These resources will be added to a new folder at the root of your Unity project. 

Close the window when you're done. 

Now you'll see that some new things have appeared in the Hierarchy. 

Let's review them. 

There's Text TMP, the UI text element. 

There's also a parent GameObject for that text element called Canvas and another GameObject called Event System. 

These GameObjects are all required by the UI tool set. 

The single most important thing to know about these additional items is that all UI elements must be the child of a Canvas to behave correctly. 

Rename the text element GameObject, CountText. 

Now, select the Canvas and press the F key to see the entire GameObject in the scene view. 

Then select the 2D toggle at the top of the scene view to change to 2D view. 

The default text color is white the size and alignment are good, but let's add some placeholder text. 

In the text field, at the top of the component add count text as your placeholder. 

Now, to avoid getting in the player's way it makes sense to display the count in the upper left of the screen when the game is playing. 

It's currently in the center of the scene, this is because the text element is anchored to the center of its parent, which is the Canvas. 

It is worth noting here that the transform component on the UI object is different from the other GameObjects in Unity. 

For UI objects, the standard transform has been replaced with a Rect transform which takes into account many specialized features necessary for a versatile UI system including anchoring and positioning. 

One of the easiest ways to move the count text element into the upper left of the screen is to anchor it to the upper left corner of the Canvas, rather than to its center. 

To do this, select the icon at the top of the Rect transform component, displaying the current anchor preset this will open the anchors and presets menu. 

Hold Shift and Alt or option and select the top left anchor point. 

Holding those keys, will set the pivot and position for the count text based on the new anchor. 

And now the UI text is up in that corner, great! 

It does look a little crumped though. 

Let's add some space between the text and the edge of the screen. 

The simplest way to do this is to change the Rect Transform components, pos X value to 10 and pos Y value to minus 10, now that looks better. 

## Display the Count

Now that you've created a UI text element to display the count, let's connect this to the Player Controller script to get it working. 

In the Project window, go to the Scripts folder and open the Player Controller script in your script editor. 

Before you can code anything related to the TextMesh Pro element, you need to add information about it to the script. 

The details about the UI tool set are held in what's called a namespace. 

You imported another namespace earlier in this project to access the Unity Input System. 

To import the namespace for TextMesh Pro into this script, create a new line underneath the input system namespace and write using TMPro; 

With this imported, you can now write code relating to the element. 

Next, underneath the public variable called speed, create a new public TextMesh Pro variable using the type, TextMesh Pro UGUI called count text. 

Make sure to add the semicolon at the end of the variable declaration. 

This variable will hold it a reference to the UI text component on the count text GameObject. 

Now, you need to set the starting value of the UI text text property and get it to change as the count value changes throughout the game. 

It's worth creating a new function for that to avoid duplicating lines of code. 

Underneath the on move function's closing curly brace, leave a space and write void, SetCountText. 

Open and closed brackets. 

Add a set of curly braces beneath this to contain the function body. 

You can now set the count text equal to the int or whole number value of count. 

To do this, write countText.text = "Count: " + count.ToString ; 

This function must be called after the line of code setting the count variable's value within the start function because count needs to have a value that can be used to set the UI text. 

Let's add a call to set count text to be end of the Start function. 

Write SetCountText, open and closed brackets, semi colon. 

The UI component's text also needs to update every time the count variable is incremented after the player collects a cube. 

Inside the if statement for the OnTriggerEnter, call the set count text function again using the same instruction. 

SetCountText, open and closed brackets, semi colon. 

Excellent, save your changes and switch back to the Unity editor. 

Now, the player controller script component has a new text property. 

Select the player GameObject in the Hierarchy, then drag and drop the count text GameObject onto the slot to reference the UI text element. 

Unity will find the TextMesh Pro UGUI component on the GameObject and correctly associate that reference. 

Before you test, you need to make one final adjustment. 

In the Hierarchy, select the Event System GameObject. 

Then, find a Standalone Input Module in the Inspector. 

You'll see a warning displayed, as this component is for the legacy Input Manager. 

But you are using the Input System. 

To fix this, select Replace with Input System UI Input Module to replace the old component with a new Input System component. 

This will be useful if you want to add UI interactions to this Unity project later. 

Now let's save, enter play mode and test. 

Great. 

Now the count is displayed and increments correctly as the player collects cubes. 

Remember to exit play mode when you're done testing. 

Your Roll-a-Ball game is almost complete. 

## End Game

To finish up your UI work, let's add a simple message for the player when they've collected all the cubes. 

First, create another UI text object. 

In the Hierarchy, select the Add button and go to UI > Text - TextMeshPro. and go to UI > Text - TextMeshPro. 

Just like before, the new UI element has been created as a child of a Canvas GameObject. 

Rename the UI GameObject WinText. 

In the Inspector, find the vertex color property in the TextMeshPro component and use the color picker to set this to black. 

Let's also make this text a little smaller. 

Set the font size to 32. 

You could also make it bold using the B button in the font style field. 

In the text field add You Win! 

Lastly, let's adjust the alignment of the text to the center of the player's screen, but raised above the middle on the vertical axis, so you won't cover the player's sphere. 

By default, the UI text element is anchored to the center of the canvas. 

So you just need to raise it and move it across. 

In the Rect Transform component, set the pos X value to about zero, and the pos Y to 130. 

Let's also set the alignment to center. 

Then, save the scene and switch back to your script editor. 

Now you need to add a reference for this UI text to the player controller script. 

But as you only need to disable and enable the text, you can just create a reference to the GameObject. 

At the top of the script with the other variable, create a new public variable of the type GameObject, and call it winTextObject. 

Remember to add a semicolon at the end of the declaration. 

Next, let's set the starting state for the object to be disabled. 

This text should only display when the player has completed the game. 

Because it's a starting state, add the code disabling the UI text to the start function. 

Write an instruction using the same method that you used to disable the collectable cube GameObjects on collision. winTextObject.SetActive ; 

You also need to set when the UI text GameObject will be activated. 

Let's add this to the SetCountText function. 

Underneath the instructions, setting the count value UI text, write if if if Then add curly braces to contain the code. 

An important note. 

If you add a different number of collectible cubes to the game, perhaps a few more than 12 or a few less, your if conditional statement must reference that number. 

Otherwise, the game end text may display too early or might not display at all. 

Inside the curly braces, set the win text object to active. winTextObject.SetActive ; winTextObject.SetActive ; winTextObject.SetActive ; 

Then save your script and return to the Unity editor. 

Just like before, the Player Controller component has a new UI text property. 

Select the Player GameObject in the Hierarchy, then drag the WinText GameObject from the Hierarchy into the slot. 

Save the scene and enter play mode to test your changes. 

So, the player can collect all the cubes, see the count go up and then receive a win message when they've collected them all, awesome. 

In the final section of this project, you'll create a build of your game and deploy it using a stand-alone player. 

You're almost finished.