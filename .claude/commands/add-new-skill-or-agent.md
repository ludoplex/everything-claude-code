---
name: add-new-skill-or-agent
description: Workflow command scaffold for add-new-skill-or-agent in everything-claude-code.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /add-new-skill-or-agent

Use this workflow when working on **add-new-skill-or-agent** in `everything-claude-code`.

## Goal

Adds a new skill or agent to the codebase, including documentation and registration in manifests/catalogs.

## Common Files

- `skills/*/SKILL.md`
- `agents/*.md`
- `manifests/install-modules.json`
- `AGENTS.md`
- `README.md`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Create new SKILL.md or agent .md file in skills/ or agents/ directory.
- Add supporting files (e.g., scripts, references, or agent prompt files) as needed.
- Update manifests/install-modules.json to register the new skill/agent.
- Update AGENTS.md and/or README.md to reflect the new addition.
- Optionally, add or update tests for the new skill/agent.

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.