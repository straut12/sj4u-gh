---
layout: page
permalink: /ref/coding/python/
title: Python
description: Intro to Python
nav: false
toc: true
---
# OOP vs Procedural
There are a couple different approaches to the over-all flow or framework of your code, how it is structured and organized. Procedural vs Object Oriented Programming (OOP).  

Procedural is a fairly simple concept. Your code is written as a sequence of steps that are executed step-by-step. This makes sense when your project only requires a small number of steps to do some basic actions. You can still use functions, subroutines, procedures to organize actions that are often repeated to help keep the code clean and efficient. For quick, simple instructions procedural will be the most straight forward solution. But is it not as easy to scale up or build upon. If you plan on building on the project and making it a larger application then structuring your code with OOP will usually be better.
(Almost all languages can do some level of procedural but the reverse is not necessarily true. There are some languages that are primarily procedural and could not do OOP. ie Fortran, C, and Basic)  

In Object Oriented Programming (OOP) you try and think of your project in terms of objects. Each object usually contains data (attributes) and procedures (methods) that can access or manipulate the data. A common term you'll here is class or the blueprint for your objects. (objects are instances of classes). My first interpretation of objects was that they are similar to complex variables that have both containers and functions built into them.  

Since STEM projects will frequently use external Python packages objects (that are using OOP) early on you will run into OOP methods. (You'll notice it from the dot notation) Dot notation means you are accessing data or running an operation connected to that object. (ie using an external library to get the temperature might call something like sensor.temperature)  

```python
import adafruit_dht

mySensor = adafruit_dht.DHT22(board.D18) # connected to gpio pin 18

temp = mySensor.temperature
```

Code example above using a DHT22 sensor and the adafruit dht library to get the temp data from the sensor.
* At the top of the code you import the adafruit DHT library (and the library is using OOP approach).
* You create a specific object representing your temp sensor (and it's using a class in the adafruit library as the blueprint)
* Then you call on the temperature method of that object to get the temperature at which point you can put it in your temp variable.​  

So understanding OOP early on will make it easier to navigate using 3rd party packages along with allowing you to better structure your code for more complex projects.  

It is also helpful to understand Python as modular programming. Python supports grouping these classes into separate files or modules. But you can easily import the module (step 1 above) to use in your own code. This helps with organization and allowing you to easily use code from other libraries. The modules are namespaced (ensures unique names) to avoid conflict when a class has the same name as a class in another module.  

**OOP Concepts**  
Two main components of OOP are classes and objects.
* Classes - this is the blueprint or template that defines the data(attributes) and available methods for acting on that data. (a complex user-defined data type)
* Objects - these are the specific instances of the class

**Python Structure with OOP**  
* MODULE (module_name.py) is a python file that contains classes/functions and will have a .(dot) extension and can be executed/imported. If using OOP it will contain objects that are used to define the data and functions to work with that data.
    * CLASS (ClassExample) is a type or container a variable can take on; just like int, float, str, list, dict. Typically uses CamelStyle naming.
        * ATTRIBUTES - variables inside a class that define the data
        * METHODS (class_method) - similar to attributes but define functions instead of data. Typically used to take some kind of action

Inheritance is the concept of building an object (child) or class based upon another (parent). The child inherits the attributes/methods of the parent class which reduces redundancy while also maintaining hierarchy. The child can add new attributes/methods that wouldn't be applicable to the parent. Two styles of OOP or inheritance models are class based inheritance (ie Python) and prototype based inheritance (ie JavaScript). This can be confusing since JavaScript can use the 'class' keyword (considered syntactic sugar) to mimic a class type language.​

**JavaScript Structure with OOP**  
​​In JavaScript instances can be instantiated via constructor functions with the 'new' keyword.  
Classes have to be specified in specific order. js can not use the class until it is defined in the code.  
Encapsulation - the bundling of data and the methods that act on that data in objects. Access to that data from outside can be restricted. Wrap all properties into one object.  
Constructor is a special method on js class. When called later using the new keyword, constructor will create one new blank js object and assign it properties and values based on the blueprint inside. It will run once when we instantiate the class using the new keyword.  
Objects in js are reference data types which means that unlike primitive data types they are dynamic.  
Inheritance - Extend the same behaviors. If property/method is not found in child it will look in parent  
Abstraction - hide the implementation that is critical or does not need to be exposed. Avoids bugs.  
Polymorphism - behavior of the object may change according to state or context.  
Instantiation - create the object. return the handler.  
Initialization (constructor) - set the initial/default values.  

# Helpful Python Commands
I will not be listing the Python syntax for loops, variables, conditionals, etc. There are many online resources for syntax including [w3schools](https://www.w3schools.com/python/)  

These are Python commands/concepts that I've come across and found useful.  

To check Python version.  
```$ ll /usr/bin/python3*```  
```$ python3 --version``` (to see which version is used with python3 command)  
 (or just python* if you want to see python 2)  

New Python versions will come out with features you may want to use. To install a new version on Linux (example below to install python3.7)  
```$ sudo add-apt-repository ppa:deadsnakes/ppa```  
```$ sudo apt install python3.7```  

Can now run ```$ python3.7``` or ```$ pip3.7```  

Next update venv to match the python version
```$ sudo apt install python3.7-venv```  

Now virtual environment can be created with
```$ python3.7 -m venv .venv```  

To add python3.7 to VScode.
After installing a new version, closing, re-opening VScode recognized the new python as an option.
Otherwise  
```$ which python3.7 ``` (will give you the path to python 3.7 binary/executable)  
Copy the path (ie. /usr/bin/python3.7)  
In VScode change interpreter in bottom left and select "enter Interpreter path.."  
Paste the /usr/bin/python3.7  

Trying to change the default version of python3 was a hassle. It broke my gnome terminal. So I just type python3.7 or python3.8 (or which ever version using at the time)  

Using pip  
Advice on stackexchange..  
Never use "sudo" with pip install  
Do not use ```$ pip install package```  
Instead run pip as a module with the -m using the specific python version you want  
```$ python3.7 -m pip install package```  

# Python pip

**Python pip**
Pip is the Python package manager. Many projects involve pip to install specific Python packages, ie the files needed for a module. Modules are python libraries that other people wrote which you can use in your code. This saves time/effort compared to trying to write all of the code from scratch. Pip packages can be found at [pypi](https://pypi.org/)  

Using pip  
Note it is recommended to install packages inside a virtual environment to avoid broken/conflicting packages.  
```(.venv)$ python3.7 -m pip install package```  

To see what packages are installed  
```(.venv)$ pip3 list ```(will list all packages installed including editables)  
Side note list vs freeze 
```(.venv)$ pip3 freeze > requirements.txt ``` will list packages you installed in a txt file and in a format that can be used to reinstall.  

**Python wheels**
Often during a pip installation you'll see reference to wheels.  Python wheels are a tool that help install python packages. From stack overflow.. if the package is not a wheel, pip tries to build a wheel for it (via setup.py bdist_wheel). If that fails you get "failed building wheel for **package**" and pip falls back to installing directly (via setup.py install). Once we have a wheel, pip can install the wheel by unpacking it correctly. pip tries to install packages via wheels as often as it can. This is because of various advantages of using wheels (like faster installs, cache-able, not executing code again etc).  

If having problems with wheels you can try installing/updating.  
```$ python3.7 -m pip install --upgrade pip setuptools wheel```  
or  
```$ pip install <package> --no-cache-dir```  

There are times I've had problems getting the pip version of a package to install and had to use apt.  
ie pip3 would not work for RPi.GPIO on a project so I used apt  
```$ sudo apt install python3-wheel```  
```$ sudo apt-get install RPi.GPIO```  

# Python venv
Using a virtual environment involves a couple extra steps but is a good practice. In my projects I only use python3 and venv for simplicity. (vs virtualenv or pipenv). The concept is that you're creating an isolated, unique python environment/folder which can be activated when working on a specific project. That environment will have its own Python and when you install python packages they will be specific to that environment. This helps handle the scenario where different projects have the same dependency; or share the same packages but different versions of that shared package which creates a dependency conflict. venv handles the conflict by isolating the project dependencies and allowing you to install a specific version of a package within that project folder; without affecting other projects. Since you start with a clean slate each time you can also use commands to see what packages are necessary for your project (and version), output them to a requirements.txt file and use it to easily install on another system. 

Note - if you want to run your python program outside the venv (during startup of RPi) then you need to put the location of the venv python being used in the shebang !# header of your python program.  
For example  
```#!/home/pi/projectA/.venv/bin/python3 ```  
You can then make it executable with   
```$ chmod +x filename.py```  
Now you can execute it with   
```$ ./filename.py```  
To see more details go to [shebang](../../../ref/coding/bash/#shebang) section  

Installation (may need to go through update/upgrade steps)  
```$ sudo apt install python3```  
```$ sudo apt install python3-pip```  
```$ sudo apt update```  
```$ sudo apt upgrade```  
```$ sudo apt install python3-venv```  

Often you'll be using a different version of python from the python3 default on your system (ie 3.8, 3.9) just replace python3 with python3.7 or python3.8 etc.  

**Quick steps**  
create a new project folder and activate the venv  
```$ mkdir project-name && cd project-name```  
```$ python3 -m venv .venv ```  
You can change the second .venv to whatever virtual env name you want. The .venv folder will contain python and packages you install.  

Now use 'source', a built-in shell command to read/execute the contents of the 'activate' file  
```$ source .venv/bin/activate ```  (a .venv will now be at the beginning of your command prompt line)

In VS code select the Python interpreter created in the venv from the menu at the bottom right of the window. Or go to command palatte, ctrl-shift-P, and then type 'Python: Select Interpreter' and choose your venv.  

**git Housekeeping Item**
Add .venv to your .gitignore list so you do not load it to github. This can be done automatically when you create the repo by selecting Python template for your git ignore.  

To turn off the venv  
```(.venv)$ deactivate ``` (when you're done and need to deactivate.)  

To remove the venv just delete the venv folder with recursive/force flags  
```$ rm -rf .venv/ ```  

**Move/Install project in another location**  
```(.venv)$ pip3 freeze ```(or $pip3 list -v or $python3 -m pip list -v) to see what packages are installed  
```(.venv)$ pip3 freeze > requirements.txt ```to output in a file for installing in another location  
or $ python3 -m pip3 freeze > requirements.txt  

Go to new location and create/activate a .venv  
```$ python3 -m venv .venv```  
```$ source .venv/bin/activate```  
install with -r for requirement install  
```(.venv)$pip3 install -r requirements.txt ```  
or $ python3 -m pip install -r requirements.txt

May have to remove pkg-resources from the requirements.txt to avoid errors when installing on another system.  

**Installing/uninstalling packages**  
```(.venv)$ python3 -m pip install package-name```  
Can use find to see where it was installed  
```(.venv)$ find . -print | grep -i package-name```  
(do not need to use * wildcards, will find files and folders)  
To uninstall  
```(.venv)$ python3 -m pip uninstall package-name ```  ​
To upgrade a package  
```$ python3 -m pip3 install --upgrade <package>```  

**Other Commands**  
Install requests (useful for making HTTP requests in Python)  
```$ python3 -m pip install requests```  

Install from source  
```$ cd <source name>```  
```$ python3 -m pip3 install . ```  

To get access to site packages go into the virtual env folder and edit pyvenv.cfg  
change include-system-site-packges=true  
Module installations will still go to venv as normal and site packages will be visible too.  

**Troubleshooting**  
Helpful command when you get 'module not found'. Can use inside the venv  
List locations (sys.path) Python is looking for modules to confirm the path of the module is included.  
```(.venv)$ python3 -c "import os; print (os.sys.path)" | tr "," "\n"```  

Some folder explanations  
* bin - the executables you use in the venv. ie activate
* include - used for compiling the python packages.
* lib - where the python version you're using for the venv is stored/installed and site package folder for dependencies.  
Example  
├── bin  
│   ├── activate  
│   ├── pip  
│   ├── pip3.6  
│   ├── pylint  
├── include  
├── lib  
         └── python3.6  

# Python Framework
Python Infrastructure  
BOTTOM  
* **MODULE** (module_name.py) is a python file that contains classes/functions and will have a .py extension and can be executed/imported.
* **PACKAGE** (packagename) is a directory with a collection of modules and will include \__init__.py (constructor) so the interpreter knows it is a package.
* **LIBRARY** is a collection of packages. More of a generic term meaning a collection of code that is designed to be re-used. A library can be a collection of packages/modules or even a single module.
* **FRAMEWORK** is a collection of libraries.
* (API or application programming interface is an interface or description of how to interact with an application)  
TOP  

Example  
The files inside of a package **temp-measure** might be  
```console
/temp-measure/ 
__init__.py 
ntc_measure.py (module)
dht11_measure.py (module)
```
To use a part of the module, import it, create an object, and then access the data or methods.
```python
import ntc_measure.py
sensorA = ntc_measure.sensorSetup(pin 18)
temp = sensorA.readtemp()
```

* **MODULE** (module_name.py) is a python file that contains classes/functions and will have a .py extension and can be executed/imported. OOP will involve objects where you combine data (attributes) and functions (methods/actions) to work with that data.
    * **CLASS** (ClassExample) is a type or container a variable can take on; just like int, float, str, list, dict. Typically uses CamelStyle naming.  
    * **OBJECT** is a specific instance of a class.  
    * **\__init__()** method is a constructor. Creates INSTANCE variables instead of CLASS variables. Initializes an object within a class. This helps avoid the issues of class attributes that are mutable (like a list) and being modified by other objects unintentionally.  
    * **ATTRIBUTES** are variables inside a class that define the data (variable_name, CONSTANT_NAME)  
    * **METHODS** (class_method) are similar to attributes but define functions instead of data. Typically used to take some kind of action.

When using a third party package (ie a package you installed with pip) it is sometimes helpful to go look at the modules to see what they're doing and what arguments can be passed.  

For example  
**speedtest-cli** is a python package that can be used to get your network download/upload speed.  
* You can either use the command line or call the modules in your python code. For the cli to work I was having to use the **--secure flag** (otherwise I got an HTTP error). 
* However it was not clear how to use the 'secure' option in python code. But if you went to the **speedtest.py** module (in a venv it was under venv/lib/python3x/site-packages) and looked at the arguments in the Speedtest class you could see there was a **secure=False** flag. Passing **secure=True** allowed Speedtest to run in the code. 
```python
class Speedtest(object):
    """Class for performing standard speedtest.net testing operations"""

    def __init__(self, config=None, source_address=None, timeout=10,
                 secure=False, shutdown_event=None):
```
* Another method to browse the Speedtest class arguments is using **pydoc**. Go into the directory and active the venv and then run  
```$ python3 -m pydoc -b ```   
It will open a browser where you can go to the site-packages and looks at the speedtest module/classes.  

**Some underscore naming conventions:**  
_(single): internal, weaker enforcement, can use to indicate the scope.  
__(double): private, stronger enforcement, I don't use my own version of these.  

SINGLE(internal)  
\_var, \_method, \_attr : Indicates it is meant for private, internal use, and not to be imported. Has 'weak' enforcement. (ie Python will not import when 'import *' used)  
var_ : Naming convention to make the variable different from a Python keyword  

DOUBLE(private)  
\__method or \__attribute  : Not recommended. Makes the method/attribute private and can't be accessed outside the class. 'Stronger' enforcement. Will cause naming   mangling (ie will become Class._Class__method)  
\__method__or \__attr__ : Indicates a special or magic Python method. Do not use for your own naming.  

- default constructor will only have one argument, self, which is referencing the instance being constructed.  
- parameterized constructor can have extra arguments you pass to it  


**Helpful Troubleshooting Commands**  

Lists locations (sys.path) Python is looking for modules (can do inside venv)  
```$ python3 -c "import os; print (os.sys.path)" | tr "," "\n"```  

Show where third party packages, using pip, will be installed   
```$ python3 -c "import site; print (site.getsitepackages())" | tr "," "\n"```  
note - There are both system (standard Python library) and site packages. This command prints site packages.  

-c command --> Specify the command to execute. This terminates the option list (following options are
passed as arguments to the command).  

-m module-name --> Searches sys.path for the named module and runs the corresponding .py file as a script. Examples in python -m section  

dir() - List all properties and methods of an object.  
globals() - Returns dict of variables defined in global symbol table (namepsace).   
locals() - Returns dict of local symbol table  

From the python interpeter - in a script use pprint(dir()) for pretty printer. (from pprint import pprint)  
Can import modules and re-run dir() to see what was loaded.  
```console
>>> import modules
>>> dir()
>>> import moduleA
>>> dir()
```
Can create an object and run dir(myObject)  
```console
>>>exec(open("file.py").read())
>>>globals()
```
Or put it inside a function with pprint(globals())  

**Python -m flag**  
The python -m flag allows you to execute a python script (file.py) from the command line by using the module name (file) rather than the entire file name (path/to/file.py). When running a single file it will execute as "main" (so you can use name="main"). This has little advantage when you're running a python script from your directory.  
```$ python3 -m file```  
vs  
```$ python3 file.py```  
Small note -   
python3 -m file --> sys.argv\[0] = full/path/to/file.py  
python3 file.py --> sys.argv\[0] = file.py  

The main -m advantage is running built-in python modules as one-liners.  See examples below.  

You can also use -m on one of your packages (folder). When running -m on a package  
First evaluates the \__init__.py (adds the "import" ability)  
Any code in imported modules will be executed (not in the name="main")  
Then runs the \__main__.py file (\__main__.py is required to run -m on a package).  

So how does it work..  
Example to create a simple web server at port 8000 from any directory  
```$ python3 -m http.server 8000```  
(can now go to http://**IPaddress**:8000 from any PC on your network)  

The command executes server.py (module) in http folder (package)  
Why does this work?   
Python looks for modules in the current directory, paths from PYTHONPATH, and the installation-dependent defaults.  

To see the defaults sys.path use  
```$ python3 -c "import os; print (os.sys.path)" | tr "," "\n"```  
This shows /usr/lib/python3.6 in my sys.path  
http/server.py is under python3.6  
/usr/lib/python3.6/  
    ├── http  
        └── \__init__.py  
        └── server.py  

An even quicker way of seeing this is using the pydoc module  
In the same directory run  
```$ python3 -m pydoc -b```  
A browser should open. Find http under python3.6 and you can navigate to server.py  

**Pydocs**  
go into a folder and run pydoc to create an HTTP server at the port specified with python documentation. Will show \__doc__ files for your classes, functions.  
```$ python3 -m pydoc -p 8001```  
```$ python3 -m pydoc -b```   (open browser using next available port)  

Python **pdb** debugger (useful for debugging in a terminal on Pi0)  
```$python3 -m pdb file.py```  

**Profilers** and **timers** (cProfile, timeit, pstats)  
```$ python3 -m cProfile -o output.cprof file.py```  
```console
>>> import pstats
>>> p = pstats.Stats('output.cprof')
>>> p.sort_stats('cumulative').print_stats(10)​​​​
```
Or view results in SnakeViz  
```$ snakeviz output.cprof```  
To install  
```$ sudo pip3 install pstats-view```  
```$ sudo pip3 install snakeviz```   

**Simple http.server**
Simple web server that allows you to browse folder/files and download a file. Can also serve static html files.  
```$ cd <directory>```  
```$ python3 -m http.server 8000```  
You can now go to **hostname**.local:8000 from any PC on your network and see your folder/files.​  
```console
​​pi@raspberrypi:$ python3 -m http.server 8000
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
10.0.0.100 - - [14/Mar/2021 11:52:10] "GET / HTTP/1.1" 200 -
```

# Package Module Importing

**Python Package-Module Import**
Packages are directories with a collection of modules.  
* Package directories will often contain a \__init__.py (which indicates it is a package)  
* When a package (or module in the package) is imported the \__init__.py is executed.  
* Code in the \__init__.py can be used for package initialization.

A module is a python file (file.py) that contains classes/functions that can be used in other python scripts. It can be located in a package or stand alone.
* A module can be imported in a python script with ```import module```
* Or executed as a script at the command line with ```$ python3 module.py```

The method for getting access to another module's classes/functions is through "import". When python sees the ```import package or module``` line it will look in the following locations for that directory/file.  
1. Current directory  
2. Directories listed in $PYTHONPATH (see PYTHONPATH section for details)
3. The installation-dependent list of directories (specific to python version)
4. Additional method is adding sys.path.append(/path/to/dir) in your code. The path will be added temporarily when your code runs.  

To see a list of your python packages (directories) you can print(sys.path). A quick way of doing this at the command line using the -c flag.  
```$ python3 -c "import sys; print (sys.path)" | tr "," "\n"```  
```/usr/lib/python3.6``` (you'll see standard python packages you've used)  
```$HOME/.local/lib/python3.6/site-packages ``` (you'll see packages you've installed with pip)  
```/usr/lib/python3/dist-packages ``` (you'll see packages installed with apt)  
If you look inside these directories you may see  
* \__init__.py (makes a directory a package and may list what modules will be loaded)  
* **directory** (this could be a subpackage)  
* **files**.py (these are the modules containing the classes/functions)  

Another way to find the location is in the interpreter. Import a module and print the \__file__ variable. (however this does not work on built-in modules)  
```console
>>> import http
>>> http.__file__ 
/usr/lib/python3.6/http/__init__.py​
```
Executing a module as a Script   
When developing a module it is useful to execute the module as a script for testing and further development (unit testing). But when importing the module into another script you typically do not want it to execute that code.  
An easy way to handle this is 
* put an "if" condition in your module where you place your "unit testing" code 
* and \__name__ == '\__main__'  

\__name__ is just a variable python assigns to the file being executed  

When a module (or script) is executed as a script (ie $ python3 module.py)  
* \__name__ = '\__main__'  

When a module is imported in another script (ie import module)  
* \__name__= module name

So in your module put the if \__name__ == '\__main__':  
my_module.py  
```python
def myfunction():​
    print("this is where the function does stuff")
    print(__name__)

if __name__ == '__main__':
    print("this is where i test the function")
    myfunction()
```

Now if you import this module in another script and call myfunction() it will only execute the myfunction. (because \__name__ will = 'my_module')  
But if you execute the module as a script. ```$ python3 my_module.py```  
It will run the code inside the "if" condition because \__name__ will = \__main__  

**Experimenting with the "import"**  
​Early on your projects will be single python scripts or a single file (file.py). But even on these early scripts you'll likely be leveraging off other python packages/modules and using "import".  There are two common methods for importing. (example below with a module)  
Options  
1. ```import module ```  
2. ```from module import function ```  (or Class)  

(note - most examples below use the function scenario. But you could also test by instantiating/creating an object with a = Class())  

Option 1. You have imported the module into the local symbol table but its contents are still private. You have to use dot notation and call the objects specifically with module.func()  

Option 2. You have imported the module objects directly so you directly call func(). The danger is if you already have a func() imported from a different module it would be over-written. You can avoid this by renaming using import 'as'   
```from module import func as thisfunc ```   
Now you use thisfunc()   

Below goes thru an example with the built-in "time" module often used for creating a short delay/pause in your code. Since "time" is a built-in module it is easier to use pydoc to find it.  
First run pydoc -b to show a list of python packages/modules  
```$ python3 -m pydoc -b```  
or  
```$ python3 -m pydoc -n 10.0.0.222 ``` (any device on network can access)  
```python
>>> python3
# Option 1 above. Import time into the current namespace
>>> import time
>>> dir()
# Since we imported "time" we'll see it listed in dir(), the current local symbol table. And we know from pydoc that "sleep" was the function inside "time". So we use it with ..
>>> time.sleep(2)

# Option 2 above. Now directly import  sleep
>>> from time import sleep
>>> dir()
# Can now see "sleep" listed. So call it directly ..
>>> sleep(2)
```

A browser window should open up with the python package/modules. You'll recognize the same directories listed from sys.path above. However the built-in modules are included here, too.  
Under Bult-in Modules select "time". Scrolls past the Classes down to the  functions and you'll find "sleep". Now use the interpreter to experiment with importing it.
Use pydoc to Explore Packages/Modules  
​Pydoc is a graphical way to explore packages/modules in a directory (packages/modules you've created in that directory and the installed packages/modules by python).    

**Create Your Own Package Module**  
The best way to get familiar with the package/mdoule concept is creating your own packages (directories) and modules (file.py).   
* Create \__init__.py inside the directories to make them packages. 
* Create simple functions with a print("function") inside the modules.
Now experiment with different import syntax.  
```console
# User
|- user-script.py
# Developer
├── package
│         ├── __init__.py
│         ├── moduleA.py
|         |       functionA()
│         ├── moduleB.py
|         |       functionB()
```  

Do iterations of changing your package \__init__.py and method of importing in the script.  
Each time check dir() to see what is loaded into the global namespace. (you can also do dir(object))  

For packages/modules that you build yourself it is easier to experiment with the import function using REPL and dir().  
Create a package/module/function setup like above.  
The function just does a simple print("this is functionA")  
​
Test 1  
package \__init__.py = from .moduleA import functionA  
$ python3  
```python
>>> import package 
>>> dir()
Try and execute the test function
>>> package.module.functionA()
>>> package.functionA()
>>> functionA()
```
quit() and try another iteration to see how dir() changes. Note - if you don't quit and keep importing thru different methods you'll notice you can call a function with multiple conventions.  

**User and Developer Modes**  
Although you may often be both the **user** and **developer** it is useful to think in both modes. You want to develop your package/__init__.py so that later you can easily user the modules.  

**\__init__ --> import package.module**  
From developer side can list which modules are visible from the package  
```python
Developer -  'specific' ​​ 
package/__init__.py
​
import package.moduleA
# import package.moduleB

# Can control what modules/functions are visible
```
User can import all (*) modules or specific modules that were listed in the \__init__.py.  
```python
​​User-script.py 
from package import *

a = moduleA.functionA()
# can not call moduleB.functionB()
```

User can import package and call package.module.function    
```python
User-script.py 
import package
​
call package.moduleA.functionA()
```
If the user knows the modules can import them specifically, regardless of the __init__.py  
```python
User-script.py - if user knows the module can still specifically import it (even if init.py does not)
import package.moduleB as mymod​
call mymod.functionB()​

from package import moduleB​
​call moduleB.functionB()
```
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/pythonimport1.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

**\__init__ --> from module import \***   

From developer side can import all (*) classes/functions or be specific. This controls what is visible to the user.  

The downside of the 'all' (*) is that the user can see all functions/classes (even when not intended to be used by the user)  

The downside of the 'specific' scenario is that you have to keep updating your __init__.py as you add more functions.   
```python
Developer -  'all' ​ 
package/__init__.py
​
from .moduleA import *
from .moduleB import *

# All functions are available to the user to import
```
```python
Developer - 'specific'​
package/__init__.py
​
from .moduleA import functionA
from .moduleB import functionB

# Can control what functions are available to the user to import
```

Since classes/functions are imported in __init__.py  the user can call package.function()  
```python
User-script.py 

import package

#call package.functionA()
package.functionA()

#call package.functionB()​
​package.functionB()
```

Or the user can directly import the function  
```python
User-script.py 

from package import functionA

call functionA()
```

**\__init__ --> from module import * (_all__ = ['module'])**

From developer side if you want to limit what is visible (what should be called by the user) then the \__all__ can be defined to specify what the user sees with "import * "  
```python
Developer -  'specific' ​ 
package/__init__.py
​
from .module import *
__all__ = [
  'moduleA',
  'functionA'
​]

# Can control what modules/functions are visible
```

User can import package modules that were listed in the __init__.py.   ​
```python
User-script.py 
from package import *

call moduleA.functionA()
​call functionA()
# can not call moduleB.functionB()
```

User can still call specific modules if the name is known.  

```python
User-script.py - if user knows the module can still specifically import it (even if init.py does not)
import package.moduleB as moduleB​
call moduleB.functionB()​

from package import moduleB​
​call moduleB.functionB()
```

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/pythonimport2.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Regardless of the package __init__.py (or if it even exists) you can import directly if the module/function names are known.  
<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/linux/pythonimport3.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>



-----------------------------  
-----------------------------  

(../../../ref/coding/starting-up/)

<div class="row">
    <div class="col-md mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/flappybird.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

<div class="row justify-content-center float-right">
    <div class="col-4-auto mt-3 mt-md-0">
        {% include figure.html path="assets/img/coding/flappybird.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

----------------------------
Images
can you col-#  col-sm-#   col-md-#   col-lg-#
Use auto to auto size around image
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).