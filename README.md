***
## Table of Contents
### 1. Introduction
### 2. Software
### 2.1 Python
### 2.2 Arduino C++
### 3. Hardware Setup
### 4. Conclusion

***

## 1. Introduction to the ESP32 IoT Telemetry Dashboard

<img width="743" height="453" alt="ESP32 Telemetry Dashboard" src="https://github.com/user-attachments/assets/58423d85-f52f-4420-91cf-fa174ff6e26f" />

This project demonstrates a complete IoT telemetry system built around the ESP32 microcontroller. The system showcases three core technical capabilities:

- **Real-Time Environmental Data Acquisition:** Using I2C sensors (BME280) to measure temperature, humidity, and atmospheric pressure with continuous data sampling and processing.
- **HTTP-Based Wireless Communication:** Implementing RESTful API communication between embedded hardware and desktop software over WiFi networks, handling JSON serialization and HTTP request/response cycles.
- **Full-Stack Dashboard Interface:** Developing a Flask-based web server with Tkinter GUI for real-time data visualization, command transmission, and system monitoring.

---

## 2. Software Architecture

<img width="461" height="301" alt="Software Stack" src="https://github.com/user-attachments/assets/dcb99a37-6968-4cb4-9dec-20f8fe47530f" />

The software stack consists of two primary components: a Python-based desktop application handling data visualization and HTTP server functionality, and Arduino C++ firmware managing sensor integration and network communication on the ESP32.

The system architecture follows a client-server model where the ESP32 acts as an HTTP client periodically posting sensor data to a Flask server running on the host machine. The server processes incoming telemetry, stores it in memory, and renders real-time graphs through a Tkinter-based GUI using Matplotlib for visualization.

This section provides step-by-step instructions for setting up both software components, establishing network connectivity, and initiating bidirectional data streaming between hardware and software layers.

### 2.1 Python

**Setup Instructions:**

1. Open your IDE (VS Code recommended).
2. Create a new project folder for the Python application.
3. Create a `main.py` file in your project directory.
4. Clone or copy the `main.py` code from this repository: [https://github.com/deneskosztyuk/_Guide-Deep-Space-Probe-Simulator/blob/main/main.py](https://github.com/deneskosztyuk/_Guide-Deep-Space-Probe-Simulator/blob/main/main.py)
5. Open the integrated terminal in VS Code (`Ctrl + `` or View > Terminal).
6. Install required dependencies by running:
   ```
   pip install requests pillow flask matplotlib
   ```

**Library Dependencies:**

- **Standard Python Libraries:**
  - `os`: Operating system interface for file and directory operations.
  - `signal`: Asynchronous event handling for graceful shutdown.
  - `threading`: Multi-threaded execution for concurrent GUI and server operations.
  - `itertools`: Efficient iteration tools for data processing.
  - `datetime`: Timestamp management for telemetry data.
  - `math`: Mathematical operations for data calculations.

- **Third-Party Libraries:**
  - `requests`: HTTP client library for network communication.
  - `tkinter`: GUI framework for desktop interface rendering.
  - `PIL (Pillow)`: Image processing for GUI assets.
    - `Image`: Image manipulation and loading.
    - `ImageTk`: Tkinter-compatible image objects.
    - `ImageSequence`: GIF animation frame handling.
  - `Flask`: Lightweight HTTP server framework.
    - `Flask`: WSGI application instance.
    - `request`: HTTP request object handling.
    - `jsonify`: JSON response serialization.
  - `Matplotlib`: Data visualization library.
    - `Figure`: Plot container for telemetry graphs.
    - `FigureCanvasTkAgg`: Tkinter backend for Matplotlib integration.
    - `GridSpec`: Subplot layout management.

After installation, all dependencies will be satisfied and the application should run without import errors.

### 2.2 Arduino IDE & C++

**Setup Instructions:**

1. Download and install Arduino IDE: [https://www.arduino.cc/en/software](https://www.arduino.cc/en/software)
2. Open Arduino IDE and navigate to `Sketch > Include Library > Manage Libraries...`
3. Install the following libraries (search by name and click Install):
   - **WiFi**: ESP32 WiFi connectivity and network stack.
   - **Wire**: I2C communication protocol implementation.
   - **Adafruit_Sensor**: Unified sensor driver interface.
   - **Adafruit_BME280**: BME280 sensor driver (temperature, humidity, pressure).
   - **HTTPClient**: HTTP request/response handling.
   - **ArduinoJson**: JSON encoding and decoding library.

**Note:** Some libraries have dependencies—the Arduino Library Manager will automatically resolve and install these.

4. Copy the `.ino` firmware code from this repository: [https://github.com/deneskosztyuk/_Guide-Deep-Space-Probe-Simulator/blob/main/.ino](https://github.com/deneskosztyuk/_Guide-Deep-Space-Probe-Simulator/blob/main/.ino)

**Network Configuration:**

Locate the following lines in the `.ino` file:

```cpp
const char* ssid = "ENTER YOUR WIFI NETWORKS NAME EXACTLY";
const char* password = "CHANGE THIS TO YOUR NETWORKS PASSWORD";
```

Replace the placeholder values:
- `ssid`: Enter your WiFi network name exactly as it appears.
- `password`: Enter your WiFi network password.

This ensures both the ESP32 and your computer are on the same local network for HTTP communication.

**Server Endpoint Configuration:**

On line 36, locate:

```cpp
http.begin("CHANGE THIS TO YOUR IPv4/sensor-data");
// Example: http.begin("192.168.1.100/sensor-data");
```

When you run the Python `main.py` script, the console will display your machine's local IPv4 address. Copy this address and replace the placeholder in the `.ino` file:

```cpp
http.begin("192.168.1.XXX/sensor-data");  // Use your actual IPv4
```

This completes the software configuration.

***

## 3. Hardware Setup

**Required Components:**

- Breadboard (400-830 tie points)
- ESP32 Development Board
- Micro USB cable (data-capable, not charge-only)
- BME280 Environmental Sensor (I2C interface)
- Male-to-male and male-to-female jumper wires
- LEDs (optional, for status indication)
- 220Ω resistor (if using LED)

**Assembly Instructions:**

1. **Power Rail Setup:**
   - Connect both positive rails (`+`) on the breadboard using a red jumper wire.
   - Connect both ground rails (`-`) on the breadboard using a black jumper wire.
   - This distributes power and ground across the entire breadboard.

2. **Power Distribution:**
   - Connect the ESP32's `3V3` pin to the breadboard's `+` rail.
   - Connect the ESP32's `GND` pin to the breadboard's `-` rail.

3. **BME280 Sensor Wiring:**
   - Place the BME280 on the breadboard.
   - Connect BME280 `GND` → Breadboard `-` rail
   - Connect BME280 `VIN` (or `VCC`) → Breadboard `+` rail
   - Connect BME280 `SCL` → ESP32 pin `22` (I2C clock)
   - Connect BME280 `SDA` → ESP32 pin `21` (I2C data)

4. **Status LED (Optional):**
   - Connect LED cathode (short leg) → Breadboard `-` rail
   - Connect LED anode (long leg) → 220Ω resistor → ESP32 pin `23`

***

## 4. Conclusion

<img width="978" height="521" alt="Dashboard Interface" src="https://github.com/user-attachments/assets/49573bd1-83e5-4767-b336-120f9fa844f6" />

**Running the System:**

1. **Upload Firmware:**
   - Click `Verify` in Arduino IDE to compile the firmware.
   - Click `Upload` to flash the ESP32.
   - Open `Serial Monitor` (magnifying glass icon or `Tools > Serial Monitor`) to verify BME280 readings.

2. **Launch Dashboard:**
   - Run `main.py` in VS Code.
   - The Flask server will start and display your local IPv4 address in the console.
   - The Tkinter GUI will launch, showing real-time telemetry data.

3. **System Operation:**
   - The ESP32 will automatically connect to WiFi and begin transmitting sensor data via HTTP POST requests.
   - The dashboard will visualize temperature, humidity, and pressure in real-time graphs.
   - You can interact with the GUI to monitor system status and performance metrics.

This project demonstrates end-to-end IoT system development, from embedded firmware and hardware integration to full-stack software architecture and real-time data visualization.

***
