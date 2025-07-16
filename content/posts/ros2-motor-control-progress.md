---
title: "ROS 2 Motor Control Progress Log — Week in Review"
date: '2025-07-17T06:20:21+08:00'
draft: false
tags: ["ros2", "arduino", "motor-control", "sabertooth", "lynxmotion", "robotics"]
categories: ["Development Log"]
summary: "From hardware setup to working ROS 2 integration, PID tuning, and automated testing — a full update on my rover’s motor control progress."
---

# 🚀 ROS 2 Motor Control Progress Log — Week in Review

I’ve finally begun the **real-world hardware integration** phase of my autonomous rover project. This week marks solid progress on multiple fronts, from hardware wiring to ROS 2 integration and testing.

---

## 🛠️ Hardware Setup Complete

The physical platform is now up and running:

- **Chassis**: Lynxmotion A4WD3 Rugged Wheeled Rover  
- **Motor controller**: Single **Sabertooth 2x12** motor driver in packetized serial mode  
- **Microcontroller**: **Arduino Mega**, interfacing with encoders and Sabertooth  
- **Host PC**: Laptop running **ROS 2 Humble** via USB serial  

✅ Serial communication, motor control, and encoder feedback have been successfully tested.

---

## 🧠 Arduino Firmware

A custom Arduino sketch has been developed which:

- Receives velocity commands in **rad/s** from ROS
- Computes **PID control** loops for each wheel
- Drives motors using the Sabertooth serial protocol
- Samples encoder ticks to calculate wheel velocities

> 🔧 PID tuning has been challenging — initial values led to slow or unstable responses. Compensation for deadband and better control loop tuning are in progress.

---

## 🧪 ROS 2 Integration

A **custom ROS 2 node** `motor_controller_node` was implemented in C++ to interface with the Arduino:

- Subscribes to `sensor_msgs/msg/JointState` to receive wheel velocity commands
- Sends those commands to the Arduino via serial
- Plans underway to publish feedback and diagnostics

A dedicated **driver library** abstracts the serial protocol, separating logic from ROS-specific code for reusability and testability.

---

## ✅ Unit Testing & GitHub Actions

To improve robustness and CI/CD:

- **Unit tests** validate packet structure, byte formatting, and checksum logic
- **GitHub Actions** run tests and format checks on push
- **Clang-format** is enforced using `.clang-format` and a required status check on the main branch

---

## 🧰 Tools & Debugging

- A **standalone serial tool** was created to send test commands and receive feedback from the Arduino
- **CuteCom** and **minicom** were used to manually test serial commands and response structure

---

## 🔮 Next Steps

- 🔧 **Add configuration interfaces**: Expose PID values and other tuning parameters via **ROS 2 services**
- 🔩 **Hardware mounting**: Secure Arduino, Sabertooth, and power system on the Lynxmotion chassis
- 🧪 **Improve PID tuning**: Address deadband issues, overshoot, and convergence delay
- 📡 **Feedback publishing**: Send total wheel distance from Arduino to ROS

---

**Stay tuned!** This project is evolving quickly now that the hardware and communication stack are working.  
👉 Check the repo: [GitHub - rugged_rover](https://github.com/reeceholland/rugged_rover)
