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


## another sub header