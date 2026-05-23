# Agents

## What Are Agents?

Agents are reusable analytical workflows defined as markdown templates. Each agent is a `.md` file that describes a specific task -- how to frame a question, explore data, build a chart, write a narrative, etc.

## How They Work

1. **Read** -- Claude reads the agent's markdown file
2. **Substitute** -- `{{VARIABLE}}` placeholders get replaced with actual values (business context, dataset name, dates, etc.)
3. **Execute** -- Claude follows the instructions in the template

Example: an agent template might say "Query `{{DATASET}}` for `{{METRIC}}` broken down by `{{DIMENSION}}`" -- at runtime, those placeholders become real values.

## Skills vs. Agents

| | Skills | Agents |
|---|---|---|
| **Location** | `.claude/skills/` | `agents/` |
| **Triggered by** | Slash commands or context | Referenced by other agents or CLAUDE.md |
| **Purpose** | Interactive tasks (kickoff, show-off) or patterns (question framing) | Reusable analytical workflows |
| **Variables** | Usually none | `{{VARIABLE}}` placeholders |
| **Chaining** | Standalone | Can depend on other agents' outputs |

## The Contract Format

Every agent starts with a CONTRACT block -- a YAML declaration that describes its inputs, outputs, and dependencies. This makes agents composable: one agent's output can feed into another agent's input.

See `CONTRACT_TEMPLATE.md` for the full format and examples.

## Build Your First Agent

There's one example agent here to study: **`data-explorer.md`** -- it profiles a
dataset, checks data quality, and summarizes what's there. Read it alongside
`CONTRACT_TEMPLATE.md` to see the shape of a real agent.

Then build your own:

1. Pick a task you do repeatedly (e.g., "compare two segments", "find anomalies")
2. Model it on `data-explorer.md`, or copy `CONTRACT_TEMPLATE.md` as a starting point
3. Write the instructions as if explaining to a junior analyst
4. Use `{{VARIABLES}}` for anything that changes between runs
5. Test it by asking Claude to use it
