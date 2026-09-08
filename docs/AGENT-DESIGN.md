# Build an agent in the starter

An agent is an AI worker that owns a defined job inside a larger system. This starter implements
agents as Claude Code project subagents in `.claude/agents/`.

Claude Code discovers these files directly. Each subagent runs with its own instructions, context,
and tool access, then returns a result to the main conversation.

## Agent file structure

A native project-subagent file has two parts:

1. A YAML configuration at the top with the agent's name, description, tools, and model.
2. Markdown instructions that define the job and how it should be completed.

Use `.claude/agents/data-explorer.md` as the working example.

## Eight questions to answer

Before building, decide:

1. **Job:** What result does the agent own?
2. **Trigger:** When should the main conversation delegate to it?
3. **Inputs:** What must arrive with the assignment?
4. **Outputs:** Which saved files and returned result does it own?
5. **Context:** What definitions and repository facts must it read?
6. **Tools:** Which actions does the job require?
7. **Boundaries:** What must it not assume, change, or claim?
8. **Completion:** What must be true before it reports success?

This course often records those decisions as a contract inside the Markdown instructions. A
contract is a design practice, not a special Claude Code file format.

## Build and evaluate

1. Write the eight decisions before creating the file.
2. Build the smallest useful agent in `.claude/agents/`.
3. Review the saved file against the design.
4. If `.claude/agents/` was created after Claude Code started, restart the session once.
5. Invoke the agent explicitly from the `@` typeahead or with `@agent-<name>` so you can see which
   worker ran.
6. Open the saved output and evaluate the job, not only the terminal summary.
7. Change the part responsible for the first material gap and run it again.

Do not create a second agent merely to hide a failure in the first one.
