---
layout: page
permalink: /ref/data-analysis/data-collection/
title: data
description: Intro to Data Analysis
nav: false
toc: true
---

# MQTT  
[Mosquitto](https://mosquitto.org/) MQTT is a lightweight messaging protocol that simplifies sending/receiving messages between devices (messages can be in the form of JSON objects). It acts as a broker to handle mqtt traffic among your devices. The messages are organized by topics. By publishing and subscribing to topics devices are able to talk to each other. For example, you can send commands from a node-red dashboard (web based), via the mqtt broker, to a RPi/esp32 to control servos or motors. And in return the the RPi/esp32 can send status (rpms) or other sensor data (voltage, current, temp) back to the node-red dashboard. The mqtt broker can run on the same RPi involved in the project or on a standalone RPi. The RPi can acts as the mqtt server/broker with node-red as the "brains".​  Another example might be collecting the water level of a tank with a sensor and turning a pump off when sensor shows level is high.

Measuring/Sending Data from a esp32/esp8266 or raspberry pi   
Data collection can be made through sensors or other instruments    
* ADC, ina219 current sensor, DHT11 temp sensor, encoder (motor rpm), etc
* I then use mosquitto (mqtt) to send the data to a server (usually a raspberry pi) and store it in a database (usually influxdb)  

> Note - When possible I give each pi a unique name during setup and use hostname.local or just hostname instead of IP address. Using the raw IP address can be problematic due to it changing.​​ Static IP can also be a hassle since you'll have to make sure your router doesn't assign that ip address to another machine when it is offline.  

Mosquitto documentation can be found at mosquitto.org/man/. It's a good reference tool for looking at the publish and subscribe functions. (Mosquitto uses port 1883)  
​
Initial setup on the device that will be used as the broker (mqtt server). You will use it both as the broker and client for initial testing.  
```sudo apt install mosquitto mosquitto-clients```  
Now run an initial test. Since the subscribe/publish are happening on the broker you do not need to include the broker IP address.  
In one window subscribe to "test" topic (-t)  
```mosquitto_sub -t "test"```   
In another window publish a msg (-m) to "test"topic (-t)  
```mosquitto_pub -m "msg from window2" -t "test"```  
You should see the message show up on the first window  

Now add a password file. I called my file "passwd" and put it in /etc/mosquitto  
```sudo mosquitto_passwd -c /etc/mosquitto/passwd <user>```  
```console
Password: <enter a password>
```  


Check the status with  ```$ systemctl status mosquitto ```  
The newer version requires a port defined for listening. You can add this in the user config file below
```sudo nano /etc/mosquitto/conf.d/user.conf```    
```console
listener 1883
```  
Note - If you look at ```sudo nano /etc/mosquitto/The mosquitto.conf``` it states it will also read user config in /etc/mosquitto/conf.d/ directory.   

If running the older version of mosquitto it opens port 1883 automatically and will shown in ```systemctl status mosquitto```  

To check that port 1883 is listening run ```sudo lsof -i -P -n | grep LISTEN```  

You can also tell mosquitto where the password file is by updating the user.conf file  
```sudo nano /etc/mosquitto/conf.d/user.conf```  
```console
listener 1883
allow_anonymous false
password_file /etc/mosquitto/passwd
```

Now test again but this time you'll have to include your user (-u) and password (-P).  
First window  
```mosquitto_sub -t "test" -u "user" -P "password"```    
Second window  
```mosquitto_pub -m "msg from window2" -t "test" -u "user" -P "password"```   

Enable mosquitto to start on boot  
```sudo systemctl enable mosquitto```  

Additional testing -  

Default QOS (quality of services) is 0 which has no confirmation that the msg was received. Testing can be done with QOS=1 to get confirmation that the messag was received.  
-V to use mqttv311 and -d for debugging  
```mosquitto_sub -V mqttv311 -t "test" -u "user" -P "password" -q1 -d```  
Now publish a msg  
```mosquitto_pub -V mqttv311 -t "test" -m "message" -u "user" -P "password" -q 1 -d```  

If you have a second device you can try sending a msg from it to the broker. Now you'll need to specify the IP address of the broker (-h) so mosquitto knows where to send the msg.  
```mosquitto_pub -h <Broker IPaddress> -m "msg device2" -t "test" -u "user" -P "password"```  

## PAHO Python  
RPi mqtt/Python specific libraries - [Paho Python client](https://eclipse.dev/paho/index.php?page=clients/python/docs/index.php) makes it easy to setup the RPi broker functionality. It includes functions for connecting to the broker, sending and receiving messages.  
install in venv ```(.venv)$ python3.7 -m pip install paho-mqtt```  
## Umqttsimple uPython  
esp32 mqtt/upython specific libraries - Umqttsimple makes it easy to setup ESP32 communication to the RPi mqtt broker using upython. It also includes functions for connecting to the broker, sending and receiving messages.

On the esp32 you will copy over a umqttsimple.py file along with your boot.py and main.py. You then import it with   
```console
from umqttsimple import MQTTClient
```
The umqttsimple.py code can also be found in github for MQTT projects  

# Node Red
[Node Red](https://nodered.org/) is a flow-based coding tool. It has a web browser-based editor where you connect different nodes to retrieve data, store data, make decisions, etc. A common setup is to use the 'mqtt in' node to listen for messages/data. Then, based on the mqtt message content (JSON format), other nodes can be connected to store the data in a database, send data to gauges/charts, use a 'mqtt out' node to send instructions (publish) to another microcontroller. To program node-red you can use both the built-in nodes or code-blocks with custom javascript code/functions. Using json along with loads/dumps makes it easy to transfer data between Python running on your microcontroller and node-red js.

Although node-red is installed on RPi's the node-red web site recommends using their script to setup on a RPi.  
```sudo apt install build-essential git ```  
```bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)  ```  
On Pi Node-RED works better with the Firefox or Chrome browser  
```node-red-stop ``` to stop Node-RED  
```node-red-start ``` to start Node-RED again  
```node-red-log ``` to view the recent log output   
```sudo systemctl enable nodered ``` to autostart Node-RED at every boot  
```sudo systemctl disable nodered ``` to disable autostart on boot  

```sudo reboot```  
To setup your node red flows go to **http://hostname.local:1880** (IP add = the address of RPi) on any device connected to the network.  
For influxdb and gauges/charts options go to menu in upper right and "manage pallette" then install  
* influxdb  
* node red dashboard  

Dashboard  
Once you have gauges/charts setup you can view them at **http://hostname.local:1880/ui**   

# API  
API (application programming interface) is a useful method for getting data from the web.  
Documentation below is for REST API using Python requests library.  
You pass the url link in the requests function and will get the requested data back  
The payload (or parameters) passes criteria  
The header can be used to pass keys  

Can use endpoint part of url to specify what resource to return. Typically found in API reference docs.  
HTTP Method	    Description	                Method
POST            Create a new resource	    requests.post()
GET	            Read existing resource	    requests.get()
PUT	            Update existing resource    requests.put()
DELETE          Delete existing resource    requests.delete()

Example with google translate  
url = "https://google-translate1.p.rapidapi.com/language/translate/v2/detect"  

```response = requests.get(url, param or payload, headers)```  

You send the url, payload, and headers.  

```python
import requests

payload = { "q": "English is hard, but detectably so" }
headers = {
	"content-type": "application/x-www-form-urlencoded",
	"Accept-Encoding": "application/gzip",
	"X-RapidAPI-Key": "",
	"X-RapidAPI-Host": "google-translate1.p.rapidapi.com"
}

response = requests.post(url, data=payload, headers=headers).json()  
```

This returned the language type of the sentence passed  
```python
example response = {'data': {'detections': [[{'isReliable': False, 'language': 'en', 'confidence': 1}]]}}
langtype = response["data"]["detections"][0][0]["language"]
```

Could then request translation  
```python
url = "https://google-translate1.p.rapidapi.com/language/translate/v2"

payload = {
	"q": "Hello, world!",
	"target": "es",
	"source": "en"
}
headers = {
	"content-type": "application/x-www-form-urlencoded",
	"Accept-Encoding": "application/gzip",
	"X-RapidAPI-Key": "",
	"X-RapidAPI-Host": "google-translate1.p.rapidapi.com"
}

#response = requests.post(url, data=payload, headers=headers)
example response = {'data': {'translations': [{'translatedText': 'Hola Mundo!'}]}}
translation = response["data"]["translations"][0]["translatedText"]
print(translation)
```