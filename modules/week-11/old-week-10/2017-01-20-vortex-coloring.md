---
title: Vortex Coloring
module: 10
jotted: true
---

# Vertex Coloring with Polybrush

PolyBrush provides a very helpful 3D artist feature called Vertex Coloring that allows you to paint on the Vertex (The points that join together to make a surface) as opposed to painting directly onto the surface of the GameObject.
Similar to how the Push/Pull feature was used for creating the Acid Pool and Death Volume the painting will use the vertex to paint across the surface. The vertices are highlighted in the screenshot and the lines represent the polygon edges between them.

This may be strange at first as you may expect to be able to paint directly onto the surface whereas with vertex painting it takes the vertex and nearest vertices along the edge of the surface to "blend" the color/textures between them.
This is handled by the Materials which utilise Shaders (scripts) to group Textures (bitmap images) and render them on a GameObject. For a more detailed explanation of these topics please go to the Materials, Shaders and Textures page of the Manual.
To show this let's jump right in and start painting!
Painting and Materials
When you created the scene earlier using the kit the Plane was already created with a shader material attached to it. In the following steps you will see how this was created and dig deeper into what makes up the visual style and materials.
Let’s begin by setting up a basic plane and attaching a material.
Unless already open, open up the ProBuilder from the Tools > ProBuilder > ProBuilder Window

Select New Shape from the ProBuilder Window

In the Shape window select Plane from the drop down

This will add a plane to the Scene view with some default values attached, position it next to the default plane created earlier. To look like the one in the demo set the Plane values in the ProBuilder window to:
Width: 20
Height:20
Width Segments: 10
Height Segments: 10

In the Inspector view set the following
Position: X: -30 Y: 0 Z: -10
This should position the plane besides the existing plane generated in the level, if it does not perfectly align using the values, adjust as necessary.

Return to the Shape tool window and click Build Plane
Looking at the Inspector, you can see the Shader that is currently attached is simply Standard Vertex Color which will show as greyed out.

To change this you can add the the Materials created already in the 3D Gamekit. To locate these you can use the helpful Search by Type button 

in the Project view and select Material from the drop down that appears.

This will update the Project view to show all the materials in the project.

In the 3D Gamekit the default Material attached to the main Plane is Default_Ground_Mat let’s filter the results further.
Type in default_ground_mat next to the t: Material text in the search field

Drag and Drop the Default_Ground_Mat.mat from the Project view onto the Plane in the Scene View to add the material to it.
Notice, in the Inspector that the Material has now updated to the Default_Ground_Mat

This will now allow you to use the material to paint on the Plane.
Go to Tools > Polybrush > Polybrush Window

Select the Paint Vertex Colors tab at the top of the Polybrush window

Notice how the texture is tiled across the Planes surface area with the default pattern on the left, and the vertex painted version on the right.

To see the textures that are currently mapped, go to Assets > 3DGamekit > Art > Textures > Environment > Ground in the Project view.
Tip: To navigate there quicker, however, you can use the Default_Ground_Mat Shader, click the arrow head next to the Material preview and select one of the textures, both methods will take you to the correct folder in the Project view.

To begin painting the new plane, select the Red paint from the Polybrush window to set the Brush Color.

This will allow you to use the mapped textures to paint, in this case the Moss_Albedo Texture to paint on the vertices.
Using the brush gizmo in the scene view, by moving it towards a vertex on the plane you can see the texture blending live in the editor, to make it permanent, simply click.
If you want to add some MuddyGround texture switch Brush Color to Blue and MuddyGround with SmallStone texture, switch Brush Color to Green in the Palette. This will give you a good variety on your surface.

Now it’s your turn to get creative with the brush! In combination with the Push/Pull, resizing of the brush and the Brush Colored textures you can create a great variety of painted meshes.
Tip: To try out the different colors you can live preview them in the editor to see the different textures and blending that is applied in the 3D Gamekit.
Removing ProBuilder Scripts from objects
When using ProBuilder created objects in your scene, it is best to remove the Probuilder scripts when you are finished editing to prevent any unnecessary overhead in your game. It removes the scripts from either individually selected objects or all ProBuilder objects in the scene.
Click on Tools > ProBuilder > Actions and then select either to strip all or simply strip from the individually selected object.

This action is permanent so ensure that you have made all necessary changes to the meshes and vertex painting before doing this, the editor will warn you before completing the action.


