
# AI strategies for SC2
Categories based on benchmarks/metrics used

## Ladder
open source alphastar recreation attempts
- see the minified (unofficial) implementation [An Introduction of mini-AlphaStar](https://arxiv.org/pdf/2104.06890.pdf) and [code](https://github.com/liuruoze/mini-AlphaStar)
-  DI-Star (Opendilab, 2021) 
- SC2IL (Metataro, 2021).

| paper                                           | yr   | benchmark | result                                                 | method                                                                     |
| ----------------------------------------------- | ---- | --------- | ------------------------------------------------------ | -------------------------------------------------------------------------- |
| [alphastar-final](../papers/alphastar-final.md) | 2019 | ladder    | beats 99.8% of active players on Battle.net, ~6.5k MMR | league training, supervised learning from replays, off policy learning A3C |


## com AI
Note: com AI all up to lvl 7 are non cheating. above that (8-10) cheat

| paper                                                                                             | yr   | benchmark | result                                                                                          | method                                                                                                   |
| ------------------------------------------------------------------------------------------------- | ---- | --------- | ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| [swarmbrain](../../sc2-papers/swarmbrain.md)                                                      | 2024 | com AI    | 100% success on all modes except hard where success is 76%, 30 matches each                     | local net for micro and LLM powered (slower) macro                                                       |
| "Efficient Reinforcement Learning for StarCraft by Abstract Forward Models and Transfer Learning" | 2021 | com AI    | can 100% beat all AI levels up to 7), and vs cheating ones can get to 90% winrate (lvl10 AI)    | train on reduced game (relaxing problem) then finetune on real game                                      |
| [efficient-rl-sc2](../../sc2-papers/efficient-rl-sc2.md)                                          | 2022 | com AI    | 99% against  level-1.<br>93% L-7. + tricks, level-8, level-9, and level-10 to 96%, 97%, and 94% | hierarchical RL: hier 1 extracted macros from experts. hier 2: hier of modular NNs. curriculum transfer. |
| [chain-of-summarization](../../sc2-papers/chain-of-summarization.md)                              | 2023 | com AI    | beat lvl 5                                                                                      | prompt based                                                                                             |


## ELO
| paper                                                   | yr   | benchmark                           | result                                                        | method                                                               |
| ------------------------------------------------------- | ---- | ----------------------------------- | ------------------------------------------------------------- | -------------------------------------------------------------------- |
| [alphastar-unplugged](../papers/alphastar-unplugged.md) | 2023 | com AI + other agents               | surpassed behavior clone AI trained in the original AlphaStar | offline RL and behavior cloning                                      |
| [roa-star](../../sc2-papers/roa-star.md)                | 2023 | other agents, alphastar replication | higher ELO with less compute. diverse strats                  | opponent modeling, scouting (exploration) bonus, new league training |

## SMAC
| paper | yr | benchmark | result | method |
| ---- | ---- | ---- | ---- | ---- |
| "The Surprising Effectiveness of PPO in Cooperative Multi-Agent Games" | 2021 | SMAC | simple PPO baseline on SMAC | PPO |
