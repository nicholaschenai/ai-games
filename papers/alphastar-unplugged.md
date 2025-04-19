---
date: 2023-08-07
time: 14:57
author: 
title: "AlphaStar Unplugged: Large-Scale Offline\rReinforcement Learning"
created-date: 2024-02-24
tags: 
paper: https://arxiv.org/pdf/2308.03526.pdf
code: https://github.com/google-deepmind/alphastar
---
Note: older ver starcraft 2 unplugged [here](https://openreview.net/pdf?id=Np8Pumfoty) submitted to neurips 2021

## Description of result
### tooling contributions
- We define a dataset (a subset of Blizzard’s release), 
- tools standardizing an API for ML
	- removing the environment interactions from the training loop significantly lowers the compute demands of StarCraft II
	- Open source code. We provide a well-tuned behavior cloning agent which forms the backbone for all agents presented in this paper

meant as offline RL research suite
### evaluation protocol
#### Training setup
We fix a dataset and a set of rules for training in order to have fair comparison between methods.
- we do not allow algorithms to use data beyond the dataset described. In particular, the environment cannot be used to collect more data
- online policy evaluation is authorized, i.e. policies can be run in the environment to measure their performance. This may be useful for hyperparameter tuning.

#### Evaluation metric. 
We propose a set of metrics to measure performance of agents.
- Elo rating (Elo, 1978) 
- Robustness is computed as one minus the minimum win rate over the set of reference agents
We evaluate our agents by playing repeated games against a fixed selection of 7 opponents: 
- the very_hard built-in bot (hardest non-cheating), 
- a set of 6 reference agents presented below.
	- including behavior cloning, 
	- offline variants of actor-critic 
	- MuZero


### result 
We improve the state of the art of agents using only offline data, and we achieve 90% win rate against previously published AlphaStar behavior cloning agent.

![](assets/Pasted%20image%2020240224180526.png)
![](assets/Pasted%20image%2020240224180543.png)



---
## How it compares to previous work


---
## Main strategies used to obtain results
Generally, our best performing agents follow a two-step recipe: 
- first train a model to estimate the behavior policy and behavior value function. 
- Then, use the behavior value function to improve the policy, either while training or during inference. 
![](assets/Pasted%20image%2020240224180029.png)

MMR conditioning: 
- At training time, the MMR of the player who generated the trajectory is passed as a vector input. 

- During inference, we can control the quality of the game played by the agent by changing the MMR input. 

- In practice, we set the MMR to the highest value to ensure the agent plays its best. 

- This is similar to Return-Conditioned Behavior Cloning (Srivastava et al., 2019) with the MMR as the reward


---

## other

modalities: 
- Arguments of the previous action are embedded as well, with the exception of the world previous argument, since we found this causes too much overfitting.
- We tried using memory in the vector modality, which can be LSTM  or Transformer XL (Dai et al., 2019). 


Memory
- Most of our results do not use memory cos "Surprisingly, we found that no memory performs better than LSTM for behavior cloning, although the final values of the losses are higher." !?


On MCTS
- Our preliminary experiments on using the full MuZero Unplugged algorithm, i.e. training with MCTS targets, were not successful. 
- We found that the policy would collapse quickly to a few actions with high (over-)estimated value. 
- While MCTS at inference time improves performance, using MCTS at training time leads to a collapsed policy. 
- We believe this is likely due to MCTS policy distribution generating out of distribution action samples with over-estimated value estimates


- also see 5.12 for other RL baselines that struggle