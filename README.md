# CrowdOracle | Enterprise Intelligence
### Anonymous Crowd Detection & Environmental Monitoring System

CrowdOracle is an integrated IoT and AI solution designed for real-time crowd analytics and environmental automation. It leverages browser-based computer vision for privacy-preserving people counting and Arduino-based hardware for temperature monitoring and automated control.

---

## 🏗️ Architecture Stack

The project consists of three interconnected modules:

1.  **Backend (Java Spring Boot)**: The central nervous system. It aggregates data from both the AI frontend and the IoT sensors, persisting it to a PostgreSQL database. It exposes REST APIs for the dashboard and manages Serial communication with the hardware.
2.  **Frontend (Web Dashboard)**: A responsive, high-fidelity UI built with TailwindCSS. It performs **Edge AI processing** using TensorFlow.js (Coco-SSD model) to detect people via the webcam directly in the browser. It sends real-time occupancy data to the backend.
3.  **IoT (Arduino Hardware)**: A dedicated sensing unit that captures environmental temperature (DHT11), controls automation hardware (DC Motors), and displays status on an LCD. It communicates with the backend via USB Serial.

## 🚀 Key Features

*   **Real-time AI Occupancy Tracking**: Detects and counts people instantly using client-side WebGL acceleration.
*   **Privacy First**: No video feed is ever sent to the server; only anonymous count statistics are transmitted.
*   **Environmental Automation**: Automatically adjusts motor speeds (simulating HVAC/Fan control) based on real-time temperature thresholds.
*   **Live Analytics Dashboard**: Visualizes occupancy trends, temperature data, and connection health.
*   **Dual-Source Data Ingestion**: Seamlessly merges data from web agents and physical sensors.

---

## 🛠️ Technology Stack

*   **Backend**: Java 21, Spring Boot 3.5.5, Spring Data JPA, Spring Security, jSerialComm.
*   **Database**: PostgreSQL.
*   **Frontend**: HTML5, Vanilla JavaScript, TailwindCSS, TensorFlow.js, FontAwesome.
*   **IoT Firmware**: C++ (Arduino), Libraries: `DHT`, `LiquidCrystal_I2C`, `Wire`.

---

## ⚙️ Prerequisites

*   **Java 21** or higher.
*   **PostgreSQL** installed and running.
*   **Maven** (optional if using the included `mvnw` wrapper).
*   **Arduino IDE** (for uploading firmware).
*   **Hardware**: Arduino Uno/Nano, DHT11 Sensor, 16x2 I2C LCD, DC Motor + Driver (L298N), Webcam.

---

## 📥 Installation & Setup

### 1. Database Setup
Ensure PostgreSQL is running and create the database:
```sql
CREATE DATABASE CrowdOracle;
```

### 2. Backend Configuration
1.  Navigate to `Backend/src/main/resources/application.properties`.
2.  Update your database credentials:
    ```properties
    spring.datasource.username=your_postgres_username
    spring.datasource.password=your_postgres_password
    ```
3.  **Critical**: Set the correct Serial Port for your Arduino (check Device Manager on Windows or `/dev/tty*` on Linux/Mac):
    ```properties
    serial.port.name=COM3  # Change to your actual port (e.g., COM4, /dev/ttyUSB0)
    serial.enabled=true
    ```

### 3. IoT Hardware Setup
1.  **Circuit Connection**:
    *   **DHT11 Signal**: Pin 7
    *   **Motor Enable (PWM)**: Pin 9
    *   **Motor IN1**: Pin 10
    *   **Motor IN2**: Pin 11
    *   **LCD**: I2C Pins (SDA/SCL)
2.  Open `Iot/AurdinoConfig.ino` in Arduino IDE.
3.  Install required libraries (`DHT sensor library`, `Adafruit Unified Sensor`, `LiquidCrystal I2C`).
4.  Upload the code to your Arduino.
5.  **Keep the Arduino connected via USB** to the computer running the Backend.

### 4. Running the Application
1.  **Start the Backend**:
    ```bash
    cd Backend
    ./mvnw spring-boot:run
    ```
    *Wait for the log: "Serial port listener started" & "Started CrowdOracleApplication".*

2.  **Launch the Dashboard**:
    *   Simply open `frontend/index.html` in a modern web browser.
    *   Allow Camera permissions when prompted.
    *   Click **"Initialize Feed"** to start AI detection.

---

## 📊 Usage Guide

1.  **The Dashboard**: The main interface shows the live camera feed with (optional) bounding boxes.
2.  **Settings**: Click the generic 'Cog' icon to configure the Backend URL (default: `http://localhost:8080`) or adjust the specific Confidence Threshold for AI detection.
3.  **Automation Logic**:
    *   Temp > 32°C → Motor Max Speed (High Load)
    *   Temp > 29°C → Motor Medium Speed
    *   Temp < 29°C → Motor Low Speed
4.  **Data Logging**: The system automatically saves records to the database every 5 seconds (configurable).

---

## 🤝 API Endpoints

*   `POST /api/crowd-data`: Receive manual/web data.
*   `GET /api/crowd-data/stats`: Get aggregated system statistics.
*   `GET /api/crowd-data/latest`: Get real-time status.

---

## 📜 License
This project is open-source.
