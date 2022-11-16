---
layout: page
permalink: /ref/hardware/monitor-headless/
title: monitor or headless
description: Intro to monitor or headless setup
nav: false
toc: true
---
# RPi Monitor or Headless
Note - This section will focus on Raspberry Pi since it has RealVNC installed and just needs to be enabled. For other Linux distros I have more info at VNC-ssh-sftp

With the RPi Imager it is easiest to setup vnc/ssh in the settings when writing the OS to the card.

RPi running Raspberry Pi OS
Two options for monitor setup - with or without (headless)
With monitor. Use a HDMI cord to connect Pi to the monitor. Depending on the Pi model you may need an adapter.
* Pi0 need mini HDMI
* Pi3(A/B) use standard HDMI
* Pi4 use micro HDMI (dual ports)
* Argon One has a Pi4-case with fan and converts to standard HDMI

> No monitor or headless requires using another PC (or RPi) to act as the viewer. 
Raspberry Pi O/S has RealVNC option built-in and includes both the viewer and server. Includes cloud, direct connection, and file transfer. Enable 'SSH' and 'VNC' in the RPi configuration/interfaces menu.

Good instructions are also on [Raspberry Pi](https://www.raspberrypi.com/documentation/computers/configuration.html) website.

# RPi Monitor BKMs
Starting point settings if you have problems with monitor setup​
To edit HDMI settings ([Raspberry Pi](https://www.raspberrypi.com/documentation/computers/configuration.html) video settings)
``` $ sudo nano /boot/config.txt ```  
Then edit  
```console
framebuffer_width=1360  
framebuffer_height=768  
hdmi_group=2  
hdmi_mode=39  
hdmi_ignore_edid=0xa5000080
```
(on cheap monitors may have to disable EDID)
Exit/Save and then Reboot
May need to go through multiple iterations to find the hdmi_group/mode that works best for your setup.

> I typically setup with a monitor and then move to headless. For name brand monitors I  don't have to change the settings. However, when using lower cost monitors I have to play with the /boot/config.txt settings on the RPi to get the resolution readable. And it is not a one size fit all. For the Raspberry Pi 4 (has dual monitor output) I had to tweak the settings differently. Some starting point settings are included here. 

# Starting Headless
The RPi imager now allows you to enter wi-fi information so it can automatically connect during install.  

To update manually 
```sudo nano /etc/wpa_supplicant/wpa_supplicant.conf ```  
Then edit  
```console
​network={
                ssid="wifi-name"
                psk="password"
                key_mgmt=WPA-PSK
}
```

# Hostname Setup
Again can be set in RPi imager settings now.

Otherwise, for any remote/headless connection you'll need to know the name of your Pi. The most reliable method is knowing the IP address, although the hostname usually works too.
​
​Hostname setup
To vnc to a pi using the hostname you can use default **pi@raspberrypi.local**. If you have more than one Pi on the network you'll want to give each Pi a unique hostname.

If you are in the GUI you can set the hostname in the RPi configuration System menu.
If you want to set it up offline just edit the /etc/hosts and /etc/hostname file and enter a name for the Pi.
```sudo nano /etc/hosts``` (leave the first line, localhost, untouched)  
Then edit hosts
```console
​127.0.1.0     localhost       
127.0.1.1     unique-name
```
For hostname ```sudo nano /etc/hostname```  
Then edit hostname
```console
unique-name
```
​Another method is using hostnamectl
```sudo hostnamectl set-hostname <unique-name>```  

Check the **/etc/hosts** and **/etc/hostname** and confirm/update.
Now restart the Pi
Access thru ssh using **pi@unique-hostname.local** or vnc to **unique-hostname.local​** (on windows you do not need the local)
Default host name for a Raspberry Pi is **raspberrypi**

# Finding IP Address
IP ADDRESS - a couple options to find the IP address
If you are on the Pi you can use any of these options
```
$ ifconfig 
$ hostname -I 
$ hostname -I | awk '{print $1}' 
```
Or you can also hover the mouse over wi-fi panel to see IP address.

If you are on another system you can use network commands
```
$ arp -a 
$ sudo nmap -sP 10.0.0.0/24
```
(use the route -n command to get gateway, ie 10.0.0.0)

A useful tool that will isolate RPi's on your network and also allow you to send some commands is RPi-hunter. It requires extra setup but may be worth it if you will have a lot of Pi's on your network. More details in Network-RPi-Hunter page  

​​RealVNC Cloud Address book - This option does not need an IP address. You will be able to access devices from your Team address book in the RealVNC Viewer.  

# ssh/sftp (cli)
(No GUI, only command lines)
* VScode has an extension 'remote - SSH'. This allows you to connect to a RPi3-4 in the VScode editor and program from your local machine. (Currently does not support Pi0. See issues for update)
​For standard ssh/sftp connection
* Need a terminal or ssh-client app like Putty to remote login to the RPi.
* Can have the RPi boot up 'To Desktop' or 'To CLI'
* Very useful for Raspberry Pi that have less RAM (RPi0/3A+).
* (SSH has to be enabled in raspi-config)
* From another PC login using
```$ ssh pi@IP-ADDRESS``` or ```$ ssh pi@hotname.local```  
* If the IP address changes (DHCP) on your pi you will need to run the following (replace 'user' with your user name and newIPaddress with IP address of device)
```$ ssh-keygen -f "/home/user/.ssh/known_hosts" -R "newIPaddress"```  
* To copy files  
```$ sftp pi@IP-ADDRESS``` see this linuxize article or sftp  
​
# RealVNC (full GUI)​
​Install RealVNC (or other) viewer on the system you'll be using as a viewer.
(Ensure VNC is enabled on the RPI)
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/hardware/host-server.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/hardware/realvnc-cloud.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Direct Connect
By typing the **IP address**or **unique-hostname.local** in a vnc viewer. Viewer and Server should be on the same network (you do not need internet).

## RealVNC Cloud
Cloud is useful since you do not use the IP address. You have an address book and up to 5 PCs for that email account. You do not need to be on the same network, you can connect from anywhere. (requires internet)
* Create an account at RealVNC 
* You can connect up to 5 PCs
* Make sure RealVNC is enabled under "Raspi-config". Rick click on the VNC symbol in task bar, go to licensing, and enter you RealVNC email/pswd info.

## ​​Virtual RealVNC
Enable RealVNC as previous instructions
* Will create a virtual desktop in memory, so RPi can boot up 'To CLI'
* Then remote terminal (ssh) to the RPi and type in command with resolution
```$ vncserver -geometry 1360x768```  
* It will create a virtual desktop and give you a new address to access it in RealVNC (ie IPADDRESS:1)
* Go to RealVNC and type in the new IPADDRESS:1
* To kill the virtual desktop, ssh back to the Pi and type
```$ vncserver -kill :<display-number>```  
Other resolutions for virtual- omit the (16:9) 
```console
vncserver -geometry 1280x768 (15:9)
vncserver -geometry 1280x800 (16:10)
vncserver -geometry 1440x900 (16:10)
vncserver -geometry 1600x900 (16:9)
vncserver -geometry 1680x1050 (16:10)
vncserver -geometry 1920x1080 (16:9)
```

# Coding Options
For coding I pick the option that best fits what the device is capable of.
* esp32 - I connect the esp32 to the USB port on a RPi3/4 and use Thonny. Later the code can be uploaded to github using command lines or VScode.
* RPi0 - I boot to cli and setup a samba share on the Pi0 where I store the python scripts/projects. I can then open the share folder/python scripts from a remote PC (or Pi4) and edit them using gedit or other gui text editor. I use the git command lines to keep it sync'd with github.
* RPi3A+,3,4 - I connect to the Pi from a remote PC (or Pi4) using the VScode remote extension. Can also use the git commands in VScode to keep it sync'd.

# Touchscreen Setup/Calibration
If you have problems with a touch screen having X, Y moving in opposite directions when trying to rotate/flip the screen .. you can try the following.

For rotation
```sudo nano /boot/config.txt```  
```console
display_hdmi_rotate=2  (or display_lcd_rotate=2)
0=0°, 1=90°, 2=180°, 3=270°
```

If X,Y move opposite direction of the touch screen try installing xserver-xorg-input-evdev  
```sudo apt install xserver-xorg-input-evdev```  
Then edit 10-evdev.conf  
```sudo nano /usr/share/X11/xorg.conf.d/10-evdev.conf```  
add lines below to "evdev tablet catchall" or "evdev touchscreen catchall" section or both  
```console
Option "InvertY" "true" 
Option "InvertX" "true" 
```
(make sure to add before the "EndSection")  
Reboot  

If still having problems try the following  
```sudo touch /etc/X11/xorg.conf.d/99-calibration.conf```  
```sudo cp -rf /usr/share/X11/xorg.conf.d/10-evdev.conf /usr/share/X11/xorg.conf.d/45-evdev.conf```  
```sudo apt install xinput-calibrator```  
Reboot  
```DISPLAY=:0.0 xinput_calibrator```  
Perform the calibration  
It will output calibration results. Copy the results and paste in 99-calibration.conf below  
```sudo nano /etc/X11/xorg.conf.d/99-calibration.conf```  
Reboot  

If still having problems try putting the Option InvertXY true flag in the 45-evdev.conf  
I did trial and error and found the /usr/share/X11/xorg.conf.d/10-evdev.conf is where I needed the invert flag.  

# Useful Terms and Tricks  
Have had issues with vnc when no monitor connected (RealVNC says can not display desktop). In /boot/config.txt I set
```console
framebuffer_width=1920
framebuffer_height=1080
hdmi_force_hotplug=1
hdmi_group=2
hdmi_mode=82
```
Have had issues with resolution not changing no matter what settings I put in config.txt. I boot to terminal (raspi-config setting) and do virtual vnc ```$ vncserver -geometry 1920x1080```

Terms  
* host - Any computer/device that is accessible over the network.
* VNC - A graphical system that gives you the ability to remotely control a host device, your PC or RPi, from another host/device.
* ssh - Terminal or command line system that give you the ability to remotely login into your PC/RPi and run commands from another device (ie, edit files using terminal text editors like nano).
* sftp - Secure method to remotely transfer files between hosts (requires the ssh server running). Similar to Putty. 
* Telnet - For reference, an older type of remote login. But not as secure as ssh.

I have found the RealVNC documentation useful
* Viewer-host - The PC you're using as the viewer
* VNC viewer - application you use to see the desktop of the server-host PC
* Server-host - The PC you're wanting to view
* VNC server - a vncserver must be running on the server-host PC​​​
​
In the scenario of headless Raspberry Pi
* VNC/ssh server will be running on the RPi, host B
* Use VNC viewer (or ssh/sftp command) on another device, host A, to access it.
* (the viewing device, host A, may sometimes be referred to as a client, too)

More VNC Terms
* ​VNC server in service mode - Replicates what you would see if sitting at the server-host computer. Provides cloud connection (no direction connection with free 'Home' subscription)
* VNC server in virtual mode - Created in memory and only the VNC user can see it. (I did not pursue getting this to work for personal use)

Other terms
* cli - command line interface
* Default login is **pi** and pswd **raspberry** (change the password)
* alt-space - While working on display setup, if a program opens up with menu outside the monitor window use “alt-space” to get a pop up menu allowing you to move it.

Some terms you'll see from output of 
```$ ifconfig```  
* eth0 or eno1:  - Ethernet adapter where you physically plug a cable (aka ethernet, network, lan cable)
* lo: - Loop back device with address 127.0.0.1 A virtual network interface used to connect to local servers running on your machine.
* wlan0 or wlpxs0: - wireless card - most likely where you'll look for IP info
* 127.0.0.1 - Loopback address that points back to the current computer. Your computer's TCP/IP will know you're connecting to a server on your own computer and not the internet.
* inet (IPv4) and inet6(IPv6) - Both are IP address types. IPv6 is newer and made it possible to have more addresses to handle growing number of devices.
* TX = Transmit
* RX = Receive

localhost - Address for current computer. The 'name' for 127.0.0.1  
```$ hostnamectl set-hostname <new-name>``` Will update /etc/hostname  
```$ sudo nano /etc/hosts```   Update also  
First line 127.0.0.1 is the loopback (lo) address  
```console
127.0.0.1 localhost                       
127.0.1.1 **custom host name**  
```
change 127.0.1.1 if using static IP    
```$ hostnamectl ```   

LAN - Local Area Network. Group of computers connected to each other on a local network.  

Public and Private IP Addresses
* IP address (Private)- The local address/number assigned to your computer (examples are 10.x.x.x or 192.168.x.x with x ranging from 0-255) within your local network. Similar to the address of your house. The address your PC is given will be unique to the network you're on at the time (A network can not have duplicate IP addresses). But if your network uses DHCP (Dynamic Host Configuration Protocol) or if you go to another network (ie coffee shop) your IP address will change. 

* Static IP address - It is possible to configure the IP address of a RPi or other system to be static (not change). But one risk is that when your RPi is turned off (your network can't see it) the network may assign that IP address to another machine. Then when you turn the RPi on it will not be able to connect because of an IP address conflict (duplicates).

* Public IP Address - Unique, public, address assigned to web sites for connecting to external locations thru the internet

* IP (Internet Protocol) - Connection-less protocol for delivering information or data packets between computers (a source and a host). Does not send an acknowledgement.
* TCP (Transmission Control Protocol) - Connection protocol that complements the IP. Provides error-checking and acknowledge of transmissions between systems to ensure data is reliable.
* TCP/IP - TCP and IP work together to connect computers and send packets of data between them (request, acknowledge, transfer)

-----------------------------  
-----------------------------  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/hardware/flappybird.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row justify-content-center float-right">
    <div class="col-4-auto mt-3 mt-md-0">
        {% include figure.html path="assets/img/hardware/flappybird.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

----------------------------
Images
can you col-#  col-sm-#   col-md-#   col-lg-#
Use auto to auto size around image
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).