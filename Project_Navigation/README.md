# Project: Navigation

Repository for the Udacity RL Specialization first project with Deep Q Learning

# Project overview
This project aims to train an Agent using Deep Q Networks and Dueling DQN Architectures.

# Enviroment & Task

The setting is a square arena populated with yellow and blue bananas. The goal for the agent is to gather as many yellow bananas as possible while steering clear of the blue ones. The agent can perform four actions: move forward, backward, turn left, and turn right. The agent's environment is represented by a 37-dimensional state space, which includes the agent’s velocity and a ray-based perception system that detects objects in the agent’s forward path. Collecting a yellow banana yields a reward of +1, whereas collecting a blue banana incurs a penalty of -1. The challenge is episodic, and to successfully complete it, the agent needs to achieve an average score of at least +13 across 100 consecutive episodes.

![Navegation](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/Project_Navigation/banana.gif)
