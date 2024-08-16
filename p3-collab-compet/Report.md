# Project 3: Collaboration and Competition

## The algorithm 

To tackle this challenge, I have studied and implemented the Multi-Agent Deep Deterministic Policy Gradient (MADDPG) algorithm described in the paper [Multi-Agent Actor-Critic for Mixed Cooperative-Competitive Environments](https://arxiv.org/abs/1706.02275). The environment for this project consists of two agents that perform similar actions based on similar observations. A straightforward approach to this project would be implementing a single pair of actor-critic networks, effectively creating one "brain" controlling both agents. This would be analogous to controlling two rackets with each hand and playing against yourself. However, the true allure of this project lies in training entirely distinct agents who are aware of each other's presence and take action to collaborate and compete in pursuit of the highest possible score. Therefore, I chose the more challenging path of training separate agents, as outlined in the MADDPG paper. MADDPG does not require that all agents share identical action and observation spaces or adhere to the same policy π. This flexibility allows for training completely different agents within the same environment. Since each agent has its private pair of actor-critic networks, they can behave in distinct ways, pursuing different goals while simultaneously interacting in the same environment. Consequently, the agents can operate in either collaborative or competitive scenarios. Although this project involves two agents, the implementation is designed to be scalable to multiple agents.

## Learning algorithm

### Training function

The training function interacts with the environment by first taking the observations of the state space from each agent (s1, s2). These observations are then sent to the Multi-Agent controller, which determines the appropriate action for each agent. The Multi-Agent controller splits the observations and forwards them to the respective agents. Each agent then uses its current policy π(s) → a, approximated by its actor-local network, to return the best-believed action. To promote exploration, the Ornstein-Uhlenbeck process is applied to add noise to these actions, with each agent managing its noise process. The resulting actions (a1, a2) are then returned to the training function. The training function triggers the environment using these actions. After executing the actions, the environment provides rewards (r1, r2) and the next observations (s'1, s'2). The experience tuple (et), which includes the initial observations, actions taken, rewards received, and next observations, is sent back to the Multi-Agent controller. This marks the completion of one iteration of the training loop, which begins anew with the next observations now serving as the current observations.

### The replay buffer

In the training function's final step, the Multi-Agent controller stores the received experience tuple in a unified replay buffer. 
𝐷.
Dt = {e1, ..., et}
et = ((st1, st2), (at1, at2), (rt1, rt2), (s't1, s't2))

### Networks' weights

After a sufficient number of experiences have been stored in the replay buffer, the Multi-Agent system randomly selects a minibatch of experiences to update the network weights. This update is performed individually for each agent. It’s important to note that each agent's critic network processes the states and actions of all agents in the environment. This allows the critic to learn about the policies of other agents, which is the key aspect of centralized training.

## Training and hyperparameters

## Result

### Plot of the rewards
This graph presents the rewards per episode during the training phase and the moving average. It demonstrates that the agents achieved a moving average reward of at least 0.5 points over 100 episodes.

## Trained Agent

## Future Work
