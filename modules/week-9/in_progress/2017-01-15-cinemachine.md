---
title: Packages
module: 9
jotted: true
---

# Packages

Unity comes with lots of built-in functionality that most games and applications need. More specialized functionalities that not all games need are offered via packages. Packages only increase the size of Projects that use them, and they are quick and easy to update.

A package is a bundle of code and assets that you can add to your Project using the Package Manager. They add functionality to your Project so you don’t have to code it yourself, such as virtual reality support, post processing effects or what you are looking for here: additional camera functionality.

To add the Cinemachine package:
1. Open the Package Manager in the Unity Editor (menu: Window > Package Manager).
2. Look for the entry Cinemachine and click on Install in the bottom right corner. 
3. The Install button will change to Installing and, when the package has finished importing, it will change to Up to date. You now have Cinemachine, which is a tool for setting up and moving cameras in your Project.

**Tip:** Click the View documentation blue link under the version number to see the documentation for a package. 

### Cinemachine Setup

Cinemachine enables you to create complex 3D camera setups, allowing movement and cuts between multiple cameras. 

You're going to use it simply to make a camera follow a target in 2D. Cinemachine also includes 2D helper functionality to constrain the camera to certain bounds, so it doesn’t show objects outside of your map borders.

1. To start using it, you need to add a Cinemachine 2D Camera to your Scene by selecting the entry Cinemachine > Create 2D Camera on the top menu bar.
2. This will create a new GameObject called CM vcam1 (for Cinemachine Virtual Camera 1). 

As explained above, Cinemachine works with multiple cameras and cutting between them, depending on the game needs, such as cutting between a shot and reverse shot in a dialogue. 

3. To enable that, Cinemachine uses Virtual Cameras, where you can choose the different settings on each Virtual Camera and then tell your actual camera (called Main Camera in your Hierarchy) which Virtual Camera is currently active so it can copy the settings. 

In your case, you will simply change the settings on the CM vcam1 Virtual Camera, and the actual Camera will copy those.

4. If you open your Game view, you should see that the Camera now appears farther away and you can see more of the world (you may even see some blue background if you didn’t paint your Tilemap further than the previous screen limit).

That's because the Camera now uses the default Virtual Camera settings which are different from your previous Camera settings.

5. To zoom-in again, look at your vcam Inspector for a property in the Lens section called Orthographic Size and set it to 5 instead of 10.

### Camera Modes

In 3D applications like Unity, cameras can have two modes:

1. Perspective: where all lines going away from the camera converge to a point, making things appear smaller as they get farther away from the camera. This is a bit like a straight road disappearing in the distance and its two sides seem to converge into a single point.
2. Orthographic: where all parallel lines stay parallel. 

Since you are in 2D, you want your camera to be in orthographic mode, because you don’t want things to get smaller with the distance. 

**Note:** The camera was already automatically set to orthographic when you created your Project with the 2D Template.

Orthographic size is simply how many units the camera fits in half of its height, because that’s how the camera works. So, because we set it to 5, we can see 10 units of the world in the vertical direction of the screen. Make sure you set the half height, so if you want your camera to be able to see 50 units of the world in its height, set it to 25.

Why are you setting the height and not the width? Well, this is because the width changes depending on the resolution the user sets for their game window. 

Screens can have a lot of resolution ratios, such as 4:3, 16:9 and 16:10, so the camera will show more or less of the world’s width depending on the screen shape, but it will always show the same vertical height. 

### Following the Main Character

Now let's get Cinemachine to set up your camera to follow your character. 

1. Just drag and drop your main character from your Hierarchy into the Follow property on the vcam Inspector.

The camera will immediately move into position so that Ruby is in the center of the screen. 

2. Let's quickly paint a bigger world in our Scene view, and add water tiles all around to stop Ruby from exiting our map. If you don’t remember how to paint on the Tilemap, you can go back to the tutorial about painting the world.  
Now when you Play the Scene you’ll see that the camera follows Ruby as she moves through your map. But one problem still exists: when Ruby walks to the edge, you can see "outside" the map. 

### Camera Bounds

To stop the camera from showing you anything outside your map, you will use the Cinemachine Confiner to define some bounds.

To add a Cinemachine Confiner, go to bottom of the Virtual Camera Inspector, click the Add Extension drop-down and select CinemachineConfiner.

The newly added component shows a warning, so you need to give it a Collider 2D to use as bounds. The Confiner can use either a Composite Collider 2D or a Polygon Collider 2D.

Let's add a Polygon Collider 2D to your Scene

1. Create a new GameObject by clicking on the Create button on the top right of the Hierarchy and choose Create Empty. 
2. Select the new GameObject, and rename it to CameraConfiner (either press F2 (Windows)/Return (Mac), click on it again once its selected or use the top box in the Inspector).
3. Click the Add Component button, search for Polygon Collider 2D and add it.
4. On the Polygon Collider 2D, click the button next to Edit Collider and move the points to place them in the middle of the water on each corner. You can delete a point (like the top one on our pentagon) by pressing the delete key as you drag it. 
5. When you have got the points in each corner, click the button next to Edit Collider again.

**Note:** Use the points settings at the end to round the value to make sure the border are straight lines. This is not required for the Confiner to work, but it is cleaner.

Then go back to your vcam1 and assign that GameObject to the Bounding Shape 2D property on your CinemachineConfiner:

Now if you try to Play the game, your character will disappear from the screen. If you check in the Scene view, you’ll see that it’s pushed outside of the world. Because the world is now inside a big Collider, the Physics System just pushes your character out. 

### Bring your Character Back

To bring your character back into the Scene, you will use Layers, just like you did with the projectile in the previous tutorial. Refer to that tutorial for a more in-depth explanation, but here is a quick reminder of the steps required:
1. In the top right corner of the Inspector, click on the Layer drop down and select Edit Layer.
2. Choose an empty slot and name it Confiner.
3. On your Confiner GameObject, set the layer drop-down to Confiner.

Select Edit > Project Settings > Physics 2D and untick all entries in the Confiner layer.

Now all objects in the Confiner layer won’t collide with anything. Go into Play mode and move Ruby around. The Camera will now stop at the edge of the map. 

### Summary
In this section you have imported the Cinemachine package into Unity and used it to handle your Camera movement. 

To get more information on how to use Cinemachine, take a look at the documentation, especially if you want to play with the Virtual Camera properties, like damping movement and follow or speed of follow.

**Tip:** Keep an eye on the Package Manager because it contains packages than can help you create your game and reduce the amount of work you have to do get something working.

In the next tutorial you will look at some graphics improvements by using the Particle System.
