# Project Report

# Objective 

This project aims to train an agent using Deep learning to navigate Unity's Banana Collector environment. In this environment, the task is to gather yellow bananas while avoiding blue ones. Employing a Deep learning algorithm, the agent successfully mastered the environment in 688 DQN and 570 episodes of Dueling DQN.

# Training Code

The notebook 'File Report.ipynb' details the implementation and training of both DQN and Dueling DQN architectures, enabling an agent to navigate through Unity's Banana Collector environment. The agent successfully solves the environment in under 1800 episodes!

# Model Weights
The model weights for successfully trained agents are stored in the **Saved Model Weights directory**. The file **model_best_DQN.pth** contains the weights for the standard DQN architecture, while **model_best_DuelingDQN.pth** houses the weights for the Dueling DQN architecture.

# Enviroment & Task

The environment is set in a square world populated with yellow and blue bananas. The agent aims to gather as many yellow bananas as possible while avoiding the blue ones. The agent can perform four actions: moving forward, backward, turning left, and turning right. The state space includes 37 dimensions that account for the agent's velocity and a ray-based perception of objects in front of the agent. Collecting a yellow banana results in a reward of +1, whereas collecting a blue banana incurs a penalty of -1. The task is structured in episodes, and to successfully solve the environment, the agent needs to achieve an average score of at least +13 across 100 consecutive episodes.

# Implementation

A deep Q-learning algorithm was employed to address the challenges posed by a particular environment. This algorithm utilizes a neural network to estimate the Q-function, which takes the state of the environment as input and outputs a Q-value for each possible action. These Q-values are then used to determine the optimal action for the agent to take. The Q-learning algorithm facilitates the learning process, which trains the neural network. However, straightforward implementations of this algorithm face two significant issues: correlated experiences and shifting targets. To overcome these issues, the algorithm incorporates two strategies: Experience Replay and Fixed Q-Targets.

### Correlated experiences 

Correlated experiences occur when the transitions experienced by an agent are related to one another, thus violating the assumption that they are independent and identically distributed. Such correlation can cause an agent to overestimate the expected rewards for certain states or actions, leading to suboptimal performance or convergence issues. To address this issue, a method known as Experience Replay is employed. This approach involves accumulating the agent's experiences in a replay buffer and then randomly sampling from this buffer to train the neural network, thereby mitigating the impact of correlated experiences.

### Correlated targets

Correlated targets occur when the target values used for policy updates are not independent, leading to a correlated learning signal. This issue can hinder or even block the convergence to the optimal policy. To address this, the Fixed Q-Targets technique is employed. This method involves utilizing two neural networks: the local and target networks. The local network selects the best action for the agent, while the target network computes the target values for the Q-Learning algorithm. The target network's weights are updated with those of the local network every four steps.

### Neural network architecture
The neural network architecture implemented in the algorithm comprises a basic, fully connected network with two hidden layers. It features an input layer with 37 neurons and an output layer with 4 neurons, with each hidden layer containing 64 neurons. The activation function applied in the hidden layers is ReLU, while the output layer utilizes the identity function for activation. The Adam optimizer, set with a learning rate 0.0005, was employed to optimize the network. PyTorch was the chosen library for crafting this neural network implementation.

### Training task

To train the agent, we implemented a loop that allowed interaction with the environment to gather and learn from experiences. A critical hyperparameter in our training process was the number of episodes. This parameter was manually adjusted to balance training duration and agent performance. In our final setup, we used 1000 episodes, though the environment was solved in just 775. Another significant hyperparameter was the number of steps per episode. This was also manually tuned to optimize training time and agent performance. A higher number of steps enables more extensive exploration of the environment but at the cost of increased training time.

Additional hyperparameters used included:

* Replay buffer size: 1000
* Batch size: 32
* Update every: 4
* Gamma: 0.99
* Tau: 1e-3
* Learning rate: 0.0005

**Plot of rewards per episode** 

![Train_DQN](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/Project_Navigation/Train_DQN.jpg)

## Future improvements

The DuelingDQNetwork class defines an enhanced architecture for Dueling Deep Q-Networks, a variant of Deep Q-Learning that more sophisticatedly approximates the state-action value function. This architecture is particularly effective in environments where the action space is large or complex.

### Architecture Overview:

* **Seed Initialization:** The network uses a fixed seed for reproducibility of results.
*  **Input and Hidden Layers:**
   * **Linear Layers:** The model begins with three fully connected linear layers (fc1, fc2, fc3) that progressively transform the input state size to more manageable 
     dimensions. Each layer increases the network's ability to capture deep features at various levels of abstraction.
   * **Batch Normalization:** Applied after each linear transformation (bn1, bn2, bn3), batch normalization helps normalize the previous layers' output, 
      improving the stability and speed of the network's training process by ensuring consistent scale across inputs.

* **Streams for Value and Advantage:**
    * **Value Stream:** Comprises a sequence of layers designed to estimate the state's overall value, independent of any specific action. This stream concludes 
      with a single output neuron, representing the value of being in a given state.
    * **Advantage Stream:** Similar in structure to the value stream, but culminates in multiple output neurons corresponding to each action's advantage—the relative importance of each action from the state.  

* **Output – Q-values:**
   * The final Q-values are computed by combining the value and the advantages. The advantage values are adjusted by subtracting their mean, ensuring that the 
      computed Q-values represent a balance between the state's estimated value and each action's relative importance.
     
### Functional Detail:

The forward method describes how a state input through the network results in action values. It applies ReLU activations post-normalization to introduce non-linearity, allowing the network to learn complex patterns. The final Q-values are derived by summing the value for the state and the centered advantages for each action, which allows the network to discriminate between available actions based on the learned policy effectively.

**Plot of rewards per episode** 

![Train_Dueling_DQN](https://github.com/1Px-Vision/Advanced-Deep-Reinforcement-Learning-Solutions/blob/main/Project_Navigation/Train_Dueling_DQN.jpg)
  

