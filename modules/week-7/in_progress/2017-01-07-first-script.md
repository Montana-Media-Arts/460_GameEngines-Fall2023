---
title: Unity Scripting
module: 7
jotted: true
---

# Unity Scripting

### Create a New Scene

This part should be review

1. Go to **File > New Scene**. Alternatively, you can use the shortcut Ctrl + N (Windows/Linux)  or Cmd + N (macOS).
2. If a popup window appears about unsaved changes, this is because you moved or changed something in the demo Scene and Unity is making sure you intend to lose those changes. Just click **Don’t Save**.
3. Now you have an empty scene called **"Untitled"**. That means the Scene is not written onto disk yet. 

Let’s save it using a proper name.  Go to **File > Save** or use the shortcut Ctrl/Cmd + S.

4. Select the folder called **Scenes**, and call your Scene **"MainScene"**.

Remember to press Ctrl/Cmd + S regularly to save your changes to disk. That way, if you exit Unity and come back to it later, you won’t lose any of your changes. 

### Import Assets into your Project

This Project is called Ruby's 2D Adventure because the main character is called Ruby, and you’re making a basic adventure game in a 2D environment. You get the idea. So let’s find Ruby! 

1. Save the images on your computer by downloading it from the Tutorial Materials.  
2. Drag and drop the file into **Art > Sprites**, so that Unity copies it into your Project folders and imports it with the default settings.
3. Because your Project was created with the 2D template, the image is automatically imported as a Sprite. 

To see this, select Ruby in the Project window and look at the Texture Type field in the Inspector:  

A **Sprite** is an image that is drawn in a game. Unity can’t use the .png image file of Ruby that you imported directly — it needs to transform it into something it can use. 

In this case, the Import Settings make it into a Sprite. 


### Use a Sprite to create a GameObject

To use the Ruby Sprite to create a GameObject:

1. Click on the small arrow next to the Ruby image in the Project folder. You will see that it “contains” what appears to be the same image, which is in fact the Sprite that Unity created for you. 
2. Drag that Sprite from the Project window to the Scene view, to add it as a new GameObject to your Scene. Use the 2D view.

If you take a look in the Inspector, you will see that Unity has added a Sprite Renderer component to the new GameObject automatically. That component draws a Sprite (specified in the Sprite slot) at the position where the GameObject is. 

3. Let’s have a play. Move the Sprite in the Scene view with the Move tool (you can use the shortcut W on your keyboard), and take a look at how the value in the Position property of the Transform component changes in the Inspector. 

Make sure you keep the Sprite inside the white rectangle, because that shows the Camera view bounds.

If you switch to the Game view, you will see your Sprite in game. If the Sprite is outside the white square bounds in the Scene view, then it is not in the camera’s view, and you won’t see it in the Game view.  


### Set coordinates for Ruby

Everything in a Scene has 3 coordinates. Coordinates are the amount of distance a GameObject is from either the center of the scene (if it has no parent) or its parent (if it has one). The three coordinates are:

Horizontal (x)
Vertical (y) 
Depth (z)

Because this game is in 2D, you can ignore depth for now.

To set the coordinates for Ruby:

1. In the Hierarchy, select the Main Camera GameObject. In the Inspector, you’ll see that its position is 0,0 — this is a Unity default position for Main Camera GameObjects. (For now, ignore the z coordinate.)
2. Change the x and y values to 0. The Main Camera GameObject will now be in the center of the Scene.
3. In the Hierarchy, select your new Ruby GameObject. 
4. In the Inspector, set the x value to -2 and the y value to 0. Ruby will now be vertically in line with the Main Camera, but to the left of it horizontally. (It might help to think of the axes of a graph: minus values will move a GameObject to the left on the horizontal axis, and down on the vertical axis).
5. Go to  File > Save and save the changes to your Scene. Alternatively, you can use the shortcut Ctrl + S (Windows/Linux) or Cmd + S (macOS). Remember to save your changes regularly throughout these tutorials! 

### Units of distance in Ruby's Adventure: 2D Beginner

The term 'unit' is often used when setting GameObject positions. You might read something like, "The Ruby GameObject's position is 2 units for x and -4 units for y."

What those units are can vary. For example:

If you are making a game about ants you might want to make the unit centimeters and use ant sprites one unit (1 cm) in size.

If you are making a game about space, you might want to set each unit to 10 meters and create a spaceship 10 units in length (100 m).

These tutorials just use 'unit' when they mention distance; if it’s helpful, you can think of each unit as 1 meter. 


### Create a New Script

Now it's time to write your first script (this shouldn't be new to you now!) and add motion to Ruby!

In Unity, scripts are a specific type of component. You can add them to a GameObject, and whatever behaviour you code in it will be applied to that GameObject.

To create a new script:

1. In the Project window, go to the root Assets folder.
2. Right-click in an empty space, and select Create > Folder.
3. Name your new folder "Scripts". This will help keep your Project organized.
4. Double-click Scripts to open the folder. 
5. Right-click and select Create > C# Script.
6. Name your new script RubyController. 

### Explore the Default Script

First, let’s explore the default script:

Double-click on the RubyController script. 

This will open the default code editor installed with Unity.

1. Recall, what does using mean?
2. For now, just remember that "class" means you are defining a new component. In this case, it is called RubyController. Everything between the opening curly brackets that start after that line and the one at the end of the file is part of your component.
Inside those curly brackets come two similar bits, both of which comprise one line starting with void, a name (Start or Update here) two parentheses, and that line is followed by two curly brackets { } on their own lines (the same as the class).
Those are functions. You’ll explore functions later — the only part to remember right now is that a function has a name (here it’s Start and Update) and that the code written between the two curly brackets will be executed by Unity at different times during the game.

#### Start function

Unity executes the code inside Start only once when the game starts. 

#### Update function (lines 14 to 17)

Unity executes the code inside Update every frame. 

To give the impression of movement, a game (just like a movie) is still images that are shown at high speed. Typically in games, 30 or 60 images show in one second. Each of those images is called a frame. Before creating that image, Unity executes the code written in the Update function of all your GameObjects.

In this Update function, you will write anything you want to happen continuously in the game (for example, reading input from the player, moving GameObjects, or counting time passing). 

### Adjust the Update Function

First you need to make the Ruby GameObject move:

Copy the following three lines of code into the two curly brackets in your Update function. 

```csharp
public class RubyController : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        Vector2 position = transform.position;
        position.x = position.x + 0.1f;
        transform.position = position;
    }
}
```

Reminder: Don’t forget to save, either with File > Save or by pressing Ctrl + S (Windows/Linux) or Cmd + S (macOS). Remember that keyboard shortcut — it’s helpful to press it whenever you finish typing any code to prevent losing work.

Let’s look at each line you added in detail.

### Declare a Variable

On the first line, you declared a variable called Position and stored Ruby’s current position in it:

Vector2 position = transform.position;

#### What are variables?

Variables are a place in your computer’s memory that you give a name. Later on, you can use that name to retrieve the value you stored there. Think of it as boxes stored on a shelf with names written on the front, and inside are values.

#### What is a Vector2 variable?

The first word before the variable name, Vector2, is the type of the variable. Types tell the computer what kind of data you want to store, so it can get the right amount of space in memory. A Vector2 is a data type that stores two numbers. Remember the Transform values in the Inspector that use x for the horizontal position, y for the vertical position and z for the depth. Those 3 numbers form a coordinate,. Because this game is 2D, you don’t need to store the z-axis position, so you can use a Vector2 here to only store the x and y positions.

#### The equals sign

The equals sign means, “store what is on the right side into what's on the left side”. So here you are telling the position variable to store the current x and y position values of the Transform.

#### Retrieving Ruby's position

Transform is a variable too (created by Unity, not you), that contains the Transform component on the GameObject that the script is on.

Think of the dot between transform and position as "contains", so here you are retrieving the position that is "contained" inside the Transform.

#### Summary
To summarize, you are storing the position contained in the Transform component inside a variable of type Vector2. Now there is a place in memory with the name "position" on it, which contains a copy of your GameObject's current position.



### Move the GameObject

In the second line, just like the first, you have used the dot to access things "contained" in something else: 

position.x = position.x + 0.1f;
Vector2 is two numbers, so it "contains" x and y. Here you are accessing the x value, which is the horizontal position.

To move your GameObject slightly to the right, you have added 0.1 to the x value and stored the result back into x. 

Now stored in the position variable in memory you have an x coordinate that is 0.1 units to the right (positive values move GameObjects to the right, and negative values move them to the left).

### Store the GameObject’s new position

In the third line, you stored that new Position inside the position of the Transform:
transform.position = position;

Before this line of code, you were only modifying a copy, like doing math on a side sheet of paper. Now that you have the new result, you need to give it back to the Transform component so that it actually knows to move the GameObject 0.1 units to the right!

The computer executes this function for every new frame, so your object will move 0.1 unit to the right in each new frame compared to the last one. This gives the illusion of the GameObject continuously moving to the right.

### Using your code in Unity

Let’s look at your code in action:

1. Go back to the Unity Editor. In the bottom right corner, you should see a small spinning wheel icon that means Unity is currently "compiling" your script.  

Note that on some computers, the compilation may be very fast and the icon appears very briefly. A Compiler is a tool that translates your code into a language that the computer can understand, because computers don’t read English! When that icon disappears, you can use your code in Unity. 

2. Select your character GameObject, then drag and drop your new script in the Inspector. Alternatively, click on the Add Component button in the Inspector and search for Ruby Controller (each new script is a new component). 
You should now see your component in the Inspector under the Transform and Sprite Renderer components, which means that the script is attached to your GameObject.  
3. Click the Play button to enter Play Mode and admire your first scripting result: a character rapidly fleeing to the right that quickly goes off the screen! 
4. Click the Play button again to exit Play Mode. You’ll notice that Ruby is back at her original place. That’s because everything done during Play Mode is “reset” when you exit Play Mode. That allows you to test and modify values without breaking your game. Remember to leave Play Mode each time you finish testing a change.
5. You can open your script again and play with the value you add to x. Try to change 0.1f into 0.3f or 0.01f, or even put a negative number! 

Every time you make a change, save the file, go back to Unity and enter Play Mode to see how it affects the movement of your GameObject. 

### Structuring Scripts

Now that you've tested it, let's have a look at the syntax of your script (that is, the way that it's written) in a little more detail. 

To refresh your memory, here are the three lines of code that you added to the Update function:

```csharp
Vector2 position = transform.position;
position.x = position.x + 0.1f;
transform.position = position;
```

### Structuring Scripts: Line Endings

Every line ends with ";". This is used by the Compiler (the tool that translates your English code into something the computer can understand) to define where an instruction ends and where the next instruction begins. 

This means you could have written: 

```csharp
Vector2 position = transform.position; position.x = position.x + 0.1f; transform.position = position;
```

But as you can see, that’s much harder to read! By convention, you should place each instruction on a new line.  

What happens if you forget the semicolon?

By convention, you should place each instruction on a new line. 
Don’t forget the ; at the end of a line! 

Let’s test what happens if you forget:

1. Try to remove ; on the first line in your script editor, and then go back to Unity. Your Console window should now have a red line with "error" in it:
Assets/RubyController.cs(17,8): error CS1525: Unexpected symbol 'position'
This is the compiler telling you it can’t understand your script. In this case, it tells you that in the RubyController script, at the line 17, column 8, there is a word "position" that wasn't expected to be there. That's because you removed the ; so the compiler continued to read past transform.position, and doesn't understand why there is another word "position" there! 
2. Double-click on the red error message. Your script editor will open in the right file at the right line. 
3. Add the ; back in, save the file, and go back to Unity.
4. The red line should now disappear, telling you that your script is working again!

### Structuring Scripts: Number Formats

After the number with a decimal point, you put an "f". This tells the computer that the number is a floating point number, so that it can store the number properly. 
The language you used to write that script, C#, has two types of decimal number: floating point and double precision. Double precision, as the name implies, uses twice the size in memory compared to floating point, but it’s twice more precise. 
For now, just remember that:

The two types aren't compatible (you can’t add a double and a float) 

Since all the Unity built-in scripts use floating points, you need to use floating point by adding an "f" to your decimal number

If you ever have an error in your Console window about a double that can’t be converted to float, that’s what the compiler means — you probably forgot to put a "f" after a decimal number! 

### Check Your Script
Your RubyController script should now look like this:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RubyController : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        Vector2 position = transform.position;
        position.x = position.x + 0.1f;
        transform.position = position;
    }
}
```

### Summary

In this section, you imported your first Asset — the Sprite for your main character, Ruby. You also wrote your first script, which allows you to move her across the screen. 

In the next section, you will tie that movement to keyboard input so you can add some interactivity to the game!



