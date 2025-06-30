---
date: '2025-06-30T14:54:16+08:00'
draft: false
title: 'Rugger Rover Introduction'
---
Some time ago, after a late night experimenting with ROS 2 simulated robots, I decided to bite the bullet and invest in some actual hardware. After searching for appropriate hardware I settled on the Lynxmotion - A4WD3 Rugged Wheeled Rover pictured below ![rugger wheeled rover](/images/lynxmotion-a4wd3-wheeled-rover.webp)
The platforms were selected for their rugged design, the all-wheel drive configuration, and their modular chassis — making them ideal for outdoor navigation experiments and hardware prototyping. The A4WD3 features four independently driven wheels, gear motors with encoders, a spacious internal bay for electronics, and aluminum construction tough enough to handle dirt paths, grass, and gravel without issue.
After unboxing and assembling the rover, I began planning how to turn it into a fully autonomous mobile robot. I had already been working with ROS 2 in simulation using Gazebo and RViz, and I wanted a real-world platform to test navigation stacks, SLAM, and sensor integration. Here's the stack I decided on:

* Raspberry Pi 4B (4GB) running ROS 2 Humble as the brain of the rover.

* Arduino Mega acting as the low-level motor controller and encoder interface.

* Two Sabertooth 2x12 motor controllers, each driving a pair of motors (left and right).

* Quadrature encoders on each motor for feedback and odometry calculation.

* A LiDAR sensor for 2D SLAM and obstacle avoidance.

The design philosophy was simple: offload real-time tasks like PID speed control and encoder decoding to the Arduino, while the Raspberry Pi handles the high-level autonomy — path planning, SLAM, and sensor fusion. The two communicate via a robust serial protocol over USB.

In upcoming posts, I’ll dive into the wiring and system integration, including how I configured the Sabertooth controllers, implemented encoder feedback loops, and created the ROS 2 nodes that bring it all together.

Stay tuned for schematics, source code, and some real-world testing footage!