ESP32 iBUS WiFi Translator for Betaflight/INAV

This project turns an ESP32 into a WiFi-based iBUS transmitter that forwards control signals from a mobile app (UDP joystick) and a web interface (AUX switches) to a Flight Controller (FC) running Betaflight/INAV.

It allows you to control a drone or robot over WiFi using any app that sends UDP packets in the 1500150015001500 format (4 channels, 1000–2000 µs).

✨ Features

📡 ESP32 runs in WiFi Access Point mode (192.168.4.1)

🎮 Receives stick input over UDP (port 1234)

🔄 Translates inputs into iBUS protocol and sends to FC via UART (default TX pin = 17)

🎛️ Web interface at http://192.168.4.1/ with AUX 1–4 switches

⚡ Instant updates (no page reloads, uses AJAX)

✅ Failsafe handled by FC:

If WiFi disconnects → ESP stops sending iBUS → FC triggers its own failsafe

If UDP packets stop → ESP continues sending the last valid channel values

🛠️ Channels mapped to AETR1234 (Roll, Pitch, Yaw, Throttle)

📐 Channel Mapping (AETR1234)
Stick / Control	UDP Channel	iBUS Channel
Throttle	Ch1	Ch4
Yaw	Ch2	Ch3
Roll	Ch3	Ch1
Pitch (inverted)	Ch4	Ch2
AUX1 (Mode 1)	Web Switch	Ch5
AUX2 (Mode 2)	Web Switch	Ch6
AUX3 (Mode 3)	Web Switch	Ch7
AUX4 (Mode 4)	Web Switch	Ch8
📦 Requirements

Hardware

ESP32 (any dev board with WiFi + UART)

Flight Controller that accepts iBUS input

Libraries

Uses built-in Arduino ESP32 core libraries (WiFi.h, WebServer.h, WiFiUdp.h)

No external iBUS library needed (iBUS frame built manually)

⚙️ Setup

Flash the ESP32 with this sketch

Connect ESP32 TX (pin 17) to the iBUS RX pad of your FC

Power up ESP32

Connect your phone to the ESP32 WiFi:

SSID: ESP_Drone

Password: 12345678

Open the UDP Joystick App and set:

Host: 192.168.4.1

Port: 1234

Open http://192.168.4.1/ in your browser for AUX switches

🖥️ Web Interface

4 switch buttons: Mode 1 → AUX1, Mode 2 → AUX2, etc.

ON = 1500, OFF = 1000

Live telemetry of current stick values (Throttle, Roll, Pitch, Yaw)

🚨 Failsafe Behavior

If WiFi disconnects → ESP stops sending iBUS → FC’s failsafe activates

If UDP packets stop → ESP keeps sending last known values

🔧 Example Use Cases

Control a drone with your phone

Use AUX switches to toggle flight modes (Angle/Horizon/Acro, GPS Hold, Return-to-Home, etc.)

Control robots, RC cars, or boats with custom UDP joystick apps
