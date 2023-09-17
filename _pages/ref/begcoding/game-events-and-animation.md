---
layout: page
permalink: /ref/begcoding/game-events-and-animation/
title: Game events and animation
description: Add more game events and animation
nav: false
toc: true
---
# Animation
Now that we have our sprites created and moving it would look much better if the sprites were animated. For this we'll start working with the **Costume** tab. Each of the sprites we're using have multiple costumes. This just means there are multiple frames for that image. This is essentially what a gif is. An image with multiple frames so it appears animated or like a short movie clip. We'll just loop through the frames in a forever loop.​

We'll cycle through the costumes using the **next costume** in the **Looks** blocks. Depending on how fast the piece of code is running you may need to insert a short **wait** to make the animation look good.

For the grasshopper and Terry we can put the **next costume** after their move step. We aren't trying to control them with the keyboard so there's no risk of interfering.

For Dino we'll need a short **wait** to make it look good. But we don't want to interfere with the Main loop that is listening for our keyboard left, right, up, down presses. If we put the **wait** in the Main loop we risk missing a keyboard press. The game will feel unresponsive. So we'll create a second  loop and have it cycle through costumes without interfering with the motion control.

Code below is for Dino sprite and then grasshopper and Terry are below.

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation1.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation2.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


# Game Outline
You've learned a lot of coding concepts with loops, if-then, variables, etc. And we have an animated scene with multiple sprites moving and even one we can control. Now we'll focus on making this into a game.​​​

If we're not careful the code will get out of control and hard to read as we add game events. It is important to stay organized and follow a structure. ​
​
## ​Initialize
* Set variables, rotation style, booleans
* Show or Hide
* Go to x:  y:  **Important**. Where you want your sprite to start
    If you have a lot of initialize steps put them in a function
​
## Game Loop
* The main loop you put your functions and events inside
* Motion controller would go in here
​
## Game Events
* These are all the interactions between sprites.
* Try and use functions to keep your code easy to read.

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation-events.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# More Game Events

## Score
The first obvious game event to add is a score. When dino eats a grasshopper (the  mouth touches a grasshopper) we want to increase our score.

Grasshopper sprite
This is an If-Then condition using **touching color** block and matching the color of Dino mouth.
1. Add If-Then with **touching color** block and using color dropper to get color of Dino's mouth (see image below)
2. Create score variable
3. Move **change score by 1** into the If-Then block
4. After changing the score delete the grasshopper clone so it doesn't keep adding more to the score​
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation-score.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
For my Dino I did a couple edits to his **Costumes**. I made every other costume a mouth or chomping costume to increase its frequency of eating a grasshopper.

I also added some delays after the costume changes for grasshopper and Terry to slow them down.

## Game Over
I then added an event for Game Over when Dino hits Terry.

Below is my final code! [Dino-grasshopper-code](https://scratch.mit.edu/projects/487067394/)
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation-dino.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation-grasshopper.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation-terry.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/animation-game.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# Recap
* Add animation by using gifs and using the **next costume** block.
* Adjust a delay after the next costume to make the animation look nice.
* Use functions and keep your code organized. One outline to follow is
* Initialize
* Game Loop
* Put motion, animation, events inside the Game Loop.
* (If you're controlling a character you'll probably need a separate animation loop to not interfere)

# Thinking Ahead
* Have fun with it and add more sprites and game events!
* Next we'll start looking at the image editor.

# [Next Section: Image Edit](/ref/begcoding/image-editing/)
-----------------------------  
-----------------------------  

