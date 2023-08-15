---
title: Work with Arrays
module: 13
jotted: true
---

# Working with Arrays

Let's get hands-on with arrays now.  We will start with a simple example and then build on the model from last week.

```js
    var x = 50;
    var y = 50;
    var diameter = 25;
    var x1 = 150;
    var y1 = 150;
    var diameter1 = 125;

    function setup()
    {
        createCanvas(800,600);
    }

    function draw()
    {
        circle(x,y,diameter);
        circle(x1,y1,diameter1)
    }
```

Remember this?  What if I wanted to create an array of those two circles instead of creating all those variables?  The code below shows how to implement this.

```js
    var myXs = [];
    var myYs = [];
    var myDiameters = [];
    function setup()
    {
        createCanvas(800,600);
        myXs[0] = 50;
        myYs[0] = 50;
        myDiameters[0] = 25;
        myXs[1] = 150;
        myYs[1] = 150;
        myDiameters[1] = 125;
    }

    function draw()
    {
        circle(myXs[0],myYs[0], myDiameters[0]);
        circle(myXs[1],myYs[1], myDiameters[1]);
    }
```

Now that's great, and it works. However, first, let's talk about what's going on.  In the first line, I define three empty arrays **myXs, myYs, and myDiameters**.  We know they are arrays because of the **[]** that is after the equals sign.

Then, I add the x,y, and diameter components to each array in the setup.  Notice the first one is called **myXs[0], myYs[0], myDiameters[0]** and the second is the **myXs[1], myYs[1], myDiameters[1]**.  When we create arrays and start adding items to them, the count always starts at zero.  Each one of these is called the **index** of an array.

Have you seen this before?  You have!  In for loops, we started with **var i = 0**.  That's because most languages start counting at zero instead of 1.

So, how can we change what we did above?

```js
    var myXs = [];
    var myYs = [];
    var myDiameters = [];
    function setup()
    {
        createCanvas(800,600);
        myXs[0] = 50;
        myYs[0] = 50;
        myDiameters[0] = 25;
        myXs[1] = 150;
        myYs[1] = 150;
        myDiameters[1] = 125;
    }

    function draw()
    {
        for(var i = 0; i < myXs.length; i++)
        {
            circle(myXs[i],myYs[i],myDiameters[i]);
        }
    }
```

Okay, so we made a few more changes here, but only in the draw.  I create a for loop - remember when I said they would come back?  Instead of printing out each index one by one, I use the array to print them all out.  This method allows me to have as many indices without having to change any code later.

The other thing to note is that I use **length** in the for loop instead of 2.  Length is a built-in property of the array that returns how many items are in the array.  That way, if we add more to the array, it still prints without having to change anything in the for loop. Let's look at that.

```js
    var myXs = [];
    var myYs = [];
    var myDiameters = [];

    function setup()
    {
        createCanvas(800,600);
        createCanvas(800,600);
        myXs[0] = 50;
        myYs[0] = 50;
        myDiameters[0] = 25;
        myXs[1] = 150;
        myYs[1] = 150;
        myDiameters[1] = 125;
        myXs[2] = 250;
        myYs[2] = 250;
        myDiameters[2] = 75;

    }

    function draw()
    {
        for(var i = 0; i < myXs.length; i++)
        {
            circle(myXs[i],myYs[i],myDiameters[i]);
        }
    }
```

However, there is one more thing we should address.  What about the number of circles we are creating in the setup?  How do we make that better?

What if we create another for loop? Yes!

```js
    var myXs = [];
    var myYs = [];
    var myDiameters = [];

    function setup()
    {
        createCanvas(800,600);
        var x = 50;
        var y = 50;
        var diameter = 25;
        for(var i = 0; i < 3; i++)
        {
            myXs[i] = x;
            myYs[i] = y;
            myDiameters[i] = diameter;
            x += 50;
            y += 50;
            diameter += 25;
        }
    }

    function draw()
    {
        for(var i = 0; i < myXs.length; i++)
        {
            circle(myXs[i],myYs[i],myDiameters[i]);
        }
    }
```

Let's look at what we did.  We created a for loop in the setup.  This time, we went from 0 to 3.  We have to put in three because we do not have a length yet.  We also create three variables for the x,y, and diameter.  Then, we filled the three arrays. Each time through the for loop, we changed the x and y by 50 pixels and increased the size of the circle diameter by 25.  The best part, though, is that nothing in the draw had to change.

Automating stuff is pretty cool, but what if I wanted ten circles?

Look at this code and see what I had to change to do it.

```js
    var myXs = [];
    var myYs = [];
    var myDiameters = [];

    function setup()
    {
        createCanvas(800,600);
        var x = 50;
        var y = 50;
        var diameter = 25;
        for(var i = 0; i < 10; i++)
        {
            myXs[i] = x;
            myYs[i] = y;
            myDiameters[i] = diameter;
            x += 50;
            y += 50;
            diameter += 25;
        }
    }

    function draw()
    {
        for(var i = 0; i < myXs.length; i++)
        {
            circle(myXs[i],myYs[i],myDiameters[i]);
        }
    }
```

Guess what?  I only changed one thing.  The 3 in the for loop in setup is not a 10.  That was it!  Crazy!

How do we apply this to the project from last week? Let's start from where we left off.

```js

    var x = 50;
    var y = 50;
    var diameter = 25;
    var mousex = 0;
    var mousey = 0;

    function setup() 
    {
      createCanvas(800, 600);
    }

    function draw() 
    {
      background(0);
      
      fill(24, 200, 29);
      circle(x, y, diameter);

      // call the function created
      controlCircle();

      ellipse(mousex, mousey, 15, 50);

      myCircle(800,600);
      myCircle(800,600);
     
    }

    /* This function controls all the variables of the circle */
    function controlCircle()
    {
        if (x >= 300) 
        {
            x = 50;
        }

        if (keyIsDown(83)) 
        {
            y += 10;
        } 
        else if (keyIsDown(87)) 
        {
            y -= 10;
        }

        if (y >= 300) 
        {
            y = 50;
        }
        
        // we call the function here.
        changeDiameter();

    }

    // This function updates the size of the circle
    function changeDiameter()
    {
        if (diameter < 200) 
        {
            diameter += 2;
        } 
        else if (diameter >= 200) 
        {
            diameter = 25;
        }

    }
    function keyPressed() 
    {
      if (key == 'd') 
      {
        x += 10;
      } 
      else if (key == 'a') 
      {
        x -= 10;
      }
    }

    function mouseMoved() 
    {  
      mousex = mouseX;
      mousey = mouseY;
    
    }

    function myCircle(x,y)
    {
        fill(Math.floor(Math.random()*256),Math.floor(Math.random()*256),Math.floor(Math.random()*256));
        circle(Math.floor(Math.random()*x)+10,Math.floor(Math.random()*y),Math.floor(Math.random()*100)+10)
    }
```

When you run this, remember random circles start appearing everywhere, which is cool if that's the effect you are after. However, if I want two random circles to appear and stay put, we can use an array for that.  Let's change the code above to the following.

```js

    var x = 50;
    var y = 50;
    var diameter = 25;
    var mousex = 0;
    var mousey = 0;
    var myXs = []; // create an array for the x coordinate
    var myYs = []; // create an array for the y coordinate
    var myDiameters = []; // create array for the diameter of circles

    function setup() 
    {
      createCanvas(800, 600);
      // create a for loop here to create the circles
        for(var i = 0; i < 2; i++)
        {
            // get all the random numbers to create a circle
            myXs[i] = getRandomX(800);
            myYs[i] = getRandomY(600);
            myDiameters[i] = getRandomDiameter(100);
        }
    }

    function draw() 
    {
      background(0);
      
      fill(24, 200, 29);
      circle(x, y, diameter);

      // call the function created
      controlCircle();

      ellipse(mousex, mousey, 15, 50);

      for(var i = 0; i < myXs.length; i++)
      {
            fill(Math.floor(Math.random()*256),Math.floor(Math.random()*256),Math.floor(Math.random()*256));
            circle(myXs[i], myYs[i], myDiameters[i]);
      }

     
     
    }

    /* This function controls all the variables of the circle */
    function controlCircle()
    {
        if (x >= 300) 
        {
            x = 50;
        }

        if (keyIsDown(83)) 
        {
            y += 10;
        } 
        else if (keyIsDown(87)) 
        {
            y -= 10;
        }

        if (y >= 300) 
        {
            y = 50;
        }
        
        // we call the function here.
        changeDiameter();

    }

    // This function updates the size of the circle
    function changeDiameter()
    {
        if (diameter < 200) 
        {
            diameter += 2;
        } 
        else if (diameter >= 200) 
        {
            diameter = 25;
        }

    }
    function keyPressed() 
    {
      if (key == 'd') 
      {
        x += 10;
      } 
      else if (key == 'a') 
      {
        x -= 10;
      }
    }

    function mouseMoved() 
    {  
      mousex = mouseX;
      mousey = mouseY;
    
    }

    function getRandomX(x)
    {
        return Math.floor(Math.random()*x)+10;
    }

    function getRandomY(y)
    {
        return Math.floor(Math.random()*y) + 10;
    }

    function getRandomDiameter(diameter)
    {
        return Math.floor(Math.random()*diameter)+10
    }

```

There a few things to notice. First, I changed the myCircle function and created three different functions. These functions return a random x, y, and diameter, respectively.  Then, I create a for loop to create the circle parameters and then print out the circles in the draw method using a for loop as well.  This time the circles don't move around but instead change color.  Cool right?

<a href="https://umontana.zoom.us/recording/share/R3VsswbEpZj7YqIbrP6IHXBXBUQe_qMmRO_K_Yia3PWwIumekTziMw" target="_new" style="font-family:Ariel; font-size:32px;">Click here for this section's Video</a>