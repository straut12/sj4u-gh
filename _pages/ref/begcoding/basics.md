---
layout: page
permalink: /ref/begcoding/basics/
title: basics
description: Basics with loops and if/then
nav: false
toc: true
---

# X-Y Coordinate
The Scratch windows works on a X-Y coordinate system. So it's important to understand what that means for moving the characters.​
1. We'll change the background to an X-Y Graph.
2. Then we'll change the sprite to a dinosaur.
3. I'll use the X and Y motion blocks to show how much and what direction the sprite moves with 100 increment. Pay special attention to positive and negative direction. 
4. Often your character will end up off the screen. To get him back just manually type a 0 for X and Y in the box to re-center him. 
5. The last button to keep an eye on is the hidden/un-hide. Make sure you have the character visible if it disappears.

Code below is for Dino sprite

# If-Then Condition
Now let's use If-Then conditions to move our sprite based on what key we press.  
If some condition is True, Then do this
<div class="row">
  <div class="col-md mt-3 mt-md-0">
    {% include figure.html path="assets/img/begcoding/basics-dino.gif" class="img-fluid rounded z-depth-1" %}
  </div>
</div>

​​​​This will be a common **control** set of code for games. 
* The If-Then block is under **Control**
* The **some condition** we'll be checking is under **Sensing**​
* I'll use **right mouse click** to duplicate the blocks to make building quicker.
* Make sure you have positive and negative values lined up to the correct keys.
* To make our code constantly check for a key press we need to put all of it in a Loop.
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/basics-loop.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Test your code out and move the dino with your arrow keys.
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/basics-dino2.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# Nested Loops
You've been using loops to keep your're program running and waiting for input from the user, you (moving your mouse or pressing a key). A method to get even more out of loops is to place one inside of another, or create nested Loops.

## Add a Second Sprite
Let's add another sprite to our dinosaur code. But instead of controlling the movement with the keyboard let's have the sprite automatically move from left to right of the screen. And then when he goes off the screen have him come back and do it again. So we need two loops.
1. Main loop to keep the character coming back to the beginning of the screen.
2. Sub loop that moves him from left to right. For this loop we'll use a **repeat until**. We want to keep moving (**repeat**) to the right **until** we hit the edge of the screen.
​Don't forget to add another sprite for this 2nd character. The code below will belong to him. I chose the grasshopper. You should have two sprites/characters now.
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/basics-grasshopper.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-4-auto mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/basics-grasshopper1.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/basics-grasshopper2.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Test your code out. The grasshopper should move across the screen while you move the dinosaur with your keyboard.

Is it starting to look like a possible game?
* What if the dinosaur got points for catching the grasshopper?
* What if the grasshopper started at random points instead of the same spot? (hint: try the green **pick random _ to _** instead of -21 for our initial position)​​
* What if we had multiple grasshoppers

> If we want to turn our dino to look to the left we can insert the "set rotation style left-right" block. And set the direction to -90

# Recap
* The window is based on a X/Y coordinate system. Max/min X is +240/-240 and max/min Y is +180/-180
* When creating multiple sets of the same type of blocks use right click, duplicate.
* Save code for later use by storing it in your backpack.
* If you're sprite disappears, check that "visible" is turned on and enter X=0, Y=0 in the coordinate boxes.
* If-Then can be used to check for a specific condition and then take an action.
* Loops can be used to keep our program running and checking for input like a key press.
* Loops can be nested. One loop keeps our program running while another loop inside of it makes a sprite move.
* A second kind of loop is "repeat until". It loops until a "condition" is met.

# Thinking Ahead
* My dino code window is getting filled up with If-Then conditions. What if I want to add more features? How can I organize and keep it cleaned up?
* Instead of moving my dino 10 steps when I press a key what if I want to move him slower or faster? Do I have to update every condition each time I want to tweak it?

# [Next Section: Func/Variables](/ref/begcoding/functions-variables/)
-----------------------------  
-----------------------------  