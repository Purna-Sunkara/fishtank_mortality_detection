# 🐟 IoT-Based Fish Mortality Risk Monitoring System

## 📌 Project Overview

The **IoT-Based Fish Mortality Risk Monitoring System** is an IoT-based solution designed to monitor water conditions and environmental parameters in fish tanks. The system uses a NodeMCU ESP8266 microcontroller and multiple sensors to monitor water quality, water level, leakage, and tank overflow conditions.

Sensor readings are transmitted to the **Blynk IoT platform** over Wi-Fi, enabling remote monitoring through a dashboard. When predefined threshold conditions are violated, the system activates a buzzer to alert users to potential risks to fish health.

The project aims to support early identification of unfavorable tank conditions and help reduce the risk of fish mortality.

> **Note:** This is a risk-monitoring system, not a direct fish-death detection system. The current implementation does not include a dissolved oxygen (DO) sensor.

## 🎯 Objectives

- Monitor fish tank environmental conditions in real time.
- Measure water temperature and surrounding humidity.
- Monitor water pH and turbidity.
- Estimate water level using an ultrasonic sensor.
- Detect water leakage and abnormal tank water levels.
- Provide visual monitoring through the Blynk IoT dashboard.
- Activate a buzzer when configured alert conditions occur.
- Support timely intervention to maintain suitable conditions for fish.

## 🛠️ Hardware Requirements

| Component | Purpose |
|---|---|
| NodeMCU ESP8266 | Main controller and Wi-Fi connectivity |
| DHT11 Sensor | Temperature and humidity monitoring |
| pH Sensor | Water pH measurement |
| Turbidity Sensor | Water clarity monitoring |
| HC-SR04 Ultrasonic Sensor | Water-level distance measurement |
| Water Leak Sensor | Leakage detection |
| Float Switches (2) | Low-water and overflow detection |
| Buzzer | Audible alerts |
| Analog Multiplexer (optional) | Allows multiple analog sensors to share an analog input |
| Jumper Wires | Circuit connections |
| Breadboard | Prototyping and circuit assembly |
| USB Cable and Power Supply | Programming and powering the controller |

## 💻 Software Requirements

- Arduino IDE
- ESP8266 board package for Arduino IDE
- Blynk IoT account
- ESP8266WiFi library
- Blynk library for ESP8266
- DHT sensor library

## ⚙️ System Architecture

```text
     Temperature & Humidity Sensor
     pH Sensor
     Turbidity Sensor
     Ultrasonic Sensor
     Leak Sensor
     Float Switches
              |
              v
      NodeMCU ESP8266
              |
          Wi-Fi Network
              |
              v
         Blynk Cloud
              |
              v
      Mobile/Web Dashboard
              |
      Real-Time Monitoring

  Abnormal Conditions
              |
              v
       Buzzer Activation
```

## 🔌 Pin Configuration

The following table reflects the pin definitions in the current source code.

| Component | NodeMCU Pin |
|---|---|
| DHT11 Data | D4 |
| Turbidity Sensor Output | A0 |
| pH Sensor Output | A0* |
| HC-SR04 Trigger | D6 |
| HC-SR04 Echo | D7 |
| Leak Sensor Digital Output | D3 |
| Low-Level Float Switch | D0 |
| High-Level Float Switch | D1 |
| Buzzer | D8 |

**Important:** The code assigns both the pH and turbidity sensors to A0. They cannot be independently measured this way without switching between them using an analog multiplexer or another suitable circuit. The current code does not implement multiplexer channel selection, so this must be added before both readings will work correctly.

The ultrasonic sensor's ECHO output may be 5 V. Use a suitable voltage divider or level shifter to protect the ESP8266's 3.3 V GPIO.

## 📊 Blynk Dashboard Configuration

Create a Blynk template named `FishMonitoring` and configure the following virtual datastreams.

| Virtual Pin | Parameter | Suggested Data Type |
|---|---|---|
| V0 | Temperature | Double |
| V1 | Humidity | Double |
| V2 | Water-Level Distance | Integer |
| V3 | pH Value | Double |
| V4 | Turbidity Raw Reading | Integer |
| V5 | Leak Sensor Status | Integer |
| V6 | Tank Water-Level Status | String |
| V7 | System Alert Message | String |
| V8 | Buzzer Status | Integer |

Add suitable value displays, labels, and status indicators to the Blynk dashboard to visualize the sensor readings and alerts.

## 🚨 Alert Conditions

The current code activates the buzzer when any of the following conditions are met:

- **pH:** Below 6.0 or above 8.5.
- **Water level:** Calculated ultrasonic distance is below 10 cm.
- **Leakage:** Leak sensor digital output is HIGH.
- **Turbidity:** Raw analog reading is above 700.
- **Low water:** The low-level float switch reads LOW.
- **Overflow:** The high-level float switch reads HIGH.

These are configurable example thresholds. Their suitability depends on the fish species, tank geometry, sensor calibration, and sensor output logic.

The system reports either an alert message or a normal status to the Blynk dashboard.

## 🔄 Working Principle

1. Sensors collect data about the tank's environmental conditions.
2. The NodeMCU ESP8266 reads the sensor outputs.
3. The program compares readings against configured alert thresholds.
4. When an alert condition is detected, the buzzer is activated.
5. Sensor readings and tank status are transmitted to Blynk Cloud over Wi-Fi.
6. The user monitors the dashboard and can take corrective action when necessary.

## 🚀 Installation and Setup

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR_USERNAME/YOUR_REPOSITORY.git
```

Navigate to the project directory:

```bash
cd YOUR_REPOSITORY
```

### 2. Configure Arduino IDE

- Install the ESP8266 board package.
- Install the Blynk library.
- Install the DHT sensor library and its required dependencies.
- Select the appropriate NodeMCU ESP8266 board and COM port.

### 3. Configure Credentials

Replace the placeholder values in the source code with your actual Blynk and Wi-Fi credentials.

```cpp
#define BLYNK_TEMPLATE_ID "YourTemplateID"
#define BLYNK_TEMPLATE_NAME "FishMonitoring"
#define BLYNK_AUTH_TOKEN "YourAuthToken"

char ssid[] = "YourSSID";
char pass[] = "YourPassword";
```

**Security:** Never upload real Wi-Fi passwords or Blynk authentication tokens to a public GitHub repository. Keep credentials in a private configuration file excluded through `.gitignore`, or use another secure configuration method.

### 4. Configure Blynk

- Create the `FishMonitoring` template.
- Configure virtual pins V0–V8.
- Add the required dashboard widgets.
- Copy the template ID and device authentication token into your local code.

### 5. Upload the Code

- Connect the NodeMCU ESP8266 to your computer.
- Select the correct board and COM port.
- Upload the sketch through Arduino IDE.
- Open the Serial Monitor if serial debugging is required.
- Verify that the device connects to Wi-Fi and Blynk Cloud.

### 6. Test the System

Test each sensor individually before testing the complete setup. Confirm the correct float-switch and leak-sensor logic, calibrate the pH sensor, and verify that the dashboard readings and buzzer responses match the expected conditions.

## 📁 Suggested Repository Structure

```text
Fish-Mortality-Risk-Monitoring/
│
├── README.md
├── src/
│   └── FishMonitoring.ino
├── docs/
│   ├── system_architecture.png
│   └── circuit_diagram.png
├── images/
│   └── project_setup.jpg
└── .gitignore
```

## ⚠️ Limitations

- Dissolved oxygen is not measured by the current system.
- The pH calculation in the supplied code is simulated and requires sensor-specific calibration for real measurements.
- The pH and turbidity inputs require proper analog multiplexing or separate measurement hardware.
- The HC-SR04 reading represents distance to the water surface, not water depth directly. Water depth must be calculated using the tank's geometry and sensor mounting height.
- Sensor thresholds and digital output polarity must be verified against the actual hardware.
- The current code does not explicitly handle failed DHT readings or ultrasonic timeouts.
- The system identifies configured abnormal conditions but does not independently confirm fish mortality.

## 🔮 Future Enhancements

- Integrate a dissolved oxygen sensor.
- Implement proper analog multiplexer channel selection.
- Add calibrated pH and turbidity measurements.
- Add mobile notifications for abnormal tank conditions.
- Develop historical data logging and graphical trend analysis.
- Improve fault handling and sensor calibration.
- Introduce species-specific risk assessment using multiple sensor readings.
- Add an activity-monitoring module to observe unusual fish behavior.

## 🌍 Applications

- Aquaculture farms
- Fish hatcheries
- Indoor fish tanks
- Research and educational laboratories
- IoT-based water-quality monitoring systems

## 👥 Project Information

**Project Title:** IoT-Based Fish Mortality Risk Monitoring System  
**Domain:** Internet of Things (IoT), Embedded Systems, Aquaculture Monitoring  
**Controller:** NodeMCU ESP8266  
**Cloud Platform:** Blynk IoT  
**Programming Language:** C/C++ (Arduino)

## 📜 License

This project is intended for educational and research purposes. Add a suitable open-source license, such as the MIT License, if you want others to reuse and modify the project under its terms.

---

**Developed to support smarter fish tank monitoring through IoT technology.**
