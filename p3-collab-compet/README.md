# Project 3: Collaboration and Competition

## Introduction

This project aims to train two agents controlling rackets to bounce a ball over a net.

![Train_tenis](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/p3-collab-compet/trained_agents.gif)


## Understanding the environment

This environment is constructed using the Unity Machine Learning Agents Toolkit (ML-Agents). This open-source Unity plugin allows games and simulations to function as environments for training intelligent agents. The project environment provided by Udacity closely resembles the Tennis environment found on the Unity ML-Agents GitHub page, though it is not identical. In this environment, two agents control rackets to volley a ball over a net. When an agent successfully hits the ball over the net, it earns a reward of +0.1. Conversely, if an agent allows the ball to hit the ground or hits it out of bounds, it incurs a penalty of -0.01. The primary objective for each agent is to keep the ball in play.

### State and action spaces

The observation space comprises 8 variables representing the ball's and the racket's position and velocity. Each agent receives a unique, local observation. This environment stacks 3 vector observations to monitor multiple past observations, allowing comparisons over time. Consequently, each agent receives an observation vector containing 24 variables. Two continuous actions are available: moving toward or away from the net and jumping.

### Solving the environment

To solve the environment, the task is episodic, and your agents must achieve an average score of at least +0.5 over 100 consecutive episodes, using the maximum score between the two agents. Here's how it works:

* After each episode, calculate the total rewards received by each agent (without applying any discount), resulting in two separate scores. From there, take the maximum score.
* This maximum score represents the episode's score.
* The environment is considered solved when the average of these scores over 100 episodes reaches or exceeds +0.5.

## Included in this repository 

* The code used to create and train the Agent
* Tennis.ipynb - notebook containing the challenge for this project
* The trained model
* checkpoint.pt
* Files describing all the packages required to set up the environment
* The Report.md file describing the development process and the learning algorithm, along with ideas for future work
* This README.md file

## Setting up the environment

This section describes how to get the code for this project, configure the local environment, and download the Unity environment with the Agents.

### Download the Unity environment with the Agents

Download the environment from one of the links below and decompress the file into your project folder.
You need only select the environment that matches your operating system:

### Adjusting the Hyperparameters

To experiment with how the Agents learn through distinct parameters, you can tune these variables by changing their values in the singleton instance of the Config class:

* device: where your code will run: for CPU, use 'CPU'; for GPU, use 'cuda:0.'
* seed: number used to initialize the pseudorandom number generator
* target_score: how many points the agents must obtain to consider the environment solved
* target_episodes: how many episodes to consider when calculating the moving average
* max_episodes: maximum number of training episodes
* actor_layers: number and size of the actor network's layers
* critic_layers: number and size of the critic network's layers
* actor_lr: learning rate for the actor's local network
* critic_lr: learning rate for the critic's local network
* lr_sched_step: how many steps before the decaying learning rate
* lr_sched_gamma: multiplicative factor of learning rate decay
* batch_normalization: whether use batch normalization for the critic or not
* buffer_size: the size of the replay buffer
* batch_size: minibatch size for the training phase
* gamma: discount factor for expected rewards
* tau: multiplicative factor for the soft update of the target networks's weights
* noise: whether use the Ornstein-Uhlenbeck noise process or not
* noise_theta: the long-term mean of the noise process
* noise_sigma: the volatility or average magnitude
  
