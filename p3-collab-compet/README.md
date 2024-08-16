# Project 3: Collaboration and Competition

## Introduction

This project aims to train two agents controlling rackets to bounce a ball over a net.

![Train_tenis](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/p3-collab-compet/trained_agents.gif)


## Understanding the environment

This environment is constructed using the Unity Machine Learning Agents Toolkit (ML-Agents), an open-source Unity plugin that allows games and simulations to function as environments for training intelligent agents. The project environment provided by Udacity closely resembles the Tennis environment found on the Unity ML-Agents GitHub page, though it is not identical. In this environment, two agents control rackets to volley a ball over a net. When an agent successfully hits the ball over the net, it earns a reward of +0.1. Conversely, if an agent allows the ball to hit the ground or hits it out of bounds, it incurs a penalty of -0.01. The primary objective for each agent is to keep the ball in play.

### State and action spaces

The observation space comprises 8 variables representing the ball's and the racket's position and velocity. Each agent receives a unique, local observation. This environment stacks 3 vector observations to monitor multiple past observations, allowing comparisons over time. Consequently, each agent receives an observation vector containing 24 variables. Two continuous actions are available: moving toward or away from the net and jumping.

### Solving the environment

To solve the environment, the task is episodic, and your agents must achieve an average score of at least +0.5 over 100 consecutive episodes, using the maximum score between the two agents. Here's how it works:

* After each episode, calculate the total rewards received by each agent (without applying any discount), resulting in two separate scores. From there, take the maximum score.
* This maximum score represents the episode's score.
* The environment is considered solved when the average of these scores over 100 episodes reaches or exceeds +0.5.
  
