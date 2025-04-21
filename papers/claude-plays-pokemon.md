---
date: 2025-04-20
time: 19:53
author: 
title: 
created-date: 2025-04-20
tags: 
paper: https://www.anthropic.com/research/visible-extended-thinking
code: 
zks-type: lit
---
- [stream](https://www.twitch.tv/claudeplayspokemon/videos)
- [architecture](https://excalidraw.com/#json=WrM9ViixPu2je5cVJZGCe,no_UoONhF6UxyMpTqltYkg)
## Description of result
Reached Lt Surge's badge (3rd one) after 35k actions

---
## How it compares to previous work
- Other deep RL-based works can complete the game with many steps
- This appears to be the first work for CA-based attempts

---
## Main strategies used to obtain results
- Base model: Claude 3.7 Sonnet thinking
### Action space (via tools)
- execute a series of button presses `use_emulator`
- goto coordinate `navigator`
- memory CRUD (see below) `update_knowledge_base`

### Observation space
- Screenshot of gameboy + overlay to show coordinates and if areas are traversable or not
- Gameboy RAM State, e.g.
	- rival
	- money
	- location / coords
	- badges
	- inventory
	- party etc

### Perception
Via LLM
### Reasoning
- LLM based
- during summarization event, reflection/critique over existing knowledge base
- prompt requires tips + tricks about tools, and short reminders of things Claude is bad at 
	- e.g. dont trust your vision
	- use KB more often than you think

### Memory
#### Working
- time series of Tools use and result
- when convo history reaches a thr, triggers summarization and that summary is used as the start of a fresh convo thread
	- causes lots of probs as impt info missing n it repeats prev explored areas
#### Semantic
Can CRUD to semantic mem in text form via a tool call
- during second run, more refined organization in terms of having 'memory files' where the agent can edit the file that is relevant to the knowledge it wants to update

---

## Limitations
- Struggles with Complex navigation: 
	- mega stuck at mount moon (~3 days) due to spatial puzzles (LLMs tend to be very bad at it) n summarization causes it to forget that it has already visited some areas so it revisits them
	- Stuck in Cerulean, exits, then returns back
- Misinterpret what it sees and gets confused by it


---

Also see my thoughts [agent-pokemon-thoughts](../concepts/agent-pokemon-thoughts.md)


