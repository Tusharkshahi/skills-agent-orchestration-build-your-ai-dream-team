# Agent team

Building Mona's Project Pulse dashboard uses a coordinated team of four specialized AI agents, orchestrated through the **GitHub Copilot CLI in a Codespace**.

## Agent roles and models

| Agent | Model | Responsibility | Definition |
|-------|-------|-----------------|------------|
| **Orchestrator** | Claude Opus 4.7 | Coordinates Planner, Coder, and Designer agents; breaks down requests into phases; manages parallel and sequential task execution | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 | Creates implementation strategies; researches codebase and dependencies; identifies edge cases and risks; produces actionable plans | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 | Implements code-oriented tasks; writes logic, fixes bugs, creates support configuration; ensures clear, testable, deterministic code | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro | Handles UI/UX design; ensures accessibility, usability, and visual clarity; creates responsive layouts and design consistency | `.github/agents/designer.agent.md` |

## Workflow

1. **Orchestrator** receives the build request
2. **Planner** researches and creates a technical implementation plan
3. **Orchestrator** phases the plan and delegates to **Coder** and **Designer** in parallel or sequence based on dependencies
4. **Coder** implements logic and application code within assigned file scopes
5. **Designer** creates UI/UX styling and visual polish within assigned file scopes
6. **Orchestrator** verifies the integrated result and reports completion

All agents operate under the principle that the learner controls git operations through Copilot CLI prompts — agents do not stage, commit, or push changes.
