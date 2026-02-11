# Functional Specification: ESP32 Weather Station

## 1. Introduction

### 1.1 Purpose of the Document
This document describes the functional and non-functional requirements for the ESP32 Weather Station. It serves as a reference for development, validation, and future expansions of the system.

### 1.2 Scope
The specification covers data acquisition of temperature, humidity, and atmospheric pressure using a BME280 sensor, the display of this data on an SSD1306 OLED display, and the underlying software architecture based on FreeRTOS.

### 1.3 Target Audience
This document is intended for developers, testers, and anyone interested in gaining a deep understanding of the weather station.

## 2. Product Overview

### 2.1 Brief Description
The ESP32 Weather Station is an autonomous device that continuously collects environmental data (temperature, humidity, atmospheric pressure) and displays it in real-time on a small OLED screen. The system is robust, energy-efficient, and leverages the advantages of a real-time operating system (FreeRTOS) for reliable and responsive performance.

### 2.2 Goals and Vision
The primary goal is to provide an easy-to-build and easy-to-use weather station that delivers precise measurements and serves as a basis for further IoT projects.

## 3. Functional Requirements

### 3.1 Sensor Data Acquisition
*   **FR-1.1:** The system MUST collect temperature readings from the BME280 sensor.
*   **FR-1.2:** The system MUST collect humidity readings from the BME280 sensor.
*   **FR-1.3:** The system MUST collect atmospheric pressure readings from the BME280 sensor.
*   **FR-1.4:** Sensor data MUST be updated at least every 2 seconds.
*   **FR-1.5:** The system MUST provide a way to safely synchronize the I2C bus between different tasks.

### 3.2 Data Display on OLED
*   **FR-2.1:** The system MUST display the current temperature on the SSD1306 OLED display.
*   **FR-2.2:** The system MUST display the current humidity on the SSD1306 OLED display.
*   **FR-2.3:** The system MUST display the current atmospheric pressure on the SSD1306 OLED display.
*   **FR-2.4:** The OLED display MUST be updated at least every 1 second.
*   **FR-2.5:** The displayed values MUST be clearly legible and include the corresponding units (e.g., °C, %RH, hPa).

### 3.3 Configuration
*   **FR-3.1:** Configuration parameters such as display dimensions, I2C addresses of sensors and displays, and sea-level pressure MUST be centrally defined in a dedicated header file (`config.h`).

### 3.4 Error Handling
*   **FR-4.1:** The system MUST output an error message to the serial console if the OLED display cannot be initialized.
*   **FR-4.2:** The system MUST output an error message to the serial console if the BME280 sensor cannot be initialized.
*   **FR-4.3:** In the event of a critical initialization error (display or sensor), the system MUST remain in an error mode or cease operation.

## 4. Non-Functional Requirements

### 4.1 Performance
*   **NFR-1.1:** Sensor data acquisition and display updates MUST occur in parallel and without noticeable delay.
*   **NFR-1.2:** The use of FreeRTOS MUST ensure efficient resource management.

### 4.2 Reliability
*   **NFR-2.1:** The system MUST operate without interruption over long periods (weeks/months).
*   **NFR-2.2:** I2C communication errors SHOULD be handled robustly to avoid system crashes.

### 4.3 Maintainability
*   **NFR-3.1:** The code MUST be modular and easy to understand, divided into logical header files.
*   **NFR-3.2:** System configuration MUST be easily adjustable by modifying `config.h`.

### 4.4 Safety (Physical)
*   **NFR-4.1:** The system SHOULD be housed in an enclosure to protect the electronics from environmental influences (external, not part of this specification).

### 4.5 Usability (Display)
*   **NFR-5.1:** The displayed information MUST be understandable at a glance.

## 5. Technical Architecture (Brief)

### 5.1 Hardware Components
*   **Microcontroller:** ESP32 (ESP32 Dev Module)
*   **Sensor:** BME280 (Temperature, Humidity, Pressure)
*   **Display:** SSD1306 OLED (I2C, 128x64 pixels)

### 5.2 Software Components
*   **Real-Time Operating System:** FreeRTOS
*   **Programming Language:** C++ (Arduino Framework)
*   **Libraries:**
    *   `Wire.h` (I2C communication)
    *   `Adafruit_GFX.h` (Graphics library for displays)
    *   `Adafruit_SSD1306.h` (Driver for SSD1306 OLED)
    *   `Adafruit_BME280.h` (Driver for BME280 sensor)

## 6. Interfaces

### 6.1 User Interface (OLED)
*   Display of temperature, humidity, and atmospheric pressure.

### 6.2 Hardware Interfaces
*   **I2C Bus:** Use of GPIO 21 (SDA) and GPIO 22 (SCL) on the ESP32.
*   **Power Supply:** 3.3V for ESP32, BME280, and SSD1306.

### 6.3 Serial Interface
*   Use of the serial interface for debugging output at 115200 baud.

## 7. Open Points / Future Expansions

*   **WLAN Connection:** Integration of a function to transmit weather data to a server or cloud service.
*   **Data Logging:** Storage of historical weather data on an SD card or in the internal flash memory of the ESP32.
*   **Web Server:** Implementation of a simple web server on the ESP32 to display data in the local network.
*   **Calibration:** Possibility to calibrate sensor data.
*   **Time:** Integration of a real-time clock (RTC) for timestamping data.
