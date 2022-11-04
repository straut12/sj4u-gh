---
layout: page
permalink: /teaching/
title: teaching
description: Set of tutorials
nav: true
toc: true
nav_order: 5
---
# Example table

Instructions for different display (/boot/config.txt) or can change to boot
to 'cli' and run headless with virtual VNC.

``` console
$ Is this code
$ or is this code
$ butwhatifihaveareallylonglineofcodeoverandasdfhapuihpapjiofn-u2puhjn;lajnd-23r;jn;alkjnv;akjds;fha;ih432p;oi4n98a-dvhpnjn;ljknfajdlf;ahdsfjasdfa8f-89dn;N:DFLKSja;dsfj  
```

Quick summary to help decide which device is best and help when downloading packages.  

Syntax highlighting is provided within `<d-code>` tags.
    
    $ sudo apt-get update

All devices below have built in wi-fi.

|model|core       |cpu           |ARM  |arm  |Hz    |RAM    |Power Usage*  
|-----|-----------|--------------|:----|:---:|------|-------|------------  
|pi0w |single core|Broadcom2835  |ARM11|armv6|1GHz  |512MB  |0.7/1.2 Watts  
|pi3A+|quad core  |Broadcom2837B0|A53  |armv7|1.4GHz|512MB  |1.2/2.3 Watts  
|pi3B |quad core  |Broadcom2837  |A53  |armv7|1.2GHz|1GB    |1.3/2.5 Watts  
|pi3B+|quad core  |Broadcom2837B0|A53  |armv7|1.4GHz|1GB    |1.7/3.0 Watts  
|pi4  |quad core  |Broadcom2711  |A72  |armv8|1.5GHz|2,4,8GB|3.5/5.0 Watts  

* Low side is idle and high side is using programs.  Numbers are estimated.  
When working on a solar or battery project need to measure the power usage for that  
specific pi setup.  Watts (Power) = I (currrent A) x V (voltage)

Notes for RPi4
- has dual display ports (having 2 screens is nice for working on projects)
- requires larger power supply and type-C charging cord.

# Some Plain Text

Some network commands

	route -n
	ifconfig
	$ sudo apt

* Low side is idle and high side is using programs.  Numbers are estimated.  
When working on a solar or battery project need to measure the power usage for that  
specific pi setup.  Watts (Power) = I (currrent A) x V (voltage)

## Notes for RPi4
- has dual display ports (having 2 screens is nice for working on projects)
- requires larger power supply and type-C charging cord.
* Low side is idle and high side is using programs.  Numbers are estimated.  
When working on a solar or battery project need to measure the power usage for that  
specific pi setup.  Watts (Power) = I (currrent A) x V (voltage)

Notes for RPi4
- has dual display ports (having 2 screens is nice for working on projects)
- requires larger power supply and type-C charging cord.
* Low side is idle and high side is using programs.  Numbers are estimated.  
When working on a solar or battery project need to measure the power usage for that  
specific pi setup.  Watts (Power) = I (currrent A) x V (voltage)

Trying to get code syntax

    route -n
    $ How do this look

Notes for RPi4
- has dual display ports (having 2 screens is nice for working on projects)
- requires larger power supply and type-C charging cord.

# Python Code

## Python Code with 3 Hash Marks

```python
# This is similar to importing functions from your backpack
import random
import time

# Initialization - Setting screen size and variables
WIDTH = 400
HEIGHT = 708
GAP = 150
SPEED = 3
GRAVITY = 0.3
FLAP_VELOCITY = -6.5

# Sprites - links to images in your folder
bird = Actor('flappy_bird/bird1', (75, 200))
pipe_top = Actor('flappy_bird/top', anchor=('left', 'bottom'))
pipe_bottom = Actor('flappy_bird/bottom', anchor=('left', 'top'))

# Main game loop
def update():
    update_pipes() # Animation function drawing pipes
    update_bird()  # Game animation/events for flappy bird

# Initial states for flappy bird sprite
bird.dead = False
bird.vy = 0

# Function that draws the background and sprites to the screen
def draw():
    screen.blit('flappy_bird/background', (0, 0))
    pipe_top.draw()
    pipe_bottom.draw()
    bird.draw()

# Function for flappy bird events
def update_bird():
    bird.vy += GRAVITY
    bird.y += bird.vy
    bird.x = 75
    if not bird.dead:
        if bird.vy < -3:
            bird.image = 'flappy_bird/bird2'
        else:
            bird.image = 'flappy_bird/bird1'
    if bird.colliderect(pipe_top) or bird.colliderect(pipe_bottom):
        bird.dead = True
        bird.image = 'flappy_bird/birddead'
    if not 0 < bird.y < 720:
        bird.y = 200
        bird.dead = False
        bird.vy = 0
        reset_pipes()

# Function for flappy bird motion controller
# Any key press will increase flappy bird Y position
def on_key_down():
    if not bird.dead:
        bird.vy = FLAP_VELOCITY

# Function to create pipes
def reset_pipes():
    pipe_gap_y = random.randint(200, HEIGHT - 200)
    pipe_top.pos = (WIDTH, pipe_gap_y - GAP // 2)
    pipe_bottom.pos = (WIDTH, pipe_gap_y + GAP // 2)

# Function for pipe motion
def update_pipes():
    pipe_top.left -= SPEED
    pipe_bottom.left -= SPEED
    if pipe_top.right < 0:
        reset_pipes()
```

# More Notes for RPi4
- has dual display ports (having 2 screens is nice for working on projects)
- requires larger power supply and type-C charging cord.
* Low side is idle and high side is using programs.  Numbers are estimated.  
When working on a solar or battery project need to measure the power usage for that  
specific pi setup.  Watts (Power) = I (currrent A) x V (voltage)

Notes for RPi4
- has dual display ports (having 2 screens is nice for working on projects)
- requires larger power supply and type-C charging cord.
* Low side is idle and high side is using programs.  Numbers are estimated.  
When working on a solar or battery project need to measure the power usage for that  
specific pi setup.  Watts (Power) = I (currrent A) x V (voltage)