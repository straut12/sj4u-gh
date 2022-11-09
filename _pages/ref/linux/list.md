---
layout: page
permalink: /ref/linux/list/
title: list
description: List of programs I often install
nav: false
toc: true
---
This is a running list of packages I find myself installing on my Debian/Ubuntu based systems. In most cases I only include the app name and indicate if it is a apt install or download. And some distros have a custom version.   

Commands ran with $ sudo apt install **package**  
(ppa: = Repository added with $ sudo apt-add-repository **PPA:info**)  
⇩= download package  

Create USB boot disk  
⇩balena etcher (AppImage)  

# Initial
Initial items after installing OS  
Package manager GUI  
* sudo apt install synaptic
* sudo apt install gdebi
* sudo apt install mlocate
* sudo updatedb  
* with regex  
```locate -r "^/home/USER/.local/.*png.*$"```  
* ​sudo apt install curl

* $ sudo su
* ```# apt install flatpak```
* Add flatpak repository to get apps
* ```# flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo```  

restart  

Ubuntu add PPAs  
* sudo apt install apt-transport-https
* sudo apt install software-properties-common
* sudo apt install sudo add-apt-repository universe

# Appearance and windows  
* sudo apt install grub-customizer (ppa:danielrichter2007/grub-customizer)
* sudo apt install gnome-tweaks
* sudo apt install mate-tweak
* sudo apt install elementary-tweaks (ppa:philip.scott/elementary-tweaks)
* (change to windows for minimize button)​  
* sudo apt install neofetch
* sudo apt install plank (ppa:ppa:ricotz/docky) optional
* Remote Connect (vnc, ssh, network)  

# Tools for finding IP address, vnc setup, ssh, file transfer
* sudo apt install openssh-server 
* ​sudo apt install  rsync 
* sudo apt install  net-tools 
* sudo apt install  x11vnc 
* sudo apt install  vinagre 
* sudo apt install  nmap​ 
* ⇩RealVNC viewer and server  

# File Sharing and Backups (overGrive, Aptik)  
* sudo add-apt-repository universe
* Google Drive desktop app
* ⇩overGrive
* Backup installed programs, configurations, home, etc
* ⇩Aptik
* Run scheduled backup of folders you specify (I backup my google drive folder)
* sudo apt install deja-dup  

# Utilities  
Disk usage, system monitor conky and sensor, disk utility
* sudo apt install baobab 
* sudo apt install gnome-disk-utility 
* sudo apt install lm-sensors 
* sudo apt install conky conky-all ​

# STEM (MQTT, INFLUXDB, NODE-RED)
* sudo apt install -y mosquitto mosquitto-clients
* sudo systemctl enable mosquitto
* mosquitto -d
* echo "username:password" > pwfile
* mosquitto_passwd -U pwfile
* sudo mv pwfile /etc/mosquitto/
* sudo nano /etc/mosquitto/mosquitto.conf
```console
allow_anonymous false
password_file /etc/mosquitto/pwfile
listener 1883
```
​* sudo reboot


* ```wget -qO- https://repos.influxdata.com/influxdb.key | sudo apt-key add -```
* source /etc/os-release
* ```echo "deb https://repos.influxdata.com/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/influxdb.list```
* sudo apt install influxdb
* sudo systemctl unmask influxdb
* sudo systemctl enable influxdb
* sudo systemctl start influxdb
* influx
* ​When you enable HTTP authentication, InfluxDB requires you to create at least one admin user before you can interact with the system.
* CREATE USER admin WITH PASSWORD '<passwd>' WITH ALL PRIVILEGES
* show users
* exit
* sudo nano /etc/influxdb/influxdb.conf
```console
1st item – Determines whether HTTP endpoint is enabled
enabled = True
4th item – The bind address used by the HTTP service
bind-address = “:8086”
auth-enabled = true
pprof-enabled = true
pprof-auth-enabled = true
ping-auth-enabled = true
```

* sudo systemctl restart influxdb
* influx -username admin -password <passwd>
* CREATE DATABASE xxxxx
* use xxxxx

* sudo apt install build-essential git
* ```bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)```
* node-red-pi --max-old-space-size=256
* sudo systemctl enable nodered.service
* node-red-log
* sudo reboot
* npm install node-red-contrib-influxdb  


# MQTT tools  
* ​⇩ mqtt-explorer (AppImage)

# Programming IDEs
* mu-editor
* sudo apt install pip3 
* sudo pip3 install --upgrade pip 
* sudo pip3 install mu-editor 
* To create a shortcut
* sudo pip3 install shortcut 
* shortcut mu-editor 
* sudo apt install mu-editor ​
* (download mu icon and place in $HOME/.local/share/icons/hicolor/48x48

# VScode
* ​⇩VScode
* VS code extensions
* Python
* Github pull requests
* Jupyter
* Pygame

# venv
For virtual environment and git integration to VScode  
* sudo apt install python3
* sudo apt install python3-pip
* sudo apt update
* sudo apt upgrade
* sudo apt install python3-venv
* sudo apt install build-essential git 

# GPIO tools  
For GPIO tools on non Raspberry Pi OS install  
* sudo pip3 install gpiozero 
* sudo apt install python3-rpi.gpio 

# Thonny
* RPi OS (32bit) is pre-installed
* Ubuntu (64bit) and RPi OS with MATE
* ```sudo pip3 install thonny ```
* ```sudo apt install thonny ```
* ```which -a thonny  ```​
* (if you have multiple versions can remove /usr/bin/thonny. I use the /usr/local/bin/thonny)
* ```sudo apt remove thonny```

(to connect to esp32)  
* ```sudo usermod -a -G dialout USER ```

# Browsers, Office, Photo, etc..
​​Browsers  
* ⇩google chrome .deb
* sudo apt install firefox 

LibreOffice  
* ```sudo apt install libreoffice libreoffice-gtk3 ```
* For elementary OS  
* ```sudo apt install libreoffice-style-elementary ```
* GIMP image editor  
* ```sudo flatpak install flathub org.gimp.GIMP ```​

# Free Up Disk Space  
* Clean out the apt cache  
* ```sudo apt clean ```
* Remove older kernels  
* ```sudo apt autoremove --purge ```
* Clean out thumbnail cache
* ```rm -rf ~/.cache/thumbnails/* ```
* Delete older logs
* ```sudo journalctl --vacuum-time=1d ```
* Clean up snapd
* ```run snapd ```script to clean up older versions
* Bleachbit
* ⇩bleachbit .deb​
* Stacer for focal+ (my preference)
* sudo apt install stacer

# RPi Deleted Unused Apps
Raspberry Pi's - delete unused programs  
* sudo apt purge wolfram-engine 
* sudo apt purge scratch 
* sudo apt purge minecraft-pi 
* sudo apt purge sonic-pi 
* sudo apt purge libreoffice*  


-----------------------------  
-----------------------------  