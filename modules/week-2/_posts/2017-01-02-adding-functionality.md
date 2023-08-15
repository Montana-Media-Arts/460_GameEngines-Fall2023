---
title: Adding Functionality
module: 2
---


# Making our Ship Functional

<div class="tab">
  <button class="tablinks active" onclick="openTab(event, 'Overview')">Overview</button>
  <button class="tablinks" onclick="openTab(event, 'RigidBody')">Physics</button>
  <button class="tablinks" onclick="openTab(event, 'Arrows')">Movement</button>
  <button class="tablinks" onclick="openTab(event, 'ToDo')">To Do</button>
</div>

<div id="Overview" class="tabcontent" style="display:block">
<p>So, let's make the ship move around!  There are two things we need to make this happen.</p>

<p>To move, the ship requires two things.  </p>
<ol>
<li>It needs what is called the <b>RigidBody2D</b></li>
<li>Script called <b>Move With Arrows</b>.</li>
</ol>
</div>

<div id="RigidBody" class="tabcontent">

<p>RigidBody2D is vital to any object because it applies physics properties like gravity and mass.</p>
<ol>
<li>In the <b>Hierarchy</b> window, select the ship.</li>
<li>In the <b>Inspector</b>, click <b>Add Component</b>.</li>
<li>In the dropdown, look for <b>Physics 2D</b> and click <b>RigidBody2D</b></li>
<li>Once the <b>RigidyBody2D</b> component is added, run the program.  What happens?</li>
<li>We don't want it to fall through the ground, so let's change the <b>Gravity Scale</b> to zero.</li>
<li>We also don't want to make the ship fly too fast; we will add some <b>Linear Drag</b>, which is also like friction.  Change the <b>Linear Drag</b> to <b>10</b>. </li>
</ol>
<p>What about making it move around?  That's where we add code. Lucky for us, we get to use a pre-built script.</p>
</div>

<div id="Arrows" class="tabcontent">

We want to also move our ship.  Now, we will add a script to help with that.
<ol>
<li>Click on the <b>Add Component</b> again</li>
<li>This time, type in the <b>Search</b> bar <b>Move With Arrows</b></li>
<li>After you add the script, press play.  If you use the arrows, the ship should move around.</li>
<li>Now, change the speed variable in the <b>Inspector</b> and see what happens.</li>
</ol>
</div>

<div id="ToDo" class="tabcontent">

<p>What if we wanted to change things around?</p>

<ol>
<li>Think for 2-3 minutes about what the RigidBody is and what it does.</li>
<li>Spend 2-3 minutes altering your project so you can move the ship using <b>WASD</b></li>
<li>Spend 2-3 minutes changing the <b>Gravity</b> and <b>Linear Drag</b>.  What happens if you go up? What happens if you go down?</li>
<li>Spend 1-2 minutes changing the speed.  What if that goes up or down?</li>
<li>Spend 5 minutes discussing this with a classmate.  What changes did you all make?</li>
<li>What was most effective?</li>
<li>Talk to the class about your findings</li>
</ol>
</div>