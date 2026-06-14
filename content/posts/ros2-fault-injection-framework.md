---
title: 'Building a ROS 2 Fault Injection Framework'
date: '2026-06-14T08:40:00+08:00'
draft: false
tags: ['ros2', 'robotics', 'fault-injection', 'testing', 'rviz', 'nav2']
categories: ['Development Log']
summary: 'A development update on ros2_fault_injection: typed injectors, YAML scenarios, runtime services, RViz controls, event publishing, and assertion-based fault testing.'
---

# Building a ROS 2 Fault Injection Framework

One of the more useful side projects to come out of the rover work has been
`ros2_fault_injection`: a ROS 2 framework for deliberately breaking parts of a robot system
in controlled, repeatable ways.

The idea started simply. I wanted to inject faults into odometry so I could see how SLAM,
Nav2, and the rest of the stack behaved when the data was not perfect. That quickly became
more interesting than a one-off test node, because the same pattern applies across a lot of
robot inputs: scans, IMU data, motor feedback, transforms, and services.

So I started turning it into a framework.

## The Problem

Robots rarely fail in neat, obvious ways.

Odometry drifts. Lidar scans drop out. IMU readings get noisy. Motor feedback can become
delayed or biased. Transforms can be wrong. Services can fail at exactly the wrong moment.

It is possible to test those cases manually, but it is not very repeatable. Change a launch
file, add a temporary node, break a topic, test something, then try to remember exactly what
was changed later.

I wanted a cleaner workflow:

- Describe a fault scenario in YAML
- Launch the system with that scenario
- Activate faults manually or on a schedule
- Watch events as faults start, stop, or change configuration
- Check whether the system behaved as expected

That became the shape of `ros2_fault_injection`.

## Current Features

The framework now has typed injectors for different parts of the ROS graph. Each injector
owns the logic for one kind of data and exposes a common interface to the controller,
scheduler, services, and RViz panel.

Current fault areas include:

- Odometry faults: position bias, yaw bias, noise, delay, dropout, and covariance scaling
- LaserScan faults: noise, bias, delay, dropout, and sector dropout
- IMU linear acceleration noise
- Joint state and motor feedback faults
- TF faults
- Service trigger faults

The scenario file defines the injectors, faults, schedules, and assertions. A fault can be
active on startup, scheduled to start later, given a duration, or controlled manually at
runtime.

## Runtime Services

I added ROS 2 services so faults can be inspected and controlled while the robot is running.

The framework can now:

- List available faults
- Report active and inactive fault states
- Activate or deactivate a fault
- Update fault configuration values at runtime
- Reload the scenario file
- Return the schema for a fault so tools can understand valid keys and value ranges

The schema service ended up being more useful than I expected. It means the UI does not need
to hard-code every possible config key. The injector owns its own schema, and callers can ask
what is valid.

## RViz Panel

The RViz panel made the framework feel much more usable.

From RViz I can now:

- See the current fault list
- Activate and deactivate faults
- Edit fault config values
- Reload a scenario
- Watch fault events
- Watch assertion events

That matters because most of the time I am already using RViz to look at the rover, map,
scan, transforms, costmaps, and planned path. Having fault controls in the same place keeps
the workflow tight.

## Event Publishing

Every meaningful change is published as an event:

- Fault activated
- Fault deactivated
- Fault configuration updated
- Assertion passed or failed

This gives the system a timeline. Instead of guessing what happened, I can see when a fault
started, what changed, and whether the expected outcome occurred.

## Assertions

The newest part of the framework is assertions.

Fault injection is useful on its own, but the more interesting question is not just “can I
break the data?” It is:

“Did the robot behave acceptably while the fault was running?”

The first assertion type checks fault events. For example, a scenario can expect that a
scheduled fault becomes active within six seconds, then becomes inactive later.

I have also started adding topic rate assertions, so a scenario can check that a topic stays
above a minimum frequency during a test. For example, `/odom` should continue publishing
above 10 Hz while a fault scenario is running.

That moves the framework closer to a repeatable robotics test harness rather than a manual
demo tool.

## Why This Helps the Rover

The rover now has enough real hardware online that robustness testing is becoming important.
It is running ROS 2 Jazzy, Nav2, SLAM Toolbox, lidar, wheel odometry, micro-ROS firmware,
motor feedback, battery monitoring, and simulation support.

That creates a lot of places where degraded data can affect autonomy.

With fault injection, I can ask practical questions:

- What happens to Nav2 when odometry yaw is biased?
- Does SLAM stay usable if part of the scan drops out?
- Do services fail gracefully?
- Does the system keep publishing critical data under fault conditions?
- Can I reproduce the same failure scenario later?

Those are much better questions than “does it work on a good day?”

## What I Learned

The biggest lesson so far is that the framework needs to stay boring internally.

The first idea was tempting: intercept DDS traffic, manipulate packets, and make it clever.
But for this stage, ROS-level injectors are the better tradeoff. They are understandable,
testable, visible in the ROS graph, and easier to control from launch files and RViz.

Keeping it as normal ROS 2 nodes, services, messages, and YAML has made the system much
easier to evolve.

## Next Steps

The next things I want to improve are:

- Finish topic rate assertions
- Add more assertion types around message content and service behavior
- Continue moving injector-specific validation into injector-owned schemas
- Improve plugin-style extension for new injectors
- Add more real-world scenarios based on rover failures

This started as a small odometry test tool. It is now turning into one of the more useful
parts of the rover development workflow.

Repo:

[github.com/reeceholland/ros2_fault_injection](https://github.com/reeceholland/ros2_fault_injection)

Docs:

[ros2-fault-injection.readthedocs.io](https://ros2-fault-injection.readthedocs.io/)
