 # EXPERIMENT-04-INTERFACING-DIGITAL-SENSOR-DHT-11-Soil-Moisture-sensor--WITH-EDGE-DEVELOPMENT-BOARD-

---

### **NAME: PRAJEETH K T**  
### **DEPARTMENT: CSE(IOT)**  
### **ROLL NO: 212222110034**  
### **DATE OF EXPERIMENT: 09-03-2026**  

---

## **AIM:**  
To interface an **Temperature and humidity sensor (DHT 11) Soil Moisture Sensor (REES52)** with the **Raspberry Pi 4** and display the sensor readings using HiveMQ cloud.

---

## **APPARATUS REQUIRED:**  
1. Raspberry Pi 4  
2. Temperature and Humidity sensor (DHT-11) and Soil Moisture Sensor (REES52)  
3. Jumper Wires  
4. Breadboard  
5. USB Cable  
6. Computer with Thonny IDE  

---

## **THEORY:**  
<img width="1293" height="744" alt="image" src="https://github.com/user-attachments/assets/3c04afa6-1517-45d2-88f1-e671d9ed1ffb" />

 ### FIGURE-01 RASPI PI 4 PINOUT DIAGRAM: 

The Raspberry Pi 4 Model B is built around a Broadcom BCM2711 system-on-chip that integrates a quad-core ARM Cortex-A72 (64-bit) CPU, VideoCore VI GPU, memory controller, and peripheral interfaces, forming a compact yet complete computer architecture where the SoC connects internally to RAM, USB 3.0 controller, Gigabit Ethernet, HDMI display, and wireless modules. Its 40-pin GPIO header provides a flexible pin configuration consisting of power pins (5 V and 3.3 V), multiple ground pins, and general-purpose input/output pins that operate at 3.3 V logic and can be programmed for digital I/O or alternate functions. Key alternate functions include I²C (SDA, SCL) for sensor communication, SPI (MOSI, MISO, SCLK, CS) for high-speed peripheral interfacing, UART (TX, RX) for serial communication, and PWM for control applications.  For communication, I2C (SDA, SCL), SPI (MOSI, MISO, SCK), and UART (TX, RX) interfaces are mapped across different GPIO pins, allowing seamless connectivity with sensors and peripherals. All GPIO pins support PWM (Pulse Width Modulation), making it useful for motor control, LED brightness adjustment, and sound applications. The BOOTSEL button enables USB mass storage mode for firmware flashing, while the DEBUG pins (SWD interface) provide debugging capabilities. With its low power consumption, flexible GPIO options, and rich interface support, the Raspberry Pi Pico is widely used for IoT, embedded systems, robotics, and automation projects.This architecture and pin multiplexing allow the Raspberry Pi 4 to act as both a general-purpose computing platform and an embedded controller, supporting rapid prototyping, hardware interfacing, and IoT applications.
## Temperature and Humidity Sensor (DHT-11):
The DHT11 is a low-cost digital sensor used to measure ambient temperature and relative humidity in embedded and IoT applications. It integrates a thermistor for temperature sensing and a capacitive humidity sensor for detecting moisture levels in the air, along with an internal 8-bit microcontroller that processes the signals and provides calibrated digital output through a single-wire communication interface. The sensor operates typically at 3.3 V to 5 V, measures temperature in the range of 0 °C to 50 °C with ±2 °C accuracy, and humidity from 20% to 80% with ±5% accuracy. Due to its simple interface, low power consumption, and reliable performance, it is widely used in weather monitoring systems, home automation, agricultural monitoring, and basic environmental data acquisition projects.

<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/5c8d35b5-4381-434f-8617-db4b8fe19154" />

### FIGURE-02 Temperature and Humidity Sensor (DHT-11)

## Soil Moisture Sensor (REES52):
The Soil Moisture Sensor (REES52) is a low-cost resistive-type sensor used to measure the volumetric water content present in soil. It operates on the principle that the electrical conductivity of soil changes with moisture level—wet soil conducts electricity better than dry soil due to the presence of water acting as a conductor between the probe electrodes. The module typically consists of two exposed metal probes and a control board with a comparator (often based on LM393), providing both analog output (for precise moisture level measurement) and digital output (for threshold-based detection). It operates at 3.3V–5V, making it compatible with microcontrollers such as Arduino and Raspberry Pi, and is widely used in irrigation control systems, smart agriculture, and automated plant watering applications.
<img width="800" height="800" alt="image" src="https://github.com/user-attachments/assets/32b40be6-1781-4459-9035-464b6064ec0f" />

 ### FIGURE-03 Soil Moisture Sensor (REES52) 

## Working Principle:
Experiment 4A
The Temperature and Humidity Sensor (DHT-11) OUT is connected to one of the GPIO pins of the Raspberry Pi 4.
The Python script sets the measure the real time temperature and Humidity output and shown in HiveMQcloud with current status and Console.
CIRCUIT DIAGRAM
Connect the Vcc of the Temperature and Humidity Sensor (DHT-11) is connected to +5V in Raspberrry Pi4.
Connect the Gnd of the Temperature and Humidity Sensor (DHT-11) is connected to Gnd in Raspberrry Pi4.
Connect the OUT to any one GPIO.


Experiment 4B
The Soil Moisture Sensor (REES52) D0 is connected one of the GPIO pins in Raspberry Pi 4.
The Soil Moisture Sensor (REES52) A0 is connected one of the GPIO pins in Raspberry Pi 4.
The Python script sets the Soil Moisture Sensor (REES52) value based on the variation in the temparature and humdity (Dry or Wet) and shown in HiveMQ Cloud and console.
CIRCUIT DIAGRAM
Connect the Soil Moisture Sensor (REES52) Vcc to any +5V.
Connect the Soil Moisture Sensor (REES52) GND to any GND.
Connect the Soil Moisture Sensor (REES52) D0 to any one GPIO. 
Connect the Soil Moisture Sensor (REES52) A0 to any one GPIO. 


## PROGRAM (Python)
### Experiment 4A
```
import Adafruit_DHT
import paho.mqtt.client as mqtt
import ssl
import time

# ---------------- DHT11 Setup ----------------
DHT_SENSOR = Adafruit_DHT.DHT11
DHT_PIN = 18   # GPIO4

# ---------------- HiveMQ Cloud Credentials ----------------
MQTT_BROKER = "97825dc4ffeb4ef69020a34b200834d7.s1.eu.hivemq.cloud"
MQTT_PORT = 8883
MQTT_USER = "hivemq.webclient.1772094968723"
MQTT_PASSWORD = "5O$4#qIHeaETiQ1js&0<"

TEMP_TOPIC = "raspberrypi/dht/temperature"
HUM_TOPIC = "raspberrypi/dht/humidity"

# ---------------- MQTT Setup ----------------
client = mqtt.Client()

client.username_pw_set(MQTT_USER, MQTT_PASSWORD)
client.tls_set(tls_version=ssl.PROTOCOL_TLS)
client.connect(MQTT_BROKER, MQTT_PORT)

print("Connected to HiveMQ Cloud")
print("Reading DHT11 Sensor...\n")

# ---------------- Main Loop ----------------
while True:
    humidity, temperature = Adafruit_DHT.read(DHT_SENSOR, DHT_PIN)

    if humidity is not None and temperature is not None:
        print(f"Temperature = {temperature} °C")
        print(f"Humidity    = {humidity} %")
        print("---------------------------")

        # Publish to HiveMQ
        client.publish(TEMP_TOPIC, temperature)
        client.publish(HUM_TOPIC, humidity)

        print("Data sent to HiveMQ\n")

    else:
        print("Sensor failure. Check wiring.")

    time.sleep(10)
 
````

### OUPUT  

![WhatsApp Image 2026-02-27 at 1 21 49 PM](https://github.com/user-attachments/assets/25d2b24e-6870-4f53-ad7a-fe125e7eadc0)
 
![WhatsApp Image 2026-02-27 at 1 22 25 PM](https://github.com/user-attachments/assets/1c310c38-ab02-4699-bef1-1bd844d374b6)

<img width="1910" height="897" alt="Screenshot 2026-02-26 143810" src="https://github.com/user-attachments/assets/efeafa40-5875-44a0-8bfd-0cfadb591cf5" />


## PROGRAM (Python)
### Experiment 4B
```
import RPi.GPIO as GPIO
import time
import paho.mqtt.client as mqtt
import json

# ---------------- GPIO Setup ----------------
DIGITAL_PIN = 23   # D0 connected to GPIO 23

GPIO.setmode(GPIO.BCM)
GPIO.setup(DIGITAL_PIN, GPIO.IN)

# ---------------- MQTT Setup ----------------
broker = "97825dc4ffeb4ef69020a34b200834d7.s1.eu.hivemq.cloud"
port = 8883
topic = "Soil Moisture"

username = "hivemq.webclient.1772178873750"
password = "9yj!U%ix7#Do3H2.VMGc"

client = mqtt.Client()
client.username_pw_set(username, password)
client.tls_set()  # Required for HiveMQ Cloud (TLS)

client.connect(broker, port)
client.loop_start()

print("Connected to HiveMQ Cloud")

# ---------------- Main Loop ----------------
try:
    while True:
        digital_value = GPIO.input(DIGITAL_PIN)

        # Convert Digital Value
        if digital_value == 0:
            moisture_status = "WET"
            humidity_value = 100
        else:
            moisture_status = "DRY"
            humidity_value = 0

        payload = {
            "temperature": humidity_value,  # shown like temperature
            "humidity": moisture_status     # shown like humidity
        }

        client.publish(topic, json.dumps(payload))
        print("Published:", payload)

        time.sleep(2)

except KeyboardInterrupt:
    GPIO.cleanup()
    print("Program Stopped")
 
````

### OUPUT  
4![WhatsApp Image 2026-02-27 at 1 52 56 PM](https://github.com/user-attachments/assets/3e51a810-7668-4f1c-bbd7-11c4cdf73424)

![WhatsApp Image 2026-02-27 at 1 58 23 PM](https://github.com/user-attachments/assets/65fa5d37-f7c0-45a4-b803-b325f1299da8)

<img width="1919" height="895" alt="Screenshot 2026-02-27 134657" src="https://github.com/user-attachments/assets/113b74b4-fbc6-43c6-890a-a64e771a0712" />


## **RESULT:**  
The **Temperature and humidity sensor (DHT 11) Soil Moisture Sensor (REES52)** was successfully interfaced with the **Raspberry Pi 4**, and real-time **Temperature, Humidity and Soil Moisture level** were read and displayed in Console and HiveMq Cloud. 

---

