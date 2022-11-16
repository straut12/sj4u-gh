---
layout: page
permalink: /ref/linux/autostart/
title: autostart
description: Intro to starting programs on boot
nav: false
toc: true
---
# Start at Boot
**Options for Starting an Application at Startup (or scheduled time)**

Below are some options for making a program automatically launch during start up. This is useful for programs setting up your desktop environment and accessibility (a dock, conky, vnc, ssh, mqtt) or if you have a python script you want to automatically start up (send output to a screen, start collecting sensor data, etc). You could also schedule a script that does some function every hour, every minute, etc.  

One thing to consider is if the program/service you're starting is graphical (GUI) and needs the desktop/login or needs to login to the network wifi. It will be helpful to have a little background on the boot/start up process.  

**Linux boot process**  
* [BIOS](https://en.wikipedia.org/wiki/BIOS) [POST](https://en.wikipedia.org/wiki/Power-on_self-test) - Power On Self Test. BIOS checks/initializes the hardware. Loads MBR.
* [MBR](https://en.wikipedia.org/wiki/Master_boot_record) - Master Boot Record. Located at beginning of boot disk. Loads GRUB2/bootloader.
* [Bootloader](https://en.wikipedia.org/wiki/GNU_GRUB) –  GRUB2 for Linux. Primary job is get the operating system kernel (Linux) loaded into memory and executing.
* [Kernel](https://en.wikipedia.org/wiki/Linux_kernel) - Located in /boot. Executes init (/sbin/init) and hands over to systemd (start up).  

**End of boot**  
**Start-up Process begins**  
* [systemd](https://en.wikipedia.org/wiki/Systemd) – System and service manager (d for daemon). The init system that takes over the boot process from the bootloader (GRUB2) and starts the rest of the system. init is the first process started. ```$ ps aux``` will show it as PID1. It will mount the file systems defined in /etc/fstab and then starts up and maintains userspace services.
End of Start Up and PC is Running 
* [userspace](https://en.wikipedia.org/wiki/User_space_and_kernel_space) - Code running outside the kernel space or after kernel is booted.  


There are multiple options for having a program start at boot  
**Startup Options**  
* systemd (most options/complex)​​  
* cron (also a scheduler)  
* "Startup Applications" in desktop settings (GNOME/MATE)  
* "autostart" file (RPi with LXDE)  
* .desktop files in autostart folder (when X session started)  

*​Older method (deprecated)*  
* rc.local  

---------------------


To help debug a program during startup (and if not using systemd) modify your ExecStart/Command temporarily to log the output of your python print statements and errors..  
example  
```/bin/bash -c '/usr/bin/python3 /home/pi/script.py > /home/pi/script.log 2>&1'```  

Explanation  
stdout=1, stderr =2  
\> tells stdout (python print statements) to go to the .log file  
2>&1 tells stderr(2) to go to the same place as stdout (1). So both will go to the .log file. (is also a way of executing a script and not having the errors show on the terminal)  

---------------------------------

# Ubuntu Options
​​Ubuntu (GNOME/MATE desktop environment) Options  
1. Startup Application (in desktop environment after login)  
2. Cron (scheduler and at startup)  
3. Systemd is the most complex but most thorough  

**Startup Application**
GNOME/MATE have a simple method for starting on boot. In settings search for "startup" and select "startup application". The command line will use the similar format as in a .desktop file.  

Some example command lines  
* /usr/lib/firefox-esr/firefox-esr  (full path to firefox-esr executable since /usr/lib is not in the $PATH)
* vncviewer (vncviewer executable is in /usr/bin/, which is in $PATH, so do not need the full path)
* sh -c 'sudo ./test.py'  (Run shell/bash and using sudo. Note-  test.py was made executable and has shebang with python version specified)
* You can check $HOME/.config/autostart for the .desktop file that is created.
* In Gnome, to disable the autostart, a X-GNOME-Autostart-enabled=false can be used in the .desktop file.
* ​Try running the shell in a terminal if it is not starting.  

​Alacarte (used on RPi) can be installed and then used to view exec commands for other application icons. See [desktop](../../../ref/linux/desktop/) section  

**Cron (scheduler and @boot)**
* to install ```$ sudo apt-get install gnome-schedule```  
* run ```$ crontab -e```
* select your editor (I use nano)
* often a delay may be necessary with "sleep 30" or other amount in the command line. See examples below. It is a good practice to use a shell file that handles the delay and launching your program. Then put the shell file in cron.  
There are directions in the crontab. Examples below in RPi Section. Or [Ubuntu community](https://help.ubuntu.com/community/CronHowto) has examples.


-------------------------------

# Raspberry Pi Options
Raspberry Pi  
1. Cron (scheduler and @reboot)
2. rc.local DEPRECATED
3. Autostart script or folder (for .desktop files)
4. Systemd is most complex. Some guidelines are at the bottom.

**1-Cron (scheduler and @boot)**
Raspberry Pi OS will have cron already installed. If not installed do  
* ```$ sudo apt update```  
* ```$ sudo apt install cron```  
* ```$ sudo systemctl enable cron```  
Once Installed 
* run $ crontab -e
* select your editor (I use nano)
There are directions in the crontab  
At the bottom you will enter the program to run  

```console
#	m	h	dom	mon	dow	command	
0	5	*	*	1	tar -zcf /var/backups/home.tgz /home/	#weekly backup
0	0	*	*	*	/home/pi/backup.sh	#this would run backup.sh every day at midnight
*	*	*	*	*	sh /home/pi/script.sh 2>&1	# run script.sh every minute
@reboot sudo python3 /home/pi/script.py &	# run script.py at boot (& means in background so pi keeps booting)
```
Save, reboot $ sudo reboot 0   
Some other useful options  
often a delay may be necessary with "sleep 30" or other amount in the command line. It is a good practice to use a shell file that handles the delay and launching your program. Then put the shell file in cron.  
```$ crontab -l ```(list scheduled tasks)  
```$ cd logs```  
```$ cat cronlog ```(view log when it doesn't run correctly)  

Try a simple test first and add this line to cron  
```@reboot echo "cron test" > /home/USER/crontest.txt 2>&1```  
Then reboot and check crontest.txt for success  

--------------------------------------

**2- Editing rc.local is often suggested but it is now deprecated**  
$ sudo nano /etc/rc.local  
Add lines similar to cron (without the @reboot) sudo python3 /home/pi/script.py &  
Add commands below the comment but leave the 'exit 0' line at the end.  Save, reboot.  
Note - as shown in shebang section you could make the files executable and add a shebang header instructing which program to use for executing.  

-----------------------------------------

**3- Auto-start using autostart script or .desktop files**  
​Autostart script Method  
For RPi running MATE or GNOME desktop follow Ubuntu instructions above.. use "startup applications"   

For RPi running default LXDE see below  
Locations (for RPi, ~ = /home/pi/)  
* ~/.config/lxsession/LXDE-pi/autostart (local)
* /etc/xdg/lxsession/LXDE-pi/autostart (global)
* If no user (~) autostart script is found it will run global script in /etc/ instead. **It does not run both automatically.** My RPi already has entries in the /etc/xdg.. script so I edited that one.  
But a better method is to 
1. create/edit the local and put your user commands in this file. 
2. Then have your local autostart the global by making the first line below..​  

```console
@/etc/xdg/lxsession/LXDE-pi/autostart
```

* Guidelines/notes
* Commands are started in parallel
* Enter the full path to your script (do not use ~ or .)
* " " and ' ' and escaping with \ are not allowed (do not try and reference a directory that has a space in it)
* Can put comments on the same line after a #
* Use -e for using a terminal (@lxterminal -e /path/to/script)
* @ means your script will be started again if it fails
* Put your script before the xscreensaver entry.  
```$ sudo nano /etc/xdg/lxsession/LXDE-pi/autostart```  

```console
@lxpanel --profile LXDE-pi
@pcmanfm --desktop --profile LXDE-pi

@compton
@plank
@/usr/bin/python3 /home/pi/test.py
​
@xscreensaver -no-splash
```

------------------------------

**[Autostart folder](https://specifications.freedesktop.org/autostart-spec/0.5/ar01s02.html) Method**  
You create a .desktop file for the program and place it in the folder below  
* ~/.config/autostart  
* /etc/xdg/autostart  
* If there is a duplicate .desktop file in both directories the ~/.config has priority  
​​
Example for application flameshot  
* Find where the executable is ```$ whereis flameshot``` returns /usr/bin/flameshot (/usr/bin/ is in $PATH so do not need full path for flameshot)  
* Now create a .desktop file and put it in the 'autostart' folder ```$ sudo nano /etc/xdg/autostart/flameshot.desktop```  
​
Reboot to confirm the application starts up. Instead of a application you could have a shell file that performs some function at startup.  

-------------------------------

# Systemd

What is [systemd](https://en.wikipedia.org/wiki/Systemd) and why would you use it? Systemd starts and manages background processes like the network, bluetooth, ssh server, printer, etc. Systemd itself is a background process. On a Raspberry Pi, if text output is selected in raspi-config, you'll see systemd updates on your screen during the boot. If you have a python script you want to start during boot you can create your own systemd service unit and use the systemctl commands to manage it. Although it is more work than just adding a line to cron or using the built-in autostart/startup script it is has more options and built-in logging. Ultimately it's up to the user to decide what option is best for their case. Inevitably you will have to use the systemctl commands anyway to start, stop, enable, disable services on many applications (vnc servers, ssh, node-red, influxd, etc).  So I decided to go ahead and learn how to create my own systemd service to understand how it works. There is a significant learning curve at first (compared to other start up methods) but I found the logging (journald) and systemctl commands to be very useful.​  

A good tutorial on using systemd is at [sparkfun](https://learn.sparkfun.com/tutorials/how-to-run-a-raspberry-pi-program-on-startup#method-3-systemd)  

## Build a Test Start-at-Boot Service
An effective method to better understand systemd is to build your own service. One of the common reasons for building your own service is for having a script start at boot or scheduled to run at specific times. This will involve creating a service unit where you specify the location of your script and what part of the boot process to start it. For the latter part it helped me to look at the boot logs and see when the current services are started with respect to different targets. By adding a logging line to my script I could see where it started in the log too. If I needed to change it I could try different settings in my service unit, reboot, and check the logs to see where it landed in the boot timeline.  

​Create a python script for the service unit to execute  
I created a python script that imported some systemd and journal (systemd logging tool) libraries to help make it evident where my python script was in the boot timeline. These modules wouldn't be necessary in a regular python script but I found them useful for my first systemd testing.  

```$ sudo apt install python3-systemd```  
```$ nano myscript.py  ```  
don't forget to make it executable with  
```$ chmod +x myscript.py```  

```python
#!/usr/bin/env python3

import logging
import systemd.daemon
from systemd.journal import JournalHandler

print('Testing a script to be started by systemd at boot')
jlogger = logging.getLogger('journal')            # Create a generic logger
jlogger.addHandler(JournalHandler())           # Add a journal type handle
jlogger.setLevel(logging.INFO)                      # Set to INFO level so all info is logged
jlogger.info('Journal updated by ' + __file__) # Add a comment to the journal
systemd.daemon.notify('READY=1')             # Notifies systemd the script ran
```

## Create Service Unit
Now start creating your service unit in /lib/systemd/system/  
**Important** - syntax is important. No spaces before/after the equal sign. And it is best to type the syntax instead of copy/paste from somewhere else. Your service may have problems if there are extra spaces.  

```$ sudo nano /lib/systemd/system/mytest.service```  
```console
[Unit]
Description=Creating my own systemd service
```

To see your unit file  
```$ systemctl list-unit-files | grep 'mytest'```  

----------------------------------


Add a Service section where you specify the location of the python script  
```$ sudo nano /lib/systemd/system/mytest.service```  

```console
[Unit]
Description=Creating my own systemd service

[Service]
ExecStart=/home/pi/myscript.py
```

----------------------------------


Now reload the daemon service and start your service  
```$ sudo systemctl daemon-reload```  
```$ sudo systemctl start mytest.service```  
```$ systemctl status mytest.service ```  (this will show the status of your service along with logs)
Could also check  
```$ journalctl --since "1 hour ago"```  

```console
● mytest.service - Creating my own systemd service
Loaded: loaded (/lib/systemd/system/mytest.service; static; vendor preset: enabled)
Active: inactive (dead)

Mar 13 13:44:36 raspberrypi systemd[1]: Started Creating my own systemd service.
Mar 13 13:44:36 raspberrypi /home/pi/myscript.py[1945]: Journal updated by /home/pi/myscript.py
Mar 13 13:44:36 raspberrypi myscript.py[1945]: Testing a script to be started by systemd at boot
Mar 13 13:44:36 raspberrypi systemd[1]: mytest.service: Succeeded.
```

----------------------------------

## Check boot log for Targets
To have our service start our myscript.py script at boot we need to add an Install section. But first I wanted to look at a boot log to see when different targets were reached.  
```$ journalctl -b | grep 'Reached target'```  

```console
Mar 09 16:42:37 raspberrypi systemd[1]: Reached target Local File Systems (Pre).
Mar 09 16:42:37 raspberrypi systemd[1]: Reached target Local Encrypted Volumes.
Mar 09 16:42:41 raspberrypi systemd[1]: Reached target Local File Systems.
Mar 09 16:42:41 raspberrypi systemd[1]: Reached target NFS client services.
Mar 09 16:42:41 raspberrypi systemd[1]: Reached target Remote File Systems (Pre).
Mar 09 16:42:41 raspberrypi systemd[1]: Reached target Remote File Systems.
Mar 09 16:42:42 raspberrypi systemd[1]: Reached target System Time Synchronized.
Mar 09 16:42:43 raspberrypi systemd[1]: Reached target System Initialization.
Mar 09 16:42:43 raspberrypi systemd[1]: Reached target Paths.
Mar 09 16:42:43 raspberrypi systemd[1]: Reached target Sockets.
Mar 09 16:42:43 raspberrypi systemd[1]: Reached target Timers.
Mar 09 16:42:43 raspberrypi systemd[1]: Reached target Basic System.
Mar 09 16:42:43 raspberrypi systemd[1]: Reached target Sound Card.
Mar 09 16:42:48 raspberrypi systemd[1]: Reached target Network.
Mar 09 16:42:54 raspberrypi systemd[1]: Reached target Bluetooth.
Mar 09 16:43:02 raspberrypi systemd[1]: Reached target Network is Online.
Mar 09 16:43:02 raspberrypi systemd[1]: Reached target Login Prompts.
#Mar 09 16:43:02 raspberrypi systemd[1]: Reached target Multi-User System.
Mar 09 16:43:02 raspberrypi systemd[1]: Reached target Graphical Interface.
```

To see the exact target name use  
```$ systemctl list-units --type=target```  
```console
UNIT                     LOAD ACTIVE SUB DESCRIPTION      
multi-user.target  loaded active active Multi-User System 
```
For initial testing I used "multi-user.target" for the WantedBy  
```$ sudo nano /lib/systemd/system/mytest.service```  
```console
[Unit]
Description=Creating my own systemd service

[Service]
ExecStart=/home/pi/myscript.py

[Install]
WantedBy=multi-user.target
```

-------------------------------------------


## Enable test.service to Start at Boot
Now we can enable our test.service to start our script (myscript.py) at boot  
```$ sudo systemctl enable mytest.service```  
Will reply that a symlink was created  
Reboot and check the journal logs of the last boot. A quick check is using egrep with the | (OR)  ​
```$ journalctl -b | egrep 'Reached target|mytest|myscript'```  

```console
Mar 13 14:13:27 raspberrypi systemd[1]: Reached target Swap.
Mar 13 14:13:28 raspberrypi systemd[1]: Reached target Local File Systems (Pre).
Mar 13 14:13:28 raspberrypi systemd[1]: Reached target Local Encrypted Volumes.
Mar 13 14:13:32 raspberrypi systemd[1]: Reached target Local File Systems.
Mar 13 14:13:32 raspberrypi systemd[1]: Reached target NFS client services.
Mar 13 14:13:32 raspberrypi systemd[1]: Reached target Remote File Systems (Pre).
Mar 13 14:13:32 raspberrypi systemd[1]: Reached target Remote File Systems.
Mar 13 14:13:33 raspberrypi systemd[1]: Reached target System Initialization.
Mar 13 14:13:33 raspberrypi systemd[1]: Reached target Paths.
Mar 13 14:13:33 raspberrypi systemd[1]: Reached target Sockets.
Mar 13 14:13:33 raspberrypi systemd[1]: Reached target Basic System.
Mar 13 14:13:33 raspberrypi systemd[1]: Reached target System Time Synchronized.
Mar 13 14:13:34 raspberrypi systemd[1]: Reached target Timers.
Mar 13 14:13:34 raspberrypi systemd[1]: Reached target Sound Card.
Mar 13 14:13:39 raspberrypi systemd[1]: Reached target Network.
#Mar 13 14:13:40 raspberrypi /home/pi/myscript.py[377]: Journal updated by /home/pi/myscript.py
#Mar 13 14:13:40 raspberrypi myscript.py[377]: Testing a script to be started by systemd at boot
#Mar 13 14:13:40 raspberrypi systemd[1]: mytest.service: Succeeded.
Mar 13 14:13:45 raspberrypi systemd[1]: Reached target Bluetooth.
Mar 13 14:13:50 raspberrypi systemd[617]: Reached target Paths.
Mar 13 14:13:50 raspberrypi systemd[617]: Reached target Timers.
Mar 13 14:13:50 raspberrypi systemd[617]: Reached target Sockets.
Mar 13 14:13:50 raspberrypi systemd[617]: Reached target Basic System.
Mar 13 14:13:50 raspberrypi systemd[617]: Reached target Default.
Mar 13 14:13:56 raspberrypi systemd[1]: Reached target Network is Online.
Mar 13 14:13:56 raspberrypi systemd[1]: Reached target Login Prompts.
Mar 13 14:13:56 raspberrypi systemd[1]: Reached target Multi-User System.
Mar 13 14:13:56 raspberrypi systemd[1]: Reached target Graphical Interface.
```

----------------------------

If the script doesn't start at the point you need it to there are many options. Here are some starting points and helpful commands. [freedesktop.org](https://www.freedesktop.org/software/systemd/man/systemd.service.html) and [archlinux.org](https://wiki.archlinux.org/title/Systemd)  
To look at current system  
```$ systemd-analyze```  
```$ systemd-analyze critical-chain```  
```$ systemd-analyze critical-chain mytest.service```  
mytest.service @9.376s  
└─basic.target @9.338s  
    └─sockets.target @9.338s  
         └─triggerhappy.socket @9.338s  
              └─sysinit.target @9.323s  

For a graphical representation  
```$ systemd-analyze plot > boot_analysis.svg```  
Then view the boot_analysis.svg in a image viewer  

Example. I had a script on an OSMC setup that I wanted to start soon after network connection to get the IP address (the connection time varies after each boot). But I didn't want the script to fail if no network connection was made. The IP address was just bonus information if available.  
```console
[Unit]
After= # Additional criteria to start the script after a target or service
[Service]
Type=idle (delayed until all active jobs are dispatched)
ExecStartPre=/bin/sleep 2 # added a 2sec delay
```
Here's the actual service file
```console
[Unit]
Description=Display on WaveShare E-Paper
After=graphical.target NetworkManager-dispatcher.service

[Service]
User=osmc
Group=osmc

Type=idle
ExecStartPre=/bin/sleep 2
ExecStart=/home/osmc/E-ink/bin/e-ink-art.py
Restart=on-failure

[Install]
WantedBy=multi-user.target
```

Another option is adding a .timer. In this approach you disable the main service (mytest.service) so it doesn't automatically start at boot. Instead you create a timer unit that is enabled to start at boot and it starts mytest.service based on a delay or schedule.  
To see current timers  
```$ systemctl list-timers```  

To create a timer (monotonic) for mytest.service  
```$ sudo nano /lib/systemd/system/mytest.timer```  
```console
[Unit]
Description=Timer for mytest.service

[Timer]
OnBootSec=2                         
Unit=mytest.service

[Install]
WantedBy=timers.target
```

Some more options including Realtimer too
```console
[Timer]
OnBootSec=5min
OnUnitActiveSec=1w    # and run every week

[Timer]
OnCalendar=weekly
Persistent=true            # if it missed last start run immediately
```
Always run the systemctl daemon-reload command after creating new unit files or modifying existing unit files.  


The system will run as root so if you're code opens files that are in your user directory (using Path.home()) you will need to add User/Group to the \[Service\]  
Example  
```console
[Service]
User=pi
Group=pi
```
If you do ```$ ls-lrt``` in your directory you can see what the User/Group is for your file.  


# Terms and systemctl commands

Quick terms
* systemd - Init system and service manager
* daemons – Non-interactive programs (typically end with 'd') that run in the background. Examples are cron, printing, ssh, ftp, filesystem mounts, etc.​
* init.d (/etc/init.d) - The init daemon is the first process started which then starts other processes, services, and daemons.
units - Resources that systemd manages
The type of resources is noted by the suffix (ie .service, .target, .timer, etc)  
* .target - Typically files are started at a "target" point during the start up process. Used to synchronize and group units, similar to run-levels. multi-user.target is a common example. Target tree at freedesktop.org shows multi-user.target will not start until basic.target services have started.
* .service - Basic units for running processes
* .timer - Unit type that can control when .service units are executed (ie add a delay or scheduled at a specific time of day). Similar to scheduling in cron.
* .wants - symbolic links are created to startup the service
* systemctl - Utility or tool for interfacing with systemd. Can manage, start/stop specific services or the entire server. Enable/disable for starting at boot.  

​To familiarize yourself with systemd it helps to see the services and targets currently in use on your PC.   
List all units in systemd path  
* ```$ systemctl list-unit-files```  

List of loaded and active units  
* ```$ systemctl list-units```  

List of inactive units  
* ```$ systemctl list-units --all --state=inactive```  

Can use --type filter to list different unit types  
To see list of available targets  
* ```$ systemctl list-unit-files --type=target```  

List of active service units. Some have exited but could be restarted.  
* ```$ systemctl list-unit-files --type=service```  

```console
$ systemctl --type=service
ssh.service loaded active running OpenBSD Secure Shell server
ufw.service loaded active exited Uncomplicated firewall
virtualbox.service loaded active exited LSB: VirtualBox Linux kernel module
```

To see only active and running service units  
```$ systemctl list-unit-files --type=service --state=running```  

```console
$ systemctl --type=service --state=running
ssh.service loaded active running OpenBSD Secure Shell server
vncserver-x11-serviced.service loaded active running VNC Server in Service Mode daemon wpa_supplicant.service loaded active running WPA supplicant 
```

To see dependencies of a specific service  
```$ systemctl list-dependencies --all <name.service>```  

To see logs from most recent boot  
```$ journalctl -b```  

Managing daemons with sysvinit vs systemd  
This topic can be confusing since there was a migration from older "init" or "SystemV init" to the newer "systemd." You'll find references to both commands. systemctl is backward compatible and recommended utility.  
1. The system first checks 'systemd' folders  
    * systemd (/lib/systemd/system and /etc/systemd/system) 
    * ```$ sudo systemctl <command> <daemon>```
    * example ```$ sudo systemctl start ssh```
2. Second it checks System V "/etc/init.d" 
    * sysvinit (/etc/init.d) 
    * ```$ sudo service <daemon> <command>```
    * example $ sudo service ssh start
    * you may also see
    * ```$ /etc/init.d/<daemon> <command>```
    * (note - there can be more folders configured to be searched)
​
Both control daemons with commands = start, stop, reload, restart, status. 
Use ```$ sudo systemctl mask/unmask <daemon>  ``` to prevent a service from starting (either automatically or manually) or to allow a service to always start.
Example with the ssh daemon (sshd). service and systemctl give the same output

```console
$ service ssh status
● ssh.service - OpenBSD Secure Shell server
Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
Active: active (running) since Sun 2021-01
Docs: man:sshd(8)
man:sshd_config(5)
Process: 521 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
Main PID: 549 (sshd)
```

```console
$ systemctl status ssh
● ssh.service - OpenBSD Secure Shell server
Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
Active: active (running) since Sun 2021-01
Docs: man:sshd(8)
man:sshd_config(5)
Process: 521 ExecStartPre=/usr/sbin/sshd -t (code=exited, status=0/SUCCESS)
Main PID: 549 (sshd)
```

-----------------------------  
-----------------------------  