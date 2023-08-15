---
title: Obstacles and Collision
module: 2
---

# Obstacles

<div class="tab">
  <button class="tablinks active" onclick="openTab(event, 'Overview')">Overview</button>
  <button class="tablinks" onclick="openTab(event, 'Collision')">Collision</button>
  <button class="tablinks" onclick="openTab(event, 'ToDo')">To Do</button>
</div>

<div id="Overview" class="tabcontent" style="display:block">

<p>Unity makes it easy to add both obstacles and collision detections!</p>

<p>You know how to add objects to the scene now.  Go ahead and add something new to the scene into which the ship can collide.</p>

<p>After you add the new object, add the RigidBody2D.  Leave the gravity on this time, though.</p>

<p>Press run, the object should fall through the floor.  Feel free to change the Gravity Scale, so it doesn't fall so fast.</p>

<p>Run the project one more time, moving the ship toward the obstacle.  When you hit the obstacle, does it go right through?</p>
</div>

<div id="Collision" class="tabcontent">

<p>How do we add collision in Unity?  Up to this point, we leveraged rectangular collision which was okay, but somewhat imprecise.  Let's use Unity's version.  </p>

<ol>
<li>Select the ship and then click the Component button again.</li>

<li>Now, click on <b>Physics 2D</b> and look for <b>Polygon Collider 2D</b>.  This type of collision makes the collision box around all of the curves and angles of the ship.  Cool! <p>Notice how it highlights?</p></li>
</ol>
<p>What about the <b>obstacle</b>?</p>

<p>Add a <b>collider</b> component to the obstacle.</p>

<p>Run the project and see if you can make the ship collide with the obstacle.</p>
</div>
<div id="ToDo" class="tabcontent">
  What about different types of collision?
  <ol>
  <li>Think for 2-3 minutes about colliders and their importance.  How do they work?</li>
  <li>Spend 2-3 minutes altering your project to add the <b>Box collider</b> to your ship.</li>
  <li>Spend 2-3 minutes altering your project to add the <b>Capsule collider</b> to your obstacle</li>
  <li>Were there any differences when they collided this time?</li>
  <li>Spend 5 minutes discussing what happened with a classmate.  Which collisions components were most effective? Why?  When might you use one over another? What are the tradeoffs?</li>
  <li>Discuss your findings with the group</li>
  </ol>
</div>
  