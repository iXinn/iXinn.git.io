---
title: Summary of A Perception-Driven Autonomous Urban Vehicle
tags: [Robotics, Computer Vision]
style: 
color: 
description: In this article, I summarize the paper A Perception-Driven Autonomous Urban Vehicle.

---

Source: [A Perception-Driven Autonomous Urban Vehicle](https://link.springer.com/chapter/10.1007/978-3-642-03991-1_5)

## Abstract

The paper discussed a designed driverless vehicle for DARPA Urban Challenge. To handle the challenge, the vehicle needs to be able to drive itself in complex real-life situations while obey the road rules. The team used many sensors of different types. The redundancy of sensors increases the need for power supplies. On another side, it makes sure the robustness of the system when any sensor fails. There are three types of sensor used: lidar (1 Velodyne and 12 Sick), radar (15 Delphi), and vision (5 Point Grey Firefly MV Cameras). The team built a low-latency, high-throughput communications framework (Lightweight Communications and Marshaling) for the communication among all the senders and receivers as well as a visualization of sensory data for development and debugging of algorithms. 

The team used a local frame in addition to GPS so that the vehicle can know its position when GPS coverage is poor. Lidars are used for near-field obstacle detection while radars are used for moving objects in the far field. Velodyne lidar is more powerful and could do most perception jobs for near-field obstacles. However, the Sick lidars fill blind spots of Velodyne, provide fault tolerance, and help detecting fast-moving obstacles (Sick Lidar is 75Hz while Velodyne is 15Hz). For Sick lidars, each area is covered by multiple Sick lidars so that the reliability of detection is higher. For Velodyne lidar, since it produces a million hits per second and requires a lot of computation, the team did data reduction by grouping a closed set of hits as “chunks”. Then, chunks are grouped into groups of connect components using Union-Find algorithm. At each time step, a new group will be associated with a group in previous time step, and Kalman filter is used to estimate the velocity of associated groups. However, since the time step is short, the system cannot estimate velocity of slow-moving obstables. Radars are used to detect moving objects outside detecting range of lidars. In addition, cameras are used for lane finding including road paint detection and lane centerline estimation.  

For planning and control algorithms, the team separates the system into navigator and motion planner. Navigator provides goal point of the vehicle while motion planner controls vehicle to reach the point. Perception data is rendered into a Drivability Map which includes infeasible regions, high-cost regions, and restricted regions. RRT algorithm with biased sampling is used for stable path planning in real-time applications.