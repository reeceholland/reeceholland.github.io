---
title: 'Rugged Rover'
date: '2026-06-14T08:10:00+08:00'
draft: false
summary: 'A ROS 2 autonomous rover platform based on the Lynxmotion A4WD3, Raspberry Pi 5, Teensy 4.1, micro-ROS, SLAM Toolbox, Nav2, RPLIDAR S2, and Unity simulation.'
tags: ['ros2', 'robotics', 'nav2', 'slam', 'micro-ros', 'embedded', 'unity']
---

# Rugged Rover

The Rugged Rover is my real-world ROS 2 autonomy platform based on the Lynxmotion A4WD3
chassis. It is the system I use to explore mobile robotics beyond the comfortable parts of
simulation: motor control, wheel odometry, lidar alignment, IMU integration, power delivery,
USB bandwidth, network stability, and navigation tuning.

![Lynxmotion A4WD3 rover](/images/lynxmotion-a4wd3-wheeled-rover.webp)

## Current Stack

- **Compute:** Raspberry Pi 5 running Ubuntu Server 24.04 and ROS 2 Jazzy
- **Motor control:** Teensy 4.1 firmware with micro-ROS
- **Driver:** Sabertooth motor controller
- **Odometry:** Quadrature encoder feedback through `ros2_control`
- **Lidar:** RPLIDAR S2
- **Depth camera:** Intel RealSense D435
- **IMU:** SparkFun Razor IMU
- **Navigation:** SLAM Toolbox and Nav2
- **Simulation:** Unity with ROS TCP Connector
- **Diagnostics:** Battery voltage monitoring and platform debug topics

## What Is Working

- Real robot bringup with ROS 2 Jazzy
- Teensy to Raspberry Pi UART micro-ROS transport
- Encoder feedback and differential drive control
- Battery voltage monitoring and critical battery guard paths
- RPLIDAR S2 bringup and SLAM Toolbox mapping
- Nav2 navigation in the real test environment
- Unity simulation launch path for testing and fault injection
- Raspberry Pi bootstrap script for repeatable setup

## Problems Solved So Far

This project has been a useful reminder that robotics progress often looks like debugging
five unrelated systems that are all failing at once.

Some of the more interesting fixes:

- Reworked motor firmware reconnect handling so the Teensy can recover when the micro-ROS
  agent restarts.
- Corrected motor gearbox assumptions after measuring encoder counts directly.
- Tuned effective wheel separation for skid-steer odometry rather than relying only on
  physical wheel spacing.
- Added battery voltage diagnostics and safety behavior.
- Moved the rover computer to a Raspberry Pi 5 for more CPU headroom.
- Tracked down USB power and bandwidth problems between the RPLIDAR, RealSense camera, and
  onboard computer.
- Tuned SLAM without loop closure first, then brought loop closure back once odometry was
  stable enough.
- Added ROS 2 linting, formatting, and bootstrap documentation so the repo is easier to
  reproduce.

## Direction

The immediate goal is to finish this rover as a reliable baseline platform. After that, I
want to build a second rover and start experimenting with multi-robot work: shared mapping,
coordinated exploration, and eventually robot teaming.

The code lives here:

[github.com/reeceholland/rugged_rover](https://github.com/reeceholland/rugged_rover)
