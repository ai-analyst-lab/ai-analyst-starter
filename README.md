# AI Analyst (Starter)

Welcome to Day 1 of the AI Analyst Bootcamp. This is your starting repo -- nearly empty on purpose. You'll build your own AI analyst from the ground up.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and working
- Python 3.10+
- Git

## Setup

### 1. Clone this repo

```bash
git clone https://github.com/ai-analyst-lab/ai-analyst-starter.git
cd ai-analyst-starter
```

### 2. Install Python dependencies

```bash
pip install -r requirements.txt
```

### 3. Get the dataset

**On the course:** NovaMart is a download, not a build. The Week 0 setup checklist links the
file and walks you through putting it in `data/practice/`. Do not run the generator; it produces
a different dataset and everyone on the course needs to be working from the same one.

**Outside the course**, or if you want your own copy to experiment with:

```bash
pip install -r data-generation/requirements.txt
python data-generation/generate.py
```

That creates ~8M rows of realistic e-commerce data in `data/practice/` -- users, orders,
sessions, clickstream events, experiments, support tickets, and more. The numbers will not match
the course dataset.

### 4. Open Claude Code

```bash
claude
```

Claude reads `CLAUDE.md` and becomes your AI Product Analyst. Try asking it a question:

> "What's the conversion rate by device type?"

> "Which acquisition channel brings the most valuable users?"

> "How does NPS differ between Plus members and non-members?"

## What's Here

```
CLAUDE.md                    ← The persona (read this first)
data-generation/             ← Scripts to generate practice data
.claude/skills/              ← Starter skills (kickoff, show-off, question-framing)
.claude/agents/              ← Native Claude Code project subagents
.knowledge/                  ← Dataset schema and connection config
docs/AGENT-DESIGN.md         ← How to design, build, and evaluate an agent
themes/                      ← Marp slide themes
templates/                   ← Slide deck template
```

## What's NOT Here (Yet)

The starter includes one example agent, but no finished analytical pipeline, multi-agent workflow,
helper library, or chart system. You build and evaluate those pieces as the course progresses.

That is the point of the starter: understand how the pieces work by building them yourself.

## Key Concepts

**CLAUDE.md** -- The persona file. This is what turns Claude Code from a general assistant into a domain-specific analyst. Everything Claude knows about its role, data, and rules comes from here.

**Skills** (`/.claude/skills/`) -- Instruction files triggered by slash commands or context. Read by Claude at runtime. Think of them as "how-to guides" for specific tasks.

**Agents** (`.claude/agents/`) -- AI workers that own defined jobs. Claude Code discovers these
project subagents directly, runs them in separate contexts with their configured tools, and returns
their results to the main conversation.

The included `data-explorer` is a working example. Read
[`docs/AGENT-DESIGN.md`](docs/AGENT-DESIGN.md) before building your own. To guarantee one explicit
run, select it from the `@` typeahead or type `@agent-data-explorer`.

## Next Steps

1. Read `CLAUDE.md` to understand the persona
2. Try `/kickoff` to introduce yourself to the community
3. Ask Claude an analytical question about NovaMart
4. Study `.claude/agents/data-explorer.md`, then build your first project subagent
5. Try `/show-off` to share what you built
