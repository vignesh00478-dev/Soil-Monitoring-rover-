# 🌱 FIELD SHIFT — Autonomous Soil Monitoring Rover

> **NASA Space Apps Challenge 2026 — Field Shift**

Field Shift is a prototype soil-monitoring rover and web dashboard designed to collect local field observations that can support crop-rotation decision making.

The NASA Field Shift challenge asks teams to create a decision-support tool combining **NASA Earth observations, local soil information, crop characteristics, and farmer priorities** to explore crop-rotation strategies that strengthen soil health and adapt farms to changing conditions.

---

## 1. 🚨 Problem

Farm decisions can change with soil and environmental conditions.

The Field Shift concept focuses on collecting local field information and combining it with broader Earth-observation information so farmers can explore suitable crop-rotation strategies.

Our prototype addresses the **local field-data collection layer** using an ESP32 rover.

The rover can:

- Collect soil sensor readings
- Monitor rain/water conditions
- Measure temperature and humidity
- Monitor MQ135 sensor values
- Detect nearby obstacles
- Stop when an obstacle is detected
- Deploy an SG90 soil-collection mechanism
- Send live readings to a web dashboard
- Store collected readings in a local database

---

## 2. 💡 Solution

We developed a prototype called **FIELD SHIFT ROVER**.

The system has three main parts:

### A. Field Rover
An ESP32 controls the rover and reads the field sensors.

### B. Web Dashboard
The dashboard provides:

- Rover START / STOP control
- Forward-drive speed control
- Live sensor monitoring
- Obstacle status
- Buzzer status
- CRU SOIL COLLECTOR control
- Soil-collection sequence status
- Sensor-history database

### C. Data Layer
Collected readings are stored in a local JSON Server database.

The collected field data can form the local-data layer for a larger crop-rotation decision-support workflow.

---

## 3. 🔌 Circuit / Hardware

### Main Controller
- ESP32 Dev Module

### Rover Movement
- L298N motor driver
- 4 × TT gear motors
- 2 motors in parallel for the left side
- 2 motors in parallel for the right side

### Sensors
- Soil moisture sensor
- Rain/water sensor
- DHT11 temperature & humidity sensor
- MQ135 air-quality sensor
- HC-SR04 ultrasonic obstacle sensor

### Soil Collection
- SG90 servo motor

### Alert and Display
- Buzzer
- 16 × 2 I2C LCD

---

## 4. 📌 ESP32 Pin Connections

| Component | ESP32 Pin |
|---|---:|
| Soil Sensor AO | GPIO 34 |
| Rain Sensor AO | GPIO 35 |
| DHT11 DATA | GPIO 19 |
| MQ135 AO | GPIO 32 |
| HC-SR04 TRIG | GPIO 5 |
| HC-SR04 ECHO | GPIO 18 |
| L298N IN1 | GPIO 26 |
| L298N IN2 | GPIO 27 |
| L298N ENA | GPIO 25 |
| L298N IN3 | GPIO 14 |
| L298N IN4 | GPIO 12 |
| L298N ENB | GPIO 33 |
| SG90 Signal | GPIO 13 |
| Buzzer | GPIO 23 |
| LCD SDA | GPIO 21 |
| LCD SCL | GPIO 22 |

> **Note:** ESP32 uses 3.3 V logic. Use appropriate level protection for sensor outputs that can exceed 3.3 V, especially HC-SR04 ECHO and MQ135 analog output.

---

## 5. ⚙️ How It Works

```text
                    ┌─────────────────────┐
                    │       ESP32         │
                    │   Main Controller   │
                    └──────────┬──────────┘
                               │
          ┌────────────────────┼────────────────────┐
          │                    │                    │
          ▼                    ▼                    ▼
     Field Sensors        L298N Driver          Dashboard
          │                    │                    │
          │                    ▼                    │
          │               4 TT Motors               │
          │                                         │
          ▼                                         ▼
    Local Field Data                         User Controls
                                                   │
                                                   ▼
                                          Soil Collection
                                                   │
                                                   ▼
                                             SG90 Servo
                                                   │
                                                   ▼
                                            Sensor Reading
                                                   │
                                                   ▼
                                            JSON Database
```

---

## 6. 🚗 Rover Operation

### Step 1 — Start
The operator presses **START** from the dashboard.

The rover moves forward.

### Step 2 — Monitor
The dashboard receives:

- Soil moisture
- Rain sensor value
- Temperature
- Humidity
- MQ135 value
- Ultrasonic distance

### Step 3 — Obstacle Detection

If the ultrasonic distance is below the configured safety threshold:

```text
ROVER → STOP
BUZZER → ON
DASHBOARD → OBSTACLE DETECTED
```

### Step 4 — Soil Collection

The operator stops the rover and presses:

```text
CRU SOIL COLLECTOR
```

The collection sequence is:

```text
Servo 0°
   ↓
Servo moves to 90°
   ↓
Sensor data collected
   ↓
Reading saved
   ↓
Servo returns to 0°
   ↓
Collection Complete
```

---

## 7. 🖥️ Web Dashboard

The dashboard contains:

### 01 / Navigation
- START
- STOP
- Travel speed
- Rover status
- Motor status

### 02 / Environment
- Soil moisture
- Rain
- Temperature
- Humidity
- MQ135
- Ultrasonic distance
- Obstacle monitor
- Buzzer status

### 03 / Sample Acquisition
- CRU SOIL COLLECTOR
- Collection protocol
- Servo position
- Collection status

### 04 / Data Archive
- Total samples
- Latest sample
- Last collection
- Search
- Date filtering
- CSV export
- Sensor-history table

---

## 8. 🎥 Demo Video

The demonstration video shows:

1. Human operator using the dashboard
2. Rover START
3. Forward rover movement
4. Live sensor data
5. Obstacle detection
6. Rover stop + buzzer alert
7. CRU SOIL COLLECTOR button
8. SG90 servo movement
9. Soil/sensor data collection
10. Database update
11. Servo returning to 0°
12. Final rover and dashboard demonstration

### Video Link

Replace this with your final video URL:

```text
[Watch the Field Shift Rover Demo](YOUR_VIDEO_LINK_HERE)
```

If you upload the video into the GitHub repository:

```text
assets/field-shift-demo.mp4
```

---

## 9. 📊 Result

The prototype demonstrates an end-to-end local field-data collection workflow:

```text
Physical Field
      ↓
ESP32 Rover
      ↓
Sensors
      ↓
Live Dashboard
      ↓
Soil Collection
      ↓
Sensor Reading
      ↓
Local Database
```

The prototype demonstrates:

- Real-time rover control
- Forward rover movement
- Obstacle-based stopping
- Buzzer alert
- Environmental sensor monitoring
- Servo-based soil collection
- Dashboard-based collection control
- Sensor-history storage

The collected local observations can serve as an input layer for a broader Field Shift decision-support system.

---

## 10. 🌍 Connection to the NASA Field Shift Challenge

The Field Shift challenge focuses on:

```text
NASA Earth Observations
        +
Local Soil Information
        +
Crop Characteristics
        +
Farmer Priorities
        ↓
Crop-Rotation Decision Support
```

Our rover focuses on the **local field observation and soil-data collection layer**.

A future version can combine rover observations with NASA Earth-observation datasets, crop information, and farmer priorities to support exploration of crop-rotation strategies.

---

## 11. 🧰 Technology Stack

### Hardware
- ESP32
- L298N
- TT Gear Motors
- SG90 Servo
- Soil Moisture Sensor
- Rain Sensor
- DHT11
- MQ135
- HC-SR04
- 16 × 2 I2C LCD
- Buzzer

### Software
- Arduino IDE
- ESP32 Arduino Core
- C++
- HTML
- CSS
- JavaScript
- JSON Server
- Git / GitHub

---

## 12. 🚀 Future Improvements

- NASA Earth-observation data integration
- Crop-specific information
- Farmer-priority inputs
- Soil-health trend analysis
- Crop-rotation strategy visualization
- Larger field-data collection
- Improved rover navigation
- Cloud database integration
- Mobile-friendly deployment

---

## 13. 📁 Project Structure

```text
field-shift-soil-rover/
│
├── index.html
├── style.css
├── app.js
├── db.json
│
├── esp32_api/
│   └── FieldShiftRover.ino
│
├── assets/
│   └── field-shift-demo.mp4
│
└── README.md
```

---

## 14. 👥 Project

**Project:** Field Shift — Autonomous Soil Monitoring Rover  
**Challenge:** NASA Space Apps Challenge 2026  
**Focus:** Local soil/environment data collection for crop-rotation decision support

---

## 🌱 From Data to Greener Tomorrows

**FIELD SHIFT**

Collect field data.  
Understand changing conditions.  
Support smarter crop-rotation decisions.
## RESULT VIDEO
![](

