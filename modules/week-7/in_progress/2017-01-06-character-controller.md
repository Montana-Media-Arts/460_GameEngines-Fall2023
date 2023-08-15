---
title: Character Controller
module: 7
jotted: false
---

# Character Controller Scripts


## Control Movement with Keyboard Input

You've created a script to move the Ruby GameObject. Now let’s adjust it so that  movement is driven by pressing a key on the keyboard.

Let's start by changing your code to make the character move horizontally. 
Instead of adding 0.1 to the x coordinate every frame, like your code did in the previous tutorial, it should only add it if the player presses the right arrow key on their keyboard. 

When the player presses the left arrow key, it should remove 0.1 so that the object moves to the left.

To do this, you’re going to use the Unity Input System, which is composed of Input Settings and input code (which Unity gives you to query the value of an axis for that frame).


### View the Default Input Settings

Let's start by looking at the default Input Settings, which define what kind of input your game uses:

Go to **Edit > Project Settings**, and select the Input page in the Settings window.

The Input page lists the Axes values for all the player input controls, such as a button on a gamepad. Values range from -1 to 1, depending on what the player is doing. 
For a thumbstick on a gamepad, for example, the horizontal axis could be set to:

-1 when the stick is to the left 
0 when it’s in the middle
1 when it’s to the right

For keyboard keys, the axis is defined by 2 keys:

A negative key that sets the axis to -1 when it’s pressed
A positive key that sets it to 1 when it’s pressed

If you look at the horizontal axis, you can see that left (for left arrow key) is on the negative button and right (right arrow) is on the positive button. 

### Modify Your Code to Use Axes

Next you need to modify your code to use Axes:
1. Double-click on your RubyController script. This will open your code editor.
2. Modify the Update function as follows:
 
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
      float horizontal = Input.GetAxis("Horizontal");
      Debug.Log(horizontal);
      Vector2 position = transform.position;
      position.x = position.x + 0.1f * horizontal;
      transform.position = position;
   }
}
```

Save your changes.

Now let’s explore the changes that you’ve made.

### Review your script changes

Create a New Float Variable 

In the first line of code you added to the Update function, you declared a new variable:

```csharp
float horizontal = Input.GetAxis("Horizontal");
```

This is a float variable (remember, that means a number with decimal points) called horizontal. It stores the result that Input.GetAxis ("Horizontal") provides.

### Call the GetAxis function
 
GetAxis is a function, like Start and Update that you encountered in the previous tutorial.

The difference here is that you don’t need to write the code that is inside the function, because Unity engineers have already done it for you. When you write the name of a function like that in script, you call it. 

### Provide a parameter

The word in between the parentheses is called a parameter. This is information you give to the function to specify what it should do. Here you’ve told the function the name of the axis that you want the value of.  You need to place the name between quote marks, so that the compiler knows it’s a word and not a special keyword like a variable name or a type.

### Dot operators

The script also shows how dot operators work for functions. Here, you called the GetAxis function “contained” inside Input. 

To keep code clean, functions that work on the same feature under the same name are grouped, and you can access them with the dot operator.


### Write the Horizontal Value to the Console

Your next line of code is another Unity function, that allows you to write something in the Console window: 

```csharp
Debug.Log(horizontal);
```

Now that you know how dot operators work, you know that Log is a function contained inside Debug. Debug contains all functions that help debug your game. 

The function takes a parameter, and writes it to the console. Here, it writes the value stored in horizontal to the console, which is the value GetAxis gave you.

### Adjust the Movement Code

Your slight change to the movement code multiplies the amount the GameObject moves by the value of the axis: 

```csharp
position.x = position.x + 0.1f * horizontal;
```

With these changes, Ruby’s movement will be as follows:

1. If you press left, the axis will be -1 (which makes the movement  position.x + -0.1f;). Ruby will move -0.1 to the left.
2. If you press right, the axis will be 1. Ruby will move 0.1 to the right.
3. If you press no key, the axis will be 0 and Ruby will not move (0.1 * 0 is 0).

### Test Your Changes

Now you can test your changes:
1. Return to the Unity Editor.
2. Press Play to enter Play Mode.
3. Press the right and left arrow keys on your keyboard. Ruby should move in the correct direction when you press these keys, and should not move when they are not pressed.
4. Look at the Console window. It should now be filled with values that change when you press the left or right arrow keys: 
5. At the top of the Console window, find the Collapse button. If you enable it, all lines that have the same value (such as 0 or 1) will collapse into one line, to take up less space in the Console window.

When the collapse button is disabled, lines appear continuously. This is because your Update function is called by Unity every frame, so it’s  writing lots of line per second (depending how many frames Unity generates in one second).

**NOTE:**

You might be curious why the values aren’t just -1, 0 or 1. This is because internally Unity will "smooth" the axis, so it doesn’t go from 0 to 1 instantly when you press the button. Instead, it goes to in-between values over a given period of time, so the movement is more smooth.

This can be disabled, but it is beyond the scope of this tutorial. For more information, see the Input Manager documentation.

### Exercise: Add Vertical Movement

Return to your code editor, and delete the line of code which writes values to the console:

```csharp
Debug.Log(horizontal); 
```

Now that you’ve checked the axis value, you don’t need this code. 

**TIP:** Remember that you can write a Debug.Log line anywhere in your scripts to output values to the Console to check variables. This can help pinpoint why something is not working as you want.

#### Add the vertical movement

Now try to add the vertical movement by yourself. Here are some tips to help you:

1. The default axis corresponding to the key up and down is called Vertical.
2. The y value stored inside a Vector2 corresponds to the vertical position.
3. Remember to save your changes regularly.
4. You can return to the previous steps of this tutorial if you want to review the process you used to add horizontal movement. The solution for this exercise is in the next step.

### Review the exercise solution

Here’s the solution to the exercise:

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
       float horizontal = Input.GetAxis("Horizontal");
       float vertical = Input.GetAxis("Vertical");
       
       Vector2 position = transform.position;
       position.x = position.x + 0.1f * horizontal;
       position.y = position.y + 0.1f * vertical;
       transform.position = position;
   }
}
```

### Exercise summary

You had already created the horizontal movement in the first part of this tutorial. For the vertical movement, you should have taken the following steps:
Create a new variable called vertical and store in it the result of Input.GetAxis(“Vertical”);. This works the same as horizontal, but the vertical axis is bound to the up arrow and down arrow keys.


Add a line of code to change the y-axis this time, based on that vertical axis value, just as we do for the horizontal axis.

And that’s it! After that, the GameObject’s position now contains the new position in x and y. If you press up and right at the same time, both will change and Ruby will move diagonally!


### Timing and Framerate
Right now, Unity is rendering 60 or more frames per second.  Let’s adjust this:

In your code editor, add the following 2 lines to the Start function: 

```csharp
public class RubyController : MonoBehaviour
{
    // Start is called before the first frame update
    void Start()
    { 
        QualitySettings.vSyncCount = 0;
        Application.targetFrameRate = 10;
    }

    // Update is called once per frame
    void Update()
    {
        float horizontal = Input.GetAxis("Horizontal");
        float vertical = Input.GetAxis("Vertical");
        
        Vector2 position = transform.position;
        position.x = position.x + 0.1f * horizontal;
        position.y = position.y + 0.1f * vertical;
        transform.position = position;
    }
}
```

This will make Unity render 10 frames per second.
1. Return to Unity Editor.
2. Press the Play button to enter Play Mode. 
3. Try to move Ruby. The game will probably feel very sluggish, and Ruby is much slower.

### Why is this?

If you look at your code, you can see that it moves Ruby 0.1 units every time Unity calls the Update function, which means every frame: 
position.x = position.x + 0.1f * horizontal;

If your game runs at 60 frames per second, Ruby will move at 0.1 * 60, so six units per second. But if the game runs at ten frames per second, like you just forced it to do, Ruby only moves 0.1 * 10, so 1 unit per second! 

If a player has a very old computer that can only run the game at 30 frames per second and another player has a computer that can run it at 120 frames per second, those two players will have a main character that moves at radically different speeds. This will make the game either harder or easier to play, depending on the machine running the game.

### Express Ruby’s Movement in Units per Second

To fix this problem, you need to express the amount that Ruby moves not in units per frames (as is currently the case), but in units per second. 

To do this, you will change the movement speed by multiplying it with the time it takes for Unity to render a frame. If the game runs at ten frames per second, each frame will take 0.1 seconds. If it runs at 60 frames per second, each frame will take 0.017 seconds. If you multiply the movement by that value, then the movement becomes expressed in seconds.

In your Code Editor, modify your script as follows:

```csharp 
public class RubyController : MonoBehaviour
{
   // Start is called before the first frame update
   void Start()
   { 
       //QualitySettings.vSyncCount = 0;
       //Application.targetFrameRate = 10;
   }
   // Update is called once per frame
   void Update()
   {
       float horizontal = Input.GetAxis("Horizontal");
       float vertical = Input.GetAxis("Vertical");
       Vector2 position = transform.position;
       position.x = position.x + 0.1f * horizontal * Time.deltaTime;
       position.y = position.y + 0.1f * vertical * Time.deltaTime;
       transform.position = position;
   }
}
```
Save your changes.

Now let’s explore the changes that you’ve made.


### Review your script changes

Add a comment for the compiler

First, you added double slashes (//) in front of the two lines that you previously added to slow down the game. These are just like the slashes that appear on the lines above the Start and Update functions. 

A double slash at the start of a line marks that line as a comment for the compiler. The Compiler ignores comment lines. Comments allow you to add information to your scripts, which is handy when you want to add reminders to yourself or others creating a game about what each bit of your code does. You can also use comments to disable instructions without deleting the lines, which is what you’ve done here. 
When you want to make the commented line active again, so that the compiler processes it, you just need to remove the double slashes (//). This saves you from having to retype (or copy and paste) lines as you experiment with your code. 

### Adjust the movement

Then you multiplied the amount of movement (0.1f * horizontal) by Time.deltaTime. 

**deltaTime**, contained inside Time, is a variable that Unity fills with the time it takes for a frame to be rendered.


### Test Your Changes

Now you can test your changes:

1. Return to Unity Editor.
2. Press Play to enter Play Mode.
3. Try to move Ruby.

Wait — your game runs fast again, but the character is now even slower than it was before. What’s happening?!

Well, you kept the amount of movement to 0.1, so that means your character will move 0.1 units per second. It will take ten seconds to cover a single unit. That’s way too slow. A running character should cover three or four units per second. 

1. Press Play to leave Play Mode.
2. Return to your Code Editor.
3. Replace the 0.1f in both lines of code with 3.0f and test again. This time your character will move much faster!
4. Now uncomment (remove the //) the two lines in Start to slow down the game and try again. The illusion of movement is a bit broken, because you don’t have enough frames rendered (cinema renders 24 frames per second, and you only have ten here). However, the character covers the same number of units per second as when the game ran faster — the character will run at the same speed on any machine, it will just look choppier on slower machines.
5. Go back to your code and delete the two lines of code in Start; you don’t need them now. You only added them to demonstrate how to control the speed of your game so that it’s the same on any machine. 

Your character now runs at the same speed, regardless of the number of frames rendered by our game. It’s now "frame independent".

### Check Your Script

Your RubyController script should now look like this: 

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
       float horizontal = Input.GetAxis("Horizontal");
       float vertical = Input.GetAxis("Vertical");
       Vector2 position = transform.position;
       position.x = position.x + 3.0f * horizontal * Time.deltaTime;
       position.y = position.y + 3.0f * vertical * Time.deltaTime;
       transform.position = position;
   }
}
```

### Summary
In this code-heavy section, you’ve handled keyboard input through axes. You’ve also made character movement the same, regardless of how many frames per second are used to render the game.

In the next section, you’ll take a break from code to see how you can pretty up that blue background and start to decorate the Scene.
