# Autonomous Line-Following Material Transport Robot

An embedded robotics system designed to automate intra-facility material handling tasks using infrared (IR) path tracking and ultrasonic obstacle detection. Developed with an Arduino UNO microcontroller and an L298N dual H-bridge motor driver, the robot provides closed-loop differential drive navigation along predefined ground tracks while preventing collisions in real time.

---

## Project Overview

In industrial environments such as warehouses, assembly floors, and hospitals, manual material handling often leads to labor bottlenecks, human error, and operational delays. This project presents an affordable, autonomous mobile robot (AMR) designed to reliably transport materials along marked routes. 

The robot detects high-contrast paths using IR reflectance sensing and continuously scans the heading path using ultrasonic echolocation. If an unexpected obstacle enters its course within a predefined safety boundary, the microcontroller overrides line tracking to halt the vehicle until clearance is restored.

### Key Features
* **Dual-Sensor IR Line Tracking**: Differentiates surface reflectivity between paths and surrounding floors for real-time directional correction.
* **Real-Time Obstacle Avoidance**: Utilizes an HC-SR04 ultrasonic sensor with a 2 cm to 100 cm active detection range to avoid collisions.
* **Closed-Loop Control**: Continuous sensor-feedback loop maintains path accuracy and fast response times.
* **Dedicated Power Isolation**: Features an 11.1V 18650 Li-ion battery pack with protective diodes against motor back-EMF spikes.
* **Modular Payload Platform**: Pre-drilled chassis allows expansion for wireless links, robotic pick-and-place grippers, and vision modules

---

## Working Methodology & Architecture

```text
                  +-------------------------------+
                  |  Power ON & Initialize System |
                  | (Arduino UNO, Sensors, Motors)|
                  +---------------+---------------+
                                  |
               +------------------+------------------+
               |                                     |
               v                                     v
     [ IR Surface Sensing ]               [ Ultrasonic Echo Sensing ]
     (Left & Right IR Sensors)            (HC-SR04 Distance Check)
               |                                     |
               v                                     v
     +-------------------+                 +-------------------+
     | Line Detected?    |                 | Obstacle < 10 cm? |
     +---------+---------+                 +---------+---------+
               |                                     |
       +-------+-------+                     +-------+-------+
       |               |                     |               |
     (Yes)            (No)                 (Yes)            (No)
       |               |                     |               |
       v               v                     v               v
 [Drive Forward/ [Search Track:        [OVERRIDE:       [Maintain Line
  Steer Pivot]    Turn Left/Right]      Halt Motors]     Navigation]
       |               |                     |               |
       +-------+-------+                     +-------+-------+
               |                                     |
               +------------------+------------------+
                                  |
                                  v
                  +-------------------------------+
                  | Continuous Closed-Loop Repeat |
                  +-------------------------------+
