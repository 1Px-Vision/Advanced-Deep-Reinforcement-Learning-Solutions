# Project 2: Continuous Control

## Description of the implementation

I have explored and implemented the Deep Deterministic Policy Gradient (DDPG) Algorithm to address this challenge.

## Development

I started this project by implementing the DDPG Algorithm, which combines the actor-critic approach with insights from the recent success of Deep Q-Network (DQN). After thoroughly reviewing the original DDPG paper, I was confident that this method could yield promising results. Thus, I accepted the challenge of solving this environment using only the principles outlined in this model. First, I designed the Actor and Critic networks to be flexible, allowing for customization of the number of hidden layers and nodes in each layer. This approach enabled easy tuning of their depth and size. Next, I developed the Agent with the initial objective of simply instantiating the networks and taking actions, which allowed me to test whether the Actor was functioning correctly. I then implemented the replay buffer to store experiences and facilitate the training phase. Subsequently, I wrote the learning function along with the soft-update method. With this step completed, I was able to verify the correctness of the entire training process. Before diving into the final phase, I incorporated an exploration noise process—the Ornstein-Uhlenbeck process—along with other minor adjustments, such as initializing the networks' weights. Finally, it was time to fine-tune the hyperparameters to solve the environment in as few episodes as possible. This phase was conducted using version 2, which operates with 20 agents simultaneously. I provide a more detailed account of this phase in the section 'Fine-tuning the Hyperparameters.


