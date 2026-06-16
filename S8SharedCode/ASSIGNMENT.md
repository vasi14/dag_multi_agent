Assignment
Build a DAG-based agent for a problem of your choice and prove the architecture is intact.

Pass the five base queries (hello, A, I, J, K) from this session. Verbatim, within the iteration and wall-clock bounds named alongside each.

- Query hello:Say hello.
- Query A: Fetch https://en.wikipedia.org/wiki/Claude_Shannon and tell me his birth
date, death date, and three key contributions to information theory.
- Query I: find the populations of London, Paris, and Berlin and tell me which two are closest in size
- Query J: Read /nonexistent/path.txt and tell me what's in it.
- Query K: For Lagos, Cairo, and Kinshasa, find current populations and growth rates
and tell me which is growing fastest. flow.py --resume s8_K_resumed_v2



Design one query that requires parallel fan-out. The query must have at least three independent sub-tasks that the Planner correctly emits as concurrent nodes. Verify that the parallel layer's wall-clock is the maximum of the branches, not the sum.

Design one query that requires a Critic verdict. Choose a property the Critic can actually verify with the tools available to it. The Critic must produce both a pass and a fail across two runs of the query, and the fail must successfully splice in a Planner recovery that produces a corrected answer.

Fill in the Coder skill. The current prompts/coder.md is a stub; replace it with a prompt that emits Python suitable for the SandboxExecutor. Demonstrate the Coder on one query where the answer requires computation the Formatter cannot reliably produce from text alone.

Add one new skill to agent_config.yaml. Choose a skill that the existing catalogue does not cover. Write its prompt file. Write one query that exercises it. The orchestrator should not need modification; if it does, the modification is reportable.

Submit README.md link clearly showing results for 1, 2, 3, 4 and 5 parts of the assignment via logs.

Architectural rules carry over. Skills are yaml entries plus prompts. The Planner emits the graph; the Executor runs it; the Critic sits between a flagged producer and its successor. The recovery classifier must continue to pass its unit tests after any change. Adding a new skill is a yaml edit and a prompt file; touching the Executor for anything but a new generic mechanism is a bug.


```

## Assignment Submissions (Steps 4 & 5)

### Part 4: Coder Skill
The Coder skill is fully implemented in [prompts/coder.md](code/prompts/coder.md) and registered in [agent_config.yaml](code/agent_config.yaml). The orchestrator routes `coder` to `sandbox_executor` automatically as a static internal successor.
- **Verification Query**: `"Calculate the 100th Fibonacci number."`
- **Result Log**: [query_coder.md](code/result_logs/query_coder.md) (Session `s8-c44d3581`)
- **Output**: `354,224,848,179,261,915,075` (an exact calculation proving successful sandbox Python code execution).

### Part 5: New Skill (Translator)
The new `translator` skill is registered in [agent_config.yaml](code/agent_config.yaml) and the prompt defined in [prompts/translator.md](code/prompts/translator.md). The Planner and Formatter prompts were also updated to support translation tasks.
- **Verification Query**: `"Translate the paragraph 'Artificial intelligence is changing the way we develop software. Multi-agent systems can coordinate to solve complex tasks.' into French."`
- **Result Log**: [query_translator.md](code/result_logs/query_translator.md) (Session `s8-9f6e1bd6`)
- **Output**: `"L'intelligence artificielle change la façon dont nous développons des logiciels. Les systèmes multi-agents peuvent se coordonner pour résoudre des tâches complexes."`
