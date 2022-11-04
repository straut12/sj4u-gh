---
layout: page
permalink: /ref/begcoding/extra-game/
title: Advanced Game
description: Create a more advanced game
nav: false
toc: true
---
# Advanced Game
Using the concepts and lessons from the previous tutorials I put together an [Asteroid game](https://scratch.mit.edu/projects/487729421) you can use for reference. Start with a game of your own and slowly build it up. The important thing is to use the tools you've learned 
* loops
* if-then
* variables
* functions (including importing)
* event listeners (broadcasting)

# Lists
Another concept added in the advanced game is lists. List is a sequence of elements that you can change, retrieve, do calculations on, etc. You can put text (strings) or numbers in a list. I used a list to keep track of who has the high score. One list for the Player name and one list for their Score. These have to stay synchronized. (In Python you could use a dictionary). At the end of the game a loop is used to compare the current score to the scores in the list and determine where it ranks. Then the player name is added.

[Asteroid game](https://scratch.mit.edu/projects/487729421)

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/extragame.gif" class="img-fluid rounded z-depth-2" %}
    </div>
</div>

Art work from [https://www.kenney.nl/assets](https://www.kenney.nl/assets)​ (There's a lot more art work you can use from kenney.nl. Check it out and add features to your games)​

To help navigate my code reference the meteor layout below
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/extragame1.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

For each Sprite/Clone loop I use Init, Main Loop, Game event layout below.

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/extragame4.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/extragame5.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/extragame6.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

>As you add more characters you can run into bugs due to events being out of sync. I used the broadcast event to handle this.

>My main character is the Starship. When the **Initialize** function completes it broadcasts a "Start Sprites" msg. The Meteor and PowerUp sprites do not start until then. This keeps them syncronized.

# Thinking Ahead
Now you're ready to progress to Python! Many of the concepts you practiced in Scratch will transfer directly to Python; loops, If-Then conditions, booleans, etc.

Other concepts will still transfer but have slightly different names and implementation
* Using functions from your backpack is similar to importing modules in Python
* Variables are even easier to use in Python. And you'll access to more advanced variables or containers called Classes (Object Oriented Programming).


-----------------------------  
-----------------------------  