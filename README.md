# CrewAI — Safe-Mode Fork (with Valta Guardrails)

> **Warning:** Standard AI agents run on an architectural "blank check."
> If an agent enters an infinite loop, hallucinates tool parameters, or
> gets stuck in a multi-agent recursive chain, it will drain your API
> accounts before you notice.

This is a functional fork of the official CrewAI tools repository.
It documents how to inject **Valta** financial guardrails directly into
your CrewAI agents — no rewriting your existing logic required.

---

## 🛠️ What This Adds

Instead of relying on LLM dashboards that show you the damage *after*
it happens, Valta adds an active spending gate:

- **Pre-Action Interception:** The agent checks its budget *before*
  any paid tool call fires — not after.
- **Hard Dollar Ceilings:** Set a strict limit per agent (e.g. $5.00)
  in your Valta dashboard. If a rogue loop tries to cross it, the call
  is blocked instantly.
- **Per-Agent Isolation:** One agent hitting its limit does not affect
  your other agents. Each has its own identity and budget.

---

## ⚡ Quick Start

### 1. Install

```bash
pip install crewai-valta
```

### 2. Add the spend guard to your agent

You do not need to rewrite your CrewAI logic. Just add `ValtaSpendTool`
to your agent's tools list:

```python
from crewai import Agent
from crewai_valta import ValtaSpendTool

guard = ValtaSpendTool(
    api_key="vk_live_...",
    agent_id="research-agent",
)

researcher = Agent(
    role="Market Researcher",
    goal="Deep dive into industry metrics",
    backstory="An autonomous research bot",
    verbose=True,
    tools=[guard],
)
```

### 3. Set your hard limit

Go to **valta.co/dashboard** → set a dollar ceiling for `research-agent`.
That limit is enforced server-side — the model cannot override it.
