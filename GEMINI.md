# Project: ESP32 Weather Station

## Overview
This project is an **ESP32-based Weather Station** that monitors environmental data using a **BME280 sensor** (Temperature, Humidity, Pressure) and displays it on an **SSD1306 OLED screen**. It also features a **Wi-Fi Access Point with a Captive Portal**, allowing users to view data and perform **OTA (Over-The-Air) firmware updates** via a web interface.

The software is built using **PlatformIO** with the **Arduino framework** and leverages **FreeRTOS** to manage concurrent tasks (Sensor reading, Display update, Web Server handling).

## Tech Stack
*   **Hardware:** ESP32 Dev Module, BME280 Sensor (I2C), SSD1306 OLED (I2C).
*   **Platform:** PlatformIO (VS Code).
*   **Framework:** Arduino.
*   **OS:** FreeRTOS (built-in to ESP32 Arduino core).
*   **Libraries:**
    *   `Adafruit BME280 Library` (Sensor)
    *   `Adafruit SSD1306` & `Adafruit GFX` (Display)
    *   `ESPAsyncWebServer` & `AsyncTCP` (Web Interface & OTA)

## Directory Structure
*   **`src/`**: Source code files.
    *   `main.cpp`: Entry point, setup, and FreeRTOS task creation.
    *   `config.h`: Central configuration (Pins, I2C addresses, Wi-Fi settings, Task priorities).
    *   `sensor.cpp/.h`: BME280 sensor logic (thread-safe).
    *   `display.cpp/.h`: SSD1306 OLED display logic.
    *   `webserver.cpp/.h`: AsyncWebServer and Captive Portal logic.
    *   `ota.cpp/.h`: OTA update handling.
*   **`platformio.ini`**: PlatformIO build configuration and dependencies.
*   **`weatherStation/`**: (Legacy) Original Arduino IDE sketch version.
*   **`weatherStation_fsd.md`**: Functional Specification Document.

## Build & Run

### Prerequisites
*   **PlatformIO Core** (CLI) or **PlatformIO IDE** (VS Code extension).

### Build Environments
The project defines two build environments in `platformio.ini`:

1.  **`env:esp32dev`** (Default):
    *   Compiles and uploads via **USB Serial**.
    *   Use this for the initial flash.

2.  **`env:esp32dev_ota`**:
    *   Compiles and uploads via **Wi-Fi (OTA)**.
    *   Target IP: `192.168.4.1` (Default AP IP).
    *   Use this for subsequent wireless updates.

### Commands

| Action | Command | Description |
| :--- | :--- | :--- |
| **Build** | `pio run` | Compiles the project for the default environment. |
| **Upload (USB)** | `pio run -e esp32dev -t upload` | Flashes firmware via USB cable. |
| **Upload (OTA)** | `pio run -e esp32dev_ota -t upload` | Flashes firmware via Wi-Fi (Connect to `Wetterstation` AP first). |
| **Monitor** | `pio device monitor` | Opens the serial monitor (Baud: 115200). |
| **Clean** | `pio run -t clean` | Cleans build artifacts. |

## Usage
1.  **Power On:** Connect the ESP32 to power.
2.  **OLED Display:** Shows current Temperature, Humidity, and Pressure immediately.
3.  **Wi-Fi Access Point:**
    *   Connect to the Wi-Fi network named **`Wetterstation`**.
    *   No password required (Open network).
4.  **Web Interface:**
    *   A Captive Portal should automatically open.
    *   If not, navigate to `http://192.168.4.1` in your browser.
    *   **Live Data:** View real-time sensor readings (auto-refreshes every 2s).
    *   **Update Firmware:** Upload a new `.bin` file to update the device wirelessly.

## Development Conventions
*   **Configuration:** All adjustable parameters (Pins, Intervals, WiFi Credentials) are located in `src/config.h`. Modify this file to change settings.
*   **Concurrency:** The system uses FreeRTOS tasks.
    *   `TaskMeasure`: Reads sensor data (High Priority).
    *   `TaskDisplay`: Updates OLED (Medium Priority).
    *   `loop()`: Handles Web Server and OTA (Low Priority/Idle).
*   **Thread Safety:** Shared resources (like I2C bus or global data) should be protected (e.g., via Mutexes) if accessed by multiple tasks, though currently, the architecture separates concerns to minimize conflict.
*   **Style:** Follow standard C++ / Arduino coding conventions.
