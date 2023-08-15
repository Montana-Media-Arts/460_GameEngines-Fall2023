---
title: Add Colors with Materials
module: 11
jotted: false
---

# Materials

Okay, let's add some more contrast between the player object and the floor plane. 

1. To add color or a texture to a model, you need to use a **material**. 

This project doesn't use any textures, but we will quickly review what's included in the URP lit material. 

2. First, let's **create a new folder** in our project to hold the **materials**. 

In the **Project window**, select the **Create menu** and then **folder**. 

**Rename** this new folder, **Materials**. 

With the **Materials** folder selected, use the **Create** menu again and this time select **Material**. 

See how the material was created in the materials folder. 

This is because you had the folder selected when you created the material. 

**Rename** this **material**, **Background**. 

Next, let's **change the color** of this background **material**. 

In the **Project** window, select the **background material** and review the **Inspector**. 

In the **surface input section**, the **first property** is **Main Map**. 

Select the **Main Maps color field**, to **open a color picker**. 

Change the color to a **pale gray**. 

You can set the **RGB** values of **130, 130 and 130**, to get the exact shade that the example game uses. 

The next map represents both **Metallic** and **Smoothness**. 

As this Unity project does not use textures, you can adjust these independently. 

First, set the **Metallic** to **zero**. 

Then change the **Smoothness to 0.25** to create a **matte** material. 

To **apply** the **material** to the plane, **select the material** in the **Project** window, then **drag it** onto the **plane** in the Scene view. 

If you drag it over the player sphere, it may try to assign itself there. 

If it does this, don't worry, use Ctrl or Command Z to undo. 

And then assign it to the plane. 

Now the background is ready. 

Let's create another material for the player. 

In the Project window, select **Create > Material**. 

Name this new material, **Player**. 

In the **Inspector**, select the Main Maps color field to open a color picker. 

Set the **RGB** values to **0, 220, and 255**, which is a nice **light blue**. 

Next, set the **Metallic to zero** and change the **Smoothness, to 0.75** for shiny finish. 

Then, as you did before, **drag the material** from the Project window onto the **player sphere** in the **Scene view**. 

But let's make one additional change and **rotate the main Directional Light** so that there is better lighting on the player. 

In the **Hierarchy**, select the **Directional Light** and identify its **Transform** component in the Inspector. 

Change its **rotation to 50, 50 and 0**. This will give it a better silhouette. 

Now you've created a player GameObject and a background play field. 

In the next section of this project, you'll get started with the game functionality and write a custom script to get the sphere moving around the play field.