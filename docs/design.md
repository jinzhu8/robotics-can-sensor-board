# Adaptive Robotic Gripper

## 1. Project Overview

### 1.1 Motivation
A fixed position/force gripping strategy for the classic claw is not the most optimized way to interact with objects of different sizes, shapes, and stiffness. I want to explore how sensing and closed-loop control can allow a robotic claw to adapt its gripping force to the object it's holding.
### 1.2 Problem Statement
How can a robotic claw use force feedback to detect contact and maintain an appropriate gripping force while avoiding excessive force?
### 1.3 Project Goals
The initial goal is to develop a two-finger parallel robotic claw capable of:
- detecting when contact is made
- measuring the force applied to the object
- controlling the fingers using feedback from the force sensor
- holding objects using a configurable target gripping force
- recording sensor and control data for analysis independent of a larger robot
## 2. Requirements
Functional reqs:
- detect contact between the gripper and an object.
- measure the gripping force.
- repeatable force-feedback control over a useful operating range
- The user should be able to specify a target gripping force.
- adjust the gripper actuator to approach and maintain the target force within the limits of the sensor.
- provide sensor and control data for analysis.
- gripper controller should operate independently of the larger robotic arm.

Electrical reqs:

Performance reqs:


## 3. System Architecture
Pimoroni's Servo 2040 was used as a reference architecture because it combines an RP2040 with servo outputs, analog sensor inputs, USB connectivity, and power/current monitoring in a platform intended for robotic applications. The project will implement the required functionality on a custom PCB rather than using the Servo 2040 directly.
                ┌─────────────────┐
                │   User Input    │
                │ Target Force    │
                └────────┬────────┘
                         │
                         ▼
┌─────────────┐   ┌───────────────┐    ┌──────────────┐
│ Force Sensor├──►│ Microcontroller├──►│ Motor Driver │
└─────────────┘   │ + Controller  │    └──────┬───────┘
                  └───────┬───────┘           │
                          │                   ▼
                          │             ┌───────────┐
                          └────────────►│ Gripper   │
                                        │ Actuator  │
                                        └───────────┘
## 4. Component Selection
| Component       | Selected Part | Reason                                       |
| --------------- | ------------- | -------------------------------------------- |
| MCU             | TBD           | Processing, ADC/interface, control loop, USB |
| Force sensor    | TBD           | Required force range/interface               |
| Motor driver    | TBD           | Compatible with gripper actuator             |
| Power regulator | TBD           | Required voltage/current                     |
| USB interface   | TBD           | Data logging/communication                   |
### 4.1 Force Sensor

The initial force sensor selected for the gripper is the Interlink FSR 402. It was selected because it is a thin, two-wire analog force sensor with a specified sensing range of approximately 0.2–20 N, making it suitable for detecting and controlling gripping force without requiring a complex bridge-based measurement circuit; its resistance-force relationship is nonlinear and exhibits saturation and hysteresis.
### 4.2 Microcontroller
The RP2040 was selected as the primary microcontroller for the gripper controller. It provides sufficient processing performance, GPIO, a built-in 12-bit ADC, and USB connectivity for communication and data logging.
The RP2040 was chosen over a more feature-rich wireless microcontroller because the project does not require wireless connectivity. Although the RP2040 ADC is nominally 12-bit, its effective resolution is lower, so the design will not assume 12-bit accuracy in the force measurement.

## 5. Circuit Design
Power:

Force-sensing circuit:
The FSR 402 is interfaced to the microcontroller using a resistive voltage divider. A fixed resistor is connected from the sensing node to ground, while the FSR is connected to the supply voltage.

\[
V_{sense}=V_{CC}\frac{R_{FIXED}}{R_{FSR}+R_{FIXED}}
\]

An initial fixed resistance of 10 kΩ will be evaluated for the divider. The resulting voltage is read by the microcontroller ADC and used as the feedback signal for the gripper force controller. The final resistor value and any additional filtering or signal conditioning will be determined after selecting the microcontroller/ADC and evaluating the expected sensor operating range.

MCU and interfaces: 
- Selected the RP2040 as the project microcontroller.
- Chosen for its integrated ADC, USB connectivity, GPIO, processing capability, and available hardware documentation.
- The design will account for the ADC's effective resolution rather than assuming ideal 12-bit performance.

Motor driver:

## 6. PCB Design
Board reqs:

Placement and routing:

Design rule/manufacturing checks

Final board:
## 7. Control System
Target Force
     │
     ▼
 Controller ◄──── Measured Force
     │
     ▼
Motor Command
     │
     ▼
Gripper
     │
     ▼
Force Sensor
     └──────────────►
Control algorithm:

Tuning/simulation:


## 8. Design Decisions & Tradeoffs
