# SmartOffice Monitor

> Real-time office environment monitoring over MQTT with a JavaFX desktop dashboard.

[![Java](https://img.shields.io/badge/Java-17-orange?logo=openjdk)](https://openjdk.org/projects/jdk/17/)
[![Build](https://img.shields.io/badge/Build-Maven-blue?logo=apachemaven)](https://maven.apache.org/)
[![JavaFX](https://img.shields.io/badge/UI-JavaFX%2017-blue?logo=java)](https://openjfx.io/)
[![MQTT](https://img.shields.io/badge/Protocol-MQTT-purple?logo=eclipse)](https://mqtt.org/)
[![License](https://img.shields.io/badge/License-MIT-green)](https://opensource.org/licenses/MIT)

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Running Locally](#running-locally)
- [Project Structure](#project-structure)
- [MQTT Topics](#mqtt-topics)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

SmartOffice Monitor is a lightweight Java desktop application that simulates and visualises office environmental data in real time. Two sensor publishers emit temperature, humidity, light status, and window status to an MQTT broker. A JavaFX subscriber application connects to the same broker, receives those readings, and renders them live in a clean GUI.

---

## Features

- **Room sensor simulation** — publishes randomised temperature (°C) and humidity (%) every second.
- **Floor sensor simulation** — publishes randomised light (ON/OFF) and window (OPEN/CLOSED) status every second.
- **Live JavaFX dashboard** — subscribes to all sensor topics and updates labels in real time.
- **Connect / Disconnect controls** — start and stop the broker connection from the UI without restarting the app.
- **Last-will messages** — each publisher registers an MQTT last-will so disconnections are announced on dedicated error topics.
- **Graceful shutdown** — closing the window stops all publisher threads cleanly.

---

## Tech Stack

| Technology | Version | Role |
|---|---|---|
| Java | 17 | Runtime & language |
| JavaFX | 17 | Desktop UI framework |
| Eclipse Paho MQTT | 1.2.5 | MQTT client library |
| HiveMQ Public Broker | — | Free MQTT broker (`broker.hivemq.com:1883`) |
| Apache Maven | 3.x | Build & dependency management |
| JUnit | 3.8.1 | Unit testing |

---

## Getting Started

### Prerequisites

- **JDK 17** or later — [Download OpenJDK](https://openjdk.org/projects/jdk/17/)
- **Apache Maven 3.6+** — [Download Maven](https://maven.apache.org/download.cgi)
- An internet connection (the app uses the public HiveMQ broker; no local broker setup required)

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/flaviu-vanca/MQTT-Based-Smart-Office-Environment-Monitoring-System.git
cd MQTT-Based-Smart-Office-Environment-Monitoring-System

# 2. Resolve dependencies and build
mvn clean package
```

### Running Locally

Launch the JavaFX application via the JavaFX Maven plugin:

```bash
mvn javafx:run
```

The dashboard window opens. Click **Connect** to start the publishers and begin receiving live sensor data. Click **Disconnect** to stop publishing and clear the display.

---

## Project Structure

```
.
├── pom.xml
└── src
    ├── main
    │   ├── java
    │   │   ├── environment_monitor
    │   │   │   ├── App.java               # Application entry point
    │   │   │   └── SmartOfficeApp.java    # JavaFX UI + MQTT subscriber
    │   │   └── publishers
    │   │       ├── FloorPublisher.java    # Publishes light & window status
    │   │       └── RoomSensorPublisher.java # Publishes temperature & humidity
    │   └── resources
    │       └── styles.css                 # JavaFX stylesheet
    └── test
        └── java
            └── com/environment_monitor
                └── AppTest.java
```

---

## MQTT Topics

| Topic | Publisher | Payload example |
|---|---|---|
| `floor/room/temperature` | `RoomSensorPublisher` | `23 °C` |
| `floor/room/humidity` | `RoomSensorPublisher` | `55 %` |
| `floor/light/ID` | `FloorPublisher` | `ON` / `OFF` |
| `floor/window/status` | `FloorPublisher` | `OPEN` / `CLOSED` |
| `floor/room/disconnect` | `RoomSensorPublisher` | Last-will on disconnect |
| `floor/disconnect` | `FloorPublisher` | Last-will on disconnect |

All messages are published to the **HiveMQ public broker** at `tcp://broker.hivemq.com:1883` with QoS 0.

---

## Contributing

Contributions are welcome.

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-feature`.
3. Commit your changes: `git commit -m "feat: describe your change"`.
4. Push to your fork and open a pull request.

Please keep pull requests focused and include a clear description of what was changed and why.

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT).
