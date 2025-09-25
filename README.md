# **🎮 ESP32 WiFi Joystick to iBUS Translator**

| Project Type | RC Translator | Board | ESP32 | Wireless | WiFi |
| :---- | :---- | :---- | :---- | :---- | :---- |
| **Input** | UDP Packets | **Output** | iBUS Protocol | **Interface** | Mobile App \+ Web |

## **This project transforms an ESP32 development board into a wireless iBUS transmitter for controlling devices with flight controllers like Betaflight, INAV, or Ardupilot. It uses a smartphone as a wireless joystick, receiving control inputs via UDP packets and translating them into the iBUS protocol for your flight controller.**

## **✨ Key Features**

* **Wireless Control:** Use a smartphone or tablet as a virtual joystick via a simple UDP app.  
* **iBUS Protocol:** Translates joystick inputs into the standard iBUS protocol, making it compatible with a wide range of flight controllers.  
* **Standalone Operation:** The ESP32 acts as its own WiFi Access Point, so no external router is needed.  
* **Web Interface:** A built-in web server allows for wireless control of auxiliary channels (switches) and provides live telemetry.  
* **Failsafe Support:** The system is designed to trigger the flight controller's failsafe protocol if the WiFi connection is lost.  
* **Minimal Dependencies:** The code uses only built-in libraries from the Arduino ESP32 core, with no external iBUS library required.

## **🛠️ Hardware Required**

| Component | Notes |
| :---- | :---- |
| **ESP32 Dev Board** | Any ESP32 board with WiFi and UART functionality. |
| **Flight Controller** | Must accept **iBUS** input. |
| **Smartphone/Tablet** | To run a UDP joystick app. |

## **🏗️ System Breakdown**

The project operates in two main parts: the wireless communication from the mobile device and the protocol translation on the ESP32.

### **ESP32 (Microcontroller)**

* **WiFi Access Point:** The ESP32 creates a WiFi network with a specified SSID and password.  
* **UDP Server:** It listens for UDP packets on port 1234 to receive joystick data.  
* **iBUS Translator:** It converts the received data into the iBUS protocol and outputs it over the UART pin.  
* **Web Server:** It hosts a simple web page with controls for four auxiliary switches (AUX1 to AUX4) and displays live telemetry.

### **Smartphone App (Client)**

* **UDP Joystick App:** You need a mobile application (e.g., a generic UDP joystick app) that can send joystick values over UDP.  
* **Control Input:** The app sends stick movements as data packets to the ESP32.

## **⬇️ Installation and Setup**

1. **Flash the ESP32:** Upload the provided sketch to your ESP32 board using the Arduino IDE.  
2. **Wire the Board:** Connect the ESP32's **UART TX pin** (in the code) to your flight controller's **iBUS RX pad**.  
3. **Connect to WiFi:** On your smartphone, connect to the WiFi network created by the ESP32 (SSID and password are in the code).  
4. **Configure App:** Set up your UDP joystick app to send packets to the ESP32's IP address (192.168.4.1) on port 1234\.  
5. **Access Web Interface:** Navigate to http://192.168.4.1/ in your browser to access the AUX switch controls.

## **⚠️ Notes**

* **Failsafe:** If the UDP connection is lost, the ESP32 will stop sending data, allowing the flight controller's built-in failsafe to activate.  
* **Power:** Ensure both the ESP32 and the flight controller have a stable power supply.  
* **Pin Configuration:** Verify the correct UART TX pin is used in the code to match your ESP32 board.
