---
title: 'Rugged Rover Update: Real SLAM, Nav2, and a Raspberry Pi 5'
date: '2026-06-14T08:30:00+08:00'
draft: false
tags: ['ros2', 'robotics', 'nav2', 'slam', 'raspberry-pi', 'micro-ros']
categories: ['Development Log']
summary: 'A progress update on the Rugged Rover: Teensy firmware, micro-ROS, RPLIDAR S2, Razor IMU, battery monitoring, SLAM Toolbox, Nav2, and the move to Raspberry Pi 5.'
---

# Rugged Rover Update: Real SLAM, Nav2, and a Raspberry Pi 5

The rover has moved a long way from the early motor-control experiments. It is now running
as a full ROS 2 Jazzy robot on a Raspberry Pi 5, with real sensors, real odometry, and Nav2
starting to behave like something I can build on.

## Hardware and Firmware

The low-level platform is now based around a Teensy 4.1 running micro-ROS firmware. It
handles encoder feedback, motor commands, battery voltage monitoring, status/debug output,
and communication with the Sabertooth motor controller.

A few important firmware improvements went in:

- Reconnect handling for the micro-ROS agent
- Cleaner debug output so serial logs do not interfere with normal operation
- Corrected motor gearbox parameters after measuring encoder counts directly
- Battery voltage publishing and critical battery guard behavior
- Platform debug messages for runtime visibility

The motor tuning process was very hands-on: command a speed, watch encoder feedback, adjust
the control behavior, and repeat until the rover moved smoothly enough for odometry and SLAM
to become meaningful.

## Sensors

The current sensor stack includes:

- RPLIDAR S2 for 2D laser scans
- SparkFun Razor IMU
- Intel RealSense D435 for depth sensing experiments
- Wheel encoders for odometry

The RPLIDAR and D435 uncovered the usual real-robot lessons: USB power, USB bandwidth, port
stability, and device startup order all matter. The lidar became much more reliable once the
Raspberry Pi power setup was fixed.

## SLAM and Navigation

SLAM Toolbox is now producing usable maps. The biggest breakthrough was getting wheel odometry
stable first. When odometry was wrong, SLAM was messy no matter how much I tuned the mapping
parameters.

The process that worked best:

1. Tune wheel odometry using measured travel and rotation tests.
2. Run SLAM without loop closure until the map is locally stable.
3. Reintroduce loop closure once odometry is trustworthy.
4. Bring Nav2 in after the map and transforms are behaving.

Nav2 is now running on the real rover, and I have been tuning the local planner, angular
velocity limits, costmaps, and transform timing. It is not finished, but it is now at the
stage where the problems are tuning problems rather than fundamental bringup problems.

## Simulation

The Unity simulation is still part of the workflow. It gives me a faster place to test
launch files, topic remapping, fault injection, and navigation behavior before moving back
onto hardware.

The real robot and simulation are now close enough that configuration drift is becoming a
thing I actively watch for.

## Repository Cleanup

I also spent time making the repository easier to reproduce:

- Added Raspberry Pi 5 bootstrap setup for Ubuntu 24.04 and ROS 2 Jazzy
- Updated bringup documentation
- Added common launch and debug commands
- Moved formatting/linting toward ROS 2 tooling
- Cleaned package metadata and copyright headers

## Next Step

The goal now is to finish this rover as a dependable baseline platform, then build another
one and start experimenting with multi-robot mapping and coordination.

The garage has officially become the test arena.

Repo:

[github.com/reeceholland/rugged_rover](https://github.com/reeceholland/rugged_rover)
