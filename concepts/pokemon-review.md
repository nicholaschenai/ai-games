A review of attempts to solve Pokemon (complete the game) with AI.
- Note: There are many variants of Pokemon games. We are referring to the type that requires exploration, catching Pokemon, battling (especially with Gym leaders to earn badges)
- Most of the works use Pokemon Red as the env


## Motivation
- Long horizon, requires complex planning
- Visual/Spatial reasoning which LLMs currently struggle at
- Requires commonsense, physical reasoning (e.g. needs to cut trees n push boulders) which LLMs struggle at times
- LLMs alone (or equipped with basic memory/tools) still fail to solve
- Easy for humans!
- RL based methods require a lot of reward crafting and resulted in reward hacking


## Strategies

| paper                                                                                                                                              | repo                                                                      | yr   | result                                               | method                                                                          |
| -------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ---- | ---------------------------------------------------- | ------------------------------------------------------------------------------- |
| [Research Notes: Running Claude 3.7, Gemini 2.5 Pro, and o3 on Pokémon Red](https://www.lesswrong.com/posts/8aPyKyRrMAQatFSnG/untitled-draft-x7cc) |                                                                           | 2025 | Identifies challenges in the 3 popular LLMs          | various exploratory analysis                                                    |
| Gemini plays pokemon                                                                                                                               |                                                                           | 2025 | Soul badge                                           | Agent. 68k actions / 500 hours. Minimap                                         |
| [claude-plays-pokemon](../papers/claude-plays-pokemon.md)                                                                                          |                                                                           | 2025 | Reached Lt Surge's badge (3rd one) after 35k actions | Claude 3.7 with writable semantic memory and summarization-based working memory |
| [PokeRL](https://drubinstein.github.io/pokerl/)                                                                                                    | [Pokemon Red Puffer](https://github.com/drubinstein/pokemonred_puffer)    | 2025 | **Beat Pokemon Red**                                 | RL with <10m param policy, reward crafting                                      |
| [Pokemon Red via RL](https://arxiv.org/pdf/2502.19920)                                                                                             | [repo](https://github.com/MarcoMeter/neroRL/tree/poke_red)                | 2025 | Reached Cerulean City (1 gym leader defeated)        | Deep RL with PPO, CNNs for perception                                           |
| [BOEY's Baselines v2](https://github.com/CJBoey/PokemonRedExperiments1/tree/master/baselines/boey_baselines2)                                      | [repo](https://github.com/CJBoey/PokemonRedExperiments1)                  | 2024 | **Beats game** in 800m steps with > 80% success rate | DeepRL                                                                          |
|                                                                                                                                                    | [PokemonRedExperiments](https://github.com/PWhiddy/PokemonRedExperiments) | 2024 | Reached Cerulean City (1 gym leader defeated)        | Deep RL                                                                         |

## Thoughts/Ideas
- Thoughts on LLM agents  [agent-pokemon-thoughts](agent-pokemon-thoughts.md)

## Challenges for AI
- [pokemon-ai-challenges](pokemon-ai-challenges.md)

## Tools

| Name                                                                                                | Desc                                                                                         |
| --------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| [Pokeagent](https://github.com/DaDevChia/Pokeagent_new)                                             | Agent environment integrated with PyBoy                                                      |
| [PyBoy](https://github.com/Baekalfen/PyBoy)                                                         | Python-based gameboy emulator with optimizations for AI use (e.g. RL, parallel environments) |
| [Pokemon Red Puffer](https://github.com/drubinstein/pokemonred_puffer)                              | Library for RL dev                                                                           |
| [Poke.AI](https://github.com/poke-AI/poke.AI)                                                       | SLAM for mapping env, on Emerald                                                             |
| [Claude plays Pokemon starter](https://github.com/davidhershey/ClaudePlaysPokemonStarter/tree/main) | starter code for claude plays pokemon                                                        |

## Other reference material
- Disassembly of Pokemon Red/Blue https://github.com/pret/pokered
- Comprehensive guide https://drubinstein.github.io/pokerl/
	- description on the game itself 
	- approach to use RL
	- various interesting engineering details