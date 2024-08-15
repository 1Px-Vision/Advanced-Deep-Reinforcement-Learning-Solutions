# Project 2: Continuous Control

## Introduction

This project aims to develop and train a double-jointed arm agent capable of keeping its hand in continuous contact with a moving target for the maximum number of time steps.

![Environment](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/Project%20%20Continuous_Control/environment_illustration.gif)

# Understanding the environment

This environment has been developed using the Unity Machine Learning Agents Toolkit (ML-Agents). This open-source Unity plugin allows games and simulations to function as environments for training intelligent agents. In this setting, a double-jointed arm can move to specified target locations. The agent receives a reward of +0.1 for each step that its hand remains within the goal location. Therefore, the agent's objective is to keep its position at the target location for as many time steps as possible.

## State and action spaces

The observation space comprises 33 variables representing the arm's position, rotation, velocity, and angular velocities. Each action is a vector containing four numbers corresponding to the torque applied to two joints. Each entry in the action vector must be a value between -1 and 1.

## Solving the environment

There are two versions of the environment.

* **Version 1: One (1) Agent**
  The task is episodic, and to solve the environment, the agent must achieve an average score of +30 across 100 consecutive episodes.
  
* **Version 2: Twenty (20) Agents**
   The challenge in solving the second version of the environment is slightly different, considering the presence of multiple agents. Specifically, the agents must achieve an average score of +30, calculated over 100 consecutive episodes across all agents.

* At the end of each episode, we sum the rewards received by each agent (without applying any discount), resulting in a score for each agent. This process yields 20 potentially distinct scores, from which we calculate the average.
* This results in an average score per episode, with the average calculated across all 20 agents.
* The environment is solved when the moving average of the scores over 100 episodes reaches or exceeds +30.

# Included in this repository
  
* The code used to create and train the Agent, Continuous_Control.ipynb with model Actor and Critic, DDPG Agent, Training NN, and plot of rewards.
* The trained model in the Saved Model Weight directory
* A **Report.md** file detailing the development process, the learning Algorithm, and ideas for future work.

# Setting up the environment


