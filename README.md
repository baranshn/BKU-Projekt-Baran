<div align="center">

# PLAYGROUND MASTER
### Arduino Smart Car — School Engineering Project

<br>

![Arduino](https://img.shields.io/badge/Arduino-C%2B%2B-00979D?style=for-the-badge&logo=arduino&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Development-orange?style=for-the-badge)
![Language](https://img.shields.io/badge/Language-C%2B%2B-00599C?style=for-the-badge&logo=cplusplus&logoColor=white)
![Platform](https://img.shields.io/badge/Platform-Arduino%20Uno%2FMega-teal?style=for-the-badge)

<br>

**A programmable Arduino-based vehicle featuring manual joystick control, autonomous obstacle avoidance, servo steering, ultrasonic sensing, and a live OLED status display.**

<br>

[Overview](#-overview) · [Hardware](#-hardware) · [Software](#-software) · [Roadmap](#-roadmap) · [Team](#-team)

---

</div>

## Overview

This project was built as part of a school engineering course. The car is programmed in C++ using the Arduino framework and operates in two distinct modes:

| Mode | Trigger | Behaviour |
|------|---------|-----------|
| **Manual** | Default on startup | Joystick X-axis = steering · Y-axis = throttle |
| **Autonomous** | Joystick button press | Ultrasonic sensor detects obstacles and steers automatically |

Live feedback (mode, speed, distance) is rendered on a 0.96" OLED display at all times.

---

## Hardware

### Component List

| Component | Model | Role |
|-----------|-------|------|
| Microcontroller | Arduino Uno / Mega | Central processing unit |
| Ultrasonic Sensor | HC-SR04 | Front obstacle detection |
| Servo Motor | SG90 / MG90S | Front axle steering |
| DC Motor | TT Motor | Rear wheel drive |
| Motor Driver | L298N | Speed & direction control |
| Joystick | KY-023 | Manual user input |
| OLED Display | 0.96" SSD1306 I2C | Live status output |
| Power Supply | 9V Battery / LiPo | Vehicle power |
| Chassis | 2WD Smart Car Kit | Physical frame |

### Pin Mapping

| Pin | Component | Notes |
|-----|-----------|-------|
| `D3` | Servo | PWM signal |
| `D5` | Motor Enable | PWM speed control |
| `D6` | Motor IN1 | Direction |
| `D7` | Motor IN2 | Direction |
| `D9` | Ultrasonic TRIG | Trigger pulse |
| `D10` | Ultrasonic ECHO | Echo return |
| `A0` | Joystick X | Steering axis |
| `A1` | Joystick Y | Throttle axis |
| `A2` | Joystick Button | Mode toggle |
| `SDA / A4` | OLED | I2C data |
| `SCL / A5` | OLED | I2C clock |

---

## Software

### Dependencies

```cpp
#include <Servo.h>            // Servo motor control
#include <Wire.h>             // I2C communication
#include <Adafruit_GFX.h>     // OLED graphics library
#include <Adafruit_SSD1306.h> // OLED SSD1306 driver
```

### Core Loop

```cpp
void loop() {
  if (joystick.buttonPressed()) toggleMode();

  if (mode == MANUAL) {
    speed    = joystick.getY();
    steering = joystick.getX();
  }

  if (mode == AUTONOMOUS) {
    distance = ultrasonic.measure();
    if (distance < THRESHOLD) avoid();
    else drive(FORWARD);
  }

  servo.setAngle(steering);
  motor.setSpeed(speed);
  display.update(mode, speed, distance);
}
```

### Project Structure
