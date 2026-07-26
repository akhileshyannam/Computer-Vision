# Computer-Vision

# Computer Vision Physical Object Tracking

A complete hands-on guide and code repository for mastering basic computer vision tracking using Python and physical hardware. 

This project tracks physical objects in real time using a camera feed processed on your computer, which then sends positioning commands via Serial to an **ESP32** controlling a 2-axis **Pan/Tilt servo mount**.

---

## ⚠️ Hardware Safety & Common Pitfalls

* **Shared Ground (Crucial):** Always connect the **GND** of your external 5V power supply directly to a **GND** pin on the ESP32. Without a common ground reference, serial signal noise will corrupt servo signals and cause erratic jitter.
* **External Power Required:** Do **not** power the servos directly from the ESP32’s 3.3V or 5V (VIN) pins. Servos draw significant current spikes during movement that will cause the ESP32 to brown out, reset unexpectedly, or permanently damage the board's onboard voltage regulator.
* **Voltage Rating Check:** Standard micro-servos (like SG90s or MG996Rs) run on 5V–6V. Feeding them higher voltages (e.g., direct 2S LiPo battery power at 7.4V+) without a buck converter will burn out their control boards.
* **Current Headroom:** Ensure your external 5V power supply can supply at least 2A to 3A to safely handle stall currents when both servos move simultaneously.

## 🛠️ Hardware Requirements

* **ESP32 Development Board**
* **Pan/Tilt Mount** (with two servos, e.g., SG90 or MG996R)
* **USB Web Camera** (connected directly to your PC)
* **External Power Supply** (5V recommended for servos)

---

## ⚙️ Configuration & Setup

Before running the scripts, update the configuration parameters to match your specific setup.

### 1. Python Configuration
In your Python tracking script, locate and update these variables near the top of the file:

```python
SERIAL_PORT = "COM9"    # Set to your ESP32 COM port (e.g., "COM3", "/dev/ttyUSB0")
BAUD_RATE   = 115200    # Must match the Serial baud rate set in your ESP32 C++ code
CAMERA_INDEX = 0        # 0 is usually your external/primary webcam (try 1, 2, etc. if needed)
```
Tip: If you have an integrated laptop camera alongside an external USB webcam, CAMERA_INDEX = 0 typically defaults to the external webcam, but you can cycle through 0, 1, 2 to find the target device.
    
2. ESP32 (C++) Pin Configuration
Open the C++ firmware in the Arduino IDE or PlatformIO. Ensure the pin assignments match your actual circuit wiring before uploading:

// Update these pins to match your physical ESP32 wiring
```C++
const int PAN_PIN  = 18; // PWM Pin for Pan Servo
const int TILT_PIN = 19; // PWM Pin for Tilt Servo
```
## 🔄 System Architecture

[ Camera Feed ] ──> [ Python / OpenCV ] ──(Serial / USB)──> [ ESP32 ] ──> [ Pan/Tilt Servos ]

Vision Processing: The computer captures video frames from the USB camera, tracks the designated object, and calculates positional offsets relative to the frame center.

Serial Transmission: Python sends coordinate data or error adjustments over USB Serial to the ESP32.

Hardware Actuation: The ESP32 parses incoming Serial commands and updates the PWM signals for the Pan and Tilt servos to keep the target centered.

## 📂 Code Iterations
This repository contains multiple iterations of both the Python vision scripts and ESP32 C++ firmware. Each file/folder represents a distinct stage of development and tests different tracking techniques (e.g., basic color masking, centroid tracking, or model-based detection).

