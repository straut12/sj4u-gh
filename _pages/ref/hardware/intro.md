---
layout: page
permalink: /ref/hardware/intro/
title: Intro
description: Intro to Hardware
nav: false
toc: true
---
# STEM Hardware
Below is the hardware that I've gravitated towards in projects. There are many other SBC options and opinions on which H/W is best and it can be a great learning experience to compare them. For example I compared ADC response time of different raspberry pi vs esp32 microcontroller [ADC comparison](https://github.com/straut12/Joystick-MCP3008-RPi-ESP32-Compare).  

I've listed five categories of hardware below. Not all the hardware is required for any given project. Depending on the complexity of the project it may require only a couple or multiple parts.

# Raspberry Pi
<div class="row float-left">
    <div class="col-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/hardware/rpi.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Raspberry Pi is a SBC (single board computer) and contains a microprocessor, RAM, SD memory, wi-fi, etc. Can be dual purpose; the **digital** brains of a project and/or a PC/laptop substitute (RPi4 is best for this. The Raspberry Pi 4 has an operating system with many pre-installed programs and can be bought with 4-8GB RAM but will take some adapting to if you're used to windows/mac). These are relative terms but in general the Raspberry Pi is low cost (< $50), low power (< 15W), and has GPIO pins to interact with digital components or switches (think on/off or low/high). When a ADC (analog-to-digital converter) is attached it can also be the **analog** brains. However, it requires extra work and coding to get the same performance as an actual microcontroller like ESP32 below. 

# esp32
<div class="row">
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/hardware/esp32.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
esp32 is a microcontroller. The microcontroller is the 'analog' brains of the projects. If you're wanting to measure varying temperature, voltage, power usage, etc a microcontroller like ESP32 will be more efficient than a Raspberry Pi. You can use C/C++ or [uPython](https://docs.micropython.org/en/latest/). I focus on uPython since I use Python for other projects and I do not need the small time savings with C/C++. You load one 'main.py' program and the ESP32 will run that program when powered on. You'll need a laptop or RaspberryPi to write the programs and load them on to the ESP32. It has built-in wifi so once you're program is running you can send/receive data over wifi via [MQTT](https://docs.stemjust4u.com/ref/data-analysis/data-collection/).  

# Peripherals
Keyboard and mouse are straight forward peripherals. Monitors can be an expensive part of the project if you don't already have one. If you have a PC/laptop you can VNC to a Raspberry Pi and work remotely.

If you'll only be working inside the power supply part is less of an issue (you can just use a 5V phone charger for many of the electronics). But if you want to make your project solar powered or portable then the power supply is critical. You'll want to learn how to safely use Lithium Ion (Li-on) batteries. These are common in 18650 size and frequently found in laptops and cordless power tools.

# Sensors and Circuits
This is external hardware you can wire to your GPIO pins on the Raspberry Pi and/or ESP32 and interact with the non-digital world. (You can also build stand alone circuit projects). There are too many parts to list here but some examples are cameras, motion sensors, voltage meter, power meter (by measuring current), buttons and switches. Many of these projects will use discrete parts like transistors and resistors and can use a bread board to connect them.

# Measuring Instruments (Metrology)
Instruments are not required to build a project. But they are very useful in understanding how the components work and trouble shooting issues. A voltmeter, oscilloscope, IR temp gun are some examples.

# Useful Terms
Different Types of Circuits
* Discrete circuit - Individual circuit parts like transistors, resistors, capacitors connected with wiring. You can build a circuit with thru-hole discrete components, solder, and wire on a breadboard. It will be very low density due to large size of parts and wiring. Also possible to use smaller, surface mount (SMD) components.
* Integrated Circuit (IC) - Known as a **chip** or **microchip**. The transistors and resistors are connected using photolithography methods. Substrate is usually silicon and built in a Semiconductor Fab. Parts are built at the sub-micron level resulting in highest density of circuits.
* Printed Circuit Board (PCB) - An insulated board (usually green or brown) often containing discrete and IC components along with connection ports, cooling fans, heat sinks, etc.​
* Single Board Computer (SBC) - Example is Raspberry Pi. A single board with all the components (processor, memory, input/output) to make a functioning computer.
* Microcontroller - Example is ESP32. Similar to a SBC but usually designed for embedded applications. Typically does not have a full fledged OS. You load a program to its memory and it will continually run that program when plugged in.


-----------------------------  
-----------------------------  
