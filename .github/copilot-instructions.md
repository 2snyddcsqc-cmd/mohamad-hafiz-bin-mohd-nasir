---
name: workspace-agent-instructions
description: "Workspace-wide instructions: require confirmation before running tools or destructive commands; avoid taking irreversible actions without explicit user approval."
---

Use these instructions to guide automated agents and chat assistants when interacting with this repository.

Rules:

1. Confirmation before tool use: For any request that would run tools, modify files, execute builds, or change repository state (for example: `run`, `build`, `install`, `git`, `rm`, `docker`, `sudo`), prompt the user with a concise summary of the exact commands and wait for explicit approval before executing.

2. Clarify ambiguous requests: If the user request is vague (single-word commands like `build`, `run all tools`, or `/nur`), ask at most two targeted clarifying questions to determine intent and scope before taking action.

3. Block destructive defaults: Do not run commands that are destructive by default (`rm -rf`, `git push` to remote branches, system package managers with elevated privileges) unless the user explicitly confirms and understands the consequences.

4. Network & secrets: Do not use or request secrets, tokens, or private keys. If an action requires credentials or network access that the agent cannot securely handle, ask the user to run the command locally and provide verification of results.

5. Build/run safety: Before running any build or test command, list the detected build files (e.g., `package.json`, `pyproject.toml`, `Cargo.toml`, `Makefile`) and propose the specific build commands. If no build configuration is found, recommend steps to add one instead of guessing commands.

6. Suggest non-destructive alternatives: When asked to perform potentially risky actions, propose a safe dry-run, a simulated output, or a checklist of steps the user can approve.

Examples (agent behavior):

- User: "Run all tools"
  - Agent: "I detected no explicit tool list — do you mean `npm run build`, `cargo build`, or something else? Please specify which commands to run."

- User: "Build"
  - Agent: "I found no `package.json`, `Makefile`, or other build files in the repository. Do you want me to create a minimal build config or describe how to build locally?"

If you want stricter enforcement (for example, automatically blocking certain commands), ask to add a `.github/hooks/PreToolUse` hook that enforces blocking rules.

Contact: respond in-thread with clarifying answers when these rules trigger.
