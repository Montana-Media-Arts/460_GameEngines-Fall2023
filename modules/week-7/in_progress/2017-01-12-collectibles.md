---
title: Health
module: 7
jotted: true
---

# Add a Health Stat to Ruby

A good example to demonstrate collectible objects are health packs that replenish the health of your character. But before you can replenish Ruby’s health, you need to give her some health to start with. 

Let’s modify your character script to add Health to it:

1.  Open your RubyController script.
2.  Modify your script as follows:

```csharp
public class RubyController : MonoBehaviour
{
    public int maxHealth = 5;
    int currentHealth;
    
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
    }
    
    void FixedUpdate()
    {
        Vector2 position = rigidbody2d.position;
        position.x = position.x + 3.0f* horizontal * Time.deltaTime;
        position.y = position.y + 3.0f * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }

    void ChangeHealth(int amount)
    {
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }

}
```


 Let’s look at these code changes in detail.  

### Create New Variables

First, you created two new variables:

```csharp
public int maxHealth = 5;
 ```


The maxHealth variable stores the maximum amount of health that Ruby can have. Let’s break it down:

The prefix is public — you’ll find out more about this later in the tutorial.
The type is int, which is a new type you haven’t encountered before. It tells the computer that you want to store an integer number that mean a whole number without decimal points. Ruby won’t be able to have 4.325 Health or 2.97412 Health — in this case, they will have five as their maximum health. 

```csharp
int currentHealth;
```

The currentHealth variable stores Ruby’s current health, which is also an integer. This variable does not have a public prefix. 


### Set Full Health at Game Start

Then, in the Start function, you initialized the current health to be the maximum health, so when the game starts your character is in full health: 

```csharp
currentHealth = maxHealth;
```

### Add a Function to Change Health

The final part of your code is new. It’s a function to change the character’s health:

```csharp
void ChangeHealth(int amount)
{
   currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
   Debug.Log(currentHealth + "/" + maxHealth);
}
```

So far, you’ve used functions in Input.GetAxis or rigidbody.MovePosition, or written code inside the default Start and Update functions, but never written your own. Let’s review it quickly.

Function declaration

The first line is called the function declaration, and looks a lot like the Start and Update functions (a function declaration tells the compiler how to compile that function):

```csharp
void ChangeHealth(int amount)
```

Let’s break it down:

First is void, which is what your function returns. Here void means “nothing”, because the function doesn’t return any value, it just changes the Health. 
Then you named the function ChangeHealth.

Unlike the Start and Update functions, there is a variable inside the parentheses, which is a parameter. The parameter is the amount of change to the Health, so you can give the function 2 or -1 (or any other integer number).

NOTE: Remember, you’ve already used parameters in rigidbody.MovePosition, (in 4 - World Interactions - Blocking Movement), where you put the new position in parentheses. You used parentheses because you were calling the MovePosition function provided by Unity. The full MovePosition function declaration is void MovePosition (Vector2 newPosition).

ChangeHealth function

Now let’s look at the body of the ChangeHealth function, which is the code inside the curly brackets: 

```csharp
{
   currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
   Debug.Log(currentHealth + "/" + maxHealth);
}
```

This sets the current health using another built-in function called Mathf.Clamp. This is because, if you try to add an amount of 2 to Health when Ruby is already at full health, it would go over the maximum health. 

Similarly, if Ruby has 1 amount of health left and you try to remove 2, Ruby will go into negative health. 

Clamping ensures that the first parameter (here currentHealth + amount) never goes lower than the second parameter (here 0) and never goes above the third parameter (maxHealth). So Ruby’s health will always stay between 0 and maxHealth.

Display health in the Console window

Finally, you’ve added code to display the current health in the Console window, with the help of Debug.Log. Every time the health changes, this script script updates the Console. 

You don’t have any graphics or text in our game to show Ruby’s health status yet, so you can check the Console to see if everything is working. 
To make it easier to read health values in the Console, you’ve used + to merge strings together into a readable phrase. In this case, you’ve inserted a slash (/) between the currentHealth and maxHealth.

Now let’s go back to the Unity Editor and see how Unity compiles the change to the script, and check your character.  


### Check Your Changes in Unity Editor

You should now see a new property when you look at the Ruby Controller component in the Inspector: 

Your script exposes a Max Health property. That’s because you put “public” in front of the maxHealth variable. 

Public means that you can access variables from outside the script, so Unity will display them inside the Inspector. It is currently set to 5 because that’s the default you defined in the script. 
If you enter 10 there, that becomes the new value. Then, when the Start function executes, the line currentHealth = maxHealth; will set current health to 10.
 


### Exercise: Expose Another Variable

As an exercise, try to expose a new variable to be able to change the speed of your character, instead of using the value of 3.0f you defined directly in the code in the previous tutorial. 

Solution

Here’s the solution:

```csharp
public class RubyController : MonoBehaviour
{
    public float speed = 3.0f;
    
    public int maxHealth = 5;
    int currentHealth;
    
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
    }
    
    void FixedUpdate()
    {
        Vector2 position = rigidbody2d.position;
        position.x = position.x + speed * horizontal * Time.deltaTime;
        position.y = position.y + speed * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }

    void ChangeHealth(int amount)
    {
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }
}
```

### What is a Trigger?

Now that you have added health to the main character, let’s add a way to retrieve Health.

For that, you will use Triggers. Triggers are a special kind of Collider. They do not block movement, but the Physics System still checks if the character will collide with them. It sends you a message when your character enters a Trigger, so you can handle that event. 

### Create a Collectible Health GameObject

Let’s start by creating a collectible Health GameObject: 

1. First, repeat all of the steps you followed in the previous tutorial to make our metal box:
    * In the Project window, go to Assets > Art > Sprites > VFX and find CollectibleHealth.
    * Import it in your Scene, and adjust the PPU value to make it the right size.
    * Add a Box Collider 2D to the new GameObject, and resize it so that it fits the Sprite better.
2. Now if you click Play, Ruby will collide with the health collectible just like she does with the box. But that’s not what you need.
3. Exit Play Mode.
4. In the Inspector, find the Box Collider 2D component. Enable the Is Trigger property checkbox.

Now when you test your game, the character will go through the health collectible. The Physics System registers the collision, but because there is no code to handle it yet, our game doesn’t react to the collision.

### Now you can add the code to handle the collision:

1. In the Project window, go to Assets > Scripts.
2. Right-click and select Create > C# script. 
3. Name the new script HealthCollectible. 
4. In the Hierarchy, select your CollectibleHealth GameObject. Drag and drop the HealthCollectible script from the Project window to the Inspector to add it to the GameObject as a component. 
5. Double-click the script file to open it in your code editor.
6. Delete the Start and Update functions — you don’t want to make anything happen when the game starts, or at every frame.
7. You do want the script to detect when Ruby collides with the collectible health GameObject and give her some Health. To do this, use the following special function name: 

```csharp
void OnTriggerEnter2D(Collider2D other)
```

TIP: Make sure you use the correct spelling for the function name and parameter type, because Unity uses them to “find” the function on your script when it needs to call it.

In the same way that Unity calls the Update function every frame, it calls this OnTriggerEnter2D function on the first frame when it detects a new Rigidbody entering the Trigger. The parameter called other will contain the Collider that just entered the Trigger.

8. Add a simple Debug.Log to the function to check what entered the Trigger, as follows:

```csharp
public class HealthCollectible : MonoBehaviour
{
void OnTriggerEnter2D(Collider2D other)
   {
       Debug.Log("Object that entered the trigger : " + other);
   }
}
```
 
9. Return to the Unity Editor and click Play. Now when Ruby touches the collectible, a message appears in the Console, telling you that Ruby has entered the Trigger.

### Give Ruby Health

To adjust the code to give Ruby Health: 

Change the Debug.Log in your script to: 

```csharp
void OnTriggerEnter2D(Collider2D other)
{
    RubyController controller = other.GetComponent<RubyController>();

    if (controller != null)
    {
        controller.ChangeHealth(1);
        Destroy(gameObject);
    }
}
```

### Review Your Code

Let’s review your new code:

Access the RubyController component

In the first line of code, you accessed the RubyController component on the GameObject of the Collider that enters the Trigger.

If statement

Then you introduced a new scripting concept, an if statement. An if statement introduces decision making into your code. It includes a condition between the two parentheses; if that condition is true, then Unity executes the code between the two curly brackets that follow. 

Otherwise, it skips those lines of code and continues after the closing curly bracket (in this case it doesn’t do anything else, because the function finishes at that point).

The condition you have used here is:

controller != null
!= means “is not equal” (as opposed to ==, which means “is equal”). So we test whether the value stored in controller is not equal to null. Null is a special value that means “nothing/empty”.

So why are you testing whether the controller variable is empty or not? Well, if you add another GameObject that moves (such as an enemy), then entering the Trigger will call that function. But then GetComponent won’t find a RubyController on that GameObject (because they are enemies, not the player character). 

As GetComponent can’t give you a RubyController, it will return null and you can’t call the function on an empty variable. Doing this test ensures that you are only giving health to Ruby, and not creating an error trying to do it on other objects.
 
### Change Ruby’s health

Inside the if block (the code inside the curly brackets after the if condition line), you changed the health of the RubyController by 1 with the function you wrote earlier. Then you called Destroy (gameObject). 

Destroy is a built-in Unity function that destroys whatever you pass to it as a parameter —  in this case, gameObject. This is the GameObject that the script is attached to (the collectible health pack).


### Let’s check your code:

1. Return to the Unity Editor. 
2. You’ll find an error on your Console: “RubyController.ChangeHealth(int)' is inaccessible due to its protection level”. 
3. Open the RubyController script.
4. Remember when you added “public” in front of the maxHealth variable, to make it appear in the Inspector? Well it’s the same for functions. 

By default, you can only access your function from inside the same script. You could call ChangeHealth in the Update function of the RubyController script, but the HealthCollectible script can’t access that function. 

To allow HealthCollectible to access it, go to the RubyController script and do the following:

In the ChangeHealth function declaration, add “public” before the “void”.

In the Start function, set the currentHealth to 1, so your character starts with low Health.

```csharp
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
        currentHealth = maxHealth;
        currentHealth = 1;
    }

    // Update is called once per frame
    void Update()
    {
        horizontal = Input.GetAxis("Horizontal");
        vertical = Input.GetAxis("Vertical");
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
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }
```

5. Now, if you go back to Unity, the error should disappear from the Console and you can Play the Scene. 

    * Get Ruby to walk to the collectible health pack and collect it! Check to see that Ruby’s health increases in the Console window when she picks up a health pack, and that it never goes above the maxHealth.

6. Save your changes. 
7. Don’t forget to create a Prefab of your HealthCollectible GameObject — then you can use that Prefab to place multiple collectibles in your Scene. 


### Check Whether Ruby Needs Health

Before you finish this tutorial, let’s look at a more advanced C# feature: Properties.

Right now, if you grab a health collectible when Ruby’s health is full, the script will still destroy the collectible. That’s because you have set it to delete the collectible when the character enters its Trigger, no matter what.

To avoid this, you can add a check to see if Ruby needs health first before it is destroyed:

1. Open your HealthCollectible script.
2. Change the script as follows:

```csharp
void OnTriggerEnter2D(Collider2D other)
{
     RubyController controller = other.GetComponent<RubyController>();

     if (controller != null)
     {
          if(controller.currentHealth < controller.maxHealth)
          {
	       controller.ChangeHealth(1);
	       Destroy(gameObject);
          }
     }
}
```

Here, you have added another test to see if Ruby’s health is less than the maximum health. If it’s equal, that test will be false, and the script will skip your if statement (because there’s no need to change the health or destroy the collectible).

But wait before switching back to Unity! You may have guessed that you will find an error in the Console, and that would be correct; currentHealth is private, so you can’t access it from there.

You could make it public, like you did before. But making everything public can cause bugs, especially for things you don’t want to be able to access and change from anywhere. 

For example, if you make currentHealth public, you could change it to 10 in another script, even if the maxHealth is just 5. To avoid that, it is best to only allow changes through functions. This enables you to do checks — just like the ChangeHealth function, which uses a clamp to prevent health never going below 0 or above maxHealth.

So you don’t want to make it public (to prevent changes from other scripts), but you still want to allow other scripts to read the value of currentHealth, even if they can’t write it. To do this, you can use properties. 


### Define a Property in RubyController

Let’s start by defining a property in the RubyController script:

1. Open your RubyController script. 
2. To define a Property in the script, add the following line of code: 

```csharp
public class RubyController : MonoBehaviour
{
    public float speed = 3.0f;
    
    public int maxHealth = 5;

    public int health { get { return currentHealth; }}
    int currentHealth;
    
    Rigidbody2D rigidbody2d;
    
// Start is called before the first frame update
    void Start()
    {
```

You’ve started the property definition like a variable, with:

An access level (public)

A type (int)
A name (health)

However, instead using a ; to finish the line,  add two { } blocks. 

In the first block, you used the get keyword, to get whatever is in the second block. 

The second block is just like a normal function, so you just return the currentHealth value. 

The compiler handles this exactly like a function, so you can write anything you want in a function inside that get block (such as declaring a variable, doing computations and calling other functions). 


### Use the Property in HealthCollectible

You’ve defined a property — now how do you use it? 

Let’s return to the if statement:
1. Open the HealthCollectible script.
2. Find your if statement in the code: 

    * if(controller.health < controller.maxHealth)
    * Properties are used like variables, not like functions. Here, health will give you the currentHealth. But it only works like that when you read it. 

If you wanted to change it to: 

```csharp
controller.health = 10;
```

Unity would give you an error in the Console. This is because you only specified a “get” operation for health, so it’s read-only.

NOTE: The set keyword and works exactly like get, but makes the property writable. If you have a set and not a get, you can only write to the property, you can’t read it. You can look up C# set properties online if you’re curious how to use them. 


### Check Your Scripts

Your RubyController script should now look like this:

```csharp
public class RubyController : MonoBehaviour
{
    public float speed = 3.0f;
    
    public int maxHealth = 5;
    
    public int health { get { return currentHealth; }}
    int currentHealth;
    
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
        currentHealth = Mathf.Clamp(currentHealth + amount, 0, maxHealth);
        Debug.Log(currentHealth + "/" + maxHealth);
    }
}
```


Your HealthCollectible script should now look like this:

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class HealthCollectible : MonoBehaviour
{
    void OnTriggerEnter2D(Collider2D other)
    {
        RubyController controller = other.GetComponent<RubyController>();

        if (controller != null)
        {
            if(controller.health  < controller.maxHealth)
            {
                controller.ChangeHealth(1);
                Destroy(gameObject);
            }
        }
    }
}
```

### Summary

In this code-heavy section, you have:

* Seen how Triggers work, and how to detect when a Rigidbody enters them to… well, trigger things! 
* Written your own function in C#
* Used private and public access levels 
* Written an if statement that only does something if a test is true

In the next section, you will do the opposite of what you did in this lesson: add GameObjects that damage Ruby. Then those health collectibles will be much more useful!
