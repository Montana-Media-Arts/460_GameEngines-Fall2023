---
title: Animation
module: 9
jotted: true
---

# Animator

This video is a more generic with SunnyLand.

<iframe width="560" height="315" src="https://www.youtube.com/embed/hkaysu1Z-N8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

To play animation on a GameObject, Unity uses a component called an Animator:  (This uses the Ruby tutorial)

1. Go into Prefab mode for the Robot Prefab (reminder: double-click on the Prefab in the Project window or on the arrow on the right of your Robot in the Hierarchy window)
2. In the Inspector, click the Add Component button and search for the Animator component.  

The most important setting on the Animator component is the Controller setting.
A Controller is the component responsible for choosing which animation to play, based on rules you will define (for example, making the character go from standing to a running animation when their speed is greater than 0).You will explore these rules later on. 

### Creating a New Controller

For now, let’s just create a new Controller and set it up on your Robot Animator:

1. In the Project window, go to the Animations folder. This folder contains pre-made animations for your Project to speed things up later on.
2. Inside the Animations folder, right-click and select Create > Animator Controller from the contextual menu. Call the controller Robot.  
3. Now in the Animator you added on the Robot Prefab (make sure you’re in Prefab Mode again if you exited it), assign your Controller in the Controller setting (either click on the little circle and find the Controller in the window that opens, or drag and drop the Controller from the Animation folder into the Controller setting).

Remember to save your changes to the Prefab with the save button on the top right corner of the Scene view. 

### Animations

Now that you have an Animator Controller, you need to create the animations that the Controller will use.  Animations are Assets that you store in your Project folder. 

To create animations inside the Unity Editor, use the Animation window:

1. Open the Animation window by selecting Window > Animation > Animation. 
If you select the Robot in the Hierarchy or are in Prefab Mode on the Robot Prefab, the Animation window will display this message: “To begin animating [Prefab/GameObject name], create an Animation Clip”. 
2. Click the Create button.
3. Now select the Animations folder as the place to save the animation clip and name it “RobotLeft.anim”.

The Animation window is split into two sections:

- On the left, properties that are animated
- On the right, the timeline showing the keyframes for each property

### Changing a Sprite

You can animate any property from any component in your GameObject over time with the Animator. It could be the Sprite Color that you would like to change over time, or the size. In  this case, you want to change the Sprite used by the Sprite Renderer.

You can create the illusion of movement by changing which Sprite the Sprite Renderer uses over time . 

1. You can find all the Sprites for your robot in the folder called: Art > Sprites > Characters in a Sprite Atlas called MrClockworkSheet (also referred to as a Sprite Sheet).
As you can see, there are multiple Sprites on a single image, just like the Tile Palette you saw earlier. In this example, you  have already split the image into different Sprites . 
2. Click on the arrow next to the image to see all the Sprites:
3. Hold shift and click on the first and last walking animation Sprites (facing left) to select all four:

MrClockworkWalkSides1
MrClockworkWalkSides2
MrClockworkWalkSides3
MrClockworkWalkSides4

4. Drag and drop these Sprites into the Animation window. This creates an animation using the four Sprites. 
5. Press the Play button on the Animation window to preview the animation:
 
As you can see, the animation is going way too fast. That’s because your animation has a sample size of 60, which is set in the Samples setting in the Animation window, above the animation properties. 

Notice that the timeline has 60 vertical lines between the 0:00 and 1:00. This makes the animation run at 60 frames per second, which means Unity renders the Sprite 60 times each second. 

You can also see that your 4 Sprites are on the first 4 entries in the line, which means that each only stays on the screen for 1/60 (so 0.016) seconds.To fix that, simply set Samples to 4 to make the animation change only 4 times each second, so each of your Sprites stays on the screen for ¼ of a second. 

Your animation should now run at a proper speed. Feel free to play with that value to make it run at the speed you want.

That’s one animation done! We need to do 3 more to do the full set! 

### Creating an Animation

1. To create an animation, click on the current animation name in the top left of the window, select Create New Clip, enter the name Right_Run and choose the Animation folder:

You may wonder how we’re going to do the walking right animation, because there is no Sprite for the robot walking right. That’s because we can simply flip the walking left animation. 

The Sprite Renderer has a Flip Property, and remember that you can animate all properties.

2. Recreate the Left animation by creating a new clip, dragging the 4 Sprites and setting Samples to 4.
3. Then click Add Property and click the triangle next to Sprite Renderer, and then click the + icon next to Flip X:

Now you can change the value of that for each frame. The current frame that you’re setting the value for is shown in the box next to the forward button on the top bar of the Animation window, and is also represented in the left side with the vertical white bar.

4. Set it to true by checking the Toggle next to the Property name while on the frame 0 and frame 4 so the Flip stays checked for the entire animation:
Notice that diamonds have appeared on the same line as the Flip Property for both the 0 and 4th frame. The diamonds mean there is a change in value at that frame. If there is no diamond, then that frame will use the last value that was set.
5. Repeat the operation (create a new clip, drag the frames, set the sample to 4) for both the running up and running down animation.
Now that you have all our walking animations for the robot, it’s time to set up the Controller you created earlier!

### Building the Controller

The Controller defines how the animations are related, as in how to go from one animation to another.

To edit the Controller:

Open the Animator window (menu: Windows > Animation > Animator). 

NOTE: Make sure that the Robot Prefab or Robot Animator is selected in your Project folder.

The Animator has two parts, with the Layers and Parameters on the left side and Animation State Machine on the right side:  

#### Part 1: Layers and Parameters

Layers are useful for 3D animations because they allow you to use animations on different parts of the character. 

Parameters are used by our scripts to give information to the Controller.

#### Part 2: Animation State Machine

An Animation State Machine is a graph of all the states of your animations and how they transition from one to another.

Right now you have the four animations created, and the first one, RobotLeft, is linked to Entry, which means that animation will play when the game starts.

You could create links between all animations to say, for example, “change the robot’s direction from left to right when the player makes the robot turn to the right”. But that would mean each animation will have three links, one for each of the other animations.

An easier way is to use a Blend Tree, which allows you to blend multiple animations depending on a Parameter. 

Here, you will blend your four animations based on the direction that the robot is moving, so the Controller will play the right animation.


### Using a Blend Tree
1. Start by deleting all the animations by selecting them and pressing delete, or right-clicking on them and choosing Delete. 
2. Then right-click somewhere in the graph and select Create State > From New Blend Tree. 
Now when the game starts, the Controller will play the Blend Tree. But right now it’s empty, so it won’t play any animations.
3. Double-click on the Blend Tree to open it.
4. Now click on that “Blend Tree” node and the Inspector will display its settings: 

- The Blend Type setting defines how many parameters the Blend tree will use to select which animation to play. You want your Blend Tree to use two parameters to control the horizontal and vertical direction changes, so set that to 2D Simple Directional. 
- So, for example, when the robot has a horizontal movement of 0 and vertical movement of -1, the Blend Tree should select the RobotDown animation, and when the robot has a horizontal movement of 1 and a vertical movement of 0, the Blend Tree should select the RobotRight animation.
- Under the Blend Type setting, you will now have two Parameters settings. The Controller uses these values to select which animation to play. 
- Right now, the Controller uses the auto-created Blend parameter, but we’re going to replace this with 2 parameters called Move X and Move Y that will have the horizontal and vertical movement amount.

### Parameters Move X and Move Y

To create new parameters:
1. Just go to the Parameters tab in the Animator window, on the left side of the window:

You need two float type parameters because you want a parameter that can go from -1.0 to 1.0. 

2. Rename the Blend parameter (which is a float type) by selecting it and then clicking on it, and then naming it Move X. 
3. Next, click the + icon next to the search bar, choose Float and name the new parameter Move Y. 

Now when you select your Blend Tree, you can choose those 2 parameters in the dropdowns at the top of the Inspector, to tell your Blend Tree it should watch those 2 parameters to choose which animation to play.

4. Then click on the + at the bottom of the Motion section and select Add Motion Field 4 times, because you want to blend four animations. Your Inspector should end up looking like this:
 
5. Now drag and drop your four animation clips into the four slots named Motion, and set their Pos X and Pos Y values to the corresponding direction, like this:

Left : Pos X = -0.5 Pos Y = 0
Right: Pos X = 0.5 Pos Y = 0
Up: Pos X = 0 Pos Y = 0.5
Down: Pos X = 0 Pos Y = -0.5

The image represents the blending, where each blue diamond is one clip, and the red dot is the position given by the value of the 2 parameters. 

6. Right now it’s in the center, as if Move X and Move Y are equal to 0. But you can see that if Move Y is set to 1 for example, the red dot will go to the top of the square (as Y is the position vertically) and the tree will then select the top animation (running up) because that’s the closest.  

You can see the value simulated for Move X and Move Y in the Animator on the Blend Tree node.

7. Or you can move the red dot directly. At the bottom of the Inspector, you’ll see a simulation of which animation the Controller would select for those given value. Press the Play button to see it in motion. 

Congratulations! You have finished setting up your animation Blend Tree! 
Now you just need to send the data from your EnemyController script to the Controller to get this working properly in your game.


### Sending parameters to the Animator Controller

To send Parameters to the Animator Controller:

1. Open the EnemyController script. 

The component we want to interact with is the Animator, and like we did the other time we need to interact with a component on the GameObject, we will retrieve it in the Start function with GetComponent and store it inside a class variable. 

```csharp
Animator animator;

void Start()
{
     rigidbody2D = GetComponent<Rigidbody2D>();
     timer = changeTime;
     animator = GetComponent<Animator>();
}
```

2. Now we need to send the Parameter values to the Animator. We can do this through the SetFloat function on the Animator, because we’re using float type parameters. 
3. The SetFloat function takes the name of the parameter as first parameter and as second parameter the current value of that parameter, here the amount of movement in a given direction, so in the Update function, you will add:

In the first part of the movement, inside the if(vertical) block, add: 

```csharp
animator.SetFloat("Move X", 0);
animator.SetFloat("Move Y", direction);
```

In this part, when your Robot moves vertically, 0 is sent to the horizontal parameter, and the direction will define whether the Robot is moving upward or downward.

4. In the else block, so when the Robot is moving horizontally,  send the inverse:

```csharp
animator.SetFloat("Move X", direction);
animator.SetFloat("Move Y", 0);
```

5. Now you can press Play and check that the right animation is chosen by the Animator for the Robot! 

### Setting Animation for the Main Character

To speed up the process (this has already been a long tutorial!), you will find  the Controller for Ruby has been provided to you in your Project.

1. You just need to add an Animator to your Ruby Prefab, and assign the RubyController Animator found in the Animation folder to the Controller slot.
2. If you open the Animator with your Prefab selected, you will see Ruby Animator Controller that look something like this: 

Ignore the Launch state for now, this is a small spoiler for the next tutorial about launching a projectile!

3. For now, just note that we have three states that are three Blend Trees: 
Moving: plays when Ruby runs.

Idle: plays when Ruby is standing still.
Hit: plays when Ruby collides with a Robot or a Damage Zone.
The white arrows between the states are Transitions. They define the movement between states. For example, there is no Transition between Hit and Launch because Ruby can’t throw a Projectile when she is damaged!

4. Click on the downward arrow between Moving and Idle, and look at the Inspector to see the setup for that Transition:

Most of those settings, like the bars on the graph, are useful for 3D where animation can be blended. In 2D, only a couple are useful for us.
Note that Has Exit Time is unchecked. That means the moving animation won’t wait to finish before the State Machine goes to the Idle animation, it will instantly change.

The most important part is the Conditions section at the bottom. A Transition will happen in two cases:

1. If there is no condition, the Transition will happen at the end of the Animation. Here, it can’t work because our animation is looping, but if you look at the Transition going from Hit to Idle, that’s what is happening: Has Exit Time is ticked, and no condition is set, meaning the hit animation will play once then transition back into Idle.
2.  Or, we can set a condition based on your parameters. Here, if the Ruby’s speed becomes less than 0.1, then the State Machine will transition from Moving to Idle.

You can click on the other Transitions to look at them and see how they work.
You may wonder why the Hit parameter looks different in the Parameters list. That’s because it’s not a float. Being hit is not a “quantity” like movement or speed, it’s a one-time event. 

That type of parameter is called a “Trigger”. If you use that parameter as the condition for a Transition, the Transition will happen if the Trigger is activated from code (in our case, when the character gets hit.)

### Modifying the RubyController Script

Now that you have seen how the Controller was set up, let’s modify your RubyController script to send those parameters to the Controller.
1.  Just like for the Robot, add an Animator variable and, in the Start function, use GetComponent to retrieve the Animator and store it in that variable. 
2. You will also add a Vector2 variable called lookDirection initialized to (1,0):

```csharp
Animator animator;
Vector2 lookDirection = new Vector2(1,0);

void Start()
{
    animator = GetComponent<Animator>();
```

Why are you storing the look direction? Because compared to the Robot, Ruby can stand still.  When she stands still, Move X and Y are both 0, so the State Machine doesn’t know which direction to use unless we tell it. 

3. Make lookDirection store the direction that Ruby is looking so you can always provide a direction to the State Machine. Indeed, if you look at the Animator parameter, it expects a Look X and a Look Y parameter.
4. You will send those Look parameters and the Speed from the Update function. You will just need to change a bit of your code. Right now, it looks like this : 

```csharp
float horizontal = Input.GetAxis("Horizontal");
float vertical = Input.GetAxis("Vertical");
```

 5.  Let’s change it to: 

```csharp
float horizontal = Input.GetAxis("Horizontal");
float vertical = Input.GetAxis("Vertical");
                
Vector2 move = new Vector2(horizontal, vertical);
        
if(!Mathf.Approximately(move.x, 0.0f) || !Mathf.Approximately(move.y, 0.0f))
{
    lookDirection.Set(move.x, move.y);
    lookDirection.Normalize();
}
        
animator.SetFloat("Look X", lookDirection.x);
animator.SetFloat("Look Y", lookDirection.y);
animator.SetFloat("Speed", move.magnitude);
```

 6. Let’s look at the changes you’ve made:

```csharp
Vector2 move = new Vector2(horizontal, vertical);
position = position + move * speed * Time.deltaTime;
```

Instead of doing x and y independently for the movement, you store the input amount in a Vector2 called move. 

7. It will do exactly the same as what you did before, but in a single line, handling x and y at the same time.

```csharp
if(!Mathf.Approximately(move.x, 0.0f) || !Mathf.Approximately(move.y, 0.0f))
```

8. Check to see whether move.x or move.y isn’t equal to 0. 
9. Use Mathf.Approximately instead of == because the way computers store float numbers means there is a tiny loss in precision. 

So you should never test for perfect equality because an operation that should end up giving 0.0f could instead give something like 0.0000000001f instead. Approximately takes that imprecision into account and will return true if the number can be considered equal minus that imprecision.

```csharp
lookDirection.Set(move.x, move.y);
```

10. If either x or y isn’t equal to 0, then Ruby is moving, set your look direction to your Move Vector and Ruby should look in the direction that she is moving. If she stops moving (Move x and y are 0) then that won’t happen and look will remain as the value it was just before she stopped moving. 

**Note:** you could have done lookDirection = move but that shows another way of assigning x and y of a vector.

```csharp
lookDirection.Normalize();
```

11. Then call Normalize on your lookDirection to make its length equal to 1. As said before, Vector2 types store positions, but they can also store directions! 
(1,0) stores the position being one unit to the right of the center of our world, but it also stores the direction right (if you trace an arrow from 0,0 to 0,1 you get an arrow pointing to the right). 

The length of a Vector defines how long that arrow is. So, for example, a Vector2 equal to (0,-2) has a length of 2 and points down. If we normalize that Vector, it will become equal to (0,-1), so still pointing down but of length 1. 

In general, you will normalize vectors that store direction because length is not important, only the direction is. 

**NOTE:** You should never normalize a vector storing a position because as it changes x and y, it changes the position!

```csharp
animator.SetFloat("Look X", lookDirection.x);
animator.SetFloat("Look Y", lookDirection.y);
animator.SetFloat("Speed", move.magnitude);
```

12. Then you have the three lines that send the data to the Animator, which is the direction you look in and the speed (the length of the move vector). 

If Ruby doesn’t move, it will be 0, but if she does then it will be a positive number. Length is always positive, remember from the example above, a vector (0,-2) has a length of 2!

Then you can press Play and try moving Ruby around to test all the animations.

The only bit left is triggering the hit animation. Sending a trigger to the Animator is done through animator.SetTrigger (“Trigger name”). So in the ChangeHealth function, inside the if(amount < 0) block, we just need to add :

```csharp
animator.SetTrigger("Hit");
```
### Check Your Scripts

Your RubyController script should now look like this:

```csharp
public class RubyController : MonoBehaviour
{
    public float speed = 3.0f;
    
    public int maxHealth = 5;
    public float timeInvincible = 2.0f;

    public int health { get { return currentHealth; }}
    int currentHealth;
    
    bool isInvincible;
    float invincibleTimer;
    
    Rigidbody2D rigidbody2d;
    float horizontal;
    float vertical;
    
    Animator animator;
    Vector2 lookDirection = new Vector2(1,0);
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
        animator = GetComponent<Animator>();
        
        currentHealth = maxHealth;
    }

    // Update is called once per frame
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
        
        Vector2 move = new Vector2(horizontal, vertical);
        
        if(!Mathf.Approximately(move.x, 0.0f) || !Mathf.Approximately(move.y, 0.0f))
        {
            lookDirection.Set(move.x, move.y);
            lookDirection.Normalize();
        }
        
        animator.SetFloat("Look X", lookDirection.x);
        animator.SetFloat("Look Y", lookDirection.y);
        animator.SetFloat("Speed", move.magnitude);
        
        if (isInvincible)
        {
            invincibleTimer -= Time.deltaTime;
            if (invincibleTimer < 0)
                isInvincible = false;
        }
    }
    
    void FixedUpdate()
    {
        Vector2 position = rigidbody2d.position;
        position.x = position.x + speed * horizontal * Time.deltaTime;
        position.y = position.y + speed * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }

    public void ChangeHealth(int amount)
    {
        if (amount < 0)
        {
            animator.SetTrigger("Hit");
            if (isInvincible)
                return;
            
            isInvincible = true;
            invincibleTimer = timeInvincible;
        }
        
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }
}

```

Your EnemyController script should now look like this:

```csharp
public class EnemyController2 : MonoBehaviour
{
    public float speed;
    public bool vertical;
    public float changeTime = 3.0f;

    Rigidbody2D rigidbody2D;
    float timer;
    int direction = 1;
    
    Animator animator;
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2D = GetComponent<Rigidbody2D>();
        timer = changeTime;
        animator = GetComponent<Animator>();
    }

    void Update()
    {
        timer -= Time.deltaTime;

        if (timer < 0)
        {
            direction = -direction;
            timer = changeTime;
        }
    }
    
    void FixedUpdate()
    {
        Vector2 position = rigidbody2D.position;
        
        if (vertical)
        {
            position.y = position.y + Time.deltaTime * speed * direction;
            animator.SetFloat("Move X", 0);
            animator.SetFloat("Move Y", direction);
        }
        else
        {
            position.x = position.x + Time.deltaTime * speed * direction;
            animator.SetFloat("Move X", direction);
            animator.SetFloat("Move Y", 0);
        }
        
        rigidbody2D.MovePosition(position);
    }
    
    void OnCollisionEnter2D(Collision2D other)
    {
        RubyController player = other.gameObject.GetComponent<RubyController >();

        if (player != null)
        {
            player.ChangeHealth(-1);
        }
    }
}
```

### Summary

That was a long section! But now you have seen how animation works in Unity. Animation clips are the Assets that store the data for the animation. 

The Controller stores the State Machine that defines how those animations relate to each other. 

And the Animator plays the animation that the Controller assigns to it and tells it to play. 

We can send data to the Controller through the Animator in our script to select the right animation based on gameplay!

In the next section we will take a look at launching Projectiles with Ruby to finally fix those broken robots!
