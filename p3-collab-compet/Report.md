# Project 3: Collaboration and Competition

## The algorithm 

To tackle this challenge, I have studied and implemented the Multi-Agent Deep Deterministic Policy Gradient (MADDPG) algorithm described in the paper [Multi-Agent Actor-Critic for Mixed Cooperative-Competitive Environments](https://arxiv.org/abs/1706.02275). The environment for this project consists of two agents that perform similar actions based on similar observations. A straightforward approach to this project would be implementing a single pair of actor-critic networks, effectively creating one "brain" controlling both agents. This would be analogous to controlling two rackets with each hand and playing against yourself. However, the true allure of this project lies in training entirely distinct agents who are aware of each other's presence and take action to collaborate and compete in pursuit of the highest possible score. Therefore, I chose the more challenging path of training separate agents, as outlined in the MADDPG paper. MADDPG does not require that all agents share identical action and observation spaces or adhere to the same policy π. This flexibility allows for training completely different agents within the same environment. Since each agent has its private pair of actor-critic networks, they can behave in distinct ways, pursuing different goals while simultaneously interacting in the same environment. Consequently, the agents can operate in either collaborative or competitive scenarios. Although this project involves two agents, the implementation is designed to be scalable to multiple agents.

## Result

### Plot of the rewards
This graph presents the rewards per episode during the training phase and the moving average. It demonstrates that the agents achieved a moving average reward of at least 0.5 points over 100 episodes.

## Trained Agent

## Future Work
