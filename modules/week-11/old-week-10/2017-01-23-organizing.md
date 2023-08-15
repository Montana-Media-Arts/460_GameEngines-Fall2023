---
title: Organizing
module: 10
jotted: true
---

# Organizing Your Scene
When creating any scene in Unity, it’s important to keep things tidy in the Hierarchy so you can easily find objects or game mechanics you are looking for. The following are some tips to keeping your projects nice and organized although are not mandatory to use the Kit.
In the Hierarchy we have created sections to help order types of objects in our Scene.

These are made of empty GameObjects. They have no function other than to help us identify the types of objects in our scene. Note that all the objects underneath each named section are not children of that section and they shouldn’t be.
Let’s create a new section to store all of our extra level decoration;
At the top of the Hierarchy go to Create > Create Empty

This will create a new and empty GameObject named GameObject at the bottom of your Hierarchy

In the Hierarchy, make sure that GameObject is highlighted blue, indicating you have it selected. We can now rename this;
Navigate to the Inspector
At the top of the Inspector click on the Name Field
Enter the name ----- Environment -----
Naming the sections in this way will help you find objects when you have a lot more.

Now let’s move CliffBig01 into the right section so we can find it easier later.
In the Hierarchy locate CliffBig01
Left Click, hold and drag CliffBig01 until it is under ----- Environment ----- then let go of the Left Click

Tip: Make sure you do not let go of Left Click on top of another object as this will make it a child.
There is a visual indicator when something will become a child of an object when dragging, it will appear like this;

Ensure you see a horizontal line when you drag the object underneath ----- Environment -----. This indicates that it will place the object underneath like this;

In the event that you accidentally place it as a child of the label, you can simply move it back and try again.


