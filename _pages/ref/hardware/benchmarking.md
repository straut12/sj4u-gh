---
layout: page
permalink: /ref/hardware/benchmarking/
title: Benchmarking
description: Intro to benchmarking
nav: false
toc: true
---
# Benchmarking Programs
Benchmarking is not necessary for any projects. But it can be informative and you'll learn a lot. Here are some options for playing around with it. I didn't spend a lot of time in this area so I don't have detailed notes. There is enough information you can google and find more though.
* sysbench (installed thru synaptic) Quick test (geared for mysql, db systems)
* phoronix-test-suite (download .deb and install gdebi) Can compare across Raspberry Pi, WIndows, Linux
* hdparm (installed thru synaptic) test is sequential (better to have test for random access)​

# RPi Hardware
SYSTEM (all components)
* CPU -  My Pi4 is over-clocked to 2.1 GHz,  Pi3's go up to 1.4GHz
* Memory - Pi4=LPDDR4, Pi3=DDR2
* Disk (microSD) older UHS1 and class 10 cards should have min write speed of 10MB/s (the card may state a higher, 'up to' speed on the package, but that doesn't mean it's what you will get). The Pi4 has a faster microSD reader (50MBps vs 25 MBps, hdparm). Running a benchmark and comparing cards (sequential and random read/write speeds) will help quantify the difference. But in general even cheaper class 10 cards have been sufficient for me on Raspberry Pi's. It all depends on what your expectations are.
* Network - Pi3 ethernet is slowest 10/100.  Pi3B+ is faster Gigabit (over USB 2) but still slower than the Pi4 Gigabit.

# Phoronix-test-suite
## RPi/Ubuntu
* Download/Install from .deb or installation from github
* Install from github - php requirements
* ```$ sudo apt install php7.3-cli php7.3-xml php7.3-gd php7.3-bz2 php7.3-sqlite3 php7.3-curl```
* Install pts from github
* ```$ cd /opt```
* ```$ wget https://github.com/phoronix-test-suite/phoronix-test-suite/releases/download/........tar.gz```
* ```$ gunzip phoronix-test-suite-xxxx.tar.gz```
* ```$ tar xvf phoronix-test-suite-xxxx.tar```
* ```$ cd phoronix-test-suite```
* ```$ ./install-sh```
Phoronix-test-suite icon is installed under 'System Tools'. Or you can type ```$ phoronix-test-suite shell ```in a terminal.

## RPi related tests
* 1907120-HV-RPIMONITO11 is a test I used to compare RPis
* 2007316-NE-RPINTEL8536 recent Pi4 to Intel test
* 1907127-HV-RASPBERRY81 (takes 3-4 hrs to run on Pi4)​

## Windows 10
Note - This is what I did to to get pts to work on windows 10. I wanted to be able to compare to my windows PC, too. There's also info in PTS website under Notes-General for PHP. Phoronix-test-suite-docs
* install cygwin for terminal commands https://www.cygwin.com/
* download PTS from github. Click on 'code' and download the ZIP.
* extract to c:\ drive
* Use cygwin terminal to go to the phoronix dir. For me -> ```$ cd /cygdrive/c/phoronix-test-suite-10.0.1```
* Run .bat file inside phoronix dir  ```$ ./phoronix-test-suite.bat```
* The .bat file also downloaded php and extracted to C:\PHP but it didn't seem to update env variables because I kept getting an error about php not installed when running phoronix-test-suite
* to fix PHP --> go to C:\PHP\ and copy php.ini-production to php.ini (needs php.ini file)
* php.ini file extensions update --> remove semi colon and update 'extension_dir="C:\PHP\ext" You're telling it where the extensions are located.
* more php.ini extensions update --> remove semi colon from extensions that are useful.  I did openssl, curl, gd2, mbstring, mysql, pdo_mysql, xmlrpc, bz2, sockets, sqlite3
* Add C:\php to path environment variable in windows.  In windows settings search for 'environment variables', then 'Advanced' tab, 'Environment Variables' button, 'edit text', add C:\PHP and C:\PHP\ext (make sure ; is between them)
* reboot
I still  had a problem with the phoronix-test-suite defining the path ($PTS_DIR) for the .php files. My computer didn't like how the path was defined. I had to modify the phoronix-test-suite file (I use notepad++). At the bottom of the file I removed the $PTS_DIR and replaced with '.' for current directory.
What my line looks like after updating -> ```$PTS_PREPEND_LAUNCH $PHP_BIN ./pts-core/phoronix-test-suite.php $@```
Now I could run ```$ ./phoronix-test-suite interactive```
I used the interactive menu until I got comfortable with some of the commands.​
Phoronix-test-suite Usage
I used the interactive option to get used to the commands
Benchmark gives you the option to upload to https://openbenchmarking.org/
From https://openbenchmarking.org/ you can compare to other tests you've ran on other systems, compare to other results people have uploaded, browse other test suites, etc.

## System monitor 
options (get plots for temp, freq, etc)
To see what sensors are supported ```$ phoronix-test-suite system-sensors```
For my Raspberry Pi 4
* cpu.freq 
* cpu.peak-freq
* cpu.usage
* hdd.read-speed Drive Read Speed (mmcblk0): 0.00 MB/s
* hdd.write-speed Drive Write Speed (mmcblk0): 0.02 MB/s
* memory.usage Memory Usage: 228 Megabytes
* sys.temp System Temperature: 43.3 Celsius 
* MONITOR=cpu.freq,cpu.peak-freq,cpu.usage,hdd.read-speed,hdd.write-speed,memory.usage,sys.temp

Other monitor options
* MONITOR=cpu.usage,gpu.temp,cpu.temp,gpu.usage, sys.temp
* MONITOR=all.usage (didn't work on my Pi4)
* TEST_TIMEOUT_AFTER= (number of minutes)

You put the options before the pts command.
example ```$ MONITOR=sys.temp phoronix-test-suite benchmark c-ray```
Ran quick test with
```$ MONITOR=cpu.peak-freq,cpu.usage,hdd.read-speed,hdd.write-speed,memory.usage,sys.temp phoronix-test-suite benchmark c-ray```

Some options from phoronix forum (from timtaw)
Fast screening - timtaw/screening-live-superfast
Light screening - timtaw/screening-live-fast

## Processor
Processor (single-threaded): SciMark (Computational Test: Fast Fourier Transform): 3 minutes
Processor (multi-threaded): Himeno Benchmark: 3 minutes, C-Ray: 6 minutes, Smallpt: 4 minutes

## Disk
Flexible IO Tester (Type: Random Read; IO Engine: POSIX AIO; Buffered: No; Direct: Yes; Block Size: 4 KB): 6 minutes
PostMark: 17 minutes
iozone (4k block size and 100MB file)
aio-stress (a-synhronous I/O)

## Memory
RAMspeed SMP (Type: Add; Benchmark: Integer): 6 minutes
CacheBench (Test: Test All Options): 20 minutes
Stream (Type: Test All Options): 41 minutes

## Network
Loopback TCP Network Performance: 3 minutes
iperf

## Web Browsing
octane

System: Hierarchical INTegration (Test: Test All Options): 51 minutes

# Sysbench
## CPU test
with prime number 20000 (phoronix test 200,000,000)
$ sysbench --test=cpu --cpu-max-prime=20000 --num-threads=4 run
look at total time

## I/O test  
1st creates test file larger than RAM, runs tests, then deletes/cleans up test file  
```$ sysbench --test=fileio --file-total-size=2G prepare
$ sysbench --test=fileio --file-total-size=2G --file-test-mode=rndrw --init-rng=on --max-time=300 --max-requests=0 run
$ sysbench --test=fileio --file-total-size=2G cleanup  ```

## Memory
```$ sysbench --test=memory run --memory-total-size=2G```  

```$ sysbench --test=memory run --memory-total-size=2G --memory-oper=read```  

look at kb/sec

## USB and SD Card Speed
```$ sudo hdparm -tT --direct /dev/sda1``` (USB drive)  

```$ sudo hdparm -tT --direct /dev/mmcblk0``` (sd card)

# Video Benchmarking (fps)
​The RPi3 does an ok job at playing video and RPi4 is even better.  But there is significant drop off in comparison to a PC/laptop. To compare performance between RPis download the raspbian version of geexlab. Unzip and then click on the executable file. Drag and drop different .xml files to see the fps.

You can also play Youtube videos and look at the details for frames dropped and fps.

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