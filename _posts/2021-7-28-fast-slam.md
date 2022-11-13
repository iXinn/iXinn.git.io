---
title: Summary of FastSLAM Paper
tags: [Robotics, Computer Vision]
style: 
color: 
description: In this article, I summarize the paper FastSLAM A Factored Solution to the Simultaneous Localization and Mapping Problem

---

Source: [FastSLAM: A Factored Solution to the Simultaneous Localization and Mapping Problem](https://www.semanticscholar.org/paper/FastSLAM%3A-a-factored-solution-to-the-simultaneous-Montemerlo-Thrun/6410aacd3f02f12bf78a8b3e5e664d263cee2183)

FastSLAM uses a modified particle filter to recursively estimates the full posterior distribution over robot pose and landmark locations. Compared to EKF-based SLAM algorithm that is in <img src="https://latex.codecogs.com/gif.latex?O(K^2)"/>, well-implemented FastSLAM can be in <img src="https://latex.codecogs.com/gif.latex?O(M\log(k))"/>in which M is the number of particles in the particle filter.  

In SLAM problem, each landmark measurements are independent when we know poses and correspondence. FastSLAM decomposes SLAM problem into a robot localization problem and a collection of landmark estimation problems conditioned on robot pose estimation. 

### SLAM Problem Definition

- Localization: estimation of pose from known landmarks
- Navigation: estimation of pose, velocities, and other states from known landmarks
- Mapping: estimation of landmark positions from known values of pose
- SLAM: joint estimation of pose and landmark positions (localization AND mapping  )

Robot Poses at Time t: <img src="https://latex.codecogs.com/gif.latex?s_t"/>

Robot Controls at Time t: <img src="https://latex.codecogs.com/gif.latex?u_t"/>

Landmark Measurements (range and bearing) at Time t: <img src="https://latex.codecogs.com/gif.latex?z_t"/>

Landmark Location in Space: <img src="https://latex.codecogs.com/gif.latex?\theta_k"/>for K immobile landmarks (points in the plane <img src="https://latex.codecogs.com/gif.latex?(x,y)"/>)

Correspondence (Index of Landmark): <img src="https://latex.codecogs.com/gif.latex?n_t\in\{1,\cdots,K\}"/>

- Motion model

  <img src="https://latex.codecogs.com/gif.latex?p(s_t|u_t,s_{t-1})"/>

- Measurement model

  <img src="https://latex.codecogs.com/gif.latex?p(z_t|s_t,\theta,n_t)"/>in which <img src="https://latex.codecogs.com/gif.latex?\theta=\{\theta_1,\theta_2,\cdots,\theta_k\}"/>

SLAM is the problem of determining the location of all landmarks <img src="https://latex.codecogs.com/gif.latex?\theta"/>​ and robot poses <img src="https://latex.codecogs.com/gif.latex?s_t"/>​ from landmark measuremnts and robot controls.

<img src="https://latex.codecogs.com/gif.latex?p(s^t,\theta|z^t,u^t)"/>if correspondences are unknown

<img src="https://latex.codecogs.com/gif.latex?p(s^t,\theta|z^t,u^t,n^t)"/>if correspondences are know (Superscript <img src="https://latex.codecogs.com/gif.latex?t"/>means a set of variables from time 1 to <img src="https://latex.codecogs.com/gif.latex?t"/>)

### FastSLAM with Known Correspondences

- Conditional independence of the SLAM problem implies that:

  <img src="https://latex.codecogs.com/gif.latex?p(s^t,\theta|z^t,u^t,n^t)=p(s^t|z^t,u^t,n^t)\cdot p(\theta|s^t,z^t,u^t,n^t)=p(s^t|z^t,u^t,n^t)\cdot \prod_kp(\theta_k|s^t,z^t,u^t,n^t)"/>

  which is a localization problem and K mapping problems.

- The localization problem is solved by a modified particle filter

- The mapping problems are solved by Kalman filters

### Particle Filter Path Estimation

Use of a filter that is similar to the Monte Carlo Localization algorithm. MCL is an application of particle filter to localization. At each point of time, the algorithm maintain a set of particles <img src="https://latex.codecogs.com/gif.latex?S_t"/>. <img src="https://latex.codecogs.com/gif.latex?m^{th}"/>particle <img src="https://latex.codecogs.com/gif.latex?s^{t,[m]}\in S_t"/>is a guess of the robot's path at time t:

<img src="https://latex.codecogs.com/gif.latex?S_t=\{s^{t,[m]}\}_m=\{s_1^{[m]},\cdots,s_t^{[m]}\}_m"/>

1. Each particle in <img src="https://latex.codecogs.com/gif.latex?S_{t-1}"/>is used to generate a guess of the robot's pose at time t:

   <img src="https://latex.codecogs.com/gif.latex?s_t^{[m]}\thicksim p(s_t|u_t,s_{t-1}^{[m]})"/>

2. Set of particles in <img src="https://latex.codecogs.com/gif.latex?S_{t-1}"/>is distributed according to <img src="https://latex.codecogs.com/gif.latex?p(s^{t-1}|z^{t-1},u^{t-1},n^{t-1})"/>. The new temporary set of particle is distributed according to <img src="https://latex.codecogs.com/gif.latex?p(s^{t}|z^{t-1},u^{t},n^{t-1})"/>, the proposal distribution of particle filtering,

3. New set <img src="https://latex.codecogs.com/gif.latex?S_t"/>is obtained by sampling from the temporary set. Each particle <img src="https://latex.codecogs.com/gif.latex?s^{t,[m]}"/>is drawn with replacement with a probability proportional to importance factor <img src="https://latex.codecogs.com/gif.latex?w_t^{[m]}=\frac{\text{target distribution}}{\text{proposal distribution}}=\frac{p(s^{t,[m]}|z^{t},u^{t},n^{t})}{p(s^{t,[m]}|z^{t-1},u^{t},n^{t-1})}"/>

### Landmark Location Estimation

This estimation <img src="https://latex.codecogs.com/gif.latex?p(\theta_k|s^t,z^t,u^t,n^t)"/>is conditioned on the robot pose, so the Kalman filters are attached to individual pose particles in <img src="https://latex.codecogs.com/gif.latex?S_t"/>. Hence, the full posterior is:

<img src="https://latex.codecogs.com/gif.latex?S_t=\{s^{t,[m]},\mu^{K,[m]},\Sigma^{K,[m]}\}"/>where <img src="https://latex.codecogs.com/gif.latex?\mu^{K,[m]},\Sigma^{K,[m]}"/>are set of means and covariances of the Gaussian representing landmarks 1 to K attached to the m particle.

When <img src="https://latex.codecogs.com/gif.latex?n_t=k"/>,

<img src="https://latex.codecogs.com/gif.latex?p(\theta_k|s^t,z^t,u^t,n^t)=p(z_t|\theta_k,s_t,n_t)p(\theta_k|s^{t-1},z^{t-1},u^{t-1},n^{t-1})"/>????

When <img src="https://latex.codecogs.com/gif.latex?n_t\neq k"/>,

<img src="https://latex.codecogs.com/gif.latex?p(\theta_k|s^t,z^t,u^t,n^t)=p(\theta_k|s^{t-1},z^{t-1},u^{t-1},n^{t-1})"/>

This is implemented using EKF. Compared to EKF-based SLAM, FastSLAM EKF only has a Gaussian of dimension two.

### Efficient Implementation

By using tree structure, the resampling process can be faster and end up with <img src="https://latex.codecogs.com/gif.latex?O(M\log(K))"/>

### Data Association

Data association is done per particle based: <img src="https://latex.codecogs.com/gif.latex?n_t^{[m]}=argmax_{n_t}p(z_t|s_t^{[m]},n_t)"/>

