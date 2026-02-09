# Ambient Event Fingerprinting (AEF)
**Non-Intrusive Event Detection using Thermodynamic Derivatives**

![Status](https://img.shields.io/badge/Status-Prototype-orange) ![Platform](https://img.shields.io/badge/Platform-ESP8266-blue) ![License](https://img.shields.io/badge/License-MIT-green)

# IN PROJGRESS>>>>>>>>>>> 

## Overview
This project explores a privacy-preserving way to detect physical events in a room—specifically **"Window Open"** vs **"AC On"**—without using cameras or microphones.

The core idea is that different events change the room's air physics in unique ways. By tracking the **rate of change** ($dT/dt$) and humidity volatility ($dH/dt$) on an ESP8266, I can distinguish between a natural draft and artificial cooling.

I built this because standard motion sensors (PIR) can't tell if a window was left open, and contact sensors are a hassle to install on every window.

## Hardware Stack
The setup is intentionally minimal to test if this can run on low-end hardware.

| Component | Role | Notes |
| :--- | :--- | :--- |
| **ESP8266 (NodeMCU)** | MCU | Handles sensor reading and logic. |
| **DHT11** | Temp/Hum Sensor | *Limitation:* 2-second refresh rate. |
| **HC-05** | Bluetooth | Used for wireless debugging/logging. |
| **Water Sensor** | Verification | (Optional) Used initially to verify draft speed. |

## The Concept
Raw temperature readings aren't enough because thermal inertia makes them slow. Instead, this project calculates the **derivative** (speed of change).

1.  **Window Open:** Causes a sharp drop in Temperature + High Variance in Humidity (chaotic airflow).
2.  **AC On:** Causes a drop in Temperature + Drop in Humidity (dehumidification effect).
3.  **Human Presence:** Slow rise in Temperature + Rise in Humidity.

By mapping these patterns, the ESP8266 can guess the "State" of the room.

## Setup & Wiring

### Circuit
* **DHT11 Data** -> D1 (GPIO 5)
* **DHT11 VCC** -> 3.3V
* **HC-05 RX/TX** -> SoftwareSerial Pins (D2/D3)

### Installation
1.  Clone the repo:
    ```bash
    git clone [https://github.com/BhashkarFulara69/Ambient-Event-Fingerprinting.git](https://github.com/BhashkarFulara69/Ambient-Event-Fingerprinting.git)
    ```
2.  Open `src/main.cpp` in Arduino IDE.
3.  Install the **DHT sensor library** (Adafruit) via the Library Manager.
4.  Upload to the ESP8266.

*Note: The sensor needs to be placed away from direct sunlight for accurate baseline readings.*

## Results So Far
Testing in a 12x12ft room:

* **Window Detection:** The system picks up a window opening within ~30-60 seconds in winter (when outside air is colder).
* **False Positives:** Sometimes triggered by the central heating turning off.
* **Sensor Lag:** The DHT11 is a bit slow. A BME280 would definitely be better for future versions, but the logic holds up.

## Known Issues
* **Summer Performance:** The model struggles when indoor and outdoor temperatures are nearly identical.
* **Drift:** The baseline needs to reset every few hours to account for time-of-day temperature changes.

## Future Plans
* Upgrade to a BME280 sensor for faster response times.
* Implement a proper rolling buffer to filter out sensor noise.
* Add a simple decision tree directly on the ESP8266 to trigger the relay automatically.

---
*Created by Bhashkar*