# DataGatherer
複数のESP32からソケット通信でデータを受信するプログラム

## Overview
This project consists of scripts for setting up a microcontroller (like an ESP32) to manage network connections and run a simple server. The main components are boot.py for network configuration and ch_main.py for handling HTTP server functionality.

You can get MicroPython library for INA219 from this link below:

<https://github.com/chrisb2/pyb_ina219.git> 

## Files
### boot.py

・Network Configuration: SSID名を定義して，password.pyをinstall 

・Wi-Fi Functions:

&emsp;・`wifiscan()`: Scans available Wi-Fi networks.

&emsp;・`connect_lab_wifi(timeout)`: Connects to a specified lab Wi-Fi network.

&emsp;・`connect_home_wifi(timeout)`: Connects to a specified home Wi-Fi network.

&emsp;・`connect_esp_wifi(timeout)`: Connects to a specified ESP Wi-Fi network.

・Access Point Functions:

&emsp;・`ap_activate()`: Activates the device's access point.

&emsp;・`ap_deactivate()`: Deactivates the device's access point.

・System Initialization: Prints a boot message and executes ch_main.py.

### ch_main.py
The script hadles the following:

・Server Setup:

&emsp;・Initializes and starts a server socket on port 80.

・Client Handling:

&emsp;・Manages client connections, receives data, and handles errors.

・Main Execution:

&emsp;・Activates the access point via `boot.ap_activate()`.

&emsp;・Starts the server and continuously accepts client connections, spawning new threads to handle each connection.

### cm_main.py
This script handles Wi-Fi connectivity and data transmission.

・Imports:

&emsp;・`boot`: Contains functions for Wi-Fi connection.

&emsp;・`socket`, `time`, `_thread`: Standard Python modules.

・Global Variables:

&emsp;・`s`: A socket object.

・Functions:

&emsp;・`send()`: Creates a socket connection to a server.

&emsp;・`send_to_server()`: Connects to Wi-Fi and sends a message to the server at regular intervals.

・Main Execution:

・Starts a new thread that runs the send_to_server function.

### get_current.py
This script interfaces with an INA219 sensor to measure and log voltage, current, and power.

・Imports

&emsp;・Modules for handling hardware interaction (Pin, I2C, RTC), INA219 sensor (INA219), logging (INFO), time, and threading (_thread).

・Constants

&emsp;・SHUNT_OHMS: The value of the shunt resistor.

・Initialization

&emsp;・Initializes I2C communication.

&emsp;・Configures the INA219 sensor.

・Functions:

&emsp;・`get_info()`:Retrieves voltage, current, and power measurements from the INA219 sensor.

&emsp;・`write_to_csv()`:Writes the measurements to a CSV file with a timestamp.

・Main Execution

&emsp;・Starts a new thread to run the write_to_csv function.

## Setup
Create a `password.py` file with the required Wi-Fi passwords:

```
LAB_WIFI_PASS = 'your_lab_wifi_password'
HOME_WIFI_PASS = 'your_home_wifi_password'
WEBREPL_PASS = 'your_webrepl_password'
```

## Usage - CH
1. Upload `boot.py`, `ch_main.py`, and `password.py` to your ESP32.
2. Execute the `boot.py` script. This will configure the network settings and start the web server.

## Output - CH
```
booted system
----  AP is activated -----
('192.168.4.1', '255.255.255.0', '192.168.4.1', '0.0.0.0')
Server started at 192.168.4.1:80
accepting......
```

CH accepts message from multiple nodes. 
```
connected from:  ('192.168.4.2', 52469)
abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz
accepting......
connected from:  ('192.168.4.3', 64312)
hello from F
accepting......
```


## Usage - CM

・Ensure the necessary hardware components (Wi-Fi module, INA219 sensor) are connected and configured properly.

・Run cm_main.py to establish Wi-Fi connectivity and start sending data to the server.

・Run get_current.py to start logging voltage, current, and power measurements to a CSV file.

## Output - CM
```
>>> ---ESPのWi-Fi[ESP_F]に接続��ます---
---- wifi is connected ----
----[192.168.4.2]に接続----
Wi-Fi connected
Connected to 192.168.4.1:80
Sent: abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz
```
The output CSV file

<img width="285" alt="image" src="https://github.com/user-attachments/assets/95decdfa-d749-44fd-a2d0-bde2a8c5a9db">

