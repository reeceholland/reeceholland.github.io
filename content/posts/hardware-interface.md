---
date: '2025-07-24T05:52:43+08:00'
draft: false
title: 'Hardware Interface'
---
# Developing a Custom ROS 2 Hardware Interface for the Rugged Rover

---

Over the past few weeks, I’ve been working on integrating a custom hardware interface for the rugged wheeled rover platform we're building. This post highlights the development process of the ROS 2 hardware interface, the decision to upgrade to a Teensy 4.1 microcontroller, and the ongoing challenges with PID tuning.

---

## Hardware Interface with `ros2_control`

The robot uses a differential drive setup controlled via a Sabertooth 2x12 motor driver. To interface this with ROS 2, I created a custom package: `rugged_rover_hardware_interfaces`, following the `ros2_control` system interface pattern.

The `SabertoothSystemInterface`:

- Exports position and velocity state interfaces for the left and right wheel joints.
- Publishes battery voltage as an additional `StateInterface`.
- Publishes joint velocity commands via `sensor_msgs/JointState`.
- Subscribes to custom feedback messages (`rugged_rover_interfaces/msg/RoverFeedback`) coming from the Arduino/Teensy, which includes encoder-based position, velocity, and battery voltage.

The interface is fully wired into the ROS 2 control loop and integrates cleanly with `controller_manager` and `joint_state_broadcaster`.

---

## Microcontroller Upgrade: From Arduino Mega to Teensy 4.1

During initial development, I used an **Arduino Mega** to read encoders, perform PID calculations, and communicate with ROS via serial. While the Mega worked well for prototyping, several limitations started to bottleneck development:

- **Limited clock speed (16 MHz)** made it hard to handle high-frequency PID control and multi-tasking serial communications.
- **Large physical footprint** made internal mounting in the chassis more difficult.
- **Lack of native USB high-speed and modern architecture** led to sporadic delays under load.

### The Decision: Teensy 4.1

To solve these issues, I’ve decided to move forward with a **Teensy 4.1**:

- **600 MHz ARM Cortex-M7** processor with 1 MB RAM gives us ample performance headroom.
- **Compact form factor** reduces space requirements in the electronics enclosure.
- **Native USB High-Speed** (480 Mbps) provides snappy ROS serial integration.
- **Multiple hardware serial ports** (up to 8!) simplifies wiring and expands options for future peripherals (e.g., GPS, IMU).

The upgrade will also enable smoother encoder sampling, non-blocking communication, and support for real-time motor control tasks on a single MCU.

---

## PID Tuning Challenges

One of the more stubborn problems has been tuning the **wheel speed PID controllers**.

- With simulated data or slow commands, everything behaves predictably.
- Under real load, slight mismatches in encoder readings or noisy sampling can cause aggressive oscillations or sluggish response.
- Integral windup occasionally causes the rover to overshoot or lock into runaway output when reversing direction.

Next steps include:

- Implementing **anti-windup** in the PID logic.
- Adding **low-pass filtering** to the encoder velocity signal.
- Dynamically scaling PID gains based on load or command profile.
- Profiling CPU usage and ensuring sampling intervals remain consistent (especially after switching to Teensy).

---

## What’s Next

- Finalize wiring for the Teensy 4.1 and update the firmware to support its serial and interrupt pin layout.
- Connect ROS 2 controllers (like `diff_drive_controller`) and test real-world trajectory tracking.
- Improve unit testing for the hardware interface and expand test coverage for safety-critical paths.
- Explore publishing diagnostics like battery voltage, current draw, and motor temperature for system health monitoring.

---

Stay tuned for future posts as I bring the full stack online—from URDF and Gazebo simulation to autonomous waypoint navigation and mission execution.



