---
layout: page
permalink: /ref/begcoding/clones-and-broadcasting/
title: Clones and Broadcasting
description: Using clones to duplicate sprites and broadcasting to communicate between sprites.
nav: false
toc: true
---
# Clones
Cloning is a great feature in Scratch for duplicating sprites, especially in a game environment. If we want to add more grasshoppers all we have to do is clone him. 
The clone can be found in **Control** blocks.​
There are three important parts to it.
1. **create clone of myself**. Insert this where you want the sprite to start.
2. **when I start as a clone**. Put in the code you want the sprite to do. For our grasshopper it is the **repeat until** movement code.​​
3. **delete this clone**. It is important to clean up and delete the clone when he moves off the screen. Otherwise our clones will keep accumulating and slow down our program.

> You may have noticed our grasshopper always starts at the same location, which makes him predictable. To add an unknown element we'll use the random operator in our code.  

First we'll insert the **random** block for the starting Y position.

We'll also add a block from the **Looks** category. We only want the grasshopper clones to show on our screen. We don't want the main grasshopper sprite to be visible. So we need to **hide** the main grasshopper and **show** the clones. Try your program without the hide/show first and you'll see what I mean.

The last item is to add a **wait** or delay after creating each clone. This is how you can control how many grasshoppers are on the screen. Play around with the number. Try 0.2, 0.4, 0.6, etc.

Code below is for Grasshopper sprite
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-grasshopper.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Play around with the **wait** time. Below I put another option for adding multiple clones at once using a **repeat X** loop.
<div class="row">
    <div class="col-10 mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-blocks1.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-5 mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-blocks2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# Broadcast Events
So far our code has been **sequence** driven. The code is executed line-by-line. But we can add a sprite and have it wait and listen for a specific event. When that event occurs we'll execute the code for that sprite. This is **event** driven and can make your scene or game much more orchestrated.

For this portion I added a couple more sprites. A planet (it will not have any code). And a Pterodactyl (named him Terry). It will help to rename your sprites and play with the size.
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-broadcast.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

There's a couple steps to add broadcast (located in Events block). One sprite will **broadcast** the event and then another sprite that is **listening** react to it.
1. I will have the grasshopper broadcast **Start Terry** msg when he hits the planet Saturn.
2. Terry our Pterodactyl will be listening. When he hears **Start Terry** he'll fly down from the top.​
​
So how do we tell when the grasshopper has hit Saturn? We use a If-Then condition inside the clone loop. There is a **sensing** block that is handy to check when two sprites come in contact.

*There's two instructions below. The first is for the grasshopper. The second is Terry (Pterodactyl)*
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-code1.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-code2.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Test your code and make sure everything works.
<div class="row">
    <div class="col mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-grasshopper.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-terry.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# AND/OR and BOOLEANS
You'll notice Terry stutters when he first receives the broadcast message and enters the screen. That's because the grasshopper is sending multiple messages as it crosses Saturn. We could fix this by using logical AND with a boolean. 
Boolean is just a variable that we set to either TRUE or FALSE
AND is used to check if multiple conditions are met
OR is used to check if either/or condition is met.
​
So when we start the program, Terry will initialize a BOOLEAN called **Terry Waiting?** to TRUE. And then we don't let the grasshopper broadcast a msg to start Terry unless both conditions below are met
Grasshopper is touching Saturn **AND**
Terry Waiting? is **TRUE**
​This will prevent us from having multiple Terry's flying at once.

You'll notice for Terry when he starts flying, the **show** block, we'll set Terry Waiting to **FALSE**. And when he is done flying we'll set it back to **TRUE**.
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-terry1.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-terry2.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-terry3.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col mt-3 mt-md-0">
        {% include figure.html path="assets/img/begcoding/clone-grasshopper3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# Recap
* Clones let us create duplicates of a sprite. Important to remember to delete the clone when it is done in the scene.
* Broadcast gives us the ability to add **event** driven code. You have one sprite broadcasting a msg and another (or multiple) listening.
* Logical AND/OR give the ability to compare multiple conditions. You can use this with a True/False boolean to keep track of events.
​​
# Thinking Ahead
* We have all our sprites created and moving on the screen. What if I want to add animation?
* What if I want to keep track how many times the Dino eats a grasshopper?

# [Next Section: Game Events/Animation](/ref/begcoding/game-events-and-animation/)
-----------------------------  
-----------------------------  


