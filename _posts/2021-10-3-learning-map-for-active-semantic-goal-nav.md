---
title: Summary of Active Semantic Goal Navigation Paper
tags: [Robotics, Computer Vision]
style: 
color: 
description: In this article, I summarize the paper Learning to Map for Active Semantic Goal Navigation

---

Source: [Learning to Map for Active Semantic Goal Navigation](https://arxiv.org/pdf/2106.15648.pdf)

## Key

learning of contextual semantic priors

## Contribution:

- **novel framework** that actively learns to generate semantic maps outside the field of view of the agent and leverages the uncertainty over the semantic classes in the unobserved areas to decide on long term goals.
- balancing exploration with exploitation during searching for semantic targets

## Approach

Learning a mapping model than can predict (hallucinate) semantics in unobserved regions of the map containing both objects (e.g. chairs, beds) and structures (e.g. floor, wall), and during testing uses the uncertainty over these predictions to plan a path towards the target.

## Our work differs from these methods in three principled ways: 

1) We formulate an active training strategy for learning the semantic maps, 
1) we exploit the uncertainty over the predictions in the planning process, and 
1) while occupancy anticipation mainly learns to extend what it already sees, applying the same concept to semantics is not trivial. The latter needs to learn semantic patterns (e.g. tables are usually surrounded by chairs) in order to infer semantic information in unobserved regions.

## Learning to Map (L2M)

- an ensemble of **hierarchical** segmentation models by choosing training samples through an information gain objective.
- estimate the model uncertainty through the disagreement of ensemble models, and show its effectiveness in defining objectives in planners to actively select long-term goals for semantic navigation.

SLAM systems rarely consider active exploration as they are not naturally compatible with task-driven learnable representations from deep learning architectures that can encode semantic information????

## Semantic Map Prediction

We formulate the semantic map prediction as a hierarchical segmentation problem.

### Input:

occupancy region: obtained by camera <img src="https://latex.codecogs.com/gif.latex?p_t"/> 

semantic segmentation: estimated by **UNet Encoder Decoder** <img src="https://latex.codecogs.com/gif.latex?\hat{s_t}"/> (In the end of UNet, the spatial dims are restored to make a prediction for each pixel in the input image.)

### Output:

Top-down semantic&occupancy local region <img src="https://latex.codecogs.com/gif.latex?\hat{m_t}"/> 

**Assume growth-truth semantic information is available**

- learn to hallucinate unseen spatial configurations based on what is already observed
- given predicted occupancy, we predict the final semantic region <img src="https://latex.codecogs.com/gif.latex?\hat{m_t}"/>

<img src="https://raw.githubusercontent.com/Siming-He/siming-he.github.io/master/assets/post/2021-10-3-learning-map-for-active-semantic-goal-nav/learning-map-for-active-semantic-goal-nav.png" style="zoom:50%;" />

## Uncertainty as an objective

We considered two types of uncertainty we face in modeling vision problems with deep learning: **aleatoric (uncertainty in the system we are modeling) and epistemic (uncertainty in model's parameters) uncertainty**

Samples with high epistemic uncertainty are associated with increased information gain.

Epistemic uncertainty are used to formulate the objective of identifying underperformed cases to improve the prediction model.

Epistemic uncertainty is approximated from the variance between the outputs of the models in the ensemble.

Using uncertainty estimation in **two ways**:

## **First**

During training of the semantic predictor we actively select locations of the map with high information gain (Section 4.1). 

**Active training:** greedy objective for information gain allows us to collect data in directions where we expect to make the most informative observations.

We select location of the grid with max information gain (max variance of model over all classes).

Alternatives of active learning strategy are compared in section (6.2).

Then the agent moves using a local policy (Section 5.3).

## Second

During object-goal navigation we actively choose long-term goals that encourage the agent to explore in search of the target object (Section 5). 

**Goal Navigation Policy:** 

Partially observable Markov decision process (S,A,O,P,R)

Defining a policy that outputs goal location as close as possible to a target class c.

S: agent's pose x and semantic predictions over time in the global map

A: set of locations <img src="https://latex.codecogs.com/gif.latex?h\cdot w"/>

O: RGB-D egocentric observation

P: transition probabilities

R: <img src="https://latex.codecogs.com/gif.latex?R(s,a)=D(s,c)-D(s',c)"/> where D is distance on the shortest path

Select informative golas by leveraging the **prdicted class probabilities** and **uncertainty estimation** over map locations.
