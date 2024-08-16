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

### Mean Squared Error Loss
For each experience in the minibatch, the Multi-Agent system computes the error between the target and predicted rewards. The target reward  𝑦 for a single experience is determined using the critic-target 𝑄𝜇′ and actor-target 𝜇′ networks. The next observations  (𝑠1′,𝑠2′) from the experience tuple are fed into the respective agent’s actor-target network 𝜇′ to predict the appropriate actions (𝑎1′,𝑎2′). These predicted next actions are then passed into the critic-target network 𝑄𝜇′ along with the next states, resulting in 𝑄𝜇′𝑖(𝑠1′,𝑠2′,𝑎1′,𝑎2′). The output of this network is the predicted expected reward for the next observations, which is then multiplied by a discount factor 𝛾. The final result is the discounted expected reward, which is added to the immediate reward 𝑟𝑖 of the agent for that experience, yielding the target reward 𝑦. The predicted reward for that experience is obtained using the critic-local network 𝑄𝜇. The current observation 𝑠𝑖 from the experience tuple, corresponding to the agent being trained in this iteration, is passed into the critic-local network along with the actions  (𝑎1,𝑎2) from the experience tuple. The error for this experience is calculated as the difference between the target and predicted rewards. These errors are squared, and the loss 𝐿 is computed as the mean of these squared errors, known as the Mean Squared Error Loss (MSE Loss), across all experience tuples in the minibatch. This loss 𝐿 is then used to update the weights of the critic-local network.

L = Es,a,r,s'[(Qμi(s1,s2,a1,a2) - y)2]

y = ri + γQμ'i(s'1,s'2,a'1,a'2) | a'j=μ'j(s'j)

### Actor-local update: policy gradient
The actor-local network μ is updated by applying the chain rule to the expected return, calculated using the recently updated critic-local network Qμ. To determine the expected return, each experience in the minibatch is processed by passing each observation (s1, s2) in the experience tuple through the respective agent's actor-local network μ to predict the corresponding action. This set of actions (a1, a2) is then input into the critic-local network Qμ along with the states: Qμi(s1, s2, a1, a2), the network's output represents the expected return and the loss used to update the weights of the actor-local network is the average of all expected returns Qμi(s1,s2,a1,a2) | aj=μj(sj).

## Network architecture

### Network input
The observation space comprises 8 variables, which correspond to the position and velocity of both the ball and racket. Each agent receives a localized observation. To account for multiple past observations, this environment stacks 3 vector observations, enabling temporal comparison. As a result, each agent is provided with an observation vector containing 24 variables, representing the current observation along with the previous two. The networks in this project process these 24 variables as input, as I chose to utilize all three vector observations rather than just the current one.

## Training and hyperparameters

## Result

### Plot of the rewards
This graph presents the rewards per episode during the training phase and the moving average. It demonstrates that the agents achieved a moving average reward of at least 0.5 points over 100 episodes.

## Trained Agent

## Future Work
