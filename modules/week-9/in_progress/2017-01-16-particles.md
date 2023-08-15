---
title: Particles
module: 9
jotted: true
---

# Preparing the Sprites

Unity offers an extensive Particle System. In this tutorial you will use it to create some particles for our game, like a smoke effect for the broken robot.
You will use Sprites as images to put on your particles. The Sprite Atlas for those images can be found in Art > Sprites > VFX and is called ParticlesSheet. 

1. Set the type to multiple, leave the PPU as 100.
2. Open your Sprite editor to slice the different Sprites. 
3. The Sprite Atlas is split into 4 columns and 4 rows. For a reminder on slicing a Sprite Atlas, see the Sprite Animation tutorial.
When you have all your Sprites, let’s create an effect. Let’s start with the smoke effect.


### Smoke Effect

To create a new Particle effect:

1. Use the Create button at the top of the Hierarchy window (select Effects > Particle System).
2. A default Particle System is created for you. This should look like white dots that shoot upwards. Rename it to SmokeEffect.
3. In the Inspector, you can see the Particle System is formed of multiple sections, all collapsable, that define all properties of the system and the particles it creates.  
4. Let's start by changing the white dot to your smoke Sprites. In the Texture Sheet Animation section, click on the circle next to it enable it. Open the section by clicking on its name.

### Picking Random Sprites

You normally use the settings in this section to animate the particle image over time. But here you will use it as a way to pick a random Sprite for each particle instead.

1. Set the Mode to Sprites.
2. Click on the + button next to the sprite entry that appeared so you get 2 of them.
3. Assign both smoke Sprites from your effect Sprite Atlas to these properties.
4. Click on the small down arrow on the right of the Start Frame property, select Random Between Two Constants, and enter 0 and 2. The system will pick a random number between 0 and 2 (excluding 2), so either 0 or 1, and use the corresponding Sprites for the particle.
5. Finally, click on the black box next to Frame over time, that shows how the frame will change over time (from frame 0 to 1) in a curve frame. You don’t want any animation at all, so right-click the right-most point and select Delete Key: 

    - Now you can see in the Scene view that the particles are using one or the other smoke Sprite randomly when they are created.

    - The next step is to change how they are created, because right now they are spread too far in too many directions.

6. Open the Shape section in the Inspector. The Scene view will show the cone in which the particles are emitted. Set the Radius to 0, because you want all particles to start from a single point (the value will be changed to 0.0001 but don’t worry, it’s just that it can’t be 0, so Unity sets it to the closest value.
7. Change the angle to something around 5 so the particles are less spread out and get generated in a straighter line: 

Your particles now will start in the right direction, but they are still moving too fast. And since all the particles are the same shape, they look too artificial. Natural looking things are chaotic, so the secret to making something look less artificial is to add randomness to it.

### Adding Randomness to Particles

In the main section at the top of the Particle System, look for these three settings:

1. Start Lifetime: A particle’s lifetime is how long it is on screen before it gets destroyed by the Particle System. If you zoom out in the Scene view, you will see that all your particles disappear around the same place. Because the particles start with the same speed and same lifetime, they end up being destroyed at the same distance. 
    - Click on the small down arrow to the right of Start Lifetime and select Random Between Two Constants. Enter 1.5 and 3. The particles will disappear earlier because now their lifetime is shorter. They also disappear in a less artificial way because they all have different lifetimes now.
2. Start Size: This is the size of the particle as it gets created. Right now, a single number is set, so all particles are the same size. Introduce a bit of randomness by selecting Random Between Two Constants, like above, and set them to 0.3 and 0.5. The particles are now smaller, of different sizes, but still move too quickly.
3. Start Speed : Let's reduce the speed the particles start moving and add some randomness by setting this to Random Between Two Constants. Set the two constants to 0.5 and 1.

The particles start to look a lot more like smoke. But it still looks strange because when the particles reach the end of their lifetime, they just pop and disappear out of existence.

### Making Particles Fade Away

To make that change less brutal, let’s make the transparency of the particles gradually go toward fully transparent as the particle’s lifetime gets closer to its maximum value. 

That way, the particle won’t pop out of existence, but slowly fade away.

1. Enable the section called Color over Lifetime by clicking on the small white circle next to its name and open the section. 
2. To make it easier to see all the sections, remember that you can close open sections by clicking on their title. Click on the white square next to Color to open the Gradient Editor.
    - This gradient shows how the color of a particle changes over its lifetime. The bottom arrow is the color and the top arrow is the transparency (called “alpha” in game art terminology).
    - You will see the color and alpha on the left side at the creation of the particle and on the right side you will see the color and alpha at the end of the particle’s life. 
    - In between, the left and right sides you can see the color and the alpha evolution the particle will follow. 
3. Right now, the particle will stay totally white and its alpha won’t change. So let’s change that: select the top right arrow, and change the alpha from 255 to 0.
    - You're nearly there! Your smoke now looks much better, but one small detail is still missing : smoke should fizzle out as time passes, so the particle size should also change during its lifetime, progressively getting smaller.
    - Just like Color, there is a section called Size Over Lifetime, so enable and open that section.
4. Click on the Size box and look at the curve that appears at the bottom of the Inspector.
    - Right now the Particle System is doing the inverse of what you want, because particles start at size 0 and grow during their lifetime to reach their max size at the end of the lifetime (this is a multiplier, so it won’t break the randomness in size that we set earlier).
5. Let's invert this by dragging the first dot up to 1 and lowering the last dot to 0.
**Note:** You can use the tangent that appears next to each point to control the curve and play with them until you get a result you like in the Scene view. For good looking smoke, you will need to make the curve stay flat until about halfway and then add a steep decrease. 

Now that your smoke looks pretty nice, it’s time to add it to your Robot.

### Particle System in code

Let's look at the Particle System in Code
1. Start by making a Prefab out of your smoke effect. Once that is done, you can delete the one in the Scene.
2. Open your Robot Prefab, then drag your smoke Prefab to make it a child of your Robot, and place it so it seems to come out of its head:
3. Save your Prefab, and try to play the game. The Robot now looks a lot more broken. But two problems appear:
    - The smoke is moving with the character. That’s not how we expect smoke to work - the particles shouldn’t follow the robot movement
    - If you get Ruby to throw a Cog at the Robot to fix it, the Robot continues to smoke. You will need to disable the smoke particle effect when the Robot is fixed. 

### Fixing the Smoke Movement

The fix for the smoke movement is quite easy:
1. If you look in the main section of the Particle System, you’ll see a Simulation Space setting that is set to Local. Change this to World. 

And to stop the smoke when the Robot gets fixed: 

2. Open the EnemyController script and add a public member of type ParticleSystem called smokeEffect.
3. Now in the Inspector of your Robot Prefab, a new slot appears. Drag your smoke effect into the slot. 
    - You may be wondering why the type is ParticleSystem and not Gameobject, since you are assigning a GameObject to it. 
    - That’s because if a public member is a Component or Script type (instead of GameObject), when you assign a GameObject to it in the Inspector, Unity stores the component of the type that is on the GameObject. 
    - This prevents you from having to do GetComponent in the script like you did before. It also stops you from assigning a GameObject that doesn’t have that component type on it to that setting. This also avoids creating a bug by mistake. 
4. Finally, in the Fix function of our EnemyController script, add:

```csharp
smokeEffect.Stop();
```

- Your smoke will now stop as you fix the Robot.
- As a side note, why are you using Stop, and not simply destroying the Particle System like you did with your projectile earlier? 
- Well, when you wonder about something, the best way to find out is by trying: 
Comment smokeEffect.Stop() and add Destroy(smokeEffect.gameObject)on the next line. 
- Play the Scene and fix the Robot.
- You can see that the smoke instantly disappears. That’s because when a Particle System is destroyed, it destroys all the particles it was currently handling, even the ones that were only just created. 
- Stop, on the other hand, simply stops the Particle System from creating particles, and the particles that already exist can finish their lifetime normally. This looks a lot more natural than all of the particles disappearing at once.

### Creativity Time

Now that you know how the Particle System works, you can try to create more particles for your games. The sprite sheet from the beginning of this tutorial has a sample particle effect for getting hit and picking up the health collectible. 

Don’t worry about doing exactly the same, just get creative and play with the different settings, learning by trial and error how they all work. 
Here’s some information that may help you achieve what you want:
Looping system 

1. The smoke effect you did was looping, so it was always generating new particles. But you can create systems that only work for a certain time then destroy themselves when they are done. To do this, go to the main Particle System section in the Inspector.

    - Untick Looping.
    - Set the Duration to how long you want your effect to last.
    - Set the Stop Action at the bottom of that section to Destroy. The Particle System will only be destroyed after all particles have died, so it won’t have the "everything disappear at once" issue that we had when manually destroying a looping system.
    - Burst emission

Your smoke effect was emitting particles at a steady rate. You can set that rate in the Emission section. 

Rate over Time controls how many particles are emitted per second. Rate over Distance can be used to make the system emit particles when it is moved, which can be useful if you want to emit smoke particles only when a car moves, for example.

But for some effects you may want to emit a lot of particles at once and that’s all. For example, an explosion generates a lot of smoke particles when created and no more after that. 

This is what the Bursts sub-section is for. Click the + button to add a burst, and set up when during the particles’ lifetime the burst should happen, how many particles should be spawn and some other settings.

1. So for example, for a hit effect particle, you could:
    - Disable Looping and set the Stop Action to Destroy.
    - Set the Rate over Time to 0, to not generate any particles over time.
    - Set a burst of 20 particles at time 0.0
    - Then, as soon as the system is created, it will generate a lot of particles and then destroy itself when they have all reached the end of their lifetime.
    - Instantiating a Particle System
    - Once you have made a Prefab out of a Particle System, just like your Projectile, it can be created with the Instantiate function. 
    - So, for an effect that happens only once (like being hit or grabbing a health collectible), you can store a reference to the Prefab in a public variable and call Instantiate when it should happen. If the Particle System is not Looping and Stop Action is set to Destroy, it will play and then be destroyed.

### Check Your Script

Your EnemyController script should now look like this:

```csharp
public class EnemyController : MonoBehaviour
{
    public float speed;
    public bool vertical;
    public float changeTime = 3.0f;

    public ParticleSystem smokeEffect;
    
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
        
        smokeEffect.Stop();
    }
}
```

### Summary

In this section we have seen how the Particle System can create effects to improve the look and feel of your game. 

The system is very powerful and allows for a lot of creativity, so please refer to the documentation on the Particle System for an explanation of all the settings and how to use them.

In the next section will add one of the biggest parts of any game: the User Interface (UI), which will display how much life Ruby has left.