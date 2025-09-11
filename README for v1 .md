🚁 ESP32 WiFi Joystick to iBUS Translator

This project turns an ESP32 into a WiFi receiver that takes joystick control packets from a phone (via UDP) and translates them into the iBUS protocol for use with flight controllers running Betaflight / INAV / Ardupilot.

It also hosts a web interface for AUX mode switching and live status monitoring.

✨ Features

📡 WiFi Access Point mode (ESP_Drone, password 12345678)

🎮 Receives UDP joystick packets (e.g. from mobile RC apps)

🔄 Translates to iBUS protocol (14 channels supported)

🕹️ Stick mapping with AETR1234 convention:

CH1 → Roll

CH2 → Pitch (inverted for Betaflight)

CH3 → Throttle

CH4 → Yaw

⚙️ AUX channels (CH5–CH8) controlled via a web UI with ON/OFF toggles

🛡️ Failsafe:

Will not arm until throttle stick is at minimum

Stops sending data if phone disconnects or no packets received

Channels reset to safe values on failsafe

🛠️ Hardware Setup

Board: ESP32 DevKit or similar

Connections:

ESP32 GPIO17 (TX) → Flight Controller RX pin (set to iBUS / Serial RX)

GND → GND

🔌 Wiring Example
ESP32 GPIO17  --->  FC UART RX (configured for iBUS input)
ESP32 GND     --->  FC GND

📶 Network & App

ESP32 runs as an Access Point:

SSID: ESP_Drone

Password: 12345678

IP: 192.168.4.1

UDP Port: 1234

Joystick app should send 16-byte packets with 4 channels in ASCII (e.g., "1500150015001500").

🌐 Web Interface

Open http://192.168.4.1/ in a browser.

Shows Throttle, Roll, Pitch, Yaw percentage

Displays connection status and failsafe status

Provides Mode1–Mode4 toggles (mapped to CH5–CH8):

OFF → 1000

ON → 1500

📜 Channel Mapping
Channel	Function	Source
CH1	Roll	Joystick X (Right stick)
CH2	Pitch	Joystick Y (Right stick, inverted)
CH3	Throttle	Joystick Y (Left stick)
CH4	Yaw	Joystick X (Left stick)
CH5	Aux1	Mode 1 toggle
CH6	Aux2	Mode 2 toggle
CH7	Aux3	Mode 3 toggle
CH8	Aux4	Mode 4 toggle
🚨 Failsafe Behavior

At startup, throttle must be below 1500 before the ESP32 starts sending iBUS data.

If UDP packets stop (>1s timeout), channels are set to:

Roll = 1500

Pitch = 1500

Yaw = 1500

Throttle = 1000

AUX channels remain at last state.

📂 Code Structure

setup(): Initializes WiFi AP, web server, UDP, and iBUS serial.

loop():

Handles web requests

Receives UDP packets and parses channels

Updates channel values (with pitch inversion)

Sends iBUS packets if safe

Applies failsafe if disconnected

sendIBUS(): Encodes channel data into iBUS packet with checksum.

buildHTML(): Renders web UI with AUX toggles and live status.

🖥️ Example Serial Log
[WiFi AP] IP: 192.168.4.1
[Web] Server started
[UDP] Listening on port 1234
[iBUS] Output initialized
[UDP] Packet received: 1500150015001500
[INFO] Throttle OK, ready to send iBUS
[AUX] Aux1 set to ON
[FAILSAFE] UDP timeout, channels set to safe
