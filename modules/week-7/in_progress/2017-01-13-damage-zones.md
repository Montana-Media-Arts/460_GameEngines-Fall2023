---
title: Damage Zones and Enemies
module: 7
jotted: true
---

# Damage Zones and Enemies

<a href="../imgs/bot.png" target="_new">Download the Bot</a>

## Reset Ruby’s Health

First, let's make sure you don’t start with Ruby's Health set to 1, which you changed in the previous section:

1. Open the RubyController script.
2. Check that the Start function sets currentHealth to maxHealth and not 1:

```csharp
currentHealth = maxHealth;
```

Now that Ruby starts the game with full health again, you’re ready to start the tutorial. 
 
### Add a Damage Zone

First, add a zone that damages Ruby. This will be exactly like the health collectible, except it will change the health by -1 and it won't destroy itself when the character triggers it. 

Previous tutorials have covered everything you need to know to add a damage zone, so have a go yourself first! You’ll find a Sprite for the zone in Assets > Art > Sprites > Environment called Damageable.  
 
### Damage Zone Solution

Now let’s review the solution:

1. Create a new GameObject from the Damageable Sprite. You can choose from the following approaches: 
    - Drag and drop the Sprite in the Hierarchy window.
    - Create a new GameObject, then add a Sprite Renderer component and assign the Sprite to that.
2. Add a Box Collider 2D to the Damageable GameObject, then resize the box to fit the Sprite and enable the Is Trigger checkbox in the Inspector.
3. Create a new script called DamageZone. In the Project window, go to Assets > Scripts folder. Right-click and select Create > C# Script.
4. Double-click the script in the Project window to open it in your code editor, then copy the following code into the new script, replacing the class already in there. The damage zone script is exactly the same as the collectable script from previous tutorial, but it changes Health by -1, and you’ve removed the call to Destroy:

```csharp
public class DamageZone : MonoBehaviour
{
    void OnTriggerEnter2D(Collider2D other)
    {
        RubyController controller = other.GetComponent<RubyController >();

        if (controller != null)
        {
            controller.ChangeHealth(-1);
        }
    }

}
```

Then add that script to your damageable GameObject. 

Press Play and get Ruby to walk around the zone. Look at your Console - it’s printing Ruby’s new health!

This is nice, but it will only damage the character when they enter the zone. If they stay there inside it, it won’t damage them anymore. You can fix that by changing the function name from OnTriggerEnter2D to OnTriggerStay2D. This function is called every frame the Rigidbody is inside the Trigger instead of just once when it enters.

Now Ruby gets damage all the time, and maybe too much! She’s getting to 0 health in a fraction of a second (well, in exactly the numbers of frames equal to her health). And you may also have noticed that you don’t get any messages on the Console when you stop moving Ruby around, so she’s not getting damaged when she stands still.

To fix the last problem, you will need to open your character Prefab and in the Rigidbody, set the Sleeping Mode to Never Sleep: 

To optimize resources, the Physics System stops computing collision for a Rigidbody when it stops moving; the Rigidbody “sleeps”. But in your case, you want the computation to happen all the time, because you need to detect whether Ruby is hurt even when she stops moving, so you will instruct the Rigidbody to never sleep.

Now, to fix the problem of Ruby losing all her health in five frames, you can make Ruby invincible for a short period of time. That way, you can ignore any damage she receives for as long as she’s invincible. Let’s go back to your RubyController script and make these modifications:

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
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
        currentHealth = maxHealth;
    }

    // Update is called once per frame
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
        
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

Let's look at the script changes in detail!

Add three new variables:

- Variable 1: A public float called timeInvincible.  Make this public because you want to be able to change it dynamically in your Inspector to tweak that value.
- Variable 2: A private bool called isInvincible to store if you are currently invincible or not. A bool (short for boolean) allows us to store either “true” or "false". It is especially useful in if statements.
- Variable 3: A private float called invincibleTimer. This will store how much time Ruby has left being invincible before reverting to being hurtable. 

Then you need to modify some functions:

#### ChangeHealth function

In ChangeHealth function, you add a check to see if you are currently hurting the character (in other words, if the change is inferior to 0, meaning you remove Health). If it’s true, you first check if Ruby is invincible already and, if she is, you are exiting the function, because she can’t be hurt right now. Otherwise, you make Ruby invincible since she is just getting hurt by setting your isInvincible bool to true and setting the invincibleTimer variable to timeInvincible.

#### Update function

In the Update function, if Ruby is invincible, you remove deltaTime from your timer. This effectively counts down time. When that time is less than or equal to zero, the timer reaches its end and Ruby’s invincibility is finished, so you remove her invincibility by resetting the bool to false. This way, next time ChangeHealth is called to hurt Ruby, you won’t exit early and damage her again, reset her invincibility and so on.

Let’s enter Play mode and test your script. If you make Ruby stay in the damage zone, she should only get damaged every two seconds, because you set two seconds as your invincible time. Play around with the values in the Inspector to change the time if you want. 


### Graphic sidenote

To take a break from code, let's highlight a feature of the Sprite Renderer. Right now, if you want to make a larger damage zone by resizing your damage zone with the Rect tool (T key), the Sprite will stretch and that looks ugly: 

But you can ask the Sprite Renderer to tile the Sprite instead or stretching it. So if you resize the damage zone to create a size that’s big enough to fit your Sprite twice, the Sprite Renderer will have drawn the Sprite multiple times side-by-side instead:

To do this:

1. First, make sure that the scale of your GameObject is set to 1,1,1 in the Transform Component
2. Then in the Sprite Renderer component, change the Draw Mode to Tiled, and change the Tile Mode to Adaptive.

This will display a warning telling you that the Sprite may display wrong.

You can follow the instruction to fix that. Select the Damageable Sprite in your Project window, and change the Mesh Type to Full Rect.

Press Apply at the bottom of the Inspector. Now if you click on your Damage Zone in your Hierarchy, the Inspector should not display the warning anymore.

Now when you resize the GameObject with the Rect tool, you will see that it stretches until it can fit two Sprites, and then it shows the two Sprites instead of over stretching! Beware: This only works if you use the Rect tool NOT the Scale tool, because scale changes the scale of your GameObject, not the tiling size that you can see under the Draw Mode entry in the Sprite Renderer Inspector.

But you can see that your Collider didn’t follow the scaling. Check the Auto Tiling property of your Box Collider 2D so it also tiles with the Sprite.

### Enemies

After having roamed around in an empty world, it is time to add some other characters to your Scene. Let’s add enemies because enemies are a kind of moving damage zone.
Save the following image on your computer by downloading it from the materials at the top of this tutorial, import it into Unity and place it in the Scene.

Just like your main character, since you want the enemies to move and collide with your main character and the environment, it needs both a Rigidbody2D and a BoxCollider2D.

Don't forget to set the gravity scale to 0 and constrain rotation like you did on your main character.

### Back and Forth

Create a new script called EnemyController, and attach it to your enemy character. Now let’s write a script to make it go up and down in a loop.

You should have a pretty good idea about what you will need from all your previous tutorials, so you can try to do it by yourself before looking at the solution. Here are a couple of important reminders :

You need to get the Rigidbody2D in a variable in your script and use MovePosition on it to move your GameObject. Remember to do this in FixedUpdate.
Don’t forget you can expose a variable with public to tweak it in the Editor, like the speed for example.

Here’s the solution:

```csharp
public class EnemyController2 : MonoBehaviour
{
    public float speed;

    Rigidbody2D rigidbody2D;
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2D = GetComponent<Rigidbody2D>();
    }
    
    void FixedUpdate()
    {
        Vector2 position = rigidbody2D.position;
        position.x = position.x + Time.deltaTime * speed;
        
        rigidbody2D.MovePosition(position);
    }
}
```

**Note:** When you go back to Unity and that script gets compiled, a warning will appear in your Console:
“warning CS0108: 'EnemyController.rigidbody2D' hides inherited member 'Component.rigidbody2D'.” 

This is because Monobehaviour, the base class you are using for your scripts, has a (now unused) member also called rigidbody2d.  

Unity tells you that your own rigidbody2d will take over it, but that’s what you want, because it is useful so you can ignore that warning, everything is fine!
Now let’s change your script so that you can move the enemy horizontally or vertically, back and forth, in the Editor.

For the direction, let’s use a public bool called vertical. You can do a test in Update to see if vertical is true and, if it is, move the enemy along the y axis instead of the x axis in your world.

```csharp
public class EnemyController : MonoBehaviour
{
    public float speed;
    public bool vertical;
    
    Rigidbody2D rigidbody2D;
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2D = GetComponent<Rigidbody2D>();
    }
    
    void FixedUpdate()
    {
        Vector2 position = rigidbody2D.position;
        
        if (vertical)
        {
            position.y = position.y + Time.deltaTime * speed;
        }
        else
        {
            position.x = position.x + Time.deltaTime * speed;
        }
        
        rigidbody2D.MovePosition(position);
    }
}
```

For the back and forth, you need a timer, just like the one you used for invincibility. 

You can make your enemy go in one direction for as long as your timer is not zero, then reverse the direction and reset the timer, and do that infinitely. 
To do this, you need to create some more variables: 

```csharp
public class EnemyController : MonoBehaviour
{
    public float speed = 3.0f;
    public bool vertical;
    public float changeTime = 3.0f;

    Rigidbody2D rigidbody2D;
    float timer;
    int direction = 1;

```

So, you have added:

- A public float changeTime that will be the time before you reverse the enemy’s direction.
- A private float timer that will keep the current value of the timer.
- An int that is the enemy’s current direction: either 1 or -1.

Now that you have the new variables set up, you can change your EnemyController function to be:

```csharp
public class EnemyController : MonoBehaviour
{
    public float speed;
    public bool vertical;
    public float changeTime = 3.0f;

    Rigidbody2D rigidbody2D;
    float timer;
    int direction = 1;
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2D = GetComponent<Rigidbody2D>();
        timer = changeTime;
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
            position.y = position.y + Time.deltaTime * speed * direction;;
        }
        else
        {
            position.x = position.x + Time.deltaTime * speed * direction;;
        }
        
        rigidbody2D.MovePosition(position);
    }
}
```
 
Let’s break these changes down:

- In the Start function, you can initialize your timer to the time before you reverse the enemy’s direction.
- In the Update, you decrement your timer, test to see if it’s less than 0 and, if it is, you can change the direction and reset the timer. Since this is not related to physics, it doesn't have to be performed in FixedUpdate.
- Finally, you can multiply your speed by the direction.

### Damage

You've got a moving enemy! Now let’s make your enemy antagonise Ruby by making it damage her when they collide.

Unlike your damage zone, you can't use a Trigger because you want the enemy Collider to be "solid" and actually collide with things. Thankfully, Unity offers a second set of functions!

Just like you used OnTriggerEnter2D, you can also use OnCollisionEnter2D, which is called when a Rigidbody collides with something. In this case, OnCollisionEnter2D is called when your enemy collides with the world or your main character. And just like you did for the damage zone, you can test to see if the enemy has collided with your main character. 

To do this, you check whether the GameObject that the enemy collided with has got a RubyController script. If it has, then you know it’s the main character, so you can damage it:

```csharp
void OnCollisionEnter2D(Collision2D other)
{
    RubyController player = other.gameObject.GetComponent<RubyController>();

    if (player != null)
    {
        player.ChangeHealth(-1);
    }
}
```

Make sure you spell the parameter type and function names correctly! 
Another change is that you don’t use other.GetComponent but other.gameObject.GetComponent. 

That's because the type of other here is Collision2D not Collider2D. A Collision2D doesn’t have a GetComponent function, but it contains lots of data about the collision, like the GameObject with which the enemy collided. So you call GetComponent on that GameObject.

Now that your enemy is done, don’t forget to turn it into a Prefab! 
Drag and drop the enemy from the Hierarchy to the Project window. This way you can place multiple enemies in the scenes. You could even make two Prefabs out of the robot, name one fast robot and the other slow robot, change the speed value and the color of both to have multiple types of enemies, etc.


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
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
        currentHealth = maxHealth;
    }

    // Update is called once per frame
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
        
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

Your DamageZone script should now look like this:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DamageZone : MonoBehaviour
{
    void OnTriggerStay2D(Collider2D other)
    {
        RubyController controller = other.GetComponent<RubyController >();

        if (controller != null)
        {
            controller.ChangeHealth(-1);
        }
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
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2D = GetComponent<Rigidbody2D>();
        timer = changeTime;
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
            position.y = position.y + Time.deltaTime * speed * direction;;
        }
        else
        {
            position.x = position.x + Time.deltaTime * speed * direction;;
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
In this section, you have made the world your character inhabits much less friendly. You now know how to damage your character with a damage zone and moving enemies. But that world is still a bit too static. 

In the next section, you will learn how to add animation to your character and your enemy!
