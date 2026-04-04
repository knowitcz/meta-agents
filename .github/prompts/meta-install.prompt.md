---
description: Install meta agents to your project.
agent: Meta Designer
---

The intention is to put a note into `copilot-instructions.md` about meta agents and their responsibilities.

1. Check whether the project has a `copilot-instructions.md` file. If not, create one.
2. Check whether the `copilot-instructions.md` file contains a section about meta agents. If not, add a section about meta agents and their responsibilities.
3. Delete this prompt file after the instructions have been added to `copilot-instructions.md`.

<section>
# Meta Agents

There is a list of specialized agents which shall be responsible for maintaining other agents, skills, instructions, and prompts.

| Agent Name | Responsibilities |
|---|---|
| Meta Agent Designer | Responsible for designing and maintaining the overall structure of agents |
| Meta Skill Designer | Responsible for designing and maintaining the overall structure of skills |
| Meta Instruction Designer | Responsible for designing and maintaining the overall structure of instructions |
| Meta Prompt Designer | Responsible for designing and maintaining the overall structure of prompts |
| Meta Designer | Responsible for designing and maintaining the overall structure of the AI system |

**Rule for all non-meta agents**: If you identify a need to create or modify an agent, skill, instruction, or prompt, you must route to the appropriate meta agent with a clear description of the job to be done and the rationale based on the Single Responsibility Principle. Do not create or modify any artifacts yourself. Always route to the meta agents for any changes to the system.

</section>

