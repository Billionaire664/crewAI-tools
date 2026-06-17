### CrewAI — Safe-Mode Fork (with Valta Guardrails)

> **Warning:** Standard AI agents run on an architectural "blank check." If an agent enters an infinite loop, hallucinates tool parameters, or gets stuck in a multi-agent recursive chain, it will drain your API accounts and credit cards before you notice.

This is a functional fork of the official CrewAI repository. It seamlessly injects **Valta** financial guardrails directly into the agent runtime loop.

* * *

### 🛠️ What This Fork Does

Instead of relying on LLM dashboards that track costs *after* they happen, this fork introduces an active execution firebreak:

*   **Pre-Action Interception:** Valta intercepts agent tool execution and LLM calls *before* they fire.
*   **Hard Dollar Ceilings:** Set a strict limit (e.g., $5.00). If a rogue loop tries to cross it, the runtime blocks instantly.
*   **Cross-Provider Tracking:** Tracks combined financial data across OpenAI, Anthropic, Vector DBs, and external tools simultaneously.

* * *

### ⚡ Quick Start

### 1\. Install Valta

bash

    pip install valta
    

Use code with caution.

### 2\. Configure Your Safe Agent

You do not need to rewrite your CrewAI logic. Just pass a Valta budget configuration directly into your agent:

python

    from crewai import Agent
    import valta
    
    # Initialize a strict $5.00 budget ceiling for this agent's lifetime
    safe_budget = valta.Budget(
        hard_limit_usd=5.00,
        api_key="vt_live_..."
    )
    
    research_agent = Agent(
        role='Market Researcher',
        goal='Deep dive into industry metrics',
        backstory='An autonomous research bot',
        verbose=True,
        budget=safe_budget # Valta intercepts loops right here
    )
    

Use code with caution.

* * *

### 🔒 Security & Privacy

Valta does not read your prompts or outputs. It only tracks metadata token counts, execution frequency, and financial state to enforce your programmatic budgets.

*To get your free API key and view your live agent financial dashboard, visit valta.co.*

* * *

*(Original CrewAI documentation continues below)*  
...
