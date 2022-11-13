---
title: Summary of Active Visual Object Search Paper
tags: [Robotics, Computer Vision]
style: 
color: 
description: In this article, I summarize the paper Active Visual Object Search in Unknown Environments Using Uncertain Semantics

---

Source: [Active Visual Object Search in Unknown Environments Using Uncertain Semantics](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=6507635)

The paper presents a probabilistic model of the search environment to allow for **prioritizing the search effort to those parts of the environment that are most promising for a specific object type**. It also shows a method for reasoning about the unexplored part of the environment for goal-directed exploration.

**Trade between exploring unknown environment and looking in known environment.**

- Constructing priors with various assumptions about initial state

- View planning
  - Art gallery algorithms

Contribution

- Large-scale environment
- Starts with no information about the specific environment
- Interleave exploration and exploitation
- Provide thorough quatatitive and qualatitive analysis

Problem formulation

- Finding an efficient strategy to localize a certain object in a large-scale unknown 3-D indoor environment. (Localize the point of interest with minimum total cost)
- Motion action and sensing action in the environment

SIFT-based method to find and estimate the pose of the target object

Search Space

- **The ability to reduce the search space is crucial for practical applications.**
- Modeling the search space as a **Place Map**
- **Assigning probabilities based on a probablistic semantic mapping framework**
  - **COLD database for place catagorization**
  - **Open Mind Indoor Common Sense Database for object-object and object-place relation**
- Reasoning about unsearched parts of the environment
- Modeling the search space on the room scale based on 
