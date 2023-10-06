---
layout: page
permalink: /ref/data-analysis/workflow/
title: data
description: Intro to Data Analysis
nav: false
toc: true
---

The work flow below is geared toward a STEM project involving hardware, circuits, coding, data collection, etc. But some of the concepts are broad and applicable to other areas.  

General data analysis terms  
- Passive data collection vs experimental (DOE) will influence what types of tools you use below
- Data storage - Relational vs non-relational vs Time Based DB. [databases](../../../ref/data-analysis/databases/)
- Data Scrubbing - Depending on the purpose of the data some cleaning up may be required. If you're monitoring a process/project then outliers will be important to track and analyze. If you're optimizing a process you want to note the outliers but may not want them to influence calculations.
- Data Mining - Drilling down through large data sets for analysis. Creating maps of the data stream to make data retrieval and organizing more efficient  
- Analysis - Tools for finding relationships, commonalities, trends. [Data Analysis](../../../ref/data-analysis/ml/)
- Visualization - Charts, plots, tables [Data visualization](../../../ref/data-analysis/data-visualization/)


Maker Space [stemjust4u](http://stemjust4u.org/)  
# STEM Workflow  
This is my general workflow I follow for projects. I often start with RPi/Python and then translate it to esp32/uPython.  

As you start working on STEM project you'll quickly run into the scenario where you want one device (ie an ESP32 microcontroller) to send data and receive commands from another device (ie a Raspberry Pi or your phone). A simple way to do this is with a MQTT-Node-Red server running on a RPi. You can also add a database (ie Influxdb) to save data for charting in Node-red or Grafana. Data analysis involves measuring, transferring, visualizing, and analyzing the data. Below are tools that I have tried out. There are obviously many more options (ie google data studio) these are just the tools I've found easy to learn and useful.  

1. MQTT - Sends data/commands between devices (remote machines and a server)
2. Influxdb(sql) - Store the data in a time based database
3. NodeRed(javascript) - Receives the data from mqtt and stores it to the influxdb. Has a gui dashboard with gauges or you can retrieve historical data and chart it. Javascript functions can be written to handle the data flow.  

For comparing different hardware options (RPi vs esp32) and power supply/memory considerations start at the [hardware section](../../../ref/hardware/raspberry-pi/#power-supply/) 

If setting up a new Raspberry Pi for the project.  

1. Use RPi imager to install the Raspberry Pi OS. (I usually install the full version and then remove programs I do not need.) Click on the options for RPi imager and you can enter setup info like a unique hostname, wifi info, etc. This makes it easier to setup without a monitor.
2. Plug the RPi and go through the setup screens. 
3. Go to 'Raspberry Pi Configuration' Menu and 'Interfaces' and turn on SSH/VNC for remote Pi operation (may also need to turn on SPI, I2C, or 1-wire, depending on the hardware you're connecting). After reboot you can note the IP address and use it with RealVNC viewer to connect remotely from a desktop PC/laptop. (Giving the RPi a unique hostname and using it is useful since the IP address will not stay constant. Or set-up a RealVNC connect account and connect even from a different wi-fi. Free for Raspberry Pi)
4. To free up space and minimize the time required for updates remove programs that are seldom used. See bottom of my list of software page. Before installing programs/packages it is always a good idea to do an update ($ sudo apt update && sudo apt upgrade)
5. (optional - to change the RPi appearance/desktop and/or customize the desktop start at RPi options. For details on how installing packages work continue on to software package section) 
6. gdebi is what I use for installing DEB packages ($ sudo apt install gdebi)
7. VScode is the main editor I use. 
8. Git is what I use for maintaining project code (follow setup of ssh key if you're going to maintain the code on github).



## Hardware 
Hardware/Circuits/Sensors/Peripherals - Adafruit libraries are what I primarily use to simplify getting data from sensors (Adafruit also has a line of SBCs and useful parts/circuits for projects). The libraries are primarily written in circuitpython to work with Adafruit SBCs but they also have a Python/RaspberryPi compatability tool called [blinka](https://learn.adafruit.com/circuitpython-on-raspberrypi-linux) that will install **board** module. Most projects will use either Raspberry Pi specific GPIO libraries called [RPi.GPIO](https://sourceforge.net/projects/raspberry-gpio-python/), which are installed on RPis by default, or more generic [gpiod](https://learn.adafruit.com/adding-a-single-board-computer-to-blinka/software-setup) C library/tools for working with Linux GPIO hardware. gpiod is the tools, libgpiod2 is the library, python3-libgpiod allows Python to use the library.  
```sudo apt-get install libgpiod2 python3-libgpiod gpiod```   

For simple gpio functions like turning an led on/off I will use [gpiozero](https://gpiozero.readthedocs.io/en/stable/). gpiozero is an interface that simplifies the code for working with the pins. If you're not working in a virtual environment then gpiozero can use the RPi.GPIO modules, installed on RPi by default, or [pigpio](http://abyz.me.uk/rpi/pigpio/download.html) modules for the pin factory. However when working in a venv I've noticed I have to specifally use pip3 to install RPi.GPIO (or pigpio) in my env for gpiozero to see the modules.  

Also when possible check that hardware/circuits work before soldering or trying to control it with software. Some examples  

1. Connect LED to 3.3V and confirm it lights up.
2. Use a servo tester to confirm the servo motors work. 
3. Use alligator clips to hook up a 18650 battery to a li ion charger and the device it will power. Confirm charging/discharging works. I've come across multiple bad chargers. They work fine with no load but would shut off with even .25 Watt load.
4. The hardware trouble shooting section is good to understand before moving on to more advanced projects.
5. For I2C make sure tools installed
    * ```​sudo apt install -y python-smbus```  
    * ```sudo apt install -y i2c-tools```  

​
## Coding  
1. Initialize an empty github repository    
2. Select the option to create the README and .gitignore (python template) github   
3. Copy the ssh link under Code  
4. Clone the empty repository -  Cloning can either be done inside an editor like VScode or going to the RPi the project will be done on (often using vnc) and clone the repo with $ git clone git@github.com:user/repo.git (ssh link copied from github). Or inside VScode connect to your github account, remote to the RPi and in the pallette type git clone. Search for the repository and select the location to clone to.
5. Start Coding - The setup I've found myself using most is VScode. I use the VScode remote connection extension to connect to the RPi's and code from another linux/windows PC. (The exception is Pi0. The VScode remote extension does not work on Pi0. So for Pi0 projects I configure the Pi0 git project folder as a samba share and use a txt editor to work on the code from a PC.)  
6. In VScode remote connect to the RPi3-4
7. Open the cloned github folder and confirm .gitignore is setup. 
8. Open the terminal and create virtual env. Can install/use other python, ie python3.7
9. $ python3.7 -m venv .venv
10. $ source .venv/bin/activate and confirm Python (.venv) interpreter selected in bottom
11. Install any required packages. ie $ python3.7 -m pip install 'package'  

Get first pass, functional code working on "main" branch  
Code, test, code, test, etc  
* I use the git commit/syncronize to keep the code uploaded to github. This keeps a backup and makes it easier to transfer and test on other devices.
* Add functionality by creating a develop branch and merging it back with the main when complete. (see functionality section below)
* If using a venv and need the program to run outside of the venv environment (ie need it to run on startup) then make sure to update the shebang #! header of your python file to point to the python path in your venv. Example #!/home/pi/projectA/.venv/bin/python3 
* You can then make it executable with $ chmod +x filename.py 
* Now you can run the program with $ ./filename.py 

​​
## MQTT/Servers/DB  
Sending commands, collecting data - Most projects involve remote commands, sending instructions, and collecting data. I use mqtt, node-red, influxdb for this. I have a Pi3B dedicated as a mqtt/node-red server. Login at hostname.local:1880 and create node-red flows. (* I give each Pi a different hostname and use hostname.local instead of IP address when possible. The IP address can be problematic when switching from stand alone to wifi connected)

## esp32/uPython  
Translate to esp32 - In the git folder create a /upython folder and copy boilerplate/template main.py code for whatever type of project. The esp32 will be connected to USB port.  Two options below for coding esp32. Both have pros/cons.
1. RPi and Thonny - Remote connect from a PC and using Thonny copy code from the upython folder to the esp32. 
    Code, test, code, test, etc. ​When completed copy the code from esp32 back to git project folder and sync using VScode.​​  
2. Linux PC and VScode with Pymakr.  Can use VScode to edit and sync directly



## Run at Startup
There are multiple methods to enable a program to run at startup. For a full list check the [autostart/start-on-boot](../../../ref/linux/autostart/) section. If using a virtual env; to statisfy dependencies make sure your python program has a shebang header pointing to the python path in your virtual env.
example.  
```console
!/home/pi/project_dir/.venv/bin/python3 
```
And make the program executable with 
```sudo chmod +x program.py```  

A quick systemd setup can look like this  
```sudo nano /lib/systemd/system/program_name.service ```  
```console
[Unit]
Description=Python Program to Run at Startup

[Service]
User=pi
Group=pi

ExecStart=/home/pi/project_dir/program.py
Restart=on-failure

[Install]
WantedBy=multi-user.target
```  

**Call graph**
A call graph can be helpful to visually see which functions are calling which functions.  

1. Install graphviz $ sudo apt install graphviz or can try linuxbrew
2. In activated venv install project graph $ python3x -m pip install git+https://github.com/fior-di-latte/project_graph.git
3. Inside project directory run $ project_graph your-program.py 
4. A .png file should be created. 
5. If a graph.dot is created but no .png then try $ dot -Tpng graph.dot > output.png 

​
Another method is pycallgraph2

1. Install graphviz $ sudo apt install graphviz or can try linuxbrew
2. In activated venv install graphviz $ python3x -m pip install pycallgraph2 
3. Inside project directory run $ pycallgraph graphviz -- ./mypythonscript.py

## ​Git branches  
If you will be adding features use git branches. "main" will be the default primary code. Work on main branch until you have preliminary working code (useful to test initial functions and confirm everything is operating)  

​Add More Functionality (branch from develop)  
To keep your initial main branch in-tact while working on features create a permanent develop branch that you branch off of. 

    $ git checkout -b develop main (branch off from develop for features)

Now create branches for features

    $ git checkout -b feature1 develop (creates feature branch and checks it out)
    Can create and work on multiple features and make commits

When finished, merge the feature back into develop branch

    $ git checkout develop (switch from feature back to develop)
    $ git merge --no-ff feature1 (merge develop and feature1)
    $ git branch -d feature1 (delete the feature1 branch)
    $ git push origin develop (update the remote repo)

When you've completed all features and want to update main

    $ git checkout main
    $ git merge develop

Bug Fixes (branch from main)

    $ git checkout -b hotfix1 main (creates hotfix branch and checks it out)
    Can work on the bug and make commits

Merge the bug fix back into main and develop branch

    $ git checkout main
    $ git merge --no-ff hotfix1
    $ git checkout develop
    $ git merge --no-ff hotfix1
    $ git push origin develop (update the remote repo)
    $ git push origin main (update the remote repo)
    $ git branch -d hotfix1 (delete the hotfix branch)

​
Other helpful commands​
```(.venv)$ python3 -m pydoc -n IPaddress (browse thru the installed package/modules)```  
   (IPaddress = 0.0.0.0)  
To find files use 
```grep -r 'name' .```   
​To browse pi use  
```python3 -m http.server​```  

> If you have a monitor hooked up to the RPi (or VNC) you can use VScode for a gui alternative to many of the command lines. Will also want to make sure you have a keyring setup with VScode for git functions.
```nano .gitconfig``` (confirm your git setup)  
Inside VScode
```console
F1
>Git: Clone
Clone from GitHub
A list of your repositories will come up. Select the Repository and the folder to clone it in.

Now you can do New Terminal, create/activate .venv and install from requirements.txt

(if using vscode ssh for remote work may need to install python extension after connecting)
```

## Copy Project  
To transfer project to another system  
Output the python packages required to a requirements.txt file  
(.venv)$ pip3 freeze > requirements.txt​  
or $ python3 -m pip3 freeze > requirements.txt  

Confirm the git remote and push to github
$ git remote -v
$ git add . 
$ git commit -m "message details"
$ git push -u origin main (or $ git push)


Go to new device/location
$ git clone git@github.com:user/repo.git (ssh copied from github)

Create/activate a .venv
$ python3 -m venv .venv
$ source .venv/bin/activate
install with -r for requirement install
(.venv)$pip3 install -r requirements.txt 
or $ python3 -m pip3 install -r requirements.txt

​(if using vscode ssh for remote work may need to install python extension after connecting)

## Create Python Package  
​Python Package - If the code/project will be used in many other projects create it as a package that you can import. This will keep your main code more organized and smaller.

1. Create a package folder with __init__.py
2. __init__.py - There are multiple approaches to the import 
    * import package.module
    * from .module import classA
3. Put the module code in the package folder and use __name__ == "__main__": to run module as stand alone script.
4. For the top script use appropriate import based on package __init__ (1:from package import module as xx or 2:import package)
5. Call the functions (var=xx.function()  var = package.classA())​

----------------------------