# Project 3: Collaboration and Competition

## The algorithm 

To tackle this challenge, I have studied and implemented the Multi-Agent Deep Deterministic Policy Gradient (MADDPG) algorithm described in the paper [Multi-Agent Actor-Critic for Mixed Cooperative-Competitive Environments](https://arxiv.org/abs/1706.02275). The environment for this project consists of two agents that perform similar actions based on similar observations. A straightforward approach to this project would be implementing a single pair of actor-critic networks, effectively creating one "brain" controlling both agents. This would be analogous to controlling two rackets with each hand and playing against yourself. However, the true allure of this project lies in training entirely distinct agents who are aware of each other's presence and take action to collaborate and compete in pursuit of the highest possible score. Therefore, I chose the more challenging path of training separate agents, as outlined in the MADDPG paper. MADDPG does not require that all agents share identical action and observation spaces or adhere to the same policy π. This flexibility allows for training completely different agents within the same environment. Since each agent has its private pair of actor-critic networks, they can behave in distinct ways, pursuing other goals while simultaneously interacting in the same environment. Consequently, the agents can operate in either collaborative or competitive scenarios. Although this project involves two agents, the implementation is designed to be scalable to multiple agents.

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

### Actor and Critic networks

**The Actor Network** takes as input 24 variables representing the observed state-space and outputs 2 values corresponding to the predicted optimal action for that observation. This means the Actor is used to approximate the optimal policy deterministically π.

**The Critic Network**, on the other hand, receives 48 variables as input, representing the observed state-space of both agents (24 variables per agent). The output from the Critic's first hidden layer is combined with the actions predicted by any Actor Network and then fed into the Critic's second hidden layer. The final output of this network is a prediction of the target value based on the observations and the estimated optimal actions for both agents. In other words, the Critic computes the optimal action-value function Q(s, a) by utilizing all agents' observations and best-estimated actions, thereby functioning as a centralized critic.

### Target and Local Networks

Although not illustrated in the figure above, every network in this model is implemented in local and target forms. This dual-network approach was introduced in the DQN paper to minimize correlations with the target values. The roles and functions of the target and local networks are detailed in the section titled [Learning Algorithm](https://storage.googleapis.com/deepmind-media/dqn/DQNNaturePaper.pdf).

### Batch Normalization

Batch normalization was applied to the critic network immediately after the first hidden layer's output was concatenated with the action values. Given that the action values are in the range [-1, 1], applying batch normalization at this stage helps prevent these values from becoming outliers compared to others. Additionally, I conducted experiments applying batch normalization across all layers of the actor network, including the input variables.

## Training and hyperparameters
Due to the architecture developed in this project, agents can have distinct reward functions and, consequently, different goals, allowing them to act either competitively or collaboratively. In a competitive scenario, agents receive rewards based solely on their actions. In contrast, in a collaborative scenario, all agents share the same rewards based on the group's collective actions. I tested both scenarios and found that the competitive setting produced more effective agents, as they were highly motivated to perform the best possible actions to maximize their rewards. Conversely, in the collaborative setting, where rewards were aggregated across all agents to produce a single reward, the agents appeared less motivated to optimize their actions, possibly because they still received rewards based on the efforts of other agents.

The values defined for the hyperparameters:

* seed: 42
* actor_layers: [128,128]
* critic_layers: [128,128]
* actor_lr: 2e-4
* critic_lr: 2e-4
* lr scheduler decay: 0
* buffer_size: 1000000
* batch_size: 128
* gamma: 0.99
* tau: 1e-3
* noise_theta: 0.15
* noise_sigma: 0.1

## Result

### Plot of the rewards
This graph presents the rewards per episode during the training phase and the moving average. It demonstrates that the agents achieved a moving average reward of at least 0.5 points over 100 episodes.

![Result_Agent](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/p3-collab-compet/Result_Collab_C.png)

The following graphs present the rewards per episode for each agent individually during the training phase. They demonstrate that, by the end of the training, each agent successfully achieved the maximum possible reward.

![Result_Agent_0](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/p3-collab-compet/Result_Collab_C_Agent.png)


## Trained Agent

The trained agents were compared with untrained ones.

### Untrained Agents

![Untrained_Agent](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/p3-collab-compet/untrained_agents.gif)

### Trained Agents
![Trained_Agent](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/p3-collab-compet/trained_agents.gif)


## Future Work

* While significant effort has been dedicated to optimizing the hyperparameters, alternative configurations may enable the agents to solve the environment even more efficiently. Therefore, additional testing could be conducted to confirm this.

* Exploring different noise process algorithms could enhance the exploration of the environment. Moreover, introducing a scheduler to decay the noise over time gradually could be beneficial.

* Both the MADDPG paper and this project utilize uniformly sampled minibatches from the replay buffer. Although some implementations of prioritized experiences were tested, future work could delve deeper into this approach.

* Introducing negative rewards could be an effective strategy to deter agents from making random moves that deviate from their objectives.

* The objective of this project was to achieve a reward of +0.5 over 100 consecutive episodes. Further experiments could investigate whether this architecture can solve the same environment with a higher target score.

* The MADDPG paper suggests a training approach involving an ensemble of policies for each agent, which could contribute to more robust multi-agent policies. This aspect was not explored in the current project but could be a focus for future research.
