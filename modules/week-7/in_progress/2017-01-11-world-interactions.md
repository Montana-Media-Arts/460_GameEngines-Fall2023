---
title: World Interactions
module: 7
jotted: true
---

# World Interactions

## What is the Physics System?

When learning about physics, you will have discovered that all object movements are a combination of forces. For example, if you try to push a box, the forces acting upon it include the following:

1. Gravity, which pulls it down
2. The force you apply to push it
3. The ground friction going against that pushing

If you want to simulate movement and collisions, you need to use all the mathematical equations to compute contacts and forces on the objects. But this a lot of complicated code to write. Because the laws of physics are the same, it’s possible to abstract this code and share it amongst all games: this is what the Physics System does. Unity has a built-in physics system that can compute object movements and collisions for you.

To avoid doing expensive mathematical operations on every object in our game, Unity only does those computations for GameObjects that have a Rigidbody 2D component attached to them. 


### Add a Rigidbody 2D Component
Let's start by adding a Rigidbody 2D component to Ruby:
1. In the Hierarchy, select the Ruby GameObject.
2. Drag the Ruby GameObject from the Hierarchy into the Prefab folder in your Project window. 
3. Double-click the Ruby Prefab to open Prefab Mode. 
4. In the Inspector, click the Add Component button.
5. Search for “Rigidbody 2D” and select the component.

**Note:** Make sure you select the 2D component — there is also  component just called Rigidbody for 3D games.  

6. Save your Prefab using the Save button, and return to your Scene.
7. Click Play to enter Play Mode and… oops, your character just fell out of the screen! 
8. Click Play again to exit Play mode.

### Disable Gravity

Ruby fell because the Rigidbody applies gravity to the GameObject (which is set by default down the y axis), so the GameObject falls. But in this case, that's not what you want. Your game has a "top down" view, which means everything is the ground — the bottom of the screen is not the “down” of your world.

Thankfully, you can disable this:

1. In the Hierarchy, select the Ruby GameObject.
2. In the Inspector, find the Rigidbody 2D component.
3. Find the Gravity Scale property and set it to 0.

Now you can click Play to test the changes and Ruby won’t fall anymore!

### Disable Gravity for the Ruby Prefab

Note how the Gravity Scale property is now in bold, with a little blue line next to it? That’s because you changed it on the Ruby in the Scene and not on the Prefab! (You didn’t go into Prefab Mode). 

If you add another Ruby to the Scene, the new one will still have Gravity Scale set to 1. This is called an override, and it allows you to make modifications to a single Prefab instance in a Scene, rather than to all instances of a Prefab.

An override will also always take precedence over a Prefab value, so if you edit the Prefab and change Gravity Scale to 0.5, all the existing Ruby would have their Gravity Scale set to 0.5 except this one (because it already has an override setting it to 0).
In this case though, you want that modification to be made to the Prefab:

1. In the Inspector, look at the Ruby GameObject header. 
2. Click the Overrides drop-down menu. A dialog window will appear with a list of the components that are changed compared to the Prefab (in this case, the Rigidbody 2D).  
3. Click Apply All so the change is applied to the Prefab.

Now if you were to add a new Ruby to the Scene, its Gravity Scale would be set to 0.

### What is a Collider?

Now that your GameObject is known by the Physics System (thanks to the Rigidbody), you need to tell the physics system which part of the GameObject is “solid”. This is done through Colliders.

Colliders are simple shapes like squares or circles that the physics system takes as the approximate shape of your GameObject, to do its collision calculations.

### Add Colliders to GameObjects

Let's start with the Ruby GameObject:

1. Open the Ruby Prefab in Prefab Mode.
2. In the Inspector, click Add Component.
3. Search for "Box Collider 2D", and add this component.

**Note:** Again, remember to use the 2D version of this component!

4. You will now see a green outline around Ruby in the Scene view: 

That's the shape of the Collider — and it’s now the shape of Ruby for the Physics System. 
5. Save the Prefab.
6. Now do the same for the MetalBox Prefab: 
    - Double-click the Prefab to open it.
    - Add a Box Collider 2D component.
    - Save the Prefab.
    - Exit Prefab Mode.

**Note:** You haven't added a Rigidbody to the box — that's correct. This is because it doesn’t need to be moved by physics, it just needs a Collider so GameObjects that do have a Rigidbody will interact with it.
7. Now press Play, and try to move Ruby through and around the box.

Hmmm... it’s still not totally right. The character jitters and rotates!

### Fix Ruby’s Rotation

First, let’s take care of the rotation. To do this, you need to tell the Physics System not to rotate the GameObject. It may be what “real” physics would do, but in this 2D game it doesn’t work.

Thankfully, the Rigidbody 2D component has a setting for this:

1. Check that Ruby is open in Prefab Mode.
2. In the Inspector, find the Rigidbody 2D component.
3. Click the small arrow next to Constraints to expand the section.
4. Enable the Freeze Rotation checkbox, to ensure the Rigidbody does not add any rotations to Ruby. 

**TIP:** If you made that change to the instance of Ruby in the Scene and not on the Prefab, use the drop-down Overrides menu to apply the changes to your Prefab.

Now you're ready to fix Ruby’s jittering.

### Why is Ruby Jittering?

Jittering happens because the Physics System uses a simplified copy of the Scene that only contains the Colliders. 

This Physics Scene make computations simpler for the  Physics System, but the Physics 
System needs to:

1. Move its copy of the GameObject in the physics Scene whenever the GameObject with the Rigidbody moves in your Scene.
2. Apply forces and compute collision.
3. Move the GameObject in your Scene to the new position calculated in the physics Scene.
4. In this case, that leads to the following events:
5. You move the character during the frame update.
6. The Physics System moves its copy to that new position.
7. The Physics System finds that the character Collider is now inside another Collider (here the box) and moves it back, so that it is no longer inside the box.
8. The Physics System synchronizes the Ruby GameObject with that new position.
9. You are constantly moving Ruby inside the box and the Physics System is moving her back. The fighting between what you tell your code to do and what the Physics System does causes the jittering.

### Fix Ruby's Jittering

To fix Ruby’s jittering, you need to move the Rigidbody itself and not the GameObject Transform, and let the Physics System synchronize the GameObject position to the Rigidbody position. That way, the physics system can stop the movement before it gets inside the box, rather than having to move Ruby after she is already in the box.

To do this, you need to modify your Ruby Controller code:
1. Double-click the RubyController script to open it. Your script should look like this:

```csharp
public class RubyController: MonoBehaviour
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
        position.x = position.x + 3.0f* horizontal * Time.deltaTime;
        position.y = position.y + 3.0f * vertical * Time.deltaTime;
        transform.position = position;
    }

}
```

2. Because all GameObjects have a Transform component by default, Unity makes the transform variable available in all scripts. But a Rigidbody component has to be manually added to a GameObject, so Unity doesn’t have it as a built-in variable. 
3. In addition to this, the physics system is updated at a different rate than the game. Update is called every time the game computes a new image, the problem is that this is called at an uncertain rate. It could be 20 images per second on a slow computer, or 3000 on a very fast one. 

For the physics computation to be stable, it needs to update at regular intervals (for example, every 16ms). Unity has another function called FixedUpdate that needs to be used any time you want to directly influence physics components or objects, such as a Rigidbody.

However, you shouldn’t read input in the Fixedupdate function. FixedUpdate isn’t continuously running, so there’s a chance a User Input will be missed.You need to add two float variables in the class to store the current horizontal and vertical input data within the Update function.
Adjust the RubyController script as follows: 

```csharp
public class RubyController : MonoBehaviour
{
    Rigidbody2D rigidbody2d;
    float horizontal; 
    float vertical;
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
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
        position.x = position.x + 3.0f * horizontal * Time.deltaTime;
        position.y = position.y + 3.0f * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }
}
```

### Review your Changes

Let's look at each new and adjusted line of code:

```csharp
Rigidbody2D rigidbody2d; 
```

This creates a new variable called rigidbody2d to store the Rigidbody and access it from anywhere inside your script.

```csharp
float horizontal;
float vertical;
```

This creates two new variables to store the input data. These variables used to be declared in the Update function, but since you now need to access them from another function (FixedUpdate), you have declared them here.

```csharp
rigidbody2d = GetComponent<Rigidbody2D>(); 
```

This code is inside the Start function, so it’s executed only once when the game starts. It tells Unity to give you the Rigidbody2D that is attached to the same GameObject that your script is attached to, which is your character.

In the Update function, you have removed all code related to movement. You’ve kept the code to read the input, this time in the two variables you declared earlier.

```csharp
Vector2 position = rigidbody2d.position; 
```

In the FixedUpdate function, you’ve added the line of code that used to be in the Update function and adjusted it to use the Rigidbody position.

```csharp
rigidbody2d.MovePosition(position); 
```

In the same way, instead of setting the new position with transform.position = position; you are now doing it with the Rigidbody position. This line of code will move the Rigidbody to where you want, but will stop it mid-way instead if it collides with another Collider in that movement.

Now when you select Play, you’ll see that your character has stopped jittering!
But Ruby still stops much too early, and it doesn’t look good at all. That’s because the size of the Colliders don’t fit the images properly.

### Resize the Colliders

As you explored earlier in this section, GameObject shapes are approximated by their Colliders for the physics system. 

If you look in the Scene view, you will see that the Collider for the box and Ruby extends far beyond what you would consider the “solid” part of the GameObject. 
Select the box or Ruby, and you will see the green rectangle around them.

For example, the shadow of the box is inside the Collider, so it’s considered solid by the Physics System. 

To resize the Collider:

1. Double-click the MetalBox Prefab (or click on the arrow to the right of the MetalBox in the Hierarchy) to enter Prefab Mode. 
2. In the Inspector, find the Box Collider component.
3. Click the Edit Collider button.
4. When you click Edit Collider, your Collider will change in the Scene view to display four little squares on the side. You can click and drag them to resize the Collider. 

If you miss-click and the little dot disappears, just go back in the Inspector and re-click the Edit Collider button. 

Why aren’t you making it the size of the box? Well that’s because your character needs to be able to go "behind" the box so that it looks better (otherwise the box would look flat on the ground).

5. Save your changes.
6. Complete the same process for the Ruby GameObject.

The Collider only covers Ruby's legs, because the character needs to be able to move on top of the GameObject a bit before colliding — this helps to make gameplay more believable.
7. Save your changes.
8. Click Play, then try to move Ruby against and around the box. Now it should look good and behave as you expect. (Remember to press Play again when you’re done, to exit Play Mode.)

You can try playing around with resizing the Colliders for both GameObjects, and find out which shapes work best for you. 

You can also go back to all the Prefabs you made in the previous tutorials and add Box Colliders to them, so Ruby can’t go through them! Since they are Prefabs, as long as you add the Collider and resize it in the Prefab Mode, all the instances you already placed in the Scene will have a Collider added. 

Don’t get overzealous through — GameObjects like the drain cover probably don’t need Colliders because Ruby should be able to walk on top of them!

### Add Tilemap Collision

Now your character collides with all our GameObjects that have a Collider. But she can still walk on water — you should make her collide with the tiles of water, so she can’t walk on them. But how can you do that?

You could add a Collider to an empty GameObject and resize it to cover the water. But that’s error prone, and if you ever want to repaint the water to be bigger, smaller or a different shape, you would have to manually change the Collider. Remember that Tilemaps are here to allow you to make easy and fast changes to the world.

Thankfully, Tilemaps can also have Colliders. Each Tile can store whether it should be colliding or not, and the Tilemap Collider will create a Collider for all the Tiles that are set up to collide.

To set up the Tilemap Collider:

1. In the Hierarchy, select the Tilemap GameObject.
2. In the Inspector, click the Add Component button.
3. Search for “Tilemap Collider 2D” and select this component. You’ll see that this adds green Collider squares to all your tiles in the Scene view: 

This is because right now, all the Tiles are setup to collide.

4. In the Project window, go to your Tile folder. Select all the tiles that aren’t water. (You can click on one, then hold Shift and click on the last Tile in the list to select them all.)
5. In the Inspector, find the Collider Type property and change it from Sprite (as it is now) to None.

Now the Tiles that you selected are no longer considered Colliders.  If you select the Tilemap in the Hierarchy, you will see in the Scene view that only the water Tiles have green squares.

6. Save your changes.
7. Click Play to enter Play Mode and try to make Ruby walk on water — she should now collide with the border.

### Optimize the Tilemap Collider

There’s one last small step to set up your Tilemap Collider. Currently, each tile is a separate Collider, as you can see in the Scene view. 

This works fine, but it creates two problems:
It’s heavier on computation for the physics system; if you have a big world, it can start slowing down your game.

It can create small problems on the borders between tiles. Since they are two Colliders side-by-side, and there is a tiny gap between them, sometimes tiny imprecisions in the computation can lead to rare occurrences where collision will still happen.
To address these issues, Unity offers a component called a Composite Collider 2D. This takes all Colliders on the objects (or on children of the objects) and makes one big Collider from them.

Let’s add and configure this component:

1. In the Hierarchy, select the Tilemap GameObject. 
2. In the Inspector, click the Add Component button.
3. Search for “Composite Collider 2D” and select this component. 
You will see that this automatically adds a Rigidbody 2D, because the composite Collider needs a Rigidbody 2D to work properly.
4. In the Tilemap Collider 2D component, enable the Used By Composite checkbox. 
5. In the Rigidbody 2D component, set the Rigidbody Body Type property to Static. 

Setting this to Static will stop your world from moving. It also helps the Physics System optimize computation, as it now knows that Rigidbody can’t move.

Now the Collider around your water tiles is a single big rectangle (you can enable and disable Used by Composite on your Tilemap Collider to compare with and without in the Scene view).

If you paint new water Tiles, Unity automatically updates the Composite Collider of the Tilemap to include those new Colliders.

### Check Your Script

Your RubyController script should now look like this:

```csharp
public class RubyController : MonoBehaviour
{
    Rigidbody2D rigidbody2d;
    float horizontal; 
    float vertical;
    
    // Start is called before the first frame update
    void Start()
    {
        rigidbody2d = GetComponent<Rigidbody2D>();
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
        position.x = position.x + 3.0f * horizontal * Time.deltaTime;
        position.y = position.y + 3.0f * vertical * Time.deltaTime;

        rigidbody2d.MovePosition(position);
    }
}
```

### Summary

In this section, you have:
1. Explored the basics of the Physics System in Unity
2. Added a Rigidbody component to make the Physics System handle objects
3. Added a Collider to make objects collide together
4. In the next section, you will extend your use of the Physics System to detect character collisions with gameplay objects (for example, collectible health packs).
