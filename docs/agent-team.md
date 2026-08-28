# Agent team

I will use GitHub Copilot CLI in a Codespace to orchestrate a four-agent team for
building Mona's Project Pulse dashboard:

| Agent | Target model | Responsibility | Definition |
| --- | --- | --- | --- |
| **Orchestrator** | Claude Opus 4.7 (copilot) | Breaks the work into phases, delegates tasks to the specialists, manages file ownership and dependencies, and verifies the integrated result. | `.github/agents/orchestrator.agent.md` |
| **Planner** | Claude Opus 4.7 (copilot) | Researches the repository and requirements, then creates the ordered implementation plan, file assignments, risks, edge cases, and validation expectations. | `.github/agents/planner.agent.md` |
| **Coder** | GPT-5.5 (copilot) | Implements the dashboard logic and assigned application or support files with clear, testable behavior, explicit errors, and runnable Project Pulse launch configuration when requested. | `.github/agents/coder.agent.md` |
| **Designer** | Gemini 3.1 Pro (copilot) | Defines and implements the dashboard's UI/UX, accessibility, information hierarchy, responsive behavior, styling, project cards, status badges, and priority treatment. | `.github/agents/designer.agent.md` |

The Orchestrator first obtains the Planner's strategy, then coordinates the
Coder and Designer in dependency-aware phases so independent work can run in
parallel without overlapping file scopes.
