---
layout: page
permalink: /ref/linux/vnc-ssh/
title: vnc ssh 
description: Intro to vnc, ssh, remote connections
nav: false
toc: true
---
# VNC Viewers
This page is focused on connecting to a Linux PC remotely (not connecting to a Windows PC with RDP). Quick explanation on terms before starting.
* Host - Any computer/device that is accessible over the network.
* Server-host - The Linux PC you're wanting to connect to will need a vnc server installed and running
* Viewer-host - You use a device (Linux/Windows PC, tablets, phone) as the viewer
​
The server and the viewer will be separate programs. There are a lot of vnc server and viewer options and direct vs cloud connection is one difference to note. Below are setups I've used with some quick pros/cons. 

**RPi systems come with Realvnc already setup and just has to be enabled. These notes are primarily for cases where a vnc server wasn't setup**

----------------------

**VNC Viewers**
<div class="row">
    <div class="col-1 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/vnc1.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-1 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/vnc2.png" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-1 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/vnc3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

* RealVNC Viewer, Remmina, and Vinagre are popular viewers
* RealVNC .deb can be downloaded from RealVNC website for installation. Remmina and Vinagre are only viewers (do not create a server). They can be installed from Software Store or Synaptic (apt)
* Remmina - once connected I select the gear icon (Preferences) and choose "Best(slowest)" for better quality image. If you have issues with Remmina not showing the VNC plugin, open Synaptic and confirm remmina-plugin-vnc is installed, then close it and run ```$ sudo killall remmina ```. Now reopen and check VNC option.  

----------------------

# VNC Servers  
<div class="row">
    <div class="col-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/servers.jpg" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Direct connect options  
* vino and x11vnc for Ubuntu families (elementary OS, Zorin(gnome), etc)
    * Viewer and host should be on same network, do  not need internet.
    * No file transfer (I use overGrive and sftp for file transfer. rsync is another option)
    ​* Currently vino comes installed on Ubuntu and integrated with Gnome system settings.  

​Cloud connect options  
* RealVNC Server - Details below are for the free "Home" subscription
    * Viewer and host do not need to be on same network. Just need internet connection.
    * Up to 5 PCs on one email account.
    * No direct connect or file transfer
    * ​Use RealVNC Viewer and login with your RealVNC account.
​A benefit with RPi using Raspberry Pi OS is that it comes with a custom "Home" RealVNC server installed with service mode (includes direct connect and file transfer). It just has to be enabled in Raspi Config menu.  
​
My Setup Preference (influenced by the fact I do Raspberry Pi STEM projects)  

* Viewer - I use RealVNC (can do direct and cloud connections)
* Server - depends on the system
    * Raspberry Pi - Built-in RealVNC server and viewer. Just enable.
    * Ubuntu families with GNOME - I enable the default vino server. To allow RealVNC viewer to connect to it I disable the vino encryption (details below)
    * Non-GNOME DE - I install x11vnc server (details below)
I also install the RealVNC Home server (cloud) on top of the x11vnc/vino server. This gives me both cloud and direct connection. I have had the best luck installing the x11vnc/vino server first and then the RealVNC Home server.

> Since I often swap between which computer is the local vs remote I install both servers and viewers on all PCs.   

------------------

# VNC Server Setup
Typically vnc servers use port 5900 to LISTEN for connections (except the RealVNC "Home" subscription with cloud). So ensure you only have one server installed to avoid port conflicts.  

Can use netsat (net-tools) or lsof (List of Open File) to see if port 5900 is in LISTEN state.  
* ```$ sudo apt install net-tools```  
Then the two commands  
* ```$ sudo netstat -tulp```  
* ```$ sudo lsof -i -P -n | grep LISTEN```  
I prefer the lsof because it shows if running with root vs USER. This is important in setting up x11vnc later. With a fresh elementary OS install I show no vnc server on port 5900
```console
$ sudo lsof -i -P -n | grep LISTEN
systemd-r  IPv4 TCP 127.0.0.53:53 (LISTEN) 
cupsd root IPv6 TCP [::1]:631 (LISTEN)         
cupsd root IPv4 TCP 127.0.0.1:631 (LISTEN)
```

-------------


## Vino
Currently Ubuntu comes with vino server installed and will lauch at startup.
You enable it thru desktop system gui "Settings" and then "Sharing". Turn the sharing/remote on and other options, ie require a password for access.  
I was able to connect with Remmina and Vinagre viewers once "sharing" is enabled. But RealVNC viewer did not like the encryption vino is using and gave an error.  
To use RealVNC viewer with vino server I had to disable the encryption with  
```$ gsettings set org.gnome.Vino require-encryption false```  
Now I was able to connect with all VNC viewers.  
If you want vino encryption enabled you'll need to use Remmina or Vinagre viewer.  
​Now check port 5900 with lsof.  
```console
$ sudo lsof -i -P -n | grep LISTEN
systemd-r  IPv4 TCP 127.0.0.53:53 (LISTEN) 
cupsd root IPv6 TCP [::1]:631 (LISTEN)         
cupsd root IPv4 TCP 127.0.0.1:631 (LISTEN)
vino-serv USER IPv6 TCP *:5900 (LISTEN)         
vino-serv USER IPv4 TCP *:5900 (LISTEN)
```

-----------------

## x11vnc
If switching from vino to x11vnc uninstall vino first  
```$ sudo apt purge vino```  
Now install x11vnc server  
```$ sudo apt install x11vnc```  
Next you set the password. The default command stores the password file in $Home/.vnc/passwd . I ran into problems when trying to get x11vnc daemon to start automatically after boot. I would get "-auth guess: failed for display=:'0'" errors in the service log from $systemctl status x11vnc. However
I could get it to work if I used "startup application" to start the service. A better solution was moving the password to an /etc/ directory. Now I could start the x11vnc service with either systemd or startup application.

**Option 1**  
Set password file in default $HOME/.vnc/passwd  
```$ x11vnc -storepasswd```  
Then for start on boot put command below in "startup applications" from desktop settings  
```console
x11vnc -auth guess -forever -loop -noxdamage -repeat -rfbauth $HOME/.vnc/passwd -rfbport 5900 -shared
```
**Option 2**  
Set password file in /etc/ directory (used ```$ sudo mkdir /etc/mkdir/ ```for examples below)  
```$ sudo x11vnc -storepasswd your-password /etc/x11vnc/passwd```  
or  
$``` sudo x11vnc -storepasswd /etc/x11vnc/passwd```  
​and follow prompts  
Now set up x11vnc to launch during startup using systemd by creating a x11vnc.service.​​  
```$ sudo nano /lib/systemd/system/x11vnc.service​```  
```console
[Unit]​​
Description=Start x11vnc at startup.
After=multi-user.target

[Service] ​
Type=simple
ExecStart=/usr/bin/x11vnc -auth guess -forever -loop -noxdamage -repeat -rfbauth /etc/x11vnc/passwd -rfbport 5900 -shared
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
**(If copy/pasting from text box make sure there is a return after the Unit/Service/Install headers or you will get an error about Invalid section header. If necessary type it from scratch in a clean file.)**  
Then get it running with  
```$ sudo systemctl daemon-reload```  
```$ sudo systemctl start x11vnc.service```  
To have it start on boot use "enable"  
```$ sudo systemctl enable x11vnc.service```  
Check port 5900 with lsof  
```console
$ sudo lsof -i -P -n | grep LISTEN
systemd-r  IPv4 TCP 127.0.0.53:53 (LISTEN) 
cupsd root IPv6 TCP [::1]:631 (LISTEN)         
cupsd root IPv4 TCP 127.0.0.1:631 (LISTEN)
x11vnc root IPv6 TCP *:5900 (LISTEN)         
x11vnc root IPv4 TCP *:5900 (LISTEN)
```
There is a third option if you don't want it running as root.​
In the x11vnc.service file set your user/group and then update ownership of the password file to match.
```console
[Service]
User=Your-USER
Group=Your-GROUP
```
And chown on the password file
```$ sudo chown Your-USER:Your-GROUP /etc/x11vnc/passwd```
Now restart the x11vnc service and check port 5900. It should be running at user-level now.

-----------------------------

## RealVNC with Home Subscription
To setup RealVNC server with the free "Home" subscription  
* Go to RealVNC website
    * ​Download the Server .deb
    * Register with an email address/password
    * Activate the 'Home' subscription
* After connecting computers (up to 5) you can come back to the RealVNC website and rename/remove the computers on your account.

Now install Real VNC server on PC you want to connect to.  ​
* Install the downloaded RealVNC server .deb from above
* ```$ sudo gdebi VNC-Server..```
* After install run the license wizard and enter your RealVNC email/login info.
* $ sudo vnclicensewiz
* Now start the server
* ```$ sudo systemctl start vncserver-x11-serviced.service```
* ​To have it start on boot each time run "enable" command
* ```$ sudo systemctl enable vncserver-x11-serviced.service```

Installing VNC viewer on the PC you want to use to view the host PC
* Download RealVNC viewer .deb from website and install
* ```$ sudo gdebi VNC-Viewer..```
* Use 'login' option and login with same email/password combo as RealVNC account.
* The host computer will show up in your Team (Home) and you can now connect
    * Will use the password you entered specifically for VNC login to that PC created in step 6 above.​

For non-Raspberry Pi PC only running RealVNC Home subscription I do not get anything on port 5900.

​For Raspberry Pi I get.
```console
$ sudo lsof -i -P -n | grep LISTEN
systemd-r  IPv4 TCP 127.0.0.53:53 (LISTEN) 
cupsd root IPv6 TCP [::1]:631 (LISTEN)         
cupsd  root IPv4 TCP 127.0.0.1:631 (LISTEN)
vncserver root IPv6 TCP *:5900 (LISTEN)         
vncserver root IPv4 TCP *:5900 (LISTEN)
```

## VNC for OSMC on RPi
x11vnc did not work for me (OSMC on Pi does not use X11)
* Script by MarkusLange
* ```$ wget https://raw.githubusercontent.com/MarkusLange/VNC-Server-install-script-for-OSMC/master/osmc_vnc_install_cli.bash```
* Make it executable:
* ```$ chmod +x osmc_vnc_install_cli.bash```
* Start the GUI with:
* ```$ sudo ./osmc_vnc_install_cli.bash```
* Follow the options
* OSMC System-Update
* Install VNC Server and Service (port 5900)
* Activate VNC Service
* You should see port 5900 LISTENing with
* ```$ netstat -tulp```
* On PC (client) side, RealVNC viewer did not work. But Vinagre and Remmina viewers worked well.


**Notes on other vnc attempts with tightvnc and tigervnc**
In my case connecting with other VNC tools was not as easy and often ended up with black screen or slow/limited functionality.   
Other VNC connections I tried  
tightvnc  
ultraVNC  
tigervnc  
vnc4server  

Remmina does not create a server. Used for viewing and RDP (Remote Desktop Protocol for Windows connections)  

Useful commands when experimenting with tigerVNC server  
$ vncserver Creates a new connection  
$ sudo netstat -tulpn See connections  
As you create more vncserver connections it will go to 5901, 5902 etc  
$ vncserver -kill :1 Kill a connection listed in netstat command  


---------------------------

# ssh and sftp

ssh and sftp are options for logging into a terminal on another computer and running command lines. Although there is no windows environment, you can still execute commands like starting a virtual vnc session, or edit/view files, copy files back and forth, even do python debugging with pudb.  

ssh has two choices for security or authentication. The default method is a simple password. But it can be updated to use key-based authentication which uses a private-public key pair.  

**Similar to vnc you need a ssh server running to allow ssh into your system. The openssh-server should be installed with the Raspberry Pi OS. And RPi Imager now lets you enable ssh and create a password in the Imager settings before you download/install to the sd card. Assuming you enabled ssh in the Imager settings you'll be able to ssh into it after you boot up.**  

**ssh setup**  
If for some reason openssh was not installed you can set it up.  
Install the ssh server  
```$ sudo apt install openssh-server ```  
lsof should show port 22 LISTENing  
```console
$ sudo lsof -i -P -n | grep LISTEN
systemd-r  IPv4 TCP 127.0.0.53:53 (LISTEN) 
cupsd root IPv6 TCP [::1]:631 (LISTEN)         
cupsd root IPv4 TCP 127.0.0.1:631 (LISTEN)
vncserver root IPv6 TCP *:5900 (LISTEN)         
vncserver root IPv4 TCP *:5900 (LISTEN)
sshd root IPv6 TCP *:22 (LISTEN)         
sshd root IPv4 TCP *:22 (LISTEN)
```
Can check the status  
```​$ systemctl status sshd```   
Enable ssh to start on boot  
```$ sudo systemctl enable sshd```   
Can test some ssh commands.  
```$ ssh <user>@<IP address>```  
Login and check if it works  
```$ exit``` to leave  

You can also use an app like PuTTY​​  
Login using the IP address and port 22  


--------------------------

**sftp for copying files**
* Login to remote PC with sftp
* ```$ sftp user@<IP address>```
* Create a test file
* ```$ touch testfile ```
* Check if you can copy a file from host to client
* ```$ get testfile ```
* Check if you can copy a file from client to host
* ```$ put testfile ```
* Note - If getting access denied errors may have to update the ssh config file on the host. These updates have worked for me if using password authentication (and not keys)
* ```$ sudo nano /etc/ssh/sshd_config```
```console
PermitRootLogin yes              
PasswordAuthentication yes ​
```

For copying directories sftp with -r (recursive)
* ```$ sftp -r user@<IP address>```

Or use rsync (much faster for a lot of files)
* ```$ rsync -avz source destination```
* ```$ rsync -avz ./dir user@<IP address>:/home/user/dir```
```console
-v (verbose)
-a (archive, preserve sym links, file permissions, etc)
-z (compress)
```

-------------------------------

# ssh Key Authentication
ssh-keygen is a way of creating a secure method to log into remote hosts and allow transfer of code/files/data between your system/client and github or another PC/server. The general concept is two files with keys (a private and matching public key) are created and stored in ~/.ssh. The public key is copied to the remote host (ie github) or server (in the server authorized_keys file). When connecting from your PC/client to the remote host/server a check will be done to confirm the private and public keys match.  

Example files for RSA algo ssh  
* Private -> **id_rsa** (The private key acts as a password, remains on your PC, and should not be shared)  
* Public -> **id_rsa.pub** (The public key is copied to remote host (ie github or server). When you login, the remote is able to use the public key for verification and grant access. (in server scenario the sshd_config will also need to be updated to disable the default simple PasswordAuthentication)  

> Note I mainly use ssh key authentication for connecting my system to github account. So the notes here are focused on that scenario  

**Detailed Steps**
**Creating the Pair of Private/Public Keys on your PC/Client Side**  
* RSA type SSH keys are what worked best for me with Raspberry Pi  

* You can run ```$ ls -al ~/.ssh ``` to see if you already have an existing key.   
* If there is an **id_rsa.pub** and **id_rsa** then you already have keys. Assuming you haven't already used it, the **id_rsa.pub** can be copied to a remote host for ssh key authentication. However if you've already used that key for another account you'll need to follow the steps below to create a new pair and give them a different name.  

**Create a Key Pair**  
* If you need to create a key pair you use ssh-keygen (will create both the private and public key)  
* Github now has a note that rsa keys need increased security. To increase security you can specify the algo (-t) and bits (-b):  
```$ ssh-keygen -t rsa -b 4096 -C "email@gmail.com"```  
* It will ask you to save the file. If the first time you can save as default. If you already have key pairs for other accounts then change the name and use the new name for commands below.  

* Start the ssh-agent in the background and add the newly created SSH key(private) to the ssh-agent. (ssh-agent keeps track of keys and passphrases so you don't have to keep entering a password/passphrase).  
* ```$ eval "$(ssh-agent -s)"```  
* ```$ ssh-add ~/.ssh/id_rsa```​  


**Sharing/Copying the Public Key to the Remote Host/Server (ie github)**  

* ​Now you can copy the public key to a remote host like github so you can upload from your local PC to your remote github account. Don't forget to use the new id_rsa.pub name if you had to change it from the default.   
* A tool you can use to copy the key is  
* ```xclip -sel clip < ~/.ssh/id_rsa.pub```  
*win```clip < C:\Users\xxx\.ssh\id_rsa.pub```  
* Or you can manually open the rsa.pub file and copy the key. Once you have the key copied you go to the remote host and paste the key in your account security settings.
* Example for github.  
* Copy the key inside ~/.ssh/id_rsa.pub  
* Go to github, then profile, settings, SSH key, new key and paste the key here.  
* Now github can authenticate your local PC.  

**Enable ssh Key Authentication for a Server/PC**  
If enabling keys for ssh to a server or another PC you paste the key into the server's authorized_keys file. The best way to do this is using the ssh-copy-id tool. Example below.  
* Login to the server
* ```$ ssh-copy-id remote-user-name@ipaddress```  
* It will reply with the RSA fingerprint and confirm you want to add it the list of known hosts.  

Or you can manually copy/paste the RSA key and paste it in the authorized_key file.  
* ```$ sudo nano ~/.ssh/authorized_keys```   
* Paste the public key into the file and make sure it is on a single line.  
```console
ssh-rsa AAB34NJD34987...   
```

By default ssh only uses password authentication. For the remote PC/server to require keys instead of just a password you need to update the sshd_config file  
```$ sudo nano /etc/ssh/sshd_config```  
Uncomment the PasswordAuthentication line and set it to Yes  

If you get a authentication error when logging in with ssh you can do a couple checks to confirm the system you're logging into.  
* Go to or remote desktop to the server  
* The server ssh key fingerprints are under /etc/ssh/ so run command below (assuming using SHA256)  
* ```ssh-keygen -lf /etc/ssh/ssh_host_ecdsa_key.pub```  
* You can compare the returned fingerprint to what the ssh authentication error showed  

To fix the mismatch on your remote machine's run the command below to remove the server from the remote known hosts file  
* ```ssh-keygen -f "$HOME/.ssh/known_hosts" -R "IP-address-of-server"```  
* Now login again and it will update the known hosts file with the server info  

---------------------------------

**Useful Network Commands for IP address and  hostname (nmap)**
When connecting to a RPi with ssh/VNC or if accessing a server port (node-red, http, python docs, etc) you'll need the IP address or hostname. Some useful commands to find IP address and host name information.  
> What is a host? Any computer or device that is accessible over the network    
The current computer can be accessed with 127.0.0.1 or localhost. (you can verify in the host table with $ cat /etc/hosts and see 127.0.0.1 linked to localhost)   

To get the IP address or hostname for access from a remote PC you can use commands below. Each gives more information.     
```$ hostname -I | awk '{print $1}' ```  
```$ hostname -I```  
```$ ifconfig ```  
For the hostname of your PC  
```$ hostnamectl``` (or simply hostname)  

Now that you have the IP address and/or hostname you can use either for accessing a remote computer/device. When using the hostname just add .local to the end. Some examples  
SSH  
```$ ssh username@10.0.0.110```  
or  
```$ ssh username@hostname.local```  

Node-red Server via a web browser (uses port 1880)  
http://10.0.0.115:1880  
or  
http://hostname.local:1880  

To access from another computer you can use the IP address hostname found above.You can also update the lookup table with the hostname used for accessing from another computer.  
```$ sudo nano /etc/hostname```  
```$ sudo nano /etc/hosts ```(here you will see the 127.0.0.1 linked to localhost. Leave this line alone. Further down will be 127.0.1.1. Update the name next to this address.  
To update the hostname of your RPi just go to $ sudo raspi-config and change the hostname in system options. ​
Now that you know the hostname you can   
Commands below are helpful for finding the IP address of a device on your network. RPi-hunter(below) is the best tool for specifically finding Raspberry Pi's on your network.  

```$ arp -a ``` (quick , but no description)  
Nmap can scan the subnet. May have some description.  
```$ sudo nmap -sP 10.0.0.0/24  ```  
Replace 10.0.0.0 with your IP address.  
(/24 means subnet of 255.255.255.X or only scan the last field, X. This is the part that will change across different devices on your network)  

Gui based IP scanner  
I have tried Angry IP scanner but it did not give me anymore information than nmap  
​nessus free version IP scanner is useful for getting security risks. But a large install (> 2 GB)  

--------------------------------

# rpi-hunter
A useful RPi tool for updating multiple RPis at once is rpi-hunter.  You can send payload commands to multiple machines.  
RPi-Hunter Installation  
* ```$ pip3 install -U argparse termcolor```  
* ```$ sudo apt -y install arp-scan tshark sshpass```  
* ```$ git clone https://github.com/BusesCanFly/rpi-hunter.git```  
* if getting no termcolor error then →  
* ```$ sudo apt-get install python-termcolor```  

Using RPi-Hunter  
* ```$ cd rpi-hunter```  
* ```$ chmod +x rpi-hunter.py```  (make it executable)  
* ```$ sudo python rpi-hunter.py -h``` (help command)  
* ```$ ./rpi-hunter.py```  
* -r (IP range)  
* Can take a while to scan all IP's (including non Pi's)  
* Better to create files with IP lists of known Pi groups  
* example below is using ./scan/rpis file  
* -f (list of Rpi Ips) default is in dir /scan/rpi_list ** using rpis  
* ```$ ./rpi-hunter.py -c ‘password’ -f ./scan/rpis --payload 'apt update'```   
* ```$ ./rpi-hunter.py -c ‘password’ -f ./scan/rpis --payload 'apt full-upgrade'```   
* ```$ ./rpi-hunter.py -c ‘password’ -f ./scan/rpis --payload 'hostnamectl'```  
* ```$ ./rpi-hunter.py -c ‘password’ -f ./scan/rpis --payload 'sudo shutdown -h now'```   



----------------------------------

**ufw Firewall**
ufw is a simple firewall. I typically have it disabled for my home STEM projects because if I enabled it I would still open ports for ssh/vnc, making the ufw even less valuable.

If you do use ufw here are some commands and common ports.  
```console
$ sudo apt install ufw
$ sudo ufw enable
$ sudo ufw disable
$ sudo ufw status verbose

Install gui for ufw
$ sudo apt install gufw

$ sudo ufw allow 5900/tcp (vnc)
​$ sudo ufw allow 20/21 (ftp)
$ sudo ufw allow 22/tcp
$ sudo ufw allow 23 (telnet)
$ sudo ufw allow 8086/tcp
$ sudo ufw allow 8088 (influx)
$ sudo ufw allow 1880 (node red)
$ sudo ufw allow 1883 (mqtt)
$ sudo ufw allow 3000 (grafana)

To close a port
$ sudo ufw deny <port>

To see description of all ports
$ nano /etc/services
```

----------------------------------

# Useful Terms

* Display - can use echo $DISPLAY to see monitor connected (will be :0 for primary display and blank if no monitor)
* Host - Any computer/device that is accessible over the network.
* VNC - A graphical system that gives you the ability to remotely control a host device, your PC or RPi, from another host/device.
* ssh - Terminal or command line system that give you the ability to remotely login into your PC/RPi and run commands from another device (ie, edit files using terminal text editors like nano).
* sftp - Secure method to remotely transfer files between hosts (requires the ssh server running). Similar to Putty. 
* Telnet - For reference, an older type of remote login. But not as secure as ssh.

* VNC server in service mode - Replicates what you would see if sitting at the server-host computer. Provides cloud connection (no direction connection with free 'Home' subscription)
* VNC server in virtual mode - Created in memory and only the VNC user can see it. I use this on Raspberry Pi when booting to command line.

Some terms you'll see from output of $ ifconfig
* eth0 or eno1:  - Ethernet adapter where you physically plug a cable (aka ethernet, network, lan cable)
* lo: - Loop back device with address 127.0.0.1 A virtual network interface used to connect to local servers running on your machine.
* wlan0 or wlpxs0: - wireless card - most likely where you'll look for IP info
* 127.0.0.1 - Loopback address that points back to the current computer. Your computer's TCP/IP will know you're connecting to a server on your own computer and not the internet.
* IPv4 and IPv6 - Both are IP address types. IPv6 is newer and made it possible to have more addresses to handle growing number of machines.
* TX = Transmit
* RX = Receive

* localhost - Address for current computer. The 'name' for 127.0.0.1
* $ hostnamectl set-hostname <new-name> Will update /etc/hostname
* $ sudo nano /etc/hosts Update also
* First line 127.0.0.1 is the loopback (lo) address
* 127.0.0.1 localhost                       
* 127.0.1.1 <custom host name> 
* change 127.0.1.1 if using static IP
* $ hostnamectl  

* LAN - Local Area Network. Group of computers connected to each other on a local network.

Public and Private IP Addresses
* IP address (Private)- The local address/number assigned to your computer (examples are 10.x.x.x or 192.168.x.x with x ranging from 0-255) within your local network. Similar to the address of your house. The address your PC is given will be unique to the network you're on at the time (A network can not have duplicate IP addresses). But if your network uses DHCP (Dynamic Host Configuration Protocol) or if you go to another network (ie coffee shop) your IP address will change. That is why it is useful to know the $ ifconfig or $ hostname -I (capital i, not L) commands. You can always get the IP address of machines you're working with on a project.
* Static IP address - It is possible to configure the IP address of a RPi or other system to be static (not change). But one risk is that when your RPi is turned off (your network can't see it) the network may assign that IP address to another machine. Then when you turn the RPi on it will not be able to connect because of an IP address conflic (duplicates).

* Public IP Address - Unique, public, address assigned to web sites for connecting to external locations thru the internet

* IP (Internet Protocol) - Connectionless protocol for delivering information or data packets between computers (a source and a host). Does not send an acknowledgement.
* TCP (Transmission Control Protocol) - Connection protocol that complements the IP. Provides error-checking and acknowledge of transmissions between systems to ensure data is reliable.
* TCP/IP - TCP and IP work together to connect computers and send packets of data between them (request, acknowledge, transfer)

* Port - A virtual communication endpoint that is software based (it is not physical hardware). Require both client and server to create a connection or socket. Identifies application or service running. Port 22=ssh, 5900=vnc, 8088=influxdb, 1880=node-red, 1883=mqtt, 3000=grafana)
* Socket - A channel or combination of the remote/client ports/addresses, and protocol.
* TCP/IP = protocol
* Client IP (network layer) + Client port (transport layer)
* Remote IP + Remote port

**Network Terms**  

Network Terms  
Netplan - network configuration utility  
*   renderer networkd vs NetworkManager  
*   /etc/netplan/*yaml  
RPi-Ubuntu 20.1 setup (/etc/netplan/01-network-manager-all.yaml)  
On my Ubuntu/Rpi renderer = NetworkManager  
NetworkManager.service = active  
* network-manager.service not present  

systemd-networkd.service  
System daemon that manages network configurations. Detects and configures network devices as they appear.  
Status on my devices    
* rpi4 inactive  
* rpiOS-mate = inactive  
* ubuntu = inactive  

Network Manger = high-level interface for the configuration of the network interfaces  
network-manager.service = NetworkManager.service  
Status on my devices  
* RPiOS with Mate = active  
* dhclient  
* If using Network Manager then disable dhcpcd  

dhcpcd  
server program that operates as a daemon on a server to provide Dynamic Host Configuration Protocol service to a network  
Status on my devices  
* RPi4 = active  
* wpa_supplicant  

-----------------------------  
-----------------------------  