---
date: 2019-10-10
time: 18:50
author: 
title: "AlphaStar: Grandmaster level in StarCraft II using multi-agent reinforcement learning"
created-date: 2024-02-23
tags:
  - AI
  - starcraft
  - RL
paper: https://www.nature.com/articles/s41586-019-1724-z.epdf?author_access_token=lZH3nqPYtWJXfDA10W0CNNRgN0jAjWel9jnR3ZoTv0PSZcPzJFGNAZhOlk4deBCKzKm70KfinloafEF1bCCXL6IIHHgKaDkaTkBcTEv7aT-wqDoG1VeO9-wO3GEoAMF9bAOt7mJ0RWQnRVMbyfgH9A%3D%3D
code: https://static-content.springer.com/esm/art%3A10.1038%2Fs41586-019-1724-z/MediaObjects/41586_2019_1724_MOESM2_ESM.zip
blog: https://deepmind.google/discover/blog/alphastar-grandmaster-level-in-starcraft-ii-using-multi-agent-reinforcement-learning/
---

## Description of result
AlphaStar was ranked above 99.8% of active players on Battle.net, and achieved a Grandmaster level for all three StarCraft II races. 
- 6,275 Match Making Rating (MMR) for Protoss, 
- 6,048 MMR for Terran and 
- 5,835 for Zerg

supervised gets it to ~3.7k, above 84% of humans

### obs
![](assets/Pasted%20image%2020240223201514.png)

### action space
![](assets/Pasted%20image%2020240223201533.png)


---
## How it compares to previous work
diff b/w jan ver [alphastar-jan](alphastar-jan.md)

- AlphaStar now has the same kind of constraints that humans play under, e.g. 
	- viewing the world through a camera 
	- stronger limits on the frequency of its actions* 
		- (in collaboration with StarCraft professional [Dario “TLO” Wünsch](https://twitter.com/LiquidTLO?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor)).
		- chart of effective APM shows similar distribution to human
- AlphaStar can now play in one-on-one matches as and against all 3 races. each race is NN
- The League training is fully automated, and starts only with agents trained by supervised learning, rather than from previously trained agents from past experiments.
- [**AlphaStar played**](https://news.blizzard.com/en-us/starcraft2/22933138/deepmind-research-on-ladder) **on the official game server,** [**Battle.net**](https://www.blizzard.com/en-gb/?ref=battle.net)**, using the same maps and conditions as human players. All game replays are available** [**here**](https://deepmind.com/research/open-source/alphastar-resources)**.**

---
## Main strategies used to obtain results

### League training
central idea extends the notion of fictitious self-play to a group of agents – the League. 
- Normally in self-play, every agent maximizes its probability of winning against its opponents; 
- In the real world, a player trying to improve may choose to do so by partnering with friends to train **particular strategies**. 
- As such, their training partners are not playing to win against every possible opponent, but are instead **exposing the flaws** of their friend, to help them become a better and more robust player. 
- key insight: playing to win is insufficient: instead, we need both 
	- main agents whose goal is to win versus everyone, 
	- exploiter agents that focus on helping the main agent grow stronger by exposing its flaws, rather than maximising their own win rate against all players 
	- this prevents the common prob of cycle chasing, i.e. in a rock-paper-scissors game it means switching strat to the direct counter instead of honing on the current strat. 
- Using this training method, the League learns all its complex StarCraft II strategy in an end-to-end, fully automated fashion.
	
![](assets/Pasted%20image%2020240223193211.png)

Figure 1 depicts some of the challenges in complex domains such as StarCraft. 
- (Top row) Players can create a variety of ‘units’ (e.g. workers, fighters, or transporters) to deploy in different strategic moves. 
	- Units have balanced strengths and weaknesses analogous to rock-paper-scissors. 
	- imitation learning enables initial agent to execute a diverse set of strategies, 
		- depicted here as a composition of units created in the game 
			- (in this example: Void rays, Stalkers and Immortals). 
	- However, because some strategies are easier to improve on, naive reinforcement learning would narrowly focus on these. 
	- Other strategies may require more learning, or have particular nuances that make them harder for the agent to perfect. 
	- This creates a vicious cycle in which some valid strategies appear less and less effective because the agent abandons them in favour of a dominant strategy. 

- (Bottom row) added agents to the League whose sole purpose is to expose weaknesses of the main agent. 
	- This means that more valid strategies will be discovered and developed, making the main agent far more robust against their opponents. 
	- employed imitation learning techniques (including distillation) to prevent AlphaStar from forgetting throughout training, and by using latent variables to represent a diverse set of opening moves.

![](assets/Pasted%20image%2020240223194341.png)

The league consists of three distinct types of agent, differing primarily in their mechanism for selecting the opponent mixture. 
- main agents: 
	- utilize a prioritized fictitious self-play (PFSP) mechanism that adapts the mixture probabilities proportionally to the win rate of each opponent against the agent; 
	- this provides our agent with more opportunities to overcome the most problematic opponents. With fixed probability, a main agent is selected as an opponent; this recovers the rapid learning of self-play 
- main exploiter agents: 
	- play only against the current iteration of main agents. 
	- Their purpose is to identify potential exploits in the main agents; 
	- the main agents are thereby encouraged to address their weaknesses. 
- league exploiter agents: 
	- use a similar PFSP mechanism to the main agents, but are not targeted by main exploiter agents. 
	- Their purpose is to find systemic weaknesses of the entire league. 
	- Both main exploiters and league exploiters are periodically reinitialized to encourage more diversity and may rapidly discover specialist strategies that are not necessarily robust against exploitation

### Policy
- NN policy, transformer over observations for self attn over all units
- To integrate spatial and non-spatial information, we introduce scatter connections. 
- To deal with partial observability, the temporal sequence of observations is processed by deep LSTM
- To manage the structured, combinatorial action space, the agent uses an auto-regressive policy and recurrent pointer network
- start w imitation learning to create an initial policy which played the game better than 84% of active players.

### overview
- RL algo based on A2C. 
- Updates were applied asynchronously on replayed experiences, off-policy learning. 
- solution is motivated by the observation that, in large action spaces, the current and previous policies are highly unlikely to match over many steps. 
- therefore use a combination of techniques that can learn effectively despite the mismatch:
	- temporal difference learning (TD(λ)), 
	- clipped importance sampling (V-trace), 
	- and a new self-imitation algorithm (UPGO) that moves the policy towards trajectories with better than average reward. 
- To reduce variance, during training only, the value function is estimated using information from both the player’s and the opponent’s perspectives.

### discovering novel strategies

- Consider a policy that has learned to build and utilize the micro-tactics of ground units. 
	- Any deviation that builds and naively uses air units will reduce performance.  
	- highly improbable that naive exploration will execute a precise sequence of instructions, over thousands of steps, that constructs air units and effectively utilizes their micro-tactics. 

- To address this issue, and to encourage robust behaviour against likely human play, we utilize human data. 
	- Each agent is initialized to the parameters of the supervised learning agent. 
	- Subsequently, during reinforcement learning, we either condition the agent on 
		- a statistic z, in which case agents receive a reward for following the strategy corresponding to z, 
		- or train the agent unconditionally, in which case the agent is free to choose its own strategy. 
	- Agents also receive a penalty whenever their action probabilities differ from the supervised policy. 
	- This human exploration ensures that a wide variety of relevant modes of play continue to be explored throughout training.

### contd
- used a latent variable which conditions the policy and encodes the distribution of opening moves from human games, which helped to preserve high-level strategies. 
- AlphaStar then used a form of distillation throughout self-play to bias exploration towards human strategies. 
- This approach enabled AlphaStar to represent many strategies within a single neural network (one for each race). 
- During evaluation, the neural network was not conditioned on any specific opening moves.

![](assets/Pasted%20image%2020240223201337.png)

---

## Other

### compute
We trained the league using 
- three main agents (one for each StarCraft race), 
- three main exploiter agents (one for each race), 
- and six league exploiter agents (two for each race). 

Each agent was trained using 32 third-generation TPUs over 44 days. During league training almost 900 distinct players were created.

![](assets/Pasted%20image%2020240223200301.png)

"AlphaStar has 139 million weights, but only 55 million weights are required during inference."

- For every training agent in the league, 
	- we run 16,000 concurrent StarCraft II matches and 16 actor tasks 
	- (each using a TPU v3 device with eight TPU cores) to perform inference. 
- The game instances progress asynchronously on preemptible CPUs (roughly equivalent to 150 processors with 28 physical cores each), but requests for agent steps are batched together dynamically to make efficient use of the TPU. 
- Using TPUs for batched inference provides large efficiency gains over previous work 
- Actors send sequences of observations, actions, and rewards over the network to a central 128-core TPU learner worker, which updates the parameters of the training agent. 
- The received data are buffered in memory and replayed twice. 
- The learner worker performs large-batch synchronous updates. Each TPU core processes a mini-batch of four sequences, for a total batch size of 512. 
- The learner processes about 50,000 agent steps persecond. 
- The actors update their copy of the parameters from the learner every 10s. 
- instantiate 12 separate copies of this actor–learner setup: one main agent, one main exploiter and two league exploiter agents for each StarCraft race. 
- One central coordinator maintains an estimate of the payoff matrix, samples new matches on request, and resets main and league exploiters. 
- Additional evaluator workers (running on the CPU) are used to supplement the payoff estimates.

### ablations

![](assets/Pasted%20image%2020240223201400.png)
![](assets/Pasted%20image%2020240223195729.png)
### supplementary 
contains the pseudocode, StarCraft II replay files, detailed neural network architecture and raw data from the Battle.net experiment.