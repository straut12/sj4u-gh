---
layout: page
permalink: /ref/begcoding/functions-variables/
title: Functions and Variables
description: Learn about functions and variables.
nav: false
toc: true
---
# Functions
In the previous section we created a set of **motion** controls to move the sprite. The motion controls take up a lot of space. If we put those blocks in their own function we can set them off to the side and just call it in our main loop. Another benefit of functions is that we can store it in our backpack and use it later in other programs. 
> Storing functions in your backpack and using them in another program is an important concept for Python. Think of it as **importing** when you drag it into another program.  

The function can be found in red **My Blocks", then **Make a Block** and hit ok (you can leave the other options to their defaults).
There are two parts to use the function (note 2 red blocks used below). 
1. Enter the code or actions you want the function to do. For our motion control it will be the If-Then conditions. If left arrow key pressed move -10 in X.
2. An then use the function by **calling** it in your program.

Code below is for the Dino ​sprite
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/func-dino.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Now our main loop is cleaner and we can easily add more code.
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/func-code.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# Variables
Another powerful tool in coding is variables or containers. Variables are containers that hold information. It can be numbers or letters (aka strings in Python). You can retrieve and modify the information at multiple points in your code. Here are just a couple ways we'll use variables in our code.
​​
1. ​If you use the same number many areas, for the same purpose, we could use one variable to represent all of them and define it at the beginning. For example our motion controller is using **10** in all four If-Then conditions. Let's replace it with one variable. That will make it easier to change it to a different value later. We only have to change one location instead of everywhere it is used, possible missing a spot.
2. If we're creating a game and we want to keep track of how many times we catch the grasshopper. Example each time we catch the grasshopper we want to increase our score by 1, we would use a variable and call it score.​​​  

First let's use a variable in our **Motion Controller** for the distance we're moving.  

> For -X and -Y we'll need to multiply our variable by -1  

There's a few steps to add a variable in our code (located in Variables block)
1. Make a variable and give it a name
2. Use the variable container in our code
3. We'll use a multiply block in our green **Operators** to handle the negative directions
4. Initialize the variable to a value using the **set** block. Notice we put it at the beginning, outside of our Main loop. We only want to assign it a value once(make it a constant). We don't want the main loop assigning a value every time it loops.

> To have the variable display on your screen while you're code is running you can keep it selected. Or de-select to have it hidden. If you're variable is a score you would want it displayed on the screen. Or if you're de-bugging and trouble shooting an issue you may want to display it.  

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/func-var.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Test your code and make sure the movement still works correctly.

## Local vs Global Variable
When you create a variable you can select **All Sprites=Global** or **This Sprite Only=Local**. Early on it is best to only do Global to keep it simple. Eventually you may need Local though so it is good to understand it.
* Global - All sprites can access and modify the variable. Example, if all sprites use a global variable called speed. If one sprites changes speed from 1 to 2 then all sprites will see the new value of 2.
* ​Local - Changes to the variable only affect the sprite you're working. Example, all sprites might have a Local variable called speed. If I change speed in one sprite from 1 to 2 it will not impact the speed variable in the other sprites.

> The concept of **local** vs **global** variable will be used a lot in coding  

# Debugging
Now that you're using variables is a good time to talk about structured debugging. Debugging is just fixing problems with your code. You've probably already been doing some debugging by replacing blocks that were out of place or the wrong one. However there will be a time that everything looks good with your code but it's still not doing what you want. Scratch does not have an official debugger with **break** and **step into** options but you can improvise.
* **Variable** - Display value. Select your variable name so value is displayed. Confirm if it is Local or Global.
* Sensing - **Ask what's your name and wait** Place this block at strategic point so you can stop your code and see if a variable is displaying what you think it should.
* **Synchronizing** - This can be hard to track down. You may have an action in one sprite that you expect to occur before an action in another sprite. However the reverse may be happening and you can't see it. If you can track down where the out-of-sync occurs try using a broadcast to force one sprite to wait for a msg from another. (broadcasting is covered in next section)
* **Delay blocks inside Input Loop** - A common gotcha is to put **wait** blocks (maybe animation, change costumes) inside a Loop you're also getting keyboard input. Your code will act weird because sometimes you're pressing a key when the code is at a **wait** block and it's not listening to the keyboard. If you're doing **wait** because of a costume change; see if you can put the animation in a separate loop by itself. If you're wanting the costume to only change when certain keys are pressed then you can use the **event** block **when key pressed** for keyboard input. 
* **Hidden blocks**. One of the most common issues I've found in classes is hidden blocks. Abandoned blocks on your scripting page are dangerous. They can get covered up by other blocks and you won't realize its there. Depending on the block it might still be interacting with your code and you can't see it. Below shows a couple examples. Your best bet is to always keep your scripting page clean and delete or drag un-used blocks off the page right away. The first shows my sprite changing costumes even though I don't see that in my code. But if I hit the centering icon in bottom right (horizontal lines next to zoom) it shows some abandoned blocks that were active. So I **delete** those and re-center.
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/func-abandoned.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Example below is sneakier. When you replace an item it's possible the replaced item goes behind the blocks. Double check for hidden blocks.
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/func-sneaky.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# Recap
* Functions are useful for keeping our code organized. We can have the function off to the side and **make a call** to it. This makes it easier to work on our main loop. We can also re-use the function. Store it in your backpack and import it into other programs.
* ​Variables are containers that hold information. They are a main part of coding and you will use them a lot. In Scratch there is an option to **set** and **change** the variable. Set will give it a **starting** value. Change is how you would **modify** it later in your code.  

# Thinking Ahead
* Functions have the ability to pass values to them. When you create a function there are options to include input/boolean. Where would this be useful?
* What if I want to add more grasshoppers to my scene? Do I have to keep adding more grasshopper sprites and copying the code to each one? (thankfully the answer is no, see next section)
* So far our code has been **sequence** based. It steps thru the code line-by-line. What if I want to add sprites and have them wait and listen for a specific event before running their code. This would be **event** driven code. In Scratch we use broadcasting for this.

# [Next Section: Clones/Broadcasting](/ref/begcoding/clones-and-broadcasting/)
-----------------------------  
-----------------------------  