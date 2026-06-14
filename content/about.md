---
date: '2026-06-14T08:00:00+08:00'
draft: false
title: 'About'
---

## About Me

I'm Reece Holland, a software engineer building practical robotics systems with ROS 2,
embedded firmware, simulation, desktop tooling, and real hardware.

Most of my recent work has been focused on an autonomous Lynxmotion A4WD3 rover. The
project started as a way to move beyond simulation and learn what breaks when a robot has
to deal with real motors, real sensors, power limits, noisy odometry, serial links, and
compute constraints.

Alongside the rover, I have been building ros2_fault_injection: a ROS 2 framework for
injecting faults into robot data streams and checking whether the autonomy stack behaves as
expected. It started with odometry faults and has grown into a configurable test tool with
multiple injectors, services, RViz controls, scenario reloads, event reporting, and
assertions.

I have also been building CONFIGURED, a C++17 and Qt 6 desktop application for creating,
validating, exporting, and Git-managing structured robotics configuration projects. That
work has been a chance to focus on the parts of software that make a tool feel real:
validation, project workflows, packaging, release notes, Linux and Windows builds, and
public documentation.

The current rover stack includes:

- ROS 2 Jazzy on Ubuntu 24.04
- Raspberry Pi 5 as the onboard computer
- Teensy 4.1 running micro-ROS firmware
- Sabertooth motor control with encoder feedback
- RPLIDAR S2 for SLAM and navigation
- Intel RealSense D435 for depth sensing experiments
- SparkFun Razor IMU integration
- Battery voltage monitoring and diagnostics
- Unity simulation for fault injection and repeatable testing
- SLAM Toolbox and Nav2 for mapping and autonomous navigation
- ros2_fault_injection for robustness testing and scenario-driven fault experiments

I enjoy the full stack of robotics: writing firmware, building ROS 2 packages, tuning
control loops, debugging transforms, making simulation useful, building supporting tools,
and then taking the robot onto the floor to see what reality thinks of the design.

This site is where I document that process: the mistakes, the fixes, the design decisions,
and the small wins that gradually turn a pile of hardware and code into software and robots
that can move with purpose.
