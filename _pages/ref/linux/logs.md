---
layout: page
permalink: /ref/linux/logs/
title: logs
description: Intro to evaluating logs
nav: false
toc: true
---
# Options For Viewing Logs

# cat
There are a couple quick options for looking at logs.  
* ```$ dmesg``` (contents of the kernel ring buffer. Also sent to syslog.)
* ```$ cat /var/log/syslog```  (can always ```$ cat /var/log/syslog | grep 'keyword'```)  
on Rpi  
* ```$ cat /var/log/messages```
​
​Another option with more functionality is journald

# journalctl
Some quick journalctl commands to get started  

* ```$ journalctl -b ``` (current boot)
* ```$ journalctl -b -1 ``` (previous boot)  
To list the boots of the system  
* ```$ journalctl --list-boots```  
* ```$ journalctl --since "1 hour ago"```  
* ```$ journalctl --since "7 days ago"```  
* ```$ journalctl --since "2019-01-01 00:00:00" --until "2019-02-01 00:00:00"```  

By services (can add options at the end. "-r" to reverse sort, "-o json-pretty" format)  
* ```$ journalctl -u ssh.service -u bluetooth.service```  

To follow add 'f  
* ```$ journalctl -u bluetooth.service -f ```  

To send to a file  
* ```$ journalctl > dump.log```  

Examples  
* ```$ journalctl --unit=systemd-networkd```  
* ```$ journalctl --unit=wpa_supplicant```  

# Python Logging (including journal)
Logging libraries give useful functions in your python script for logging (info, debugging, errors). And you can add a journal option to output to the journal.​  

Couple approaches I use  
1. **basicConfig()**. Quick, simple logging with basicConfig(). With basicConfig() you can configure the root logger but you can not configure custom loggers. Have to use handlers and formatters.
2. **RotatingFileHandler/JournalHandler**. When you want to output to multiple places (console/standard output stream, a log file, and the journal) it is better to use Handlers. RotatingFileHandler lets you specify the max size of the log and enables rolling log files.  

## RotatingFileHandler
Using RotatingFileHandler for logging will create 3 loggers in this example for three outputs
1. console
2. regular log
3. error log  

```python
#!/usr/bin/env python3
import logging, sys, os
from logging.handlers import RotatingFileHandler

def setup_logging(log_dir):
    # Main logger
    #main_logger = logging.getLogger()
    main_logger = logging.getLogger(__name__)
    main_logger.setLevel(logging.INFO)
    log_file_format = logging.Formatter("[%(levelname)s] - %(asctime)s - %(name)s - : %(message)s in %(pathname)s:%(lineno)d")
    log_console_format = logging.Formatter("[%(levelname)s]: %(message)s")

    console_handler = logging.StreamHandler()
    console_handler.setLevel(logging.INFO)
    console_handler.setFormatter(log_console_format)

    exp_file_handler = RotatingFileHandler('{}/exp_debug.log'.format(log_dir), maxBytes=10**6, backupCount=5) # 10MB file
    exp_file_handler.setLevel(logging.DEBUG)
    exp_file_handler.setFormatter(log_file_format)

    exp_errors_file_handler = RotatingFileHandler('{}/exp_error.log'.format(log_dir), maxBytes=10**6, backupCount=5)
    exp_errors_file_handler.setLevel(logging.WARNING)
    exp_errors_file_handler.setFormatter(log_file_format)

    main_logger.addHandler(console_handler)
    main_logger.addHandler(exp_file_handler)
    main_logger.addHandler(exp_errors_file_handler)
    return main_logger

if __name__ == "__main__":
    main_logger = setup_logging(os.path.dirname(os.path.abspath(__file__)))
    main_logger.info("setup logging module")
    mylist = [1, 2]
    try:
        print(mylist[4])
    except Exception as e:
        main_logger.error("exception error", exc_info=True)
```

## Add JournalHandler
Add JournalHandler for outputting logs to journal (useful with boot)  
```$ sudo apt install python3-systemd```  

```python
#!/usr/bin/env python3
import logging, sys, os, systemd.daemon
from systemd.journal import JournalHandler
from logging.handlers import RotatingFileHandler

def setup_logging(log_dir):
    # Main logger
    # If main/journal=INFO    then ALL (both loggers) info's get journal'd and logged
    # If main=CRIT,journ=INFO then only journal.info's get journal'd
    # If main=INFO,journ=CRIT then nothing journal'd but main info is logged
    # Use $ journalctl --since "1 hour ago" to check

    main_logger = logging.getLogger(__name__)
    journal_logger = logging.getLogger('journal')

    main_logger.setLevel(logging.CRITICAL)
    journal_logger.setLevel(logging.INFO)  # use logging.CRITICAL to turn logging off

    log_file_format = logging.Formatter("[%(levelname)s] - %(asctime)s - %(name)s - : %(message)s in %(pathname)s:%(lineno)d")
    log_console_format = logging.Formatter("[%(levelname)s]: %(message)s")
    journal_format = logging.Formatter("[%(levelname)s] - %(name)s - : %(message)s in %(pathname)s:%(lineno)d")

    journal_logger.addHandler(JournalHandler())

    console_handler = logging.StreamHandler()
    console_handler.setLevel(logging.INFO)
    console_handler.setFormatter(log_console_format)

    exp_file_handler = RotatingFileHandler('{}/exp_debug.log'.format(log_dir), maxBytes=10**6, backupCount=5) # 10MB file
    exp_file_handler.setLevel(logging.DEBUG)
    exp_file_handler.setFormatter(log_file_format)

    exp_errors_file_handler = RotatingFileHandler('{}/exp_error.log'.format(log_dir), maxBytes=10**6, backupCount=5)
    exp_errors_file_handler.setLevel(logging.WARNING)
    exp_errors_file_handler.setFormatter(log_file_format)

    main_logger.addHandler(console_handler)
    main_logger.addHandler(exp_file_handler)
    main_logger.addHandler(exp_errors_file_handler)
    return main_logger, journal_logger

if __name__ == "__main__":
    main_logger, journal_logger = setup_logging(os.path.dirname(os.path.abspath(__file__)))   
    main_logger.info("Testing logger output")
    journal_logger.info(__file__ + ' Marking journal')

    mylist = [1, 2]
    try:
        print(mylist[4])
    except Exception as e:
        main_logger.error("exception error", exc_info=True)
        journal_logger.error("exception error", exc_info=True)
```

**Short method**
```python
from systemd import journal
journal.write("Test outputting to journal")
```


-----------------------------  
-----------------------------  