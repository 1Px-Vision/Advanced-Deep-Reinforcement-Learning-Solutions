# Project 2: Continuous Control

## Introduction

This project aims to develop and train a double-jointed arm agent capable of keeping its hand in continuous contact with a moving target for the maximum number of time steps.

![Environment](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/Project%20%20Continuous_Control/environment_illustration.gif)

## Included in this repository 

* The code used to create and train the Agent
* Continuos_Control.ipynb - notebook containing the challenge for this project
* The trained model
* checkpoint.pt
* Files describing all the packages required to set up the environment
* The Report.md file describing the development process and the learning algorithm, along with ideas for future work
* This README.md file

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

This section outlines the steps to obtain the project code, set up the local environment, and download the Unity environment and the Agents.

## Download the Unity environment with the Agents

Select and download the appropriate environment for your operating system from the links below, then decompress the file into your project folder:

* Version 1: One (1) Agent : https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/one_agent/Reacher_Linux_NoVis.zip
* Version 2: Twenty (20) Agent : https://s3-us-west-1.amazonaws.com/udacity-drlnd/P2/Reacher/Reacher_Linux_NoVis.zip
 
## Adjusting the Hyperparameters

* target_score: How many points your agent must obtain to consider the environment solved
* target_episodes: How many episodes to consider when calculating the moving average
* n_episodes: Maximum number of training episodes
* max_t: Maximum number of time steps per episode
* random_seed: The number used to initialize the pseudorandom number generator
* batch_size: Minibatch size
* buffer_size: Replay buffer size
* gamma: Discount factor for expected rewards
* lr_actor: Learning rate for the local actor's network
* lr_critic: Learning rate for the local critic's network
* tau: Multiplicative factor for the soft update of the target networks' weights
* noise_decay: Multiplicative factor for the noise-process rate decay
* fc_layers for the actor-network: Number and size of the actor network's layers
* fc_layers for the critic network: Number and size of the critic network's layers

