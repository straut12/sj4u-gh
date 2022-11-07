---
layout: page
permalink: /ref/linux/software/
title: software
description: Intro to installing software or packages
nav: false
toc: true
---
# Repo/Package Background
What are Repositories?  
Repositories are directories, typically remote, containing deb packages/files used to install a software application. For example the Raspberry Pi APT repositories are at http://raspbian.raspberrypi.org/raspbian/. This is where your PC will get the necessary information for installing packages when you type $ sudo apt install or use Synaptic. Your system searches the APT repository for the package (needs to find one that also matches your system architecture) and if found, will install it.   

How does my system know where these repositories are?  
Each system has a configuration file that list the external repository locations. The repository locations will vary by your Linux distro and release. For RPi/Ubuntu you will have the official raspberrypi.org site and ubuntu.com set from the beginning. But you can add more repository locations to broaden the packages and revisions available to you.  

What are packages?  
Packages are how you install software/applications. This is an area that differs from installing Software in Windows and can take a little time to get used to. A package is a group of files which contain executables, configuration, and other files needed to install/run that program. A common install method is using the Software Store GUI or command line to install a package. At that point your system searches the remote repositories, and if a match is found for your architecture, will transfer files and install it. More details on where these remote locations exist, which repository locations are enabled on your system, and how to add more locations is at source/repository.  

> Note on dependencies - Since programs often use some of the same files these 'common' files are usually put into a separate package to reduce duplicates. So for your program to work it will need these packages installed too (a dependency). The GUI's mentioned below, Synaptic package manager(apt) and gdebi​ will take care of the dependencies.

There are two types of packages
1. Binary files are for a specific architecture (Intel/AMD, ARM) and not human readable. Most common to install. 
2. Source packages include the source code so they can be used on any system. However you'll have to take extra steps to compile and install the package. (./configure && make && make install​)

>  Good instructions for configure/make to install source code are at​ [itsfoss](https://itsfoss.com/install-software-from-source-code/). And more detailed explanations on installing packages are at [Debian docs](https://www.debian.org/doc/manuals/debian-faq/pkg-basics.en.html#package) and [Ubuntu Community](https://help.ubuntu.com/community/InstallingSoftware)

# Repo, sources.list, adding PPA

Helpful links  
[Ubuntu Community](https://help.ubuntu.com/community/Repositories)  
[Debian wiki](https://wiki.debian.org/DebianRepository)  

**Location and Priority of Repositories (sources.list)**
Where is this list of allowed repositories kept on my system? How do I modify it?  For Raspberry Pi/Ubuntu there is a file /etc/apt/sources.list and a directory /etc/apt/sources.list.d/ which specify package locations for APT to search. You can view the sources.list thru the command line or a GUI.  
Ubuntu Software & Updates  
<div class="row">
    <div class="col-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/repo1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
**/etc/apt/sources.list**  
```console
type	repository URL	distribution	component/categories
Ubuntu Example	
deb	http://security.ubuntu.com/ubuntu/	focal-security	main restricted
deb	http://security.ubuntu.com/ubuntu/	focal-security	universe
deb	http://security.ubuntu.com/ubuntu/	focal-security	multiverse
deb	http://us.archive.ubuntu.com/ubuntu/	focal-backports	main restricted universe multiverse
deb	https://cloud.r-project.org/in/linux/ubuntu	focal-cran40/	
```
**Raspberry Pi Synaptic Example**  
<div class="row">
    <div class="col-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/rpi.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
For GUI version  
Raspberry Pi specific  
* Open Synaptic package manager
* View/update sources under 'Settings/Repositories'
Non-Raspberry Pi
* View/update repositories with "Software & Updates" app. In the first "Software" tab will be main, universe, restricted, multiverse.  

**Next tab (seen below), has "Other Software" option to add more repositories.**  

* Note - if you go to "Settings/Repositories" in Synaptic it will open the "Software & Updates" tabs.
* More details for adding a repository in the GUI from [ubuntu](https://help.ubuntu.com/community/Repositories/Ubuntu)  
<div class="row">
    <div class="col-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/ubuntu2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

For command line option  
```$ sudo nano /etc/apt/sources.list```  
* or in dir --> /etc/apt/sources.list.d/ (repository files in here must end with .list)  
* ```$ ​ls /etc/apt/sources.list.d​​​```  

Priority of Multiple Repository Locations  
If there are multiple repository locations allowed and the package you want to install exist in several of them then a priority is followed.
* If the packages have the same priority, the package with a higher version number (most recent) wins.
* If packages have different priorities, the one with the higher priority wins.  
To view priorities of a package ```$ apt-cache policy **package**```  
To view global priorities of the repositories $ apt-cache policy  
It is possible to manually change priorities. More advanced instructions are in Debian org docs  

Raspberry Pi and Ubuntu examples of sources.list   
```console
type	repository URL	distribution	component/categories
Raspberry Pi Example	
deb	http://raspbian.raspberrypi.org/raspbian/	buster	main contrib non-free
Ubuntu Example	
deb	http://security.ubuntu.com/ubuntu/	focal-security	main restricted
deb	http://security.ubuntu.com/ubuntu/	focal-security	universe
deb	http://security.ubuntu.com/ubuntu/	focal-security	multiverse
deb	http://us.archive.ubuntu.com/ubuntu/	focal-backports	main restricted universe multiverse
deb	https://cloud.r-project.org/in/linux/ubuntu	focal-cran40/		
```

**Description of the fields**
**deb** = std archive containing binary packages normally used (.deb packages)  
**deb-src** = source packages, not normally used.  
The repository URL (http) address  
The distribution/code name of your system  
```run $ lsb_release -cs ```  
RPi main consists of DFSG-compliant packages, which do not rely on software outside this area to operate. These are the only packages considered part of the Debian distribution.   

The default Ubuntu repositories  
* Main -> Canonical-supported free, open-source software (Software installed during initial setup. Canonical supported/updated)
* Universe -> Community-maintained free and open-source software (Where much of the software center will download from)
* Restricted -> Proprietary drivers for devices (ie hardware drivers)
* Multiverse -> Software restricted by copyright or legal issues (a little more questinable to install but could be needed)

**Add Another Repository to sources.list**
To add an additional repository you'll usually need two items  
1. Public key (optional)
2. http link to their repository
Necessary information should be specified in the package installation documentation.  
Many providers will specify a public key to authenticate downloaded packages, but not all. See example below for r.  

To add repositories use Software & Updates/Synaptic gui or command add-apt-repository  

**Software & Updates GUI Option**  
* Go to the "other" tab and select "add"  
* Enter the APT line as listed in package documentation  

**Command add-apt-repository Option**
​Make sure add-apt-repository tool is installed (a good reference is linuxize article)  
The add-apt-repository Python script allows you to add an APT repository to either /etc/apt/sources.list or to a separate file in the /etc/apt/sources.list.d directory.  
To install it  
* $ sudo apt update
* $ sudo apt install apt-transport-https software-properties-common  

**How to use the add-apt-repository**   
(if needed, add the key noted in package documentation)
```$ sudo apt-key adv --keyserver <keyserver for your system> --recv-keys <key from documentation>```  
Now add the repository to your sources.list
```$ sudo add-apt-repository repository```  
* Repository can be in std format like deb http://repo.tld/ubuntu distro component or a PPA repository in the ppa:**user**/**ppa-name** format.
* ```$ man add-apt-repository``` for options  
* Ubuntu 18.04 and newer the add-apt-repository will also update the package index if the repository public key is imported. Otherwise run a sudo update before installing.​  
```$ sudo apt install <package>```  

**An example to install latest r revision 4.0 from r-project repository​ (details, including key number, found at cran.r-project.org website)**  
```console
Import the repository public key (shorted for tutorial)
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C5564
Add the http link found on cran.r-project.org website to sources.list with 
​$ sudo add-apt-repository 'deb https://cloud.r-project.org/in/linux/ubuntu focal-cran40/' 
Start installing packages from the newly enabled repository
$ sudo apt install r-base
```
**Adding PPA Repository**
PPA stands for Personal Package Archives. It allows you to install software created by other Linux users without having to wait for it to get added to the official repository. Launchpad platform hosts the PPA files.   
Instead of updating sources.list this will create a file in /etc/apt/sources.d dir and register the public key.  
```$ sudo add-apt-repository <PPA-name> ```  ​
​Now install the package  
```$ sudo apt install <package> ```  
Terms to Know​  
* Package index - A database that holds records of available packages from the repositories enabled in your system.​ (run sudo apt update to update the index)  
* Repository - Directory containing deb packages/files used to install a software application. Typically a remote location, but also could be local.   

# Installing Packages
Since I focus on Debian/Ubuntu/Raspberry Pi OS systems these notes will cover apt/.deb scenarios.

Should you use GUI (graphical user interface) or CLI (Command Line Interface)? Many apps you use to install programs will have both a gui version and respective command lines. It can be useful to know both. Although the terminal takes some practice there are often more options. And when doing STEM projects you are likely to find a scenario where you will boot straight to a cli and do not have the desktop.  
Three primary gui's I use are the Software Store, Synaptic, and gdebi. Synaptic and gdebi are straight forward, they both handle Debian packages, and you can see a list of packages installed with either thru the Synaptic gui or various commands.  (more details below). The Software Store can vary, so it is a good idea to scroll down and glance at the "Source" when you are looking at programs "installed" or "to install". If it was installed with snap or flatpak it should indicate such.  
example of each  
Source: Snap Store  
Source: dl.flathub.org (flatpak)  
<div class="row">
    <div class="col-8 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/software1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

As a fail-safe it is good to know a few commands for each of the methods. This will help you handle programs where different versions exist across different package managers.  
For example, to see what version of gimp each manager would install run commands below. Can also compare to your system's Software Store (version and source).
``` console
# dpkg/apt/Synaptic gui
$ apt search gimp
gimp/bionic,now 2.10.14
```
``` console
# Snap
​$ snap find​ gimp
gimp 2.10.22
```
``` console
# Flatpak
​$ flatpak search gimp
GNU Image   2.10.22
```  

To see what version of a package is currently installed run the commands below (varies by package manager). Depending on your system and how the package was installed the Software Store may also show some details. Best method is the command line.  
``` console
# dpkg/apt/Synaptic gui
$ apt -qq list gimp --installed
gimp/bionic,now 2.10.14
```
``` console
# Snap
​$ snap list
gimp  2.10.22
​$ snap info gimp
```
``` console
# Flatpak
$ flatpak list
GNU Image   2.10.22
```  

The Software Store, Synaptic (apt install) and gdebi will cover most of the methods for installing packages initially. On a Raspberry Pi, the ```sudo apt install``` will be primary. For RPi the 'Software Store' is under 'Preferences' as Add/Remove Software (PiPackages)

As you use your Linux PC more you'll come across more applications you want to install and it will involve secondary methods. Below are my notes on all install methods.

# Common Installation Methods
Before installing Check the Installation Documentation from the package provider and run the option that matches your system.
Then make sure your system is up to date. Most systems, including Raspberry Pi OS, will notify you there are updates and ask you to download/install. Otherwise you can run the commands.  
```$ sudo apt update && sudo apt upgrade```  
or can use full-upgrade which will remove packages if necessary to update the whole system. Confirm which packages are going to be removed.
older version
```$ sudo apt-get update && sudo apt-get dist-upgrade```  
or on Linux distros check the 'Software Update' center.

## Software Store
This method is most similar to traditional Windows installation and does not require the command line. After downloading you can click on the file and open with the systems software installer. In the background it will likely use snap or flatpak as the method.

However you'll notice when searching for 'how to install **package name**' most of the answers will use command lines. If it is an 'apt' or 'dpkg' command you can also use the 2 GUIs below.

## Synaptic Package Manager
Used for most remote packages (http, ftp type locations). 
* ​If package is not in default repository will have to add location to sources (will be listed in package documentation)
* **GUI** you search and then mark/apply the program to install. It will also install the dependencies.
* **Terminal** equivalent
```$ sudo apt install <package> ```  
or the older
```$ sudo apt-get install <package>​```  
More terminal commands available in the 'command line' section below.
apt is newer (came out in 2014) and has equivalent commands to apt-get. A visual difference is that apt shows an install progress bar at the screen bottom.  

## Gdebi
Used for .deb packages you download to your local system. Then start gdebi app, 'open' the .deb file, and 'install package'. It will also install dependencies. The synaptic package manager can be used to uninstall deb packages.
Terminal equivalent option 1
```$ sudo gdebi <path/to/file.deb>```  
Terminal equivalent option 2
```$ sudo dpkg -i <path/to/file.deb>```  
and if you have to fix dependencies
```$ sudo apt-get -f install```  

Both Synaptic package manager (apt) and gdebi (.deb) can be installed from the preferences/add-remove software option in Pi menu or Ubuntu software store or with  
```$ sudo apt install synaptic```  
```$ sudo apt install gdebi```  

> Synaptic package manager is a GUI for apt. Synaptic package manager and gdebi are front-end/high-level apps that use dpkg (back-end, low level tool) to install/remove debian packages. They check for dependency packages in the repositories and install them. 

Other methods for other types of applications  
- git  
- wget, curl
- AppImage download (no install)
- Binary distribution (tarball) 
- Flatpak
- Linuxbrew​
- Python Packages (pip)

# Command Lines for Installing/Uninstalling
Don't forget to use tab auto-completion to help with package names  
```console
Install Local, Downloaded .deb File
gdebi command (my preferred method)
$ sudo gdebi <path to the .deb> 
apt 
$ sudo apt install <path to the .deb>
dpkg 
$ sudo dpkg -i <path to the .deb>
if you need to fix dependency errors
$ sudo apt-get install -f
```
```console
Install Remote Pkg from Repository
apt command
$ sudo apt install <package name>
Can use wildcard to get plugins, etc
$ sudo apt install package*
If you don't know the exact name can search
$ apt search <keyword>
To install Python Packages - PyPI
​$ pip3 install <package name>
(ideally in project folder with venv)
```
```console
​​Find Packages Already Installed
Search using keyword
$ apt list --installed | grep -I <keyword>​
$ apt list --installed
$ dpkg --list
$ apt list --upgradeable
List details of a package
$ apt show <package name>
$ dpkg -l <package name> 
Also shows if installed by remote or local
$ apt -qq list <package name> --installed
```
```console
Uninstall/Remove Packages
To uninstall and remove data/config files
$ sudo apt purge <program name> 
Only uninstall
​$ sudo apt remove <program name>  
To help find the program name to uninstall
$ sudo apt list --installed | grep -I <keyword>​ 
For Python packages (either method)
$ pip3 uninstall <package name> 
$ python3 -m pip uninstall <package name>
```
```console
Clean Up Old Files
Remove older files from apt cache  
$ sudo apt-get autoremove​
Remove older files from repository 
$ sudo apt-get clean
Remove dependencies not needed
$ sudo apt-get autoremove
```
```console
Helpful Commands for Commands
Show reference manual  
$ man <program or command>
Display information on a command
$ whatis <command>
```
```console
Finding Executables​​
Location of binary/source files 
$ whereis <program name> 
List pathname of executable
$ type <program name>
$ which <program name> 
Show the version being used​
​$ <program name> --version 
If you want a newer version you may have to uninstall and use a different method like flatpak or linuxbrew to install a newer version
```
```console
Trouble Shooting Install Errors
System will not let you run sudo apt
$ sudo dpkg --remove --force-remove-reinstreq <package> 

Checks for dependency problems
$ sudo dpkg --configure -a  
Fix dependency problems
$ sudo apt-get -f install   
Then refresh repository index
$ sudo apt-get update
Then retry installation
```

# Other Methods
Flatpak, Snap, ​git, pip, shell, curl, wget, flatpak, AppImage

## Flatpak
[Flatpak](https://flatpak.org/) Install and Commands
Flatpak came up when trying to install the most recent version of gimp. Per gimp install Flatpak would have faster updates, most recent revisions. According to Flatpak web site it will be a more universal installer, making packages available to more Linux distros. Installing went fairly smooth, other than fixing the error mentioned below.  

Apps can be downloaded/installed from [flathub](https://flathub.org/apps)  
Select 'install' after selecting app or download and install from folder  

Start files/icons typically found in /var/lib/flatpak/exports/share/ and ~/.local/share/flatpak/exports/share/applications  
​
Note - One consequence of being a "universal installer" is that Flatpak will take up a quite a bit more disk space than apt installed packages. My /usr/var/lib/flatpak folder is usually multiple GB. To check space I use "disk usage analyzer". Then there are some cleanup commands at the bottom.  
```$ sudo apt install baobab ```  

```console
To install  run as root (see below)​​
$ sudo su
# apt install flatpak
Add flatpak repository to get apps
# flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
restart
```
Note - If you see the $XDG_DATA_DIRS error below you can try logging out/logging in to start a new session. This will update the variable and add the flatpak paths to it.  
```$ echo $XDG_DATA_DIRS```  
Shows you the value. Now run another flatpak command and check if the error is gone.  
```console
Note that the directories '/var/lib/flatpak/exports/share' '/home/pi/.local/share/flatpak/exports/share' are not in the search path set by the XDG_DATA_DIRS environment variable, so applications installed by Flatpak may not appear on your desktop until the session is restarted.
```
However, on Raspberry Pi it's possible the variable keeps getting over-ridden.​
https://www.raspberrypi.org/forums/viewtopic.php?t=275001 helped me fix it.
For my RPi system I had to add the "Environment variable" info to my desktop.conf  
```​$ sudo nano  /etc/xdg/lxsession/LXDE-pi/desktop.conf```  
```console
[Environment_variable]
XDG_DATA_DIRS=/var/lib/flatpak/exports/share:/home/pi/.local/share/flatpak/exports/share:/usr/share/fkms/:/usr/local/share/:/usr/share/raspi-ui-overrides:/usr/share/:/usr/share/gdm/:/var/lib/menu-xdg/
```  
There were other desktop.conf when I did a search in /etc/ but the LXDE-pi file is what worked for me.​​

​Evidently the XDG_DATA_DIRS variable is getting over-written and not keeping the flatpak update. Adding the updated path in desktop.conf with flatpaks folders is a work-around.  
**Flatpak Commands**  
```console
$ flatpak --help
Look for app to install
$ flatpak search <name>
Install an app
$ sudo flatpak install <remote name application name>

List configured remote repos
$ flatpak remotes
Show available runtimes and apps
$ flatpak remote-ls

List installed apps
$ flatpak list --app
To update installed apps/runtimes to latest version
$ flatpak update

Run an app
$ flatpak run <name>
Uninstall an app
$ sudo flatpak uninstall <name>

example with gimp
$ flatpak search gimp
Description	Application	Version	Branch	Remotes
GNU Image Manipulation Program - Create…	org.gimp.GIMP	2.10.22	stable	flathub

$ sudo flatpak install flathub org.gimp.GIMP
$ flatpak run org.gimp.GIMP

other method
$ sudo flatpak install https://flathub.org/repo/appstream/org.gimp.GIMP.flatpakref

Clean up and Troubleshooting
To remove runtimes and extensions that are not used by installed applications, use:
$ flatpak uninstall --unused
After running --unused, fix inconsistencies with your local installation, use:
$ flatpak repair
```
**Raspberry Pi Specific**  
If you want to add a desktop icon  
* Go to Main Menu -> Preferences > Main Menu Editor
* Go to "other" and do New Item
* For command use the flatpak command
* example - **flatpak run org.gimp.GIMP​**  

For the icon you can download a logo from the internet or you can search on another system where the icon exists. One challenge is that the icon name is often an abbreviated version of the package name and may be hard to predict.  
I usually find the icons in the /usr directory. Go to the /usr directory and do a   
```$ find . -name *.png```  
or a more specific find. It will return a lot of images but will help track it down. The /usr/share/applications or /usr/application-name or /usr/share/pixmaps are good starting points too. I save the .png icon files in my pictures folder for easier selection later.  

If you want a "start menu" option for running flatpak and doing flatpak updates  
* Go to Main Menu -> Preferences > Main Menu Editor  
* Go to "other" you want to put it in and do New Item  
For command use the flatpak commands  
Example to list flatpak apps installed  
* ```lxterminal -e "bash -c 'flatpak list --app;bash'"```  
Example to update flatpak apps (your system may be doing this automatically)  
* ```lxterminal -e "bash -c 'flatpak update;bash'"```  

## Snap
Snap Install and Commands. I do not use snap often however it is built into some of the Software Stores and good to know its commands.  
​
Note - Snap will take up a quite a bit more disk space than apt installed packages. To check space I use "disk usage analyzer" and then there is a script below for cleaning up previous revisions.  
To see disk usage  
```$ sudo apt install baobab ```  

Snap commands
```console
$ sudo apt install snapd
Search for app
$ snap find <app>
Show info on a app
$ snap info <app>
List installed apps
$ snap list
Remove an app
$ sudo snap remove <app> 
```
**Clean Up Older Snap Versions of Apps**
Older versions of snap apps can start to take up a lot of space. Here is a script to remove them from Canonical Snapcraft team.  
```console
#!/bin/bash
# Removes old revisions of snaps
# CLOSE ALL SNAPS BEFORE RUNNING THIS
set -eu
snap list --all | awk '/disabled/{print $1, $3}' |
    while read snapname revision; do
        snap remove "$snapname" --revision="$revision"
    done
```
Then make executable  
```console
$ sudo chmod +x snap-cleaner.sh
Run with sudo privileges
$ sudo ./snap-cleaner.sh
```

## Python pip
Python packages install with apt or pip3
* Ideally inside a virtual env (keeps the packages local to that project).
* ```​$ pip3 install <package name> ```  
If having problems with "failed building wheel" try ..
* ```$ pip3 install --upgrade pip setuptools wheel```  
* ``` pip install <package> --no-cache-dir```  
From stack overflow.. if the package is not a wheel, pip tries to build a wheel for it (via setup.py bdist_wheel). If that fails you get "failed building wheel for **package**" and pip falls back to installing directly (via setup.py install). Once we have a wheel, pip can install the wheel by unpacking it correctly. pip tries to install packages via wheels as often as it can. This is because of various advantages of using wheels (like faster installs, cache-able, not executing code again etc).

Less frequently you will 
* download/extract the package,
* then cd into dir and run
* ```$python setup.py install ```  
To uninstall can use either method
* ```$ pip3 uninstall <package name> ```  
* ```$ python3 -m pip uninstall <package name>```  

## wget,curl,git,shell
**curl or wget**. Both are command line tools for downloading packages from ftp and http.
* ```$ wget <ftp/http....location>  ```  

**git** clone for installing git projects. Follow the documentation in the github (usually README.md or a file called 'install') 
* ```$ git clone <HTTPS/SSH link from remote GitHub>​ ```  

**shell file**
shell installation file. Download file and read install documentation.
After downloading and extracting. Follow documentation to run a 
* ```$ bash <installation file.sh> ```  
or if it is installation file
* ```$ ./<installation file> ```  

## AppImage
There's no installation or extracting files. You download the .AppImage file, make it executable, and run it.  
My preference to integrate with the desktop is..
* file.AppImage -> $HOME/.local/bin
* file.desktop -> $HOME/.local/share/application (create this)
* icon -> $HOME/.local/share/icons (extract from appimage file)  
Detailed steps
* Download **file**.AppImage file to Downloads
* [Icon] Copy and change extension to .zip
* [Icon] Open .zip, go to /usr/share and extract icons folder to $HOME/.local/share/
* Move original .AppImage to $HOME/.local/bin
* Right click and select 'allow to execute' in permission or
* $ sudo chmod +x **file**.AppImage
* Create **file**.desktop in $HOME/.local/share/applications
* Exec=**file**.AppImage (assuming /home/USER/.local/bin is in your $PATH, otherwise give the full path location)
* Icon=icon-name.png (name that was extracted to your icon folder)
* Right click and select 'allow to execute' in permission or
* $ sudo chmod +x **app-name**.desktop
* May need to logout/login (or reboot) for icon to show up in Start menu   

RPi - Preferences-Add/Remove software (uses PiPackages manager for GNOME)  

Ubuntu - GNOME software center, Linuxbrew, snap (have not tried snap on RPi)​  
Download/Extract a Binary Distribution (tarball with /bin, /lib, /share)  

## Download/extract a Binary Distribution
If you download a tarball (.tar.gz or .tar.xz) and it has the files inside /bin, /lib, /share folders then you need to extract them to corresponding folders.  

Here's a method to do it by command line (you will need sudo)  
* First unarchive (you need to get the /bin, /lib, /share folders outside of the download folder)
* (for .gz)
    * $ tar xzf **archive.tar.gz** 
* or (for .xz)
    * $ tar xvf **archive.tar.xz**
    * $ cd **archive** ​
​
Example below..  
/bin, /lib, /share folders are in the 'node-v14.15..' folder  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/bin1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Want to get the folders into an archive outside the 'node' folder.  

Re-archive the /bin, /lib, /share, etc folders into a folder archive called 'local'
* use c  for compress
* ```$ tar czf local.tar.gz *```  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/bin2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Where to copy/install the files?  
* Distribution managed programs will typically be put in /usr/
* Programs not managed by the distribution package manager are usually put in /usr/local/​
​
Copy/install the files by unarchiving in /usr/local/ (-C is used to specify /usr/local/)
* Use x for extract
* $ sudo tar xzf local.tar.gz -C /usr/local/ 
* To confirm binary is in correct location (may have to close and re-open a terminal for the $PATH to update)
* $ which **package** 
* /usr/local/bin/**package**

Side note - this is not normal.. but if a program requires changing ownership run the chwon command after you unarchive and before you copy/install.
* ```$ sudo chown -R root:root <folder> ```  

# Downloading the Right Package
Picking the right package for your system. Synaptic(apt) knows what type of system you have and will install the correct version. However, if you are manually downloading an installation file you'll need to know what type of system you have and choose the right one.  
You can find OS info with ```$ cat /etc/os-release ```  
and CPU hardware with ```$ lscpu ```  
(note on RPi look at the Cortex-xx and reference ARM wiki core table for 32/64 bit info)

Another nice all-in-one tool is 
```$ screenfetch``` or ```$ neofetch```  

It varies across sites but usually there will be some comments or descriptions with the download file to help you make a decision.  
* ​RPi is an arm CPU​
* RPi with RPi OS = .deb arm
* RPi = ARMyy(cpu type)  

PCs that are Intel/AMD CPU  
* Linux PC = x86/x64 (you will need to know if 32 or 64 bit)
* x86 = 32bit
* x64 (or x86_64) = 64bit
* AMD64 will run on both AMD and Intel 64 bit
* Ubuntu (debian type) 64 bit = deb x64  
(32 bit software can usually run on 64 bit system, but not vice versa)  

# Compressed Archived Files (zip)
Quick info on compressed files
Tar is an archiver Tape Archiver.
* .tar = uncompressed archive
* Archives multiple files into single file
* No compression
Gzip is a tool that compresses the file to reduce its size
* .gz = compressed using gzip
* When used on a .tar can get high level of compression since all files are compressed together.
Tar and Gzip can be used together for a compressed archived file
* .tar.gz = tarball; compressed archive file (common in linux)

Zip is a tool that archives and compresses at the same time (common in windows)
* .zip = compressed archive file
* Each file is compressed independently

**Tar Switches**
```console
-x (extract an archive)
    -C (specify dest dir)

-c (create an archive)
    -z (use gzip)
    -j (use bzip2)
-v (display progress)
-f (specify name of archive)
```
Can use either archiver option in File Explorer or command line
​
To Create Archive/Compress
* Create archive, use gzip to compress, and display progress.
* ```$ tar -czvf archive-name.tar.gz /path/to/dir ```  
* or using zip (use wild cards like * for all files)​  
* ```$ zip file.zip files-to-zip.txt ```  
* (zip all files and -v will show status)
* ```$ zip -v file.zip * ```  
* or if gzip without archive
* ```$ gzip file.txt```  
* or if it has spaces
* ```$``` gzip "file name.txt" ```  

To Unzip/Extract/Decompress
* for .xz compressed
* ```$ tar -xvf archive.tar.xz ```  
* for .gz compressed (and extract to a specific directory)
* ```$ tar -xzvf archive.tar.gz -C /tmp ```  
* gzip command for gz file
* ```$ gzip -d file.gz```  
* unzip command for zip file
* ```$ unzip file.zip```  

# Install Errors
If you get **Msg Can Not Uninstall/Re-install an Older Package(Dependencies) Already Existing on Your System.**  ​
--ignore-installed, --force-reinstall, -f  
​​​
​This has happened to me a couple times and was not too difficult to fix.  You'll have to watch the status of the program you're wanting to install and note which dependencies it is getting hung up on. Then try and force that dependency to re-install/upgrade over the previous.  
First  
```$ sudo -H pip3 install --ignore-installed -U 'dependency listed in error msg'```  
Or
```$ pip3 install --upgrade --force-reinstall --no-cache-dir 'dependency listed in error msg'```  
Now you can go back and try installing the original program. It may hang up on a different dependency and you'll need to repeat this process.  

Or try removing the dependency giving problems and re-run the install of the original program  
```$ sudo apt-get remove python-'dependency listed in error msg'```  
```$ sudo apt install -f```  


-----------------------------  
-----------------------------  
