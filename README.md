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

### 3. Generate the dataset

```bash
pip install -r data-generation/requirements.txt
python data-generation/generate.py
```

This creates ~8M rows of realistic e-commerce data in `data/practice/` -- users, orders, sessions, clickstream events, experiments, support tickets, and more.

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
.claude/skills/              ← 3 starter skills (kickoff, show-off, question-framing)
agents/                      ← Empty -- you'll build your first agent here
.knowledge/                  ← Dataset schema and connection config
themes/                      ← Marp slide themes
templates/                   ← Slide deck template
```

## What's NOT Here (Yet)

No pre-built agents. No pipeline. No helpers. No chart library. You build what you need.

That's the whole point of Day 1: understand how the pieces work by building them yourself.

## Key Concepts

**CLAUDE.md** -- The persona file. This is what turns Claude Code from a general assistant into a domain-specific analyst. Everything Claude knows about its role, data, and rules comes from here.

**Skills** (`/.claude/skills/`) -- Instruction files triggered by slash commands or context. Read by Claude at runtime. Think of them as "how-to guides" for specific tasks.

**Agents** (`/agents/`) -- Markdown templates with `{{VARIABLE}}` placeholders. Define reusable analytical workflows. Read the template → substitute variables → execute.

## Next Steps

1. Read `CLAUDE.md` to understand the persona
2. Try `/kickoff` to introduce yourself to the community
3. Ask Claude an analytical question about NovaMart
4. Build your first agent (check `agents/CONTRACT_TEMPLATE.md` for the format)
5. Try `/show-off` to share what you built
