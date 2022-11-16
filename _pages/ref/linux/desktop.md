---
layout: page
permalink: /ref/linux/desktop/
title: Desktop Appearance
description: Intro to customizing your desktop appearance
nav: false
toc: true
---
# Linux Desktop Terms
The Raspberry Pi is a great coding/project system but the out-of-box Raspberry Pi OS/LXDE appearance is pretty basic. (note DE refers to Desktop Environment). However with a few **tweaks** you can quickly improve the appearance. Most of the items on this page will focus on Raspberry Pi but they can often be applied to other Linux Distros, too.  

Another wrinkle to Linux distros that differ from Windows/macOS is that you can install multiple desktop environments on a system and use a windows manager to login to which ever you want for that session.  

Ubuntu also has distribution/desktop environment pairs with specific names.  
* (Ubuntu) Budgie desktop
* (Ubuntu) MATE desktop
* (Kubuntu) KDE desktop (KDE = Plasma)
* (Xubuntu) Xfce desktop
* (Lubuntu) LXQt desktop  

Some DE options on top of Raspberry Pi OS Lite (or full version) with tutorials
* MATE desktop
* XFCE desktop

**Some Useful Terms**  
<div class="row">
    <div class="col-7 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/diagram.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Shmuel Csaba Otto Traian, CC BY-SA 3.0, via Wikimedia Commons  

[Desktop Environment (DE)](https://en.wikipedia.org/wiki/Desktop_environment) - also referred as graphical interface. Some examples are GNOME, Cinnamon, MATE, XFCE, KDE. The part you interact with. Fully integrated system that builds upon the Window Manager. Requires a Display Server and Window Manager. A system can have multiple DEs and you can use a  Display Manager to select which one you want to use during login.  
[Display Server](https://en.wikipedia.org/wiki/Windowing_system#Display_server) - (aka graphical server) Common example is Xorg. Provides the primitive framework for the DE. Uses display server protocol like X windows (aka X11, X, Xserver) or Wayland. Required for a DE.​​​​​  
[Window Manager](https://en.wikipedia.org/wiki/Window_manager) - Common example is Openbox or Marco (older examples are twm and fvwm). It is system software, usually part of the DE, that manages the placement and appearance/menu/buttons of the windows. Can be used with DE or stand alone (requires X windows but not a DE)  
[Login or Display Manager (DM)](https://en.wikipedia.org/wiki/X_display_manager) - Common example is LightDM and GDM (GNOME Display Manager). It is a graphical login manager that starts the greeter (or auto login) and the display servers. But it is not required. ​You can have a setup that boots to cli and if you want a desktop you run $ startx and it will launch the DE. startx is a front end to xinit which can manually start the display server (instead of using a display manager). xinit will use .xinitrc and .xserverrc to determine the client/server to run (/etc/X11/xinit) . Recommended to start with a 'light' OS (no default DE installed) and build it from the ground up if wanting to customize these settings.  

# Capture Current DE/Theme
Before changing the appearance of your desktop it is good to know what you're current settings are. A nice tool to see the details of your system is neofetch.
```$ sudo apt install neofetch```  
then run with  
```$ sudo neofetch```  
(screenfetch is another option and installed/used with similar commands, but has fewer details) 

Below are examples of an RPi4 with different installs  
Raspberry Pi OS  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/rpios.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Ubuntu  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/ubuntu.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

# Options for RPi Customizing
Depending on the method you use the level of difficulty to change the Raspberry Pi desktop appearance varies. Below is my ranking.
1. Easiest - Start with RPi OS/LXDE, download new themes/icons, update with ```$ lxappearance```  
2. More involved - Start with RPi OS/LXDE, convert to MATE DE. Use both the ```$ mate-tweak``` command and "appearance" menu in desktop to customize
3. Option for RPi4 with 4GB+ RAM - Install Ubuntu OS. A lot of progress has been made on making Ubuntu install on RPi easy. And it's now on the RPi imager. But it requires more system resources and works best on a RPi4 with 4GB+ of RAM. Also one negative point is that it doesn't come with RealVNC service mode pre-installed like Raspberry Pi OS does. Ubuntu/gnome tweaks can be made in the appearance menu and ```$ gnome-tweaks```  

I am using options 1,2 on both RPi 3 (1GB) and Rpi4 (4GB) with no issues and the appearance is upgraded. Installing MATE DE on option 2 is quite a bit more work (details below). Raspberry Pi OS was tailored for Raspberry Pi hardware so it usually performs the best (fastest, most reliable). Another advantage to the RPi OS is that you can install the full version with all recommended software and RealVNC service mode. Option 3 you get the most appearance change (windows, fonts) and works well with a monitor. Ubuntu OS will not have RealVNC service mode pre-installed. I have installed [alternate vnc servers](../../../ref/linux/vnc-ssh/) but they did not work as well as RealVNC service mode for me (big advantage of RealVNC service mode is the cloud connection).  

# Theme Icons Fonts
There are some built-in options you can play with in the appearance/tweaks menus mentioned. But to make bigger changes you'll need to download themes/fonts. One of the easier methods I found was to install a desktop and save the theme/font files from that desktop on a thumb drive. Then you can copy these theme/font files to other systems or setups. Details below.

**Theme Icons Font Download Methods**  
1. Install a new desktop and save the theme, icons, fonts (examples below)  
2. Download and install .deb yaru-theme-icons at [launchpad.net](https://launchpad.net/ubuntu/+source/yaru-theme)​  
3. Download themes/icons from [gnome-look.org](https://www.gnome-look.org/browse) or [mate-look.org](https://www.mate-look.org/browse)  
    * It is helpful if the icons are in a tarball, ie *.tar.gz or *.tar.bz2
    * Inside **Appearances** select icon install and then the tarball
4. Download fonts from [fonts.google.com](https://fonts.google.com/)  
    * Extract to **$HOME/.local/share/fonts**
    * Can also install font-manager ```$ sudo apt install font-manager```  

**Applying New Themes Icons Fonts**
As mentioned there are multiple ways to install themes, icons, and fonts. The most basic method is to download or copy from another system and place them in your .local folder. 
My steps
1. Installed Ubuntu/GNOME and Ubuntu/MATE on other systems (see details further below)
2. Zipped them to a tarball (used .tar.gz for icons and was easy install on LXDE)
    * Location of files
    * **$HOME/.local/share/themes**
    * **$HOME/.local/share/icons**
    * **$HOME/.local/share/fonts**
    * (on some systems they may go directly under $HOME. ie $HOME/.fonts)
    * Can run ```$ neofetch```  to see if your system is using GTK2, 3, 4, etc.
3. Copied the themes/icons to a USB stick.
4. On your new system copy the files from your USB stick to the same **$HOME/.local/share/** locations
5. Depending on the system use one of these methods below to install the new theme  

**Quick steps to update the appearance**  
**LXDE**
* ```$ lxappearance```  then go to widget and Icon Theme. The files you copied into $HOME/.local/share/themes and icons should be available. (If getting segmentation fault error when running lxappearance can try ```$ sudo apt remove lxappearance-obconf``` and retry.)
* For icons it's fastest to compress into tarball format tar.gz and then use install option. (compress is option in file explorer, right click. If tar.gz is not an option install xarchiver or engrampa. I installed engrampa with; ```$ sudo apt install engrampa```)  

**MATE**
* Preferences - Look and Feel - Appearances. The "themes" tab should show the files you copied in. To change icons select the "customize" option and then "icons" tab.
* Mate-tweak ```$ sudo apt install mate-tweak``` You can make additional appearance/dock changes here.  

**GNOME**
* Tweaks ```$ sudo apt install gnome-tweaks``` Then go to Appearances. There are drop down menus for themes and icons.  

# Docks
​​​Docks will show an app launcher menu bar on your desktop. This will make it feel more like a standard PC.  
I have tried out  
* Plank/docky
* Cairo dock
* Dash to dock for gnome (similar to Ubuntu default dock)

> You do not want multiple docks enabled at once. When switching from one to the other I would disable the current dock first.  

## Plank
**Plank dock install instructions**  
* ```$ sudo apt install software-properties-common```  
* ```$ sudo add-apt-repository ppa:ricotz/docky```   (Skip for RPi. Install from default repo)
* Compton is also needed for RPi OS/LXDE to get transparency
* ```$ sudo apt install plank compton```  
* ```$ compton```  
* ```$ plank```  
Add plank and compton to "start up" (use autostart or startup script method)  

LXDE (RPi OS) specific - Compton is a standalone compositor for X Windows (can be installed on top of current window manager). It is a fork of the xcompmgr compositor and supports openGL. It enables the transparency on Raspberry Pi OS.  

​MATE specific - To get the transparency on Ubuntu Mate enable Marco Compositor for windows manager (in settings). Sometimes had to disable, then enable to see the affect.  

To customize plank hold ctrl key and right click on the dock. Then select preferences.  

To install themes  
* Download a theme for plank (ie www.pling.com).
* Extract to $HOME/.local/share/planks/themes.
* Then go to Plank preferences and select the theme.
​
For autostart/startup  
* Ubuntu - add "plank" to "startup applications" (search for "startup" in settings)
* ​Raspberry Pi OS - add compton and plank to "autostart" (/etc/xdg/lxsession/LXDE-pi/autostart)  

Example Plank (bottom placement)
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/plank.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**Other docks**  

**Cairo dock install instructions**  
Cairo is a more advanced dock and has a lot of customization features.
More information at [glx-dock](http://glx-dock.org/)
To install 
(Note - for Raspberry Pi the cairo-dock-team PPA was not located so I skipped the ppa install)
* ```$ sudo add-apt-repository ppa:cairo-dock-team/ppa```  
* ```$ sudo apt update```  
* ```$ sudo apt install cairo-dock cairo-dock-plug-ins```  

After rebooting run the Cairo app ```$ cairo-dock``` and follow the instructions. It starts with a 2D theme but if you right click and go to "Cairo Configuration" you can select a 3D theme.
​For autostart/startup
* Under configure select "Launch Cairo-Dock on startup" .. OR
* Ubuntu - add "cairo-dock" to startup applications (search for "startup" in settings)
* Raspberry Pi OS - add "cairo-dock" to "autostart" (/etc/xdg/lxsession/LXDE-pi/autostart)  

Example Cairo-dock with 3D theme  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/cairo.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**Raspberry Pi uses 'alacarte' to edit the Start Menu/Launcher**  
Note - alacarte can be installed on Ubuntu too.  
```$ sudo apt install alacarte```  
I've noticed my changes in alacarte revert to the 'Other' Menu. So I only add custom app launchers in the 'Other' menu. 
* Go to "Start" "Preferences" "Main Menu Editor"
* Go to "Other" select "New Item"
* When you add a New Item you fill in the same primary fields as a [].desktop file](../../../ref/linux/software/)
* Icon (click on the image)
* Name (name of app)
* Command - This will be the same as the Exec field in a .desktop file
* /usr/bin/<application> # (path to executable binary file)
* lxterminal -e 'commands' # (start terminal and execute commands, shell file, python file, etc)
* bash -c 'commands' # (run shell commands with ability to pass arguments/variables)
* lxterminal -e 'bash -c "commands"' # (wrap the bash command inside the terminal command)  

**Ubuntu Specific**  
Tweaks and extensions are the common apps for modifying the dock appearance in Ubuntu/GNOME  

First ensure universe repo is enabled in "Software & Updates" or run command below.  
```$ sudo add-apt-repository universe```  

Add Tweaks to add more appearance options  
```$ sudo apt install gnome-tweaks```  
A "Tweaks" icon should be added or you can search for it on the desktop.  

**Dash to dock**  
To install Dash to dock
* Enable GNOME extensions in google chrome at https://extensions.gnome.org/ 
* Search for "dash to dock" and turn it "ON" (upper right)
* Now go to "extensions" app and enable the "Dash to dock"​​
    * Pre Ubuntu 20 was able to use "Tweaks" to enable Dash to Dock or Ubuntu Dock.
    * After upgrading to Ubuntu 20.04 there is an "Extensions" on the desktop you use to enable/disable the Ubuntu docks.
* Hit the gear icon and you can adjust appearance settings.
* May have to reboot for changes to take affect.

(If you run into problems try the alternative
```$ sudo apt install gnome-shell-extension-dashtodock```  
Then go to extensions)​  

On RPi - Installing compton and enabling during startup will allow transparency  
Example Dash to dock Enabled (bottom placement)  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/dash.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>  

# System Monitor
System monitors are useful for seeing how much memory/CPU resources are being used. The CPU freq/temp can is helpful when over-clocking an RPi4. System monitors on the desktop also improve the appearance.  
For a quick, built-in app try htop   
```$ htop```  
​
## Conky  
[Conky](https://conky.sourceforge.net/config_settings.html) is a more advanced option that can stay visible on your desktop. It has a transparent option and works on RPi and other Linux distros. You can also build-in gauges with Lua scripts.  
To install  
```$ sudo apt install conky conky-all```  

A couple helpful commands while testing.  
To start  
```$ conky```  
To stop  
```$ killall conky ```  

The default conky is pretty basic but you can easily customize it. 
1. create a .conkyrc in your home folder
2. test with ```$ conky```  
3. Build on this file to create the final system monitor you want. Examples below can be used for a starting point. 
```$ sudo nano .conkyrc​​​```  

**Some Options**  
* Transparency is probably the nicest feature. For true transparency need composite manager (RPi OS - compton; MATE - marco)
* Set with..
```console
own_window = true,
own_window_transparent = true,
own_window_argb_visual = true,
own_window_type = 'desktop'
```
​Partial transparency
```console
own_window_argb_value  = xx, # (0-255, I have not tried this yet)  
```
You may have to try a couple variants for your system. [conky config settings](https://conky.sourceforge.net/config_settings.html)

> Take note there is an older and newer format for the conky config settings  

I use separate partitions for my backup and share drives and conky helps monitor them.
* / (this will be your root)
* /home (if you created your home dir on a separate partition)
* /media/USER/.. (if you mount a partition in the file explorer)
* /mnt (if you change the partition to "automount" at boot)  

​If you change your partition to "automount" you'll want "Identify As LABEL=xxx". That label can be found in /mnt. Put this location in .conkyrc to monitor. Details in backup section.  

**Raspberry Pi Conky**  
Raspberry Pi Specifics  
Note - for ° symbol on RPi.  
RPi character map can be enabled with [compose](https://help.ubuntu.com/community/GtkComposeTable)  
```$ sudo raspi-config```  
Localization Options  
Keyboard  
Go thru options til you get to 'Compose Key' and select one (ie Right Alt)  
Keyboard combination descriptions are in ubuntu compose table  
(ie - Alt-o-o will insert ° and Alt-u-/ gives µ)  

Autostart at boot - Conky may need a short delay. This can be done with a 5-8 sec sleep before launching. I've found sometimes I have to open a terminal and restart conky with ```$ conky```   

Exec=conky --daemonize --pause=5  
Or  
Exec=lxterminal -e "sleep 5;conky &"  

Conky can be [started at boot](../../../ref/linux/autostart/) with a couple methods.  
1. Use the autostart file at /etc/xdg/lxsession/LXDE-pi/autostart (can not use " " or ' ')
2. Or create a .desktop file and put it in the autostart folder. Example below.  
​```$ sudo nano /etc/xdg/autostart/conky.desktop```  
```console
[Desktop Entry]
Name=conky
Type=Application
Exec=lxterminal -e "sleep 5;conky &"
Terminal=false
Comment=system monitoring tool
Categories=Utility
```

**Ubuntu (other Linux distro) Specifics**  
Ubuntu  
Note - GNOME character map is  
```$ sudo apt install gucharmap```  
Then run gucharmap for the gui. You can copy/paste special characters like °C.  

Autostart - Ubuntu has a "startup application" program you can find in the desktop. Search "startup" and add conky there  

command = /usr/bin/conky (since /usr/bin is in $PATH could also just put "command = conky")  

Can add a delay with​  
Exec=conky --daemonize --pause=5  
Or  
Exec=gnome- terminal -e "sleep 5;conky &"  

Example PC/RPi4 using Ubuntu with Dual Monitor  
(remove the xinerama_head 2 for a single monitor setup)  
conky config file in new .conkyrc format is below    
<div class="row">
    <div class="col-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/conky1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>  

```console
conky.config = {
  alignment = 'top_right',
  xinerama_head = 2,
  background = false,
  border_width = 0.5,
  cpu_avg_samples = 4,
  default_color = 'white',
  default_outline_color = 'grey',
  default_shade_color = 'black',
  draw_borders = true,
  draw_graph_borders = true,
  draw_outline = false,
  draw_shades = false,
  use_xft = true,
  font = 'DejaVu Sans Mono:size=10',
  gap_x = 30,
  gap_y = 30,
  maximum_width = 200,
  minimum_height = 5,
  minimum_width = 5,
  net_avg_samples = 2,
  double_buffer = true,
  out_to_console = false,
  out_to_stderr = false,
  extra_newline = false,
  own_window = true,
  own_window_colour = '000000',
  own_window_class = 'Conky',
  own_window_argb_visual = true,
  own_window_type = 'dock',
  own_window_transparent = true,
  own_window_hints = 'undecorated,below,sticky,skip_taskbar,skip_pager',
  stippled_borders = 0,
  update_interval = 1,
  uppercase = false,
  use_spacer = 'none',
  show_graph_scale = false,
  show_graph_range = false
}

conky.text = [[
  # $color${font}${font Arial:size=44}$alignr${time %H:%M}$font$color${font Arial:size=15}
  # $alignr${time %a}, ${time %d %b %Y}$font$color

  ${font Arial:bold:size=10}${color red}SYSTEM ${color DarkSlateGray} ${hr 2}
  $font${color white} Kernel $alignr $kernel
  File System $alignr${fs_type}
  Uptime $alignr $uptime

  ${font Arial:bold:size=10}${color red}CPU ${color DarkSlateGray}${hr 2}
  $font${color white} Frequency $alignr${freq_g cpu0}Ghz
  Usage $alignr $cpu %
  Temperature $alignr ${acpitemp}°C
  CPU1 ${cpu cpu1}% ${cpubar cpu1}
  CPU2 ${cpu cpu2}% ${cpubar cpu2}
  CPU3 ${cpu cpu3}% ${cpubar cpu3}
  CPU4 ${cpu cpu4}% ${cpubar cpu4}

  ${font Arial:bold:size=10}${color red}MEMORY ${color DarkSlateGray}${hr 2}
  $font${color white} MEM $alignc $mem / $memmax $alignr $memperc%
  $membar
  $font${color white}SWAP $alignc $swap / $swapmax $alignr $swapperc%
  $swapbar

  ${font Arial:bold:size=10}${color red}HDD ${color DarkSlateGray}${hr 2}
  $font${color white} /home $alignc ${fs_used /home} / ${fs_size /home} $alignr ${fs_free_perc /home}%
  ${fs_bar /home}

  ${font Arial:bold:size=10}${color red}TOP MEM  ${color DarkSlateGray}${hr 2}
  ${color white}  $font${top_mem name 1}${alignr}${top mem 1} %
  $font${top_mem name 2}${alignr}${top mem 2} %
  $font${top_mem name 3}${alignr}${top mem 3} %
  $font${top_mem name 4}${alignr}${top mem 4} %
  $font${top_mem name 5}${alignr}${top mem 5} %

  ${font Arial:bold:size=10}${color red}TOP CPU  ${color DarkSlateGray}${hr 2}
  ${color white}  $font${top name 1}${alignr}${top cpu 1} %
  $font${top name 2}${alignr}${top cpu 2} %
  $font${top name 3}${alignr}${top cpu 3} %
  $font${top name 4}${alignr}${top cpu 4} %
  $font${top name 5}${alignr}${top cpu 5} %
]]
```

## Conky Gauges with Lua Scripts
Lua scripts give the ability to add graphical gauges. Lua code built off SeaJey, conkyOrange, and Lunatico
* Add the Lua code below to .conky folder in home directory
* Example - /home/USER/.conky/seamod_rings.lua  
You call the lua script in your .conkyrc file
* Examples (old and new format)
* lua_load ~/.conky/seamod_rings.lua
* lua_load = '~/.conky/seamod_rings.lua',
* Will have to adjust offsets/voffsets in .conkyrc to get text lined up

**Lua script tweaks**
* Each graph has a name='nvidia', arg='gpufreq' that follow conky format
* For wlan will have to adjust based on $ ifconfig
* Adjust the graph transparency (alpha) based on your background
    * graph_bg_alpha and graph_fg_alpha

Graph is 270° so you have to adjust graph_unit_angle accordingly  
graph_unit_angle = 270 / max_value  
So if plotting on a 0-100% scale  
* max_value = 100
* graph_unit_angle=2.7
For my NVIDIA GPU with max speed of 2000MHz  
* max_value=2000
* graph_unit_angle=0.135
* Also had to adjust graph_fg_alpha to 0.035
​
​(Current RPi format with Lua gauges below is the old conky format)

Nvidia command ```$ nvidia-smi``` will show Nvidia details.  
On RPi with Ubuntu I had to add a ```$cpu ```in the .conkyrc or Lua would not update the graphs for the cpu's.  
To get sensor info ```$ sudo apt install lm-sensors```  

## Conky/Lua Gauge Examples
RPi4 using Raspberry Pi OS with Dual Monitor  ​
4 core CPU  
<div class="row">
    <div class="col-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/conky2.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
Remove xinerama_head 2 for single monitor  
.conkyrc config file with call to Lua script. Note using old conky format   

```console
background yes
use_xft yes
xftfont 123:size=8
xftalpha 0.1
update_interval 0.5
total_run_times 0
own_window yes
own_window_type desktop
own_window_transparent yes
double_buffer yes
minimum_size 250 5
maximum_width 400
draw_shades no
draw_outline no
draw_borders no
draw_graph_borders no
default_color gray
default_shade_color red
default_outline_color green
alignment top_right
# xinerama_head 2
gap_x 10
gap_y 10
no_buffers yes
uppercase no
cpu_avg_samples 2
net_avg_samples 1
override_utf8_locale no
use_spacer right

lua_load ~/.conky/seamod_rings.lua
lua_draw_hook_post main

TEXT
${font Arial:bold:size=10}${color4}SYSTEM ${hr 2}
${offset 15}$font${color3}$sysname $kernel $alignr $machine
${offset 15}$font${color3}Uptime: $uptime $alignr ${acpitemp}C
${offset 15}$font${color3}Frequency $alignr${freq_g cpu0}Ghz

# Showing CPU Graph
${voffset 45}
${offset 90}${font Arial:bold:size=10}${color5}CPU
# Showing TOP 5 CPU-consumers
${offset 75}$font${color4}${top name 1}
${offset 75}$font${color2}${top name 2}
${offset 75}$font${color2}${top name 3}
${offset 75}$font${color3}${top name 4}
${offset 75}$font${color3}${top name 5}

#Showing memory part with TOP 5
${voffset 55}
${offset 90}${font Arial:bold:size=10}${color5}MEM
${offset 75}$font${color4}${top_mem name 1}${alignr}${top mem 1}
${offset 75}$font${color2}${top_mem name 2}${alignr}${top mem 2}
${offset 75}$font${color2}${top_mem name 3}${alignr}${top mem 3}
${offset 75}$font${color3}${top_mem name 4}${alignr}${top mem 4}
${offset 75}$font${color3}${top_mem name 4}${alignr}${top mem 5}

# Showing disk partitions: root, home
${voffset 60}
${offset 90}${font Arial:bold:size=10}${color5}DISKS
${voffset 25}
${offset 75}$font${color2}root(free) ${alignr}$font${fs_free /}
${offset 75}$font${color2}root(used) ${alignr}$font${fs_used /}
${offset 75}$font${color2}home(free) ${alignr}$font${fs_free /home}
${offset 75}$font${color2}home(used) ${alignr}$font${fs_used /home}

# Wireless data
${voffset 55}
${offset 90}${font Arial:bold:size=10}${color5}INTERNET
${voffset 40}
```  

Lua script gauges 4 core CPU and wifi  
* /home/USER/.conky/seamod_rings.lua  

~~~ lua
--==============================================================================
--  Author  : SeaJey
--  Version : v0.1
--  License : Distributed under the terms of GNU GPL version 2 or later
--  This version is a modification of lunatico_rings.lua which is modification of conky_orange.lua 
--  conky_orange.lua:    http://gnome-look.org/content/show.php?content=137503&forumpage=0
--  lunatico_rings.lua:  http://gnome-look.org/content/show.php?content=142884
--==============================================================================

require 'cairo'


--------------------------------------------------------------------------------
--                                                                    gauge DATA
gauge = {
{
    name='cpu',                    arg='cpu0',                  max_value=100,
    x=70,                          y=130,
    graph_radius=30,
    graph_thickness=7,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=64,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu1',                  max_value=100,
    x=70,                          y=130,
    graph_radius=40,
    graph_thickness=7,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=40,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu2',                  max_value=100,
    x=70,                          y=130,
    graph_radius=55,
    graph_thickness=7,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=30,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu3',                  max_value=100,
    x=70,                          y=130,
    graph_radius=65,
    graph_thickness=7,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=4,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='memperc',                arg='',                      max_value=100,
    x=70,                          y=300,
    graph_radius=54,
    graph_thickness=10,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=42,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.5,
    caption='',
    caption_weight=1,              caption_size=10.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='fs_used_perc',           arg='/home/',                     max_value=100,
    x=70,                          y=470,
    graph_radius=52,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=32,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Home',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='fs_used_perc',           arg='/',                     max_value=100,
    x=70,                          y=470,
    graph_radius=39,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=22,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Root',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='downspeedf',           arg='wlan0',                     max_value=100,
    x=70,                          y=660,
    graph_radius=54,
    graph_thickness=7,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1,
    txt_radius=64,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Down',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='upspeedf',           arg='wlan0',                     max_value=100,
    x=70,                          y=660,
    graph_radius=42,
    graph_thickness=7,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1,
    txt_radius=30,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Up',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
}

-------------------------------------------------------------------------------
--                                                                 rgb_to_r_g_b
-- converts color in hexa to decimal
--
function rgb_to_r_g_b(colour, alpha)
    return ((colour / 0x10000) % 0x100) / 255., ((colour / 0x100) % 0x100) / 255., (colour % 0x100) / 255., alpha
end

-------------------------------------------------------------------------------
--                                                            angle_to_position
-- convert degree to rad and rotate (0 degree is top/north)
--
function angle_to_position(start_angle, current_angle)
    local pos = current_angle + start_angle
    return ( ( pos * (2 * math.pi / 360) ) - (math.pi / 2) )
end


-------------------------------------------------------------------------------
--                                                              draw_gauge_ring
-- displays gauges
--
function draw_gauge_ring(display, data, value)
    local max_value = data['max_value']
    local x, y = data['x'], data['y']
    local graph_radius = data['graph_radius']
    local graph_thickness, graph_unit_thickness = data['graph_thickness'], data['graph_unit_thickness']
    local graph_start_angle = data['graph_start_angle']
    local graph_unit_angle = data['graph_unit_angle']
    local graph_bg_colour, graph_bg_alpha = data['graph_bg_colour'], data['graph_bg_alpha']
    local graph_fg_colour, graph_fg_alpha = data['graph_fg_colour'], data['graph_fg_alpha']
    local hand_fg_colour, hand_fg_alpha = data['hand_fg_colour'], data['hand_fg_alpha']
    local graph_end_angle = 270

    -- background ring
    cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, 0), angle_to_position(graph_start_angle, graph_end_angle))
    cairo_set_source_rgba(display, rgb_to_r_g_b(graph_bg_colour, graph_bg_alpha))
    cairo_set_line_width(display, graph_thickness)
    cairo_stroke(display)

    -- arc of value
    local val = value % (max_value + 1)
    local start_arc = 0
    local stop_arc = 0
    local i = 1
    while i <= val do
        start_arc = (graph_unit_angle * i) - graph_unit_thickness
        stop_arc = (graph_unit_angle * i)
        cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
        cairo_set_source_rgba(display, rgb_to_r_g_b(graph_fg_colour, graph_fg_alpha))
        cairo_stroke(display)
        i = i + 1
    end
    local angle = start_arc

    -- hand
    start_arc = (graph_unit_angle * val) - (graph_unit_thickness * 2)
    stop_arc = (graph_unit_angle * val)
    cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
    cairo_set_source_rgba(display, rgb_to_r_g_b(hand_fg_colour, hand_fg_alpha))
    cairo_stroke(display)

    -- graduations marks
    local graduation_radius = data['graduation_radius']
    local graduation_thickness, graduation_mark_thickness = data['graduation_thickness'], data['graduation_mark_thickness']
    local graduation_unit_angle = data['graduation_unit_angle']
    local graduation_fg_colour, graduation_fg_alpha = data['graduation_fg_colour'], data['graduation_fg_alpha']
    if graduation_radius > 0 and graduation_thickness > 0 and graduation_unit_angle > 0 then
        local nb_graduation = graph_end_angle / graduation_unit_angle
        local i = 0
        while i < nb_graduation do
            cairo_set_line_width(display, graduation_thickness)
            start_arc = (graduation_unit_angle * i) - (graduation_mark_thickness / 2)
            stop_arc = (graduation_unit_angle * i) + (graduation_mark_thickness / 2)
            cairo_arc(display, x, y, graduation_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
            cairo_set_source_rgba(display,rgb_to_r_g_b(graduation_fg_colour,graduation_fg_alpha))
            cairo_stroke(display)
            cairo_set_line_width(display, graph_thickness)
            i = i + 1
        end
    end

    -- text
    local txt_radius = data['txt_radius']
    local txt_weight, txt_size = data['txt_weight'], data['txt_size']
    local txt_fg_colour, txt_fg_alpha = data['txt_fg_colour'], data['txt_fg_alpha']
    local movex = txt_radius * math.cos(angle_to_position(graph_start_angle, angle))
    local movey = txt_radius * math.sin(angle_to_position(graph_start_angle, angle))
    cairo_select_font_face (display, "ubuntu", CAIRO_FONT_SLANT_NORMAL, txt_weight)
    cairo_set_font_size (display, txt_size)
    cairo_set_source_rgba (display, rgb_to_r_g_b(txt_fg_colour, txt_fg_alpha))
    cairo_move_to (display, x + movex - (txt_size / 2), y + movey + 3)
    cairo_show_text (display, value)
    cairo_stroke (display)

    -- caption
    local caption = data['caption']
    local caption_weight, caption_size = data['caption_weight'], data['caption_size']
    local caption_fg_colour, caption_fg_alpha = data['caption_fg_colour'], data['caption_fg_alpha']
    local tox = graph_radius * (math.cos((graph_start_angle * 2 * math.pi / 360)-(math.pi/2)))
    local toy = graph_radius * (math.sin((graph_start_angle * 2 * math.pi / 360)-(math.pi/2)))
    cairo_select_font_face (display, "ubuntu", CAIRO_FONT_SLANT_NORMAL, caption_weight);
    cairo_set_font_size (display, caption_size)
    cairo_set_source_rgba (display, rgb_to_r_g_b(caption_fg_colour, caption_fg_alpha))
    cairo_move_to (display, x + tox + 5, y + toy + 1)
    -- bad hack but not enough time !
    if graph_start_angle < 105 then
        cairo_move_to (display, x + tox - 30, y + toy + 1)
    end
    cairo_show_text (display, caption)
    cairo_stroke (display)
end


-------------------------------------------------------------------------------
--                                                               go_gauge_rings
-- loads data and displays gauges
--
function go_gauge_rings(display)
    local function load_gauge_rings(display, data)
        local str, value = '', 0
        {% raw %}str = string.format('${%s %s}',data['name'], data['arg']){% endraw %}
        str = conky_parse(str)
        value = tonumber(str)
        draw_gauge_ring(display, data, value)
    end

    for i in pairs(gauge) do
        load_gauge_rings(display, gauge[i])
    end
end

-------------------------------------------------------------------------------
--                                                                         MAIN
function conky_main()
    if conky_window == nil then 
        return
    end

    local cs = cairo_xlib_surface_create(conky_window.display, conky_window.drawable, conky_window.visual, conky_window.width, conky_window.height)
    local display = cairo_create(cs)

    local updates = conky_parse('${updates}')
    update_num = tonumber(updates)

    if update_num > 5 then
        go_gauge_rings(display)
    end

    cairo_surface_destroy(cs)
    cairo_destroy(display)

end
~~~

PC running Zorin with Dual Monitor ​and NVIDIA GPU
6 core CPU
<div class="row">
    <div class="col-4 mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/conky3.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
.conkyrc config file with new format and call to Lua script.  
Remove xinerama_head 2 for single monitor.  
```console
conky.config = {
  alignment = 'top_right',
  xinerama_head = 2,
  background = false,
  border_width = 0.5,
  cpu_avg_samples = 2,
  color1 = 'DDDDDD',
  color2 = 'AAAAAA',
  color3 = '888888',
  color4 = 'EF5A29',
  color5 = '77B753',
  default_color = 'FFFFFF',
  default_outline_color = 'grey',
  default_shade_color = 'black',
  double_buffer = true,
  draw_borders = false,
  draw_graph_borders = true,
  draw_outline = false,
  draw_shades = false,
  use_xft = true,
  font = 'Arial:size=9',
  gap_x = 30,
  gap_y = 30,
  minimum_height = 5,
  minimum_width = 5,
  net_avg_samples = 2,
  no_buffers = true,
  out_to_console = false,
  out_to_stderr = false,
  extra_newline = false,
  own_window = true,
  own_window_colour = '000000',
  own_window_class = 'Conky',
  own_window_argb_visual = true,
  own_window_type = 'desktop',
  own_window_transparent = true,
  stippled_borders = 0,
  update_interval = 1,
  uppercase = false,
  use_spacer = 'none',
  show_graph_scale = false,
  show_graph_range = false,

  lua_load = '~/.conky/seamod_rings.lua',
  lua_draw_hook_post = 'main',

}

conky.text = [[
${font Arial:bold:size=10}${color4}SYSTEM ${hr 2}
${offset 15}$font${color3}$sysname $kernel $machine
# ${offset 15}$font${color3}$nodename
${offset 15}$font${color3}Uptime: $uptime $alignr ${acpitemp}°C
${offset 15}$font${color3}Frequency $alignr${freq_g cpu0}Ghz

# CPU  info Graph
${voffset 40}
# ${offset 145} ${cpugraph 40,183 666666 666666}${voffset -25}
${offset 90}${font Arial:bold:size=10}${color5}CPU
# List Top 5  CPU process
${offset 75}$font${color4}${top name 1}
${offset 75}$font${color2}${top name 2}
${offset 75}$font${color2}${top name 3}
${offset 75}$font${color3}${top name 4}
${offset 75}$font${color3}${top name 5}

#List  Memory Top 5
${voffset 55}
${offset 90}${font Arial:bold:size=10}${color5}MEM
${offset 75}$font${color4}${top_mem name 1}${alignr}${top mem 1}
${offset 75}$font${color2}${top_mem name 2}${alignr}${top mem 2}
${offset 75}$font${color2}${top_mem name 3}${alignr}${top mem 3}
${offset 75}$font${color3}${top_mem name 4}${alignr}${top mem 4}
${offset 75}$font${color3}${top_mem name 4}${alignr}${top mem 5}

# Disk info disk partitions: root, home
${voffset 55}
${offset 90}${font Arial:bold:size=10}${color5}DISKS
# ${offset 120}${diskiograph 33,183 666666 666666}${voffset -30}
${voffset 25}
${offset 75}$font${color2}root(free) ${alignr}$font${fs_free /}
${offset 75}$font${color2}root(used) ${alignr}$font${fs_used /}
${offset 75}$font${color2}home(free) ${alignr}$font${fs_free /home}
${offset 75}$font${color2}home(used) ${alignr}$font${fs_used /home}

# GPU 
${voffset 45}
${offset 90}${font Arial:bold:size=10}${color5}GPU
${voffset 40}             

# ${color2}GPU Temp: ${alignr}${color0}${nvidia temp} °C
# ${color2}Fan Speed: ${alignr}${color0}${execi 5 nvidia-settings -q [fan:0]/GPUCurrentFanSpeed -t} %
# ${color2}GPU Clock: ${alignr}${color0}${nvidia gpufreq} MHz
# ${color2}Mem Clock: ${alignr}${color0}${nvidia memfreq} MHz
# ${color2}Mem Used: ${alignr}${color0}${execi 5 nvidia-settings -q [gpu:0]/UsedDedicatedGPUMemory -t} / ${exec nvidia-settings -q [gpu:0]/TotalDedicatedGPUMemory -t} MiB
# ${color4}${hr 2}
]]
```
Lua script gauges 6 core CPU, NVIDIA GPU
/home/USER/.conky/seamod_rings.lua  
~~~ lua
--==============================================================================
--  Date    : 05/02/2012
--  Author  : SeaJey
--  Version : v0.1
--  License : Distributed under the terms of GNU GPL version 2 or later
--  This version is a modification of lunatico_rings.lua wich is modification of conky_orange.lua 
--  conky_orange.lua:    http://gnome-look.org/content/show.php?content=137503&forumpage=0
--  lunatico_rings.lua:  http://gnome-look.org/content/show.php?content=142884
--==============================================================================

require 'cairo'


--------------------------------------------------------------------------------
--                                                                    gauge DATA
gauge = {
{
    name='cpu',                    arg='cpu0',                  max_value=100,
    x=70,                          y=130,
    graph_radius=14,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=64,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu1',                  max_value=100,
    x=70,                          y=130,
    graph_radius=20,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=40,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu2',                  max_value=100,
    x=70,                          y=130,
    graph_radius=40,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=30,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu3',                  max_value=100,
    x=70,                          y=130,
    graph_radius=46,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=4,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu4',                  max_value=100,
    x=70,                          y=130,
    graph_radius=60,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=30,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu5',                  max_value=100,
    x=70,                          y=130,
    graph_radius=66,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=4,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='memperc',                arg='',                      max_value=100,
    x=70,                          y=300,
    graph_radius=54,
    graph_thickness=10,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=42,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.5,
    caption='',
    caption_weight=1,              caption_size=10.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='fs_used_perc',           arg='/home/',                     max_value=100,
    x=70,                          y=470,
    graph_radius=52,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=32,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Home',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='fs_used_perc',           arg='/',                     max_value=100,
    x=70,                          y=470,
    graph_radius=39,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=22,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Root',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='nvidia',           arg='gpufreq',                     max_value=2000,
    x=70,                          y=660,
    graph_radius=52,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=0.135,        graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.035,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=64,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='GPU MHz',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='nvidia',           arg='temp',                     max_value=100,
    x=70,                          y=660,
    graph_radius=39,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=30,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Temp °C',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
}

-------------------------------------------------------------------------------
--                                                                 rgb_to_r_g_b
-- converts color in hexa to decimal
--
function rgb_to_r_g_b(colour, alpha)
    return ((colour / 0x10000) % 0x100) / 255., ((colour / 0x100) % 0x100) / 255., (colour % 0x100) / 255., alpha
end

-------------------------------------------------------------------------------
--                                                            angle_to_position
-- convert degree to rad and rotate (0 degree is top/north)
--
function angle_to_position(start_angle, current_angle)
    local pos = current_angle + start_angle
    return ( ( pos * (2 * math.pi / 360) ) - (math.pi / 2) )
end


-------------------------------------------------------------------------------
--                                                              draw_gauge_ring
-- displays gauges
--
function draw_gauge_ring(display, data, value)
    local max_value = data['max_value']
    local x, y = data['x'], data['y']
    local graph_radius = data['graph_radius']
    local graph_thickness, graph_unit_thickness = data['graph_thickness'], data['graph_unit_thickness']
    local graph_start_angle = data['graph_start_angle']
    local graph_unit_angle = data['graph_unit_angle']
    local graph_bg_colour, graph_bg_alpha = data['graph_bg_colour'], data['graph_bg_alpha']
    local graph_fg_colour, graph_fg_alpha = data['graph_fg_colour'], data['graph_fg_alpha']
    local hand_fg_colour, hand_fg_alpha = data['hand_fg_colour'], data['hand_fg_alpha']
    local graph_end_angle = 270

    -- background ring
    cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, 0), angle_to_position(graph_start_angle, graph_end_angle))
    cairo_set_source_rgba(display, rgb_to_r_g_b(graph_bg_colour, graph_bg_alpha))
    cairo_set_line_width(display, graph_thickness)
    cairo_stroke(display)

    -- arc of value
    local val = value % (max_value + 1)
    local start_arc = 0
    local stop_arc = 0
    local i = 1
    while i <= val do
        start_arc = (graph_unit_angle * i) - graph_unit_thickness
        stop_arc = (graph_unit_angle * i)
        cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
        cairo_set_source_rgba(display, rgb_to_r_g_b(graph_fg_colour, graph_fg_alpha))
        cairo_stroke(display)
        i = i + 1
    end
    local angle = start_arc

    -- hand
    start_arc = (graph_unit_angle * val) - (graph_unit_thickness * 2)
    stop_arc = (graph_unit_angle * val)
    cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
    cairo_set_source_rgba(display, rgb_to_r_g_b(hand_fg_colour, hand_fg_alpha))
    cairo_stroke(display)

    -- graduations marks
    local graduation_radius = data['graduation_radius']
    local graduation_thickness, graduation_mark_thickness = data['graduation_thickness'], data['graduation_mark_thickness']
    local graduation_unit_angle = data['graduation_unit_angle']
    local graduation_fg_colour, graduation_fg_alpha = data['graduation_fg_colour'], data['graduation_fg_alpha']
    if graduation_radius > 0 and graduation_thickness > 0 and graduation_unit_angle > 0 then
        local nb_graduation = graph_end_angle / graduation_unit_angle
        local i = 0
        while i < nb_graduation do
            cairo_set_line_width(display, graduation_thickness)
            start_arc = (graduation_unit_angle * i) - (graduation_mark_thickness / 2)
            stop_arc = (graduation_unit_angle * i) + (graduation_mark_thickness / 2)
            cairo_arc(display, x, y, graduation_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
            cairo_set_source_rgba(display,rgb_to_r_g_b(graduation_fg_colour,graduation_fg_alpha))
            cairo_stroke(display)
            cairo_set_line_width(display, graph_thickness)
            i = i + 1
        end
    end

    -- text
    local txt_radius = data['txt_radius']
    local txt_weight, txt_size = data['txt_weight'], data['txt_size']
    local txt_fg_colour, txt_fg_alpha = data['txt_fg_colour'], data['txt_fg_alpha']
    local movex = txt_radius * math.cos(angle_to_position(graph_start_angle, angle))
    local movey = txt_radius * math.sin(angle_to_position(graph_start_angle, angle))
    cairo_select_font_face (display, "ubuntu", CAIRO_FONT_SLANT_NORMAL, txt_weight)
    cairo_set_font_size (display, txt_size)
    cairo_set_source_rgba (display, rgb_to_r_g_b(txt_fg_colour, txt_fg_alpha))
    cairo_move_to (display, x + movex - (txt_size / 2), y + movey + 3)
    cairo_show_text (display, value)
    cairo_stroke (display)

    -- caption
    local caption = data['caption']
    local caption_weight, caption_size = data['caption_weight'], data['caption_size']
    local caption_fg_colour, caption_fg_alpha = data['caption_fg_colour'], data['caption_fg_alpha']
    local tox = graph_radius * (math.cos((graph_start_angle * 2 * math.pi / 360)-(math.pi/2)))
    local toy = graph_radius * (math.sin((graph_start_angle * 2 * math.pi / 360)-(math.pi/2)))
    cairo_select_font_face (display, "ubuntu", CAIRO_FONT_SLANT_NORMAL, caption_weight);
    cairo_set_font_size (display, caption_size)
    cairo_set_source_rgba (display, rgb_to_r_g_b(caption_fg_colour, caption_fg_alpha))
    cairo_move_to (display, x + tox + 5, y + toy + 1)
    -- bad hack but not enough time !
    if graph_start_angle < 105 then
        cairo_move_to (display, x + tox - 30, y + toy + 1)
    end
    cairo_show_text (display, caption)
    cairo_stroke (display)
end


-------------------------------------------------------------------------------
--                                                               go_gauge_rings
-- loads data and displays gauges
--
function go_gauge_rings(display)
    local function load_gauge_rings(display, data)
        local str, value = '', 0
        {% raw %}str = string.format('${%s %s}',data['name'], data['arg']){% endraw %}
        str = conky_parse(str)
        value = tonumber(str)
        draw_gauge_ring(display, data, value)
    end

    for i in pairs(gauge) do
        load_gauge_rings(display, gauge[i])
    end
end

-------------------------------------------------------------------------------
--                                                                         MAIN
function conky_main()
    if conky_window == nil then 
        return
    end

    local cs = cairo_xlib_surface_create(conky_window.display, conky_window.drawable, conky_window.visual, conky_window.width, conky_window.height)
    local display = cairo_create(cs)

    local updates = conky_parse('${updates}')
    update_num = tonumber(updates)

    if update_num > 5 then
        go_gauge_rings(display)
    end

    cairo_surface_destroy(cs)
    cairo_destroy(display)

end
~~~

Lua script 8 core CPU ​and NVIDIA GPU
/home/USER/.conky/seamod_rings.lua  
~~~ lua
--==============================================================================
--  Date    : 05/02/2012
--  Author  : SeaJey
--  Version : v0.1
--  License : Distributed under the terms of GNU GPL version 2 or later
--  This version is a modification of lunatico_rings.lua wich is modification of conky_orange.lua   
--  conky_orange.lua:    http://gnome-look.org/content/show.php?content=137503&forumpage=0
--  lunatico_rings.lua:  http://gnome-look.org/content/show.php?content=142884
--==============================================================================

require 'cairo'


--------------------------------------------------------------------------------
--                                                                    gauge DATA
gauge = {
{
    name='cpu',                    arg='cpu0',                  max_value=100,
    x=70,                          y=130,
    graph_radius=7,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x1A1A1A,      graph_bg_alpha=0.1,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.3,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=64,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu1',                  max_value=100,
    x=70,                          y=130,
    graph_radius=14,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x1A1A1A,      graph_bg_alpha=0.1,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.3,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=40,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu2',                  max_value=100,
    x=70,                          y=130,
    graph_radius=25,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x1A1A1A,      graph_bg_alpha=0.1,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.3,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=30,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu3',                  max_value=100,
    x=70,                          y=130,
    graph_radius=32,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x1A1A1A,      graph_bg_alpha=0.1,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.3,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=4,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu4',                  max_value=100,
    x=70,                          y=130,
    graph_radius=43,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x1A1A1A,      graph_bg_alpha=0.1,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.3,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=64,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu5',                  max_value=100,
    x=70,                          y=130,
    graph_radius=50,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x1A1A1A,      graph_bg_alpha=0.1,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.3,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=40,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu6',                  max_value=100,
    x=70,                          y=130,
    graph_radius=61,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x1A1A1A,      graph_bg_alpha=0.1,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.3,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=30,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='cpu',                    arg='cpu7',                  max_value=100,
    x=70,                          y=130,
    graph_radius=68,
    graph_thickness=5,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x1A1A1A,      graph_bg_alpha=0.1,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.3,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=4,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='',
    caption_weight=1,              caption_size=9.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='memperc',                arg='',                      max_value=100,
    x=70,                          y=300,
    graph_radius=54,
    graph_thickness=10,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=42,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.5,
    caption='',
    caption_weight=1,              caption_size=10.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.3,
},
{
    name='fs_used_perc',           arg='/mnt/GoogleDrive/',                     max_value=100,
    x=70,                          y=470,
    graph_radius=52,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=32,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Share',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='fs_used_perc',           arg='/',                     max_value=100,
    x=70,                          y=470,
    graph_radius=39,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=22,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=23,
    graduation_thickness=0,        graduation_mark_thickness=2,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Root',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='nvidia',           arg='gpufreq',                     max_value=2000,
    x=70,                          y=660,
    graph_radius=52,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=0.135,        graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.035,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=64,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='GPU MHz',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
{
    name='nvidia',           arg='temp',                     max_value=100,
    x=70,                          y=660,
    graph_radius=39,
    graph_thickness=9,
    graph_start_angle=180,
    graph_unit_angle=2.7,          graph_unit_thickness=2.7,
    graph_bg_colour=0x7a7a7a,      graph_bg_alpha=0.3,
    graph_fg_colour=0xFFFFFF,      graph_fg_alpha=0.35,
    hand_fg_colour=0xEF5A29,       hand_fg_alpha=1.0,
    txt_radius=30,
    txt_weight=0,                  txt_size=9.0,
    txt_fg_colour=0xEF5A29,        txt_fg_alpha=1.0,
    graduation_radius=28,
    graduation_thickness=0,        graduation_mark_thickness=1,
    graduation_unit_angle=27,
    graduation_fg_colour=0xFFFFFF, graduation_fg_alpha=0.3,
    caption='Temp °C',
    caption_weight=1,              caption_size=12.0,
    caption_fg_colour=0xFFFFFF,    caption_fg_alpha=0.5,
},
}

-------------------------------------------------------------------------------
--                                                                 rgb_to_r_g_b
-- converts color in hexa to decimal
--
function rgb_to_r_g_b(colour, alpha)
    return ((colour / 0x10000) % 0x100) / 255., ((colour / 0x100) % 0x100) / 255., (colour % 0x100) / 255., alpha
end

-------------------------------------------------------------------------------
--                                                            angle_to_position
-- convert degree to rad and rotate (0 degree is top/north)
--
function angle_to_position(start_angle, current_angle)
    local pos = current_angle + start_angle
    return ( ( pos * (2 * math.pi / 360) ) - (math.pi / 2) )
end


-------------------------------------------------------------------------------
--                                                              draw_gauge_ring
-- displays gauges
--
function draw_gauge_ring(display, data, value)
    local max_value = data['max_value']
    local x, y = data['x'], data['y']
    local graph_radius = data['graph_radius']
    local graph_thickness, graph_unit_thickness = data['graph_thickness'], data['graph_unit_thickness']
    local graph_start_angle = data['graph_start_angle']
    local graph_unit_angle = data['graph_unit_angle']
    local graph_bg_colour, graph_bg_alpha = data['graph_bg_colour'], data['graph_bg_alpha']
    local graph_fg_colour, graph_fg_alpha = data['graph_fg_colour'], data['graph_fg_alpha']
    local hand_fg_colour, hand_fg_alpha = data['hand_fg_colour'], data['hand_fg_alpha']
    local graph_end_angle = 270

    -- background ring
    cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, 0), angle_to_position(graph_start_angle, graph_end_angle))
    cairo_set_source_rgba(display, rgb_to_r_g_b(graph_bg_colour, graph_bg_alpha))
    cairo_set_line_width(display, graph_thickness)
    cairo_stroke(display)

    -- arc of value
    local val = value % (max_value + 1)
    local start_arc = 0
    local stop_arc = 0
    local i = 1
    while i <= val do
        start_arc = (graph_unit_angle * i) - graph_unit_thickness
        stop_arc = (graph_unit_angle * i)
        cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
        cairo_set_source_rgba(display, rgb_to_r_g_b(graph_fg_colour, graph_fg_alpha))
        cairo_stroke(display)
        i = i + 1
    end
    local angle = start_arc

    -- hand
    start_arc = (graph_unit_angle * val) - (graph_unit_thickness * 2)
    stop_arc = (graph_unit_angle * val)
    cairo_arc(display, x, y, graph_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
    cairo_set_source_rgba(display, rgb_to_r_g_b(hand_fg_colour, hand_fg_alpha))
    cairo_stroke(display)

    -- graduations marks
    local graduation_radius = data['graduation_radius']
    local graduation_thickness, graduation_mark_thickness = data['graduation_thickness'], data['graduation_mark_thickness']
    local graduation_unit_angle = data['graduation_unit_angle']
    local graduation_fg_colour, graduation_fg_alpha = data['graduation_fg_colour'], data['graduation_fg_alpha']
    if graduation_radius > 0 and graduation_thickness > 0 and graduation_unit_angle > 0 then
        local nb_graduation = graph_end_angle / graduation_unit_angle
        local i = 0
        while i < nb_graduation do
            cairo_set_line_width(display, graduation_thickness)
            start_arc = (graduation_unit_angle * i) - (graduation_mark_thickness / 2)
            stop_arc = (graduation_unit_angle * i) + (graduation_mark_thickness / 2)
            cairo_arc(display, x, y, graduation_radius, angle_to_position(graph_start_angle, start_arc), angle_to_position(graph_start_angle, stop_arc))
            cairo_set_source_rgba(display,rgb_to_r_g_b(graduation_fg_colour,graduation_fg_alpha))
            cairo_stroke(display)
            cairo_set_line_width(display, graph_thickness)
            i = i + 1
        end
    end

    -- text
    local txt_radius = data['txt_radius']
    local txt_weight, txt_size = data['txt_weight'], data['txt_size']
    local txt_fg_colour, txt_fg_alpha = data['txt_fg_colour'], data['txt_fg_alpha']
    local movex = txt_radius * math.cos(angle_to_position(graph_start_angle, angle))
    local movey = txt_radius * math.sin(angle_to_position(graph_start_angle, angle))
    cairo_select_font_face (display, "ubuntu", CAIRO_FONT_SLANT_NORMAL, txt_weight)
    cairo_set_font_size (display, txt_size)
    cairo_set_source_rgba (display, rgb_to_r_g_b(txt_fg_colour, txt_fg_alpha))
    cairo_move_to (display, x + movex - (txt_size / 2), y + movey + 3)
    cairo_show_text (display, value)
    cairo_stroke (display)

    -- caption
    local caption = data['caption']
    local caption_weight, caption_size = data['caption_weight'], data['caption_size']
    local caption_fg_colour, caption_fg_alpha = data['caption_fg_colour'], data['caption_fg_alpha']
    local tox = graph_radius * (math.cos((graph_start_angle * 2 * math.pi / 360)-(math.pi/2)))
    local toy = graph_radius * (math.sin((graph_start_angle * 2 * math.pi / 360)-(math.pi/2)))
    cairo_select_font_face (display, "ubuntu", CAIRO_FONT_SLANT_NORMAL, caption_weight);
    cairo_set_font_size (display, caption_size)
    cairo_set_source_rgba (display, rgb_to_r_g_b(caption_fg_colour, caption_fg_alpha))
    cairo_move_to (display, x + tox + 5, y + toy + 1)
    -- bad hack but not enough time !
    if graph_start_angle < 105 then
        cairo_move_to (display, x + tox - 30, y + toy + 1)
    end
    cairo_show_text (display, caption)
    cairo_stroke (display)
end


-------------------------------------------------------------------------------
--                                                               go_gauge_rings
-- loads data and displays gauges
--
function go_gauge_rings(display)
    local function load_gauge_rings(display, data)
        local str, value = '', 0
        {% raw %}str = string.format('${%s %s}',data['name'], data['arg']){% endraw %}
        str = conky_parse(str)
        value = tonumber(str)
        draw_gauge_ring(display, data, value)
    end

    for i in pairs(gauge) do
        load_gauge_rings(display, gauge[i])
    end
end

-------------------------------------------------------------------------------
--                                                                         MAIN
function conky_main()
    if conky_window == nil then 
        return
    end

    local cs = cairo_xlib_surface_create(conky_window.display, conky_window.drawable, conky_window.visual, conky_window.width, conky_window.height)
    local display = cairo_create(cs)

    local updates = conky_parse('${updates}')
    update_num = tonumber(updates)

    if update_num > 5 then
        go_gauge_rings(display)
    end

    cairo_surface_destroy(cs)
    cairo_destroy(display)

end
~~~

Conky documentation  
​http://conky.sourceforge.net/config_settings.html  
http://conky.sourceforge.net/variables.html  
https://wiki.archlinux.org/index.php/conky  

# Installing New DE
## RPi OS (full version) with MATE 
* (got help from [sirozha](https://forums.raspberrypi.com/viewtopic.php?t=260974) on forum)
* Used RPi Imager to install full version of RPi OS
* Then added MATE desktop with
* ```$ sudo apt-get install mate-core mate-desktop-environment```  
* Next installed
* ```$ sudo update-alternatives --config x-session-manager```  
* Selected the "Mate" Desktop Environment and reboot.
* After booting getting the network setup could be the trickiest part. The system will still be using dhcpcd (you can confirm with ```$ systemctl status dhcpcd```). To let network manager take over install network-manager and disable dhcpcd with following commands
* ```$ sudo apt install network-manager-gnome```  
* Then stop the dhcpcd
* ```$ sudo systemctl disable dhcpcd```  
* ```$ sudo /etc/init.d/dhcpcd stop```  
* Reboot and setup your wifi
* cli for updating wifi for network manager
* To see available connections $ nmcli dev wifi
* To change ```$ sudo nmcli dev wifi con "SSID" password "pswd" name "SSID"```  
* You can do additional setup with
* ```$ sudo raspi-config```  
* Interface Options - Enable ssh and vnc
* Performance Options - Increase GPU memory to 128 (or 256)
* Localisation Options - WLAN Country set to US and Keyboard (enable Compose for special characters)
* Advanced Options - GL Driver, enable Full KMS
* reboot
* Confirm wifi locale with $ sudo iw reg get
* If not US set it with
* ```$ sudo iw reg set US```  
* Set crda
* ```$ sudo nano /etc/default/crda (REGDOMAIN=US)```  
* Reboot
* Added mate-tweak ```$ sudo apt install mate-tweak```  
* Ran mate-tweak and changed to Fedora panels
* Switch to marco compositor.
* Used Preferences, Look and Feel, Appearances to change themes and icons.

## ​Ubuntu with MATE
* Download is at [ubuntu-mate.org](https://ubuntu-mate.org/raspberry-pi/)
* Used balena etcher to burn to micro SD card, insert into RPi, turn on and follow setup instructions.
* Installed mate-tweak ```$ sudo apt install mate-tweak```  
* Use Preferences, Look and Feel, Appearances to change themes and icons.
* Added [x11vnc](../../../ref/linux/vnc-ssh/) for vnc connection
​
## ​Ubuntu with GNOME3
* Used RPi Imager for Ubuntu desktop
* Added tweaks ```$ sudo apt install gnome-tweaks```  
* Can update appearance in tweaks
* Activated vino vnc server in "settings" and "sharing" but disabled encryption so I can use RealVNC to connect (otherwise have to use remmina/vinagre)
* Disable encryption ```$ gsettings set org.gnome.Vino require-encryption false```   

# Examples of New Themes
## RPi OS/LXDE Adwaita Theme/Icons
(1.0 GB RAM used)  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/theme1.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## RPi OS/LXDE Adwaita Theme Yaru Icons Cairo dock
(1.0 GB RAM used)  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/theme2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Ubuntu (20.1)/MATE with Ambiant-MATE Theme/Icons
(2+ GB RAM used)  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/theme3.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Ubuntu (20.1)/GNOME3 with Adwaita Theme, Yaru Icons
(2+ GB RAM used)  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/theme4.gif" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

-----------------------------  
-----------------------------  