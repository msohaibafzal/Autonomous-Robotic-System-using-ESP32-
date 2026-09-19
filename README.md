# 🤖 Autonomous Robo Cop

An embedded robotics project built using **ESP32** and **ESP32-CAM**, combining intruder detection, fire detection, autonomous fire extinguishing, and remote-controlled vehicle movement.

## Overview

The **Autonomous Robo Cop** integrates sensors, actuators, microcontrollers, motor control, and wireless communication into a robotic system for security and fire-safety applications.

The project consists of two main subsystems:

* **Robo Cop System** — detects intruders, fire, smoke, and gases and activates the corresponding alert and fire-extinguishing mechanisms.
* **Remote Control Car** — provides a mobile robotic platform controlled through an ESP32-CAM and L298N motor driver.

### Key Features

* PIR-based intruder detection
* IR-based fire detection
* MQ2-based smoke and gas detection
* Audible alert using a passive buzzer
* Relay-controlled fire-response mechanism
* Servo-controlled water-pump positioning
* Water-based fire extinguishing
* ESP32-CAM-based remote vehicle control
* Wi-Fi communication
* Forward, backward, left, and right movement

---

## System Architecture

```text
                         AUTONOMOUS ROBO COP
                                  │
                 ┌────────────────┴────────────────┐
                 │                                 │
                 ▼                                 ▼
          ROBO COP SYSTEM                  REMOTE CONTROL CAR
                 │                                 │
        ┌────────┼────────┐                    ESP32-CAM
        │        │        │                        │
       PIR      IR       MQ2                     Wi-Fi
        │        │        │                        │
        │        │        └──► Smoke Detection     ▼
        │        │                            L298N Driver
        │        └──────────► Fire Detection       │
        │                                         ▼
        └──────────────────► Intruder         DC Motors
                              Detection           │
                                                  ▼
                                               Wheels

                    Fire / Smoke Detection
                              │
                              ▼
                        Relay Module
                              │
                       ┌──────┴──────┐
                       ▼             ▼
                  Servo Motor    Water Pump
                       │             │
                       └──────┬──────┘
                              ▼
                     Fire Extinguishing
```

---

# 1. Robo Cop System

The Robo Cop subsystem monitors the environment and responds to security and fire-related events.

## Hardware Components

| Component        | Quantity | Purpose                            |
| ---------------- | -------: | ---------------------------------- |
| ESP32            |        1 | Main microcontroller               |
| PIR Sensor       |        1 | Intruder / motion detection        |
| IR Sensors       |        3 | Fire detection                     |
| MQ2 Smoke Sensor |        1 | Smoke and gas detection            |
| Passive Buzzer   |        1 | Audible alert                      |
| 5V Relay Module  |        1 | Controls the fire-response circuit |
| Servo Motor      |        1 | Positions the water-pump mechanism |
| Water Pump       |        1 | Fire extinguishing                 |
| Water Tank       |        1 | Water storage                      |

---

## Intruder Detection

A **PIR sensor** is used to detect human motion.

```text
PIR Sensor
     │
     ▼
Motion Detected
     │
     ▼
   ESP32
     │
     ▼
Intruder Detection
     │
     ▼
   Alert
```

### Connection

```text
PIR OUT → GPIO18
```

---

## Fire Detection

Three IR sensors are used to detect fire-related infrared radiation.

```text
                IR Sensors
               /    │    \
              ▼     ▼     ▼
           GPIO35 GPIO32 GPIO33
              \     │     /
               \    │    /
                 ESP32
                    │
                    ▼
              Fire Detected
```

### Connections

```text
IR Sensor 1 → GPIO35
IR Sensor 2 → GPIO32
IR Sensor 3 → GPIO33
```

---

## Smoke & Gas Detection

An **MQ2 sensor** is used to detect smoke and gases such as smoke, LPG, methane, and propane.

### MQ2 Connections

```text
VCC → 3.3V
OUT → GPIO18
GND → GND
```

The MQ2 sensor also contributes to the alert mechanism by activating the passive buzzer when smoke is detected.

---

# 2. Autonomous Fire Extinguishing

The fire-extinguishing mechanism combines the detection sensors, passive buzzer, relay, servo motor, water pump, and water tank.

```text
Fire / Smoke Detected
          │
          ▼
        ESP32
          │
     ┌────┴────┐
     ▼         ▼
  Buzzer      Relay
                │
                ▼
          Servo Motor
                │
                ▼
           Water Pump
                │
                ▼
       Fire Extinguishing
```

The relay controls the fire-response circuit, while the servo mechanism positions the water-pump assembly.

## Robo Cop Connections

### MQ2 Sensor

```text
VCC  → 3.3V
OUT  → GPIO18
GND  → GND
```

### IR Sensors

```text
IR 1 → GPIO35
IR 2 → GPIO32
IR 3 → GPIO33
```

### Passive Buzzer

```text
Positive → GPIO19
Negative → GND
```

### Relay Module

```text
VCC → 3.3V
GND → GND
IN  → GPIO26
```

### Servo Motor

```text
RED    → 3.3V
BROWN  → GND
ORANGE → GPIO5
```

### Water Pump

```text
Water Pump → Relay Module
```

---

# 3. Remote Control Car

The second subsystem provides the mobile robotic platform.

## Hardware Components

| Component          | Purpose                                 |
| ------------------ | --------------------------------------- |
| ESP32-CAM          | Main controller and Wi-Fi communication |
| Car Chassis        | Physical structure                      |
| 4 Motors & Wheels  | Vehicle movement                        |
| L298N Motor Driver | Motor control                           |
| 14V Battery        | Power supply                            |
| Connecting Wires   | Electrical connections                  |

---

## Remote Control Architecture

```text
              User
               │
               ▼
      Remote Control Interface
               │
               ▼
              Wi-Fi
               │
               ▼
           ESP32-CAM
               │
               ▼
        L298N Motor Driver
          ┌────┴────┐
          ▼         ▼
     Left Motors  Right Motors
          │         │
          └────┬────┘
               ▼
             Wheels
               │
               ▼
        Vehicle Movement
```

---

## Remote Car Connections

### L298N Motor Driver

```text
GPIO14 → IN1 → Left Motor Forward
GPIO15 → IN2 → Left Motor Backward

GPIO13 → IN3 → Right Motor Forward
GPIO12 → IN4 → Right Motor Backward
```

### Power

```text
5V  → ESP32-CAM 5V
GND → ESP32-CAM GND
```

### PIR Sensor

```text
VCC → 5V
GND → GND
OUT → GPIO2
```

### DC Motors

```text
Left Motor
    └──► L298N OUT1 / OUT2

Right Motor
    └──► L298N OUT3 / OUT4
```

---

# 4. System Operation

### Intruder Detection

```text
Motion
  ↓
PIR Sensor
  ↓
ESP32
  ↓
Intruder Detected
  ↓
Alert
```

### Fire Detection

```text
Environment
     │
 ┌───┴────┐
 ▼        ▼
IR       MQ2
Sensors  Sensor
 │        │
 └───┬────┘
     ▼
   ESP32
     │
     ▼
Fire Response
```

### Fire Extinguishing

```text
Fire Detected
     ↓
   ESP32
     ↓
Relay Activated
     ↓
Servo + Water Pump
     ↓
Fire Extinguishing
```

### Vehicle Movement

```text
User Command
     ↓
ESP32-CAM
     ↓
L298N Motor Driver
     ↓
DC Motors
     ↓
Vehicle Movement
```

Supported directions:

* Forward
* Backward
* Left
* Right

---

# 5. Software

The ESP32-CAM can be programmed using the **Arduino IDE** or another suitable ESP32-compatible development environment.

The software handles:

* Sensor input
* Detection conditions
* Alert generation
* Relay activation
* Servo control
* Water-pump control
* Remote-control commands
* Motor-control signals

---

# 6. Repository Structure

```text
Autonomous-Robo-Cop/
│
├── README.md
│
├── src/
│   ├── robo_cop/
│   │   └── robo_cop.ino
│   │
│   └── remote_car/
│       └── remote_car.ino
│
├── hardware/
│   ├── robo_cop/
│   │   └── schematic.png
│   │
│   └── remote_car/
│       └── schematic.png
│
├── documentation/
│   └── project_report.pdf
│
└── media/
    ├── robo_cop.jpg
    └── demo.jpg
```

---

# 7. Technologies

### Hardware

* ESP32
* ESP32-CAM
* PIR Sensor
* IR Sensors
* MQ2 Smoke Sensor
* Servo Motor
* Water Pump
* Passive Buzzer
* Relay Module
* L298N Motor Driver
* DC Motors
* Battery

### Software

* Arduino IDE
* ESP32 development environment

### Concepts

* Embedded Systems
* Robotics
* IoT
* Sensor Interfacing
* Actuator Control
* Motor Control
* PWM
* Wireless Communication
* Automation

---

# 8. Issues Encountered

During development, several hardware-related challenges were encountered:

* PIR sensor sensitivity required adjustment.
* Servo motor calibration was required for accurate water-pump positioning.
* PWM signals became unstable when multiple signals were used.
* High current consumption caused camera brownout issues.
* IR flame sensors were affected by external infrared sources.

---

# 9. Design Considerations

For reliable operation:

* Ensure sufficient power for the motors and ESP32-CAM.
* Implement appropriate error-handling and safety mechanisms.
* Calibrate sensors before operation.
* Test the robotic system in a controlled environment before using it in more complex environments.

---

# 10. Limitations

* PIR sensitivity requires calibration.
* IR flame sensors can be affected by external infrared sources.
* PWM stability was an issue during development.
* High current consumption caused camera brownout problems.
* Reliable operation requires appropriate power management.
* Testing should be performed in controlled environments.

---

# 11. Future Improvements

Potential improvements include:

* More robust fire and smoke detection
* Improved sensor calibration
* Better power-management design
* More reliable PWM control
* Improved wireless control
* Additional safety mechanisms
* More autonomous navigation
* Enhanced environmental monitoring
* More sophisticated robotic decision-making

---

# 12. Conclusion

The **Autonomous Robo Cop** demonstrates the integration of embedded systems, sensors, actuators, motor controllers, and wireless communication into a practical robotic platform.

The Robo Cop subsystem provides intruder detection, fire detection, smoke detection, alerts, and an automated fire-extinguishing mechanism. The remote-control subsystem provides a mobile robotic platform using an ESP32-CAM and L298N motor driver.

Overall, the project demonstrates practical applications of **embedded systems, robotics, IoT, and automation** for security and fire-safety scenarios.
