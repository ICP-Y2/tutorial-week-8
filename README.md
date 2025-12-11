# tutorial-week-8

**Individual/Group work**
### 1.	Make a sensor node using ESP32 and DHT22 sensor connected to Pin 15.
### 2.	Make ESP32 micropython project with selected sensor.
### 3.	Understand the source code provided and implement in ESP32 Wokwi.
```
import network
import time
from machine import Pin
import dht
import ujson
from umqtt.simple import MQTTClient

# MQTT Server Parameters
MQTT_CLIENT_ID = "micropython-weather-demo"
MQTT_BROKER    = "broker.mqttdashboard.com"
MQTT_USER      = ""
MQTT_PASSWORD  = ""
MQTT_TOPIC     = "C2/G1/DHT"

sensor = dht.DHT22(Pin(15))

print("Connecting to WiFi", end="")
sta_if = network.WLAN(network.STA_IF)  # set station mode for using wifi
sta_if.active(True)
sta_if.connect('Wokwi-GUEST', '')  # SSID and password of Wifi
#if not connected wait for connection
while not sta_if.isconnected():
  print(".", end="")
  time.sleep(0.1)
print(" Connected!")

#connecting to MQTT HiveMQ broker
print("Connecting to MQTT server... ", end="")
client = MQTTClient(MQTT_CLIENT_ID, MQTT_BROKER, user=MQTT_USER, password=MQTT_PASSWORD)
client.connect()
print("Connected!")

#Read the DHT Sensor and send data using MQTT, evet 5 seconds
while True:
  sensor.measure() 
  #create json of temperature and humidity
  message = ujson.dumps({
    "temp": sensor.temperature(),
    "humidity": sensor.humidity(),
  })
  print(message)
  client.publish(MQTT_TOPIC, message) #publish(topic, msg, retain=False, qos=0)
  time.sleep(5)

```
### 4.	Download and Install Node.js
### 5.	Install Node-Red
```
npm install -g node-red
```
### 6.	Start Node-red by running command in the terminal
```
node-red
```
### 7.	Access via:
```
http://localhost:1880
```
### 9.	In Node-Red: Go to Setting > Manage Palate 
> On Install type search for: following palates and install
> 1.	`@flowfuse/node-red-dashboard`
### 9.	Add Mqtt In Node and configure broker and topic
   
<img width="659" height="408" alt="image" src="https://github.com/user-attachments/assets/0c4d3086-4b01-4cd0-8314-7a8958586291" />

### 10. Add change node
<img width="632" height="218" alt="image" src="https://github.com/user-attachments/assets/92b00341-5a57-4d1e-aa23-cff60e077f80" />

### 11.	 Add chart from dashboard 2 palette. 
> Set Group and Page for each UI Nodes
> 1.	A Page will have different Groups
> 2.	Groups are logical grouping of UI components
> 3.	Eg. Room1 and Room2 can be two pages
> 4.	Room1 can have Lights, Door, Window, Temperature as Groups
### 12.	 Deploy the Flow created in the Node-red and 

<img width="844" height="240" alt="image" src="https://github.com/user-attachments/assets/0c42e5a8-2f7d-42ef-9ef7-757c2a25ca39" />

`Start the simulation in the Wokwi and observe the chart update on Node-red dashboard.`
#### Learning Reflection 
```




```

