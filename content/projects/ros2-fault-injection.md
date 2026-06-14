---
title: 'ROS 2 Fault Injection'
date: '2026-06-14T08:15:00+08:00'
draft: false
summary: 'A ROS 2 fault injection framework for testing robot robustness with configurable odometry, laser scan, IMU, TF, service, and assertion scenarios.'
tags: ['ros2', 'robotics', 'fault-injection', 'testing', 'rviz', 'nav2']
---

# ROS 2 Fault Injection

`ros2_fault_injection` is a framework I have been building to make robot robustness testing
more practical. The goal is simple: inject controlled faults into a ROS 2 system, observe how
the autonomy stack behaves, and turn those experiments into repeatable scenarios rather than
one-off manual tests.

The project started with odometry faults for the rover, then grew into a more general ROS 2
package with typed injectors, YAML scenarios, services, an RViz panel, runtime configuration,
and assertion events.

## What It Does

The framework can sit between raw robot data and the rest of the autonomy stack. It receives
a real topic or service request, applies configured fault behavior, and republishes or
forwards the result.

Current fault areas include:

- Odometry faults such as position bias, yaw bias, noise, dropout, delay, and covariance
  inflation
- LaserScan faults such as noise, bias, delay, dropout, and sector dropout
- IMU linear acceleration noise
- Joint state and motor feedback faults
- TF faults for transform-level testing
- Service trigger faults

Scenarios are described in YAML so they can be launched, repeated, changed, and reviewed.

## Runtime Control

The package exposes ROS 2 services for:

- Listing available faults
- Reading current fault status
- Activating and deactivating faults
- Updating fault configuration at runtime
- Reloading a scenario file
- Querying the schema for a fault configuration

I also built an RViz panel so faults can be inspected and controlled while the robot or
simulation is running. That made the tool much more usable during rover testing because I
could activate a fault, watch the event stream, and immediately see how Nav2 or SLAM
responded.

## Assertions

The newest part of the framework is scenario assertions. These are checks that describe what
should happen during a fault experiment.

For example, a scenario can assert that:

- A scheduled fault becomes active within a deadline
- A fault later becomes inactive
- A topic continues publishing above a minimum frequency

Assertion results are published as ROS 2 events, which makes the framework feel less like a
manual demo tool and more like the start of a repeatable robotics test harness.

## Why I Built It

On the rover, failures are rarely clean. Odometry can drift, lidar scans can drop out, motor
feedback can become noisy, transforms can be wrong, and services can fail at awkward times.
Testing those cases by manually breaking things is slow and inconsistent.

This framework gives me a way to ask better questions:

- What happens to Nav2 if odometry yaw is biased?
- Does SLAM keep behaving if scan data drops in one sector?
- Does the robot continue publishing usable data during a fault?
- Can I reproduce the same failure scenario tomorrow?

That makes it useful both for the Unity simulation and for the real rover.

## Direction

The next areas I want to improve are broader assertion support, cleaner plugin-style
injector extension, better documentation, and more realistic fault scenarios based on what
the physical rover actually does when hardware starts misbehaving.

Repos:

[github.com/reeceholland/ros2_fault_injection](https://github.com/reeceholland/ros2_fault_injection)

[ros2-fault-injection.readthedocs.io](https://ros2-fault-injection.readthedocs.io/)
