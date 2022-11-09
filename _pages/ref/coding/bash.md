---
layout: page
permalink: /ref/coding/bash/
title: Bash Shell Scripts and shebang
description: Intro to bash and shell scripts and shebang
nav: false
toc: true
---
Shell and bash are an integral part of operating systems and it's useful to learn some basic commands. They are essentially programming languages allowing you to run commands, execute files, pass arguments, define variables, etc. Although there are multiple shell languages this page focuses on BAsh (Bourne Again Shell based on the older Bourne Shell). You can confirm you're using bash with..  
```$ echo $SHELL```  
or a better command is   
```$ ps | grep $$```  
At the end it will list your shell  

To see list of valid shells installed on your system​  
```$ cat /etc/shells```  
To see shell assigned to a user  
```$ grep <user> /etc/passwd```  

Bash is useful for both one-liners and building a file of bash commands, or script. Here's an example of a bash one-liner (cd to Desktop and list contents)  
```$ bash -c 'cd Desktop; ls'```  
Anything you type on a command line, like 'ls', 'pwd', 'cp <file>' or even more advanced commands can be put into a bash command. When you have multiple command lines, and maybe you want them to run at a specified time interval, it makes sense to create a bash script and combine them into a single file.  

Bash scripts are commonly found in Raspberry Pi projects. Inevitably you will be trouble shooting an item, open up a file, and find yourself looking at a bash script. Knowing some basic syntax will be a big help. Note the order that bash scripts are loaded. When you start a terminal these files, containing commands and environment variables, are executed in this order.  
```console
/etc/profile
~/.bash_profile
~/.bash_login
~/.profile
```

# Writing a bash Script  
Shell/bash is a scripting language (like Python and JavaScript) compared to compiled languages (ie C/C++, Java).  

You can execute a script by simply typing the code/commands in a regular terminal command line.  

Ultimately you will want to group the commands in a shell/bash or python file and execute the entire group of commands at once. But for experimenting either way will work. If you chose single command line then you'll notice the prompt will change from $ to >. It will also let you know when you are inside a if/then condition or loop and you need to finish it before it outputs a result.  
​​
Some quick examples comparing to Python code..  
(note - for Python you need to indent/tab inside the 'if' condition.  
And you hit 'enter' a second time to close the 'if' condition and see a result.  
Python interpreter  
```$ python3```  
```python
>>> a=1      
>>> b=2    
>>> if b > a:    
...          print('b is greater') 
...     
b is greater      
>>> ctrl-D
```

Compare Python syntax to Bash  
```bash
$ a=1   
$ b=2      
$ if [[ $b -gt $a ]]; then  
> echo "b is greater"   
> fi         
b is greater    
$  
```

Some tutorials [learnshell](https://www.learnshell.org/) and one-liners at [onceupon](https://onceupon.github.io/Bash-Oneliner/#awk)  

-------------------------------------------------------------------

# shebang
**The shebang #! Header for bash (and python)**  
Shell/Python scripts typically start with a shebang as the header, giving instructions on what interpreter to run that script (bash, shell, python2, python3). By adding a shebang header, and making the file executable, you eliminate the need for specifying the shell/python in the command line and possibly making a mistake.  

Some shebang examples from wiki..  
Execute using Bourne shell  
```#!/bin/sh ```  
Execute using Bash shell  
```#!/bin/bash ```  
Execute using Bash shell. More universal method with the 'env' (see below)  
```#!/usr/bin/env bash  ```  
Execute the file using Python3  
```#!/usr/bin/env python3 ```  

this is a common header for a bash script  
```#!/bin/bash ```  

But another method to improve portability, and account for shell, python, etc being installed in a different location on different systems, is to use.  
```#!/usr/bin/env ```  

It can interpret your $PATH, so  
```#!/usr/bin/env bash  ```  
and  
```#!/usr/bin/env python3 ```  
work because it uses the first bash or python found in your $PATH.  

------------------------------------------

# bash example  
​Create a simple bash script that outputs "hello from bash"  
```$ touch hello.sh ``` Create the 'hello' script file  
```$ nano hello.sh ``` Edit the 'hello' script using nano and add shebang plus a line to print 'hello from bash' to the screen  

```bash
#!/usr/bin/env bash      
echo 'hello from bash'  
exit and save
```
Make the file executable.  
```$ chmod +x hello.sh```  
Execute the script  
```$ ./hello.sh```  
Note - The '.' tells your system the location of the file is in the current directory. If you just type $ hello.sh your system doesn't intuitively know the script is in the current directory and will give an error that 'command not found'. But you can always run it from any directory by telling your system the full path. In my example I would use  
```$ /home/pi/test/hello.sh. ```  
If you want to run your script without having to pass the location, just as you can run executables like 'ls', 'cd dir', 'pwd' from any directory then look at [$PATH](../../../ref/linux/terminal/#path) instructions.  

​Python examples  
A python file hello.py that is made executable  
```$ nano hello.py```  
```python
​# no shebang in first attempt  
print("hello")                            
```
```$ chmod +x hello.py```  
When you execute  
```$ ./hello.py```  
You get an error. With no shebang, your system does not know which interpreter to use. The extension doesn't matter like it does in windows. So it tries running the python script with the default shell script and fails.  
But by adding a shebang with python3 specified.  
```python
#!/usr/bin/env python3
print("hello")                   
```
Now when you execute   
```$ ./hello.py```  
it prints out hello   

You can also create a bash file to run your python script  
Create a bash file called hello-script.sh, with the lines below, and make it executable.  
```$ nano hello-script.sh ```  
```bash
#!/usr/bin/env bash            
/home/pi/hello.py               
```
then run with   
```$ ./hello-script.sh ```  
and it executes the python script printing out hello  
You could add more lines to the hello-script.sh file to run multiple commands.  

--------------------------------------

# More bash Scripts

```bash
echo "this will output some text to the screen"

# assign a variable
VAR1="english" 

echo "I am taking $VAR1 in college" 
# note you need $ in front of variable for parameter expansion, to output the value. echo with double quote will let you include variables​, too.

# Can also use ${} brackets to let bash know the exact variable name
# example
echo " I am taking $a101" 
# does not work because it looks like the variable name is $a101, but if you do..
echo " I am taking ${a}101 class"
# it works

Single quote will let you assign a command
VAR2='pwd'
​# assign a command utility to a variable using 'cmd'

# You can assign output from commands using $( command )
VAR3=$( echo $PATH )
echo "my path is $VAR3"

# Assign a default value during parameter expansion
echo "I have ${VAR4:-5} classes"
​# VAR4 is not defined so it will use default value of 5
```

------------------------------------------

## Passing arguments and read input
```bash
# To read input from the user​​
echo "what is your name?"  
read VAR4                               
echo "your name is $VAR4" 

# To pass an argument during the command line execution list it at the end
# It will assign $1, $2, $3 etc to each argument passed
​$ ./myscript.sh friday
echo "today is $1"

# Some defaults
# $0 =name of the bash script
# $1-9 can be used to pass arguments (see passing arguments below)
# $# =how many arguments were passed
# $@ =all arguments passed
# $$ =process ID of current shell
```

------------------------------------------

## If/else Conditional and Case Statements
```bash
# General syntax​​
if [ condition ] ; then
elif [condition ] ; then
else
fi

&& is AND
|| is OR

#example
$ ./myscript.sh friday 

# note - use quotation inside comparison
if [ "$1" == "saturday" ] || [ "$1" == "sunday" ] ; then 
    echo "you are right this is weekend"
else
    echo "wrong $1 is a weekday"
fi
# elseif is just elif
# -n means 'if not empty'

# Case syntax
case $1 in
    monday) echo -n "it is monday";;
    tuesday) echo -n "it is tuesday";;
   *) echo -n "not sure what day";;
esac

# using OR

case $1 in
    monday | tuesday | wednesday) echo -n "it is weekday";;
    saturday | sunday) echo -n "it is weekend";;
   *) echo -n "not sure what day";;
esac
```

------------------------------------------

## Comparison Operators
```bash
# Numerical Comparison
$ ./compare.sh 1 2 
var1=$1
var2=$2                                                    
if [ "$var1" -lt "$var2" ] ; then               
    echo "$var1 is less than $var2"       
else                                                           
    echo "$var1 is greater than $var2" 
fi                                                                

-lt (less than)
-gt (greater than)
-le (less than or equal)
-ge (greater than or equal)
-eq (is equal)
-ne (is not equal)

# String Comparison
$ ./compare.sh hey hay
var1=$1
var2=$2                                                            
if [ "$var1" == "$var2" ] ; then                       
    echo "$var1 is same as $var2"                 
else                                                                   
    echo "$var1 is NOT the same as $var2" 
fi                                                                        

= (is same)
== (is same)
!= (is different)
-z "$var1" (is empty)

```

------------------------------------------

## Arrays
```bash
# Assign array values inside () and space delimited​​
DARRAY=(1 2 hello)
DARRAY[3]="today"
echo ${DARRAY[2]}
echo ${DARRAY[3]}
echo ${#DARRAY[@]} # number of elements in the array
echo ${DARRAY[${#DARRAY[@]}-1]} # next to last element
echo ${DARRAY[@]} # all array elements
```

------------------------------------------

## Operators and More Parameter Expansion
```bash
# Numerical Operators
var1 + var2 (+, -, *, /, %, **)

# String Operators and Parameter Expansion
var="north by northwest"

echo ${#var} # string length

# ${var/search/replace}  / only does 1st occurrence
echo ${var/north/south}
# ${var//search/replace}  // does all occurrence
echo ${var//north/south}

# ${var:offset:length} slice a string
echo ${var:2:3}
echo ${var:0:3} # first 3 characters
echo ${var:(-5):3} # for negative offset need () to avoid thinking it is a 'default' value.
# can also use variables
x=4
echo ${var:0:$x}

# Prefix/Suffix Manipulation. If there is a pattern match it will remove it. Can use * wildcard.
# ${var#pattern} and ${var%pattern}
filename=dummy.sh
echo ${filename#*.}
# The # removes the matching prefix
echo ${filename%.*}
# The % removes the matching suffix
```

------------------------------------------

## Loops
```bash
# For loop thru array
DARRAY=(1 2 3 4 5)
for X in ${DARRAY[@]} ; do
    echo "number is $X"
done

# For loop with range
for X in {1..10} ; do
    echo "number is $X"
done

# For loop thru string
TEXT='red yellow blue black'
for COLOR in $TEXT ; do
    echo $COLOR
done

# While loop. Note the ((var)) construct to manipulate a variable
COUNTER=0
MAX=10
while [ $COUNTER -lt $MAX ] ; do
   echo "counter is $COUNTER"
   COUNTER=$(($COUNTER + 1)) or just ((COUNTER++))
done

# Until loop
COUNTER=0
MAX=10
until [ $COUNTER -gt $MAX ] ; do
   echo "counter is $COUNTER"
   COUNTER=$(($COUNTER + 1)) or just ((COUNTER++))
done

break      # exit immediately and skip remainder of loop
continue # skip the rest of this loop iteration and begin the next

# Example of another While loop for guessing game using
# different types of syntax
file="highscore"
attempts=0
guess=0
rannumber=$(( $RANDOM % 100 + 1 ))
# (( rannumber = RANDOM % 100 + 1 )) is another way
echo -e  "Guess a number between 1 and 100\n"
# -e to exit on error
while (( guess != rannumber )); do
    ((attempts++))
    read -p "attempt ${attempts}: guess is " guess
    # -p lets it read with a prompt
    [ "$guess" -lt "$rannumber" ] && echo "Too Low"
    if (( guess > rannumber )); then
        echo "Too High"
    fi
done
echo "Correct. You guessed it in ${attempts} tries\n"
read -p "Enter your name - " name
echo $name $attempts >> $file

echo "\nHigh Scores"
cat $file 
```

------------------------------------------

## Select Menu Loop
```bash
# Will create a menu with the values in the list and wait for selection​COLORS='red yellow green blue exit'
PS3='Pick a color ' # change menu description
select COLOR in $COLORS
do
    if [ $COLOR == 'exit' ] ; then
        break
    fi
    echo "You picked $COLOR"
done
```

------------------------------------------

## Functions
```bash
# Create a function
function dsum {
    echo "$(($1 + $2))"
}
​
dsum 2 2 # pass values to the function
```

------------------------------------------

## File Utilities
```bash
# Useful file utilities
FILE="test.sh"​
if [ -e "$FILE" ] ; then
    echo "$FILE exists"
fi
# -d is used for directory
```

----------------------------------------------

Some Useful Terms  
- Interpreter is a computer program that directly executes instructions written in a programming or scripting language, without requiring them to be compiled into a machine language program. (from wikipedia)  
- Wrapper is a script that takes more complex commands (and parameters passed to the commands) and works to simplify it, making it easier to re-use.  
- Parameter expansion occurs when you place the $ symbol in front of a variable (it will replace the variable name with value of the variable). It also allows  you to perform functions and even modify the value during the expansion process.  

-----------------------------  
-----------------------------  