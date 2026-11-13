# Agent team for Mona's Project Pulse dashboard

Using GitHub Copilot CLI in a Codespace, I will orchestrate the work with this custom agent team from the repository's `.github/agents` folder.

- Coder — Model: GPT-5.5 (copilot). Responsible for implementing code, fixing bugs, and building the application logic for the Project Pulse dashboard within the scope assigned by the orchestrator. Definition: `.github/agents/coder.agent.md`.
- Designer — Model: Gemini 3.7 Flash (copilot). Responsible for the dashboard's UX, information hierarchy, accessibility, interaction flow, and styling so the interface feels polished and easy to use. Definition: `.github/agents/designer.agent.md`.
- Orchestrator — Model: Claude Opus 5.5 (copilot). Responsible for coordinating the work across specialists, breaking the project into phases, delegating scoped tasks, and verifying that everything fits together. Definition: `.github/agents/orchestrator.agent.md`.
- Planner — Model: Claude Opus 5.5 (copilot). Responsible for researching the repo, identifying dependencies and edge cases, and producing the implementation plan the orchestrator can turn into actionable work. Definition: `.github/agents/planner.agent.md`.

This team gives me a clear division of labor: Planner defines the strategy, Orchestrator coordinates execution, Designer shapes the user experience, and Coder builds the working dashboard implementation.
