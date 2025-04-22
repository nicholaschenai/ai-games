Inspired by [Building Machines That Learn and Think Like People](../../agi-potential-notes/papers/building-machines.md) (Lake _et. al._ 2017). Building human-like intelligence requires various components that are still missing in existing AI systems. Here are a list of challenges framed in the Pokemon game, which can test the AI's abilities in these components.

---
## Developmental "start-up software"
### Intuitive Physics
- Solve the boulder pushing puzzles, cutting of tree blocking the route via teaching Pokemon the 'cut' move
### Intuitive Psychology
- Recognizing NPCs, their role in the game and whether they are friend or foe

## Rapid model building
### Compositionality
Building a strong team requires strong compositional reasoning: e.g. the team will consist of 6 Pokemon, each of them has up to 4 moves (which can be taught via HMs), and items (e.g. potions) can be used in battles. Each of these elements have various choices and the AI must reason about them in such a compositional manner to be efficient

### Causality
Can we build AI which innately figures out how the battle or catching system works (e.g. status conditions, item effects) by understanding causal models (e.g. this move caused the opponents DEF to fall, therefore it will receive more damage)?

### Learning to learn
- Can we have a system that, after playing through one variant (e.g. Kanto) of the game, be reasonably proficient in another variant (e.g. Johto)?
- Generalizing from previous games -- Can we build AI which plays other similar games, and transfer this knowledge to complete Pokemon faster / avoid common mistakes?
- Generalizing from world knowledge: Can we build AI which intuitively identifies game characteristics like type effectiveness (e.g. fire is weak to water, grass is weak to fire, water is weak to lightning) without having to trial and error?

---

## Challenge: Can AI adapt to new tasks (e.g. sidequests) which humans can do so easily? 
Deep RL models currently are tailored to the main quest and can't take instructions for sidequests! These challenges require AI to learn models that enable them to adapt to these new tasks:

 - [beating trainers with level 1 Pokemon](https://www.youtube.com/watch?v=QI_TF9ac9Ak)
 - Catching all the Pokemon (which includes figuring out that different versions have different Pokemon and trading is required)
 - Discovering the mini games and easter eggs e.g. surfing Pikachu
 - Explain and guide a beginner human to complete the game
 - What if game parameters are changed? e.g. catch rates, spawn locations

---
## Overall efficiency
Current SOTA is inefficient (deep RL systems beat the game with 800m steps, agent systems spend at least 500h and still cannot complete the game), whereas humans complete the game in ~25 hours. Is it possible to build an AI system that can complete the game in a more human-like time limit, in both training and inference?


Humans can grasp the game quickly, especially when they watch others play e.g. infer goals of the game, objects and interactions, even learn from mistakes of others. Can we build a system that rapidly learns by watching others? E.g. Given an AI system and feeding in videos of gamers playing through the game, the AI should be able to complete the game quickly.
