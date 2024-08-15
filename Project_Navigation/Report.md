# Project Report

# Objective 

This project aims to train an agent using Deep learning to navigate Unity's Banana Collector environment. In this environment, the task is to gather yellow bananas while avoiding blue ones. Employing a Deep learning algorithm, the agent successfully mastered the environment in 786 DQN and 570 episodes of Dueling DQN.

# Enviroment & Task

The environment is set in a square world populated with yellow and blue bananas. The agent aims to gather as many yellow bananas as possible while avoiding the blue ones. The agent can perform four actions: moving forward, backward, turning left, and turning right. The state space includes 37 dimensions that account for the agent's velocity and a ray-based perception of objects in front of the agent. Collecting a yellow banana results in a reward of +1, whereas collecting a blue banana incurs a penalty of -1. The task is structured in episodes, and to successfully solve the environment, the agent needs to achieve an average score of at least +13 across 100 consecutive episodes.

# Implementation

A deep Q-learning algorithm was employed to address the challenges posed by a particular environment. This algorithm utilizes a neural network to estimate the Q-function, which takes the state of the environment as input and outputs a Q-value for each possible action. These Q-values are then used to determine the optimal action for the agent to take. The Q-Learning algorithm facilitates the learning process, which trains the neural network. However, straightforward implementations of this algorithm face two significant issues: correlated experiences and shifting targets. To overcome these issues, the algorithm incorporates two strategies: Experience Replay and Fixed Q-Targets.

## Correlated experiences 

## Correlated targets

## Neural network architecture
