# My thoughts on current agent approaches in completing Pokemon
- See the various approaches in [pokemon-review](pokemon-review.md) (currently only 2, [claude-plays-pokemon](claude-plays-pokemon.md) and gemini plays pokemon). 
- My views tend to be biased towards framing problems via the lens of [cognitive-architectures](../../agi-potential-notes/concepts/cognitive-architectures.md)
## On memory maintenance
- Mostly summarization based, I think its a mistake.
	- Still need to retain all mem as we dont know when it will be useful
	- summarization assumes we know what will be important in the future. 
	- supported by papers like [enhancing-agents-episodic](../../agi-potential-notes/papers/enhancing-agents-episodic.md)
- When it does something and gets stuck, e.g. when it confuses an object for something else, need a way to update its past beliefs e.g. "I thought this object does X, but from the interaction outcome, it seems to do Y" or a "scientist mode" to test out hypothesis about something it is unsure of

## On memory retrieval
Episodic memory is known to retrieve via partial-state matching. 
- This has been known for a long time especially in the era of cognitive architectures, but largely forgotten in the current cycle and missing from current agents
- One term of retrieval strength is state similarity
- Thats why we get "this looks familiar" feelings, which can help LLM avoid repeating things it has done before. 
- Example of modern agents that implement this: [arigraph](../../agi-potential-notes/papers/arigraph.md)
- For visual memory, can store features of CNN, (works just like text embeddings) for similar experience matching

## On visual memory
- Needs to do standard saliency / object detection in computer vision, its a form of 'attention' that will prevent a large spam of text representation in the LLM context


## On hierarchical decomposition
- Current approaches (especially since models are tuned towards text representations) reason about with coordinates. Humans dont really think this way, more like 'go straight until LANDMARK then turn left', and even distances are estimates.
- 2  separate processes: Higher level task, then micro level controller. Example: [learning-grounded-action](../../agi-potential-notes/papers/learning-grounded-action.md)

## On Modalities
- Missing the audio modality? Could provide vital information! e.g. the 'warning' like sounds when HP is low, object interactions

## On relying on LLMs for reasoning
- A ton of LLM calls even for minute things like navigating thru dialogue -- action outputs due to reasoning not efficiently stored and used (procedural memory)!
- Can use operators from [soar](../../agi-potential-notes/papers/soar.md) instead which can act automatically given condition matching. These operators can be things like actions, or simplifications/elaborations of the problem.
	- So the agent can use 'macros' like the skills library in [voyager](../../agi-potential-notes/papers/voyager.md)
	- But unlike Voyager, the trigger of the action doesnt need to be LLM based, can be rule based (but LLM figures out the rule)