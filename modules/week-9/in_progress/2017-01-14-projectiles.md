---
title: Projectiles
module: 9
jotted: true
---

# Creating the Projectile

Now you will look at how you can create your Projectile:

1. To create your projectile, you will use the sprite in the Art > Sprites > VFX folder called CogBullet. 
2. To make your cog smaller, select it in the Project window, set the Pixels Per Unit to 300 in the Inspector and click Apply. You can play around with that value if you want to make the projectile smaller or bigger.
    - Drag and drop the Sprite in the Hierarchy window.
    - You need to give your projectile GameObject a BoxCollider2D and a Rigidbody2D to detect any collisions. This is important because the cog will be moving and might hit something. 
    - This time, the default Box Collider that is the full size of the Sprite will work fine because you want all of your sprite to collide with things. Make sure to set the Gravity Scale of the Rigidbody2D to 0. 

3. Now let's create a new script called Projectile in the scripts folder,
Instead of moving the Projectile yourself as you did for the main character or enemies, this time you will use the Physics System. 
    - By giving a force to a Rigidbody, the Physics System moves the RigidBody for you based on the force, and you don’t have to change its position in the Update function manually.


### The Physics System

You will now learn how you can create your Physics System:

1. First, you will need to create a Rigidbody2d variable. Then store the Rigidbody in the Start function as you did previously. 

You will need to create a Rigidbody2d variable and store our Rigidbody in it in the Start function like you did previously.

```csharp
Rigidbody2D rigidbody2d;
    
void Start()
{
    rigidbody2d = GetComponent<Rigidbody2D>();
}
```

2. Then create a function called Launch. Use a Vector2 direction as a parameter, and a float force. You’ll use these to move the Rigidbody: the higher the force, the faster it goes.

This function calls AddForce on the Rigidbody with the force being the direction multiplied by the force. When that force is added, the Physics Engine will move the Projectile every frame based on that force and direction. 

```csharp
public void Launch(Vector2 direction, float force)
{
    rigidbody2d.AddForce(direction * force);
}
```

3. Because you want to detect collision, you will need an OnCollisionEnter function. 

For now, you’ll just Destroy the object, but you’ll change this to fix the robot later on:

```csharp
void OnCollisionEnter2D(Collision2D other)
{
    //we also add a debug log to know what the projectile touch
    Debug.Log("Projectile Collision with " + other.gameObject);
    Destroy(gameObject);
}
```

4. Add that script to your Projectile, and make a Prefab out of the Projectile. When your Projectile is a Prefab, you can delete the one in the Scene, because you don’t want a Projectile by default in the Scene.  

### Launch the Projectile

Now that you have a Prefab of your Projectile, you need to throw it so you can fix those robots:

1. Create a public variable in our RubyController script that you will call projectilePrefab. You need to make it a GameObject type because that’s what Prefabs are; GameObjects saved as Assets. And because you’ve made it public, it appears in your Editor as a slot where you can assign any GameObject. 
2. Drag and drop the projectile Prefab into that slot. Remember that if you did that in the Scene, it will be an override to your Prefab (with a blue line next to the entry), so use the Overrides drop-down in the top right corner to apply them to the Prefab.
3. Next, write a Launch function in the RubyController script that will be called when you want to launch a Projectile (such as when a keyboard key is pressed). 

```csharp
void Launch()
{
    GameObject projectileObject = Instantiate(projectilePrefab, rigidbody2d.position + Vector2.up * 0.5f, Quaternion.identity);

    Projectile projectile = projectileObject.GetComponent<Projectile>();
    projectile.Launch(lookDirection, 300);

    animator.SetTrigger("Launch");
}
```

### What is Instantiate?

Instantiate is a Unity function that you haven’t seen yet. Instantiate takes an object as the first parameter and creates a copy at the position in the second parameter, with the rotation in the third parameter. 

Let's look at the steps to Instantiate:
1. The object you will copy is your Prefab and you will place it at the position of your Rigidbody (but offset it a bit upward so the object is near Ruby’s hands not her feet), with a rotation of Quaternion.identity. 
Quaternions are mathematical operators that can express rotation, but all you need to remember here is that Quaternion.identity means "no rotation".
2. Then you will get the Projectile script from that new object and call the Launch function you wrote earlier with the direction being where your character is looking and the force value field set at 300. 
    - The force value is set very high because it is expressed in Newton units. This value is a good value for your game, but if you want to play with the strength, you can expose a public float variable and use that as the force.
    - Also, you can see that a trigger has been set for your Animator (see how this works in the previous tutorial). This will make your Animator play the launching animation!
3. The last part is to detect when the player presses a key, and call Launch when they do. 

Add this to the end of the Update function in the RubyController script:

```csharp
if(Input.GetKeyDown(KeyCode.C))
{
   Launch();
}
```

### Launching a Cog
To launch a Cog you will need to:

1. Use the Input class you saw earlier, but this time you will use GetKeyDown, which allows you to test a specific keyboard key press, here the key “C”. That will only work on a keyboard.
2. If you want to make it work across devices, you can use Input.GetButtonDown with an axis name, like you did for movement, and define which button that axis corresponds to in the Input settings (Edit > Project Settings > Input). For an example, take a look at the Axes > Fire1.
3. Now press Play and then C to launch a Cog. You should either see no Cog or a Cog briefly appearing then disappearing immediately. 


### Fixing Error Lines

Your console has two error lines:

1. A null reference exception
    - If you double-click on the null reference exception error, it will open your Projectile script at the line Rigidbody2d.AddForce (direction * force), which means your Rigidbody2d variable is empty (contains null), despite us getting the Rigidbody in Start. 
    - That’s because Unity doesn’t run Start when you create the object, but on the next frame. So when you call Launch on your projectile, just Instantiate and don’t call Start, so your Rigidbody2d is still empty. To fix that, rename the void Start() function in the Projectile script to void Awake(). 
    - Contrary to Start, Awake is called immediately when the object is created (when Instantiate is called), so Rigidbody2d is properly initialized before calling Launch.
2. A log telling us the Cog collided with Ruby.
    - Now for your second problem: your Projectile colliding with Ruby. This is because you create it on Ruby, so as soon as it’s created, the Physics System lets you know that the projectile Rigidbody is colliding with Ruby collider. And you call Destroy in your OnCollisitonEnter function, so it gets destroyed immediately.
    - You could test to see if the object collides with Ruby and then don’t destroy the projectile. But you’ll still have the problem that when it collides, it will stop moving immediately.

### Layers and Collisions

The right way to fix this collision is to use Layers. Layers allow you to group GameObjects together so they can be filtered. Your aim is to make a Character layer to put your Ruby GameObject in, and a Projectile layer to put all your projectiles in. 

Then you can tell your Physics System that the Character and Projectile layers can’t collide, so it will ignore all collisions between objects in those layers.

1. To see which layer a GameObject is in, click the Layers drop-down in the top right corner of the Inspector. All objects start in the Default layer (layer number 0). Your game can have up to thirty-two different layers. 
2. If you click on it, a pop up will show some built-in layer already defined by Unity, but you want to create your own, so choose Add Layer. This will open the Layer manager. 

Layers 0 to 7 are locked by Unity and you can’t change them. So in Layer 8 enter Character and in Layer 9 enter Projectile.

3. Now open your Ruby Prefab and change its Layer to Character. Save the Prefab and do the same to the Projectile Prefab, but this time setting its layer to Projectile.
4. Then open Edit > Project Settings > Physics 2D and look to the Layer Collision Matrix the part at the bottom, to see which layers collide with which: 

By default, all are ticked, so all layers collide with every other layer, but you want to uncheck the intersection between the Character row and Projectile column, so those two layers don’t collide anymore.
5. Now you can enter Play Mode, and your cog won’t collide with the Ruby anymore, but it still collides with other objects like boxes or enemies. 

### Fixing the Robot

The first step is to write a function in the Enemy script to fix the robot and handle how the robots react when they are fixed. For now, your Projectile just destroys itself on collision. But the goal is to fix those aggressive broken robots with it. 

To fix your Robot:

1. To write a function in the Enemy script ,add a bool variable called broken and initialize it to true so that the robot starts broken. 
2. At the beginning of the Update function, add a test to see if the robot is not broken. Then exit the function if it’s the case. You need to add this code to both Update and FixedUpdate.  

When fixed, your Robot will stop moving.  
For this reason,exiting the update function early will stop the code that moves them to be executed:

```csharp
void Update()
{
	//remember ! inverse the test, so if broken is true !broken will be false and return won’t be executed.
if(!broken)
{
    return;
}
 
void FixedUpdate()
{
  if(!broken)
  {
      return;
  }
```

3. And to finish, write the function that can be called to fix the robot. This should be very simple: 

```csharp
//Public because we want to call it from elsewhere like the projectile script
public void Fix()
{
    broken = false;
    rigidbody2D.simulated = false;
}
```

You have just set broken to false and the simulated property of the Rigidbody2d to false.

This removes the Rigidbody from the Physics System simulation, so it won’t be taken into account by the system for collision, and the fixed robot won’t stop the Projectile anymore or be able to hurt the main character.

4. Now all that is left to do is to modify the OnCollisionEnter2D function from the Projectile script. Get an EnemyController from the object the Projectile collided with, and if the object gets one, it means you have fixed that enemy. 

```csharp
void OnCollisionEnter2D(Collision2D other)
{
    EnemyController e = other.collider.GetComponent<EnemyController>();
    if (e != null)
    {
        e.Fix();
    }
    
    Destroy(gameObject);
}
```

**NOTE:** You have also removed the Debug.log  because you  don’t need it anymore.

Now the Projectile will fix the robot who will stop moving. You can then go against them and won’t be damaged. 

### Cleanup

One minor issue with your current solution is that, if you get Ruby to throw the Cog and it doesn’t collide with anything, the Cog will keep on going outside of the screen for as long as the game runs. 

As the game progresses, this could cause a performance problem if suddenly you have 500 cogs moving outside of the view.

To fix this, you will simply need to check the cog’s distance from the center of the world and, if it is far enough away from Ruby that she'll never reach it (let’s say 1000 for your game), then you will destroy the Cog.

Let’s add this Update function to the Projectile script: 

```csharp
void Update()
{
    if(transform.position.magnitude > 1000.0f)
    {
        Destroy(gameObject);
    }
}
```

Remember that position can be seen as a vector from the center of our world to where your object is, and magnitude is the length of that vector. So the magnitude of the position is the distance to the center.

Of course there are other ways to handle this, depending on the game. For example, you could get the distance between the character and the cog (with the function Vector3.Distance (a,b) to compute the distance between the position a and position b). 

Or you could use a timer in the Projectile script, so when the Cog is launched you set your timer to something like 4 seconds, you would decrement it in the Update function and then destroy the Cog when the timer reaches 0.


### Optional : Animating the Fixed Robot

This step is optional as it won’t add features to the game and isn’t related to the Projectile, but it will make the game prettier. 

You will add the animation for the fixed robot. Please refer to the previous section for more details on how to create animations and use the Animator.

1. Create a new animation clip for the enemy (using the MrClockworkFixed frames 1, 2, 3 and 4) called RobotFixed. Remember to set the Sample to 4. 
2. In the Enemy Animator, create a transition from your running Blend Tree to that new animation. Don’t forget to disable Has Exit Time. There’s no need to do a transition the other way around because, once fixed, our robot will stay fixed.
3. Create a parameter of type Trigger called Fixed, and set it as the condition for the transition:

Now in your Enemy script, in the Fix function, just add the line: 

```csharp
animator.SetTrigger("Fixed");
```

Enter Play mode and launch your Cog on a robot to fix it. It should now happily dance!


### Check Your Scripts

Your RubyController script should now look like this:

```csharp
public class RubyController : MonoBehaviour
{
    public float speed = 3.0f;
    
    public int maxHealth = 5;
    
    public GameObject projectilePrefab;
    
    public int health { get { return currentHealth; }}
    int currentHealth;
    
    public float timeInvincible = 2.0f;
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
        
        if(Input.GetKeyDown(KeyCode.C))
        {
            Launch();
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
            if (isInvincible)
                return;
            
            isInvincible = true;
            invincibleTimer = timeInvincible;
        }
        
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }
    
    void Launch()
    {
        GameObject projectileObject = Instantiate(projectilePrefab, rigidbody2d.position + Vector2.up * 0.5f, Quaternion.identity);

        Projectile projectile = projectileObject.GetComponent<Projectile>();
        projectile.Launch(lookDirection, 300);

        animator.SetTrigger("Launch");
    }
}
```

Your EnemyController script should now look like this:

```csharp
public class EnemyController : MonoBehaviour
{
    public float speed;
    public bool vertical;
    public float changeTime = 3.0f;

    Rigidbody2D rigidbody2D;
    float timer;
    int direction = 1;
    bool broken = true;
    
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
        //remember ! inverse the test, so if broken is true !broken will be false and return won’t be executed.
        if(!broken)
        {
            return;
        }
        
        timer -= Time.deltaTime;

        if (timer < 0)
        {
            direction = -direction;
            timer = changeTime;
        }
    }
    
    void FixedUpdate()
    {
        //remember ! inverse the test, so if broken is true !broken will be false and return won’t be executed.
        if(!broken)
        {
            return;
        }
        
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
    
    //Public because we want to call it from elsewhere like the projectile script
    public void Fix()
    {
        broken = false;
        rigidbody2D.simulated = false;
        //optional if you added the fixed animation
        animator.SetTrigger("Fixed");
    }
}
```

Your Projectile script should now look like this:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Projectile : MonoBehaviour
{
    Rigidbody2D rigidbody2d;
    
    void Awake()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
    }
    
    public void Launch(Vector2 direction, float force)
    {
        rigidbody2d.AddForce(direction * force);
    }
    
    void Update()
    {
        if(transform.position.magnitude > 1000.0f)
        {
            Destroy(gameObject);
        }
    }
    
    void OnCollisionEnter2D(Collision2D other)
    {
        EnemyController e = other.collider.GetComponent<EnemyController>();
        if (e != null)
        {
            e.Fix();
        }
    
        Destroy(gameObject);
    }
}
```

### Summary

This tutorial dived deeper into the Physics System with how layers are used or how force  works on a RigidBody so that the Physics System moves objects for you.
In the next tutorial, you will finally make the world bigger by allowing your camera to move and follow your player around.
