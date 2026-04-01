---
name: add-or-update-install-target
description: Workflow command scaffold for add-or-update-install-target in everything-claude-code.
allowed_tools: ["Bash", "Read", "Write", "Grep", "Glob"]
---

# /add-or-update-install-target

Use this workflow when working on **add-or-update-install-target** in `everything-claude-code`.

## Goal

Introduces or updates an install target (e.g., new IDE, harness, or integration) with scripts, schemas, and manifest registration.

## Common Files

- `.*/install.js`
- `.*/install.sh`
- `.*/uninstall.js`
- `.*/uninstall.sh`
- `manifests/install-modules.json`
- `schemas/ecc-install-config.schema.json`

## Suggested Sequence

1. Understand the current state and failure mode before editing.
2. Make the smallest coherent change that satisfies the workflow goal.
3. Run the most relevant verification for touched files.
4. Summarize what changed and what still needs review.

## Typical Commit Signals

- Create or update install scripts (e.g., install.js, install.sh, uninstall.js, uninstall.sh) in a new or existing directory.
- Update schemas (e.g., ecc-install-config.schema.json, install-modules.schema.json) to reflect new target.
- Update manifests/install-modules.json to register the target.
- Update or create scripts/lib/install-targets/*.js for logic.
- Update tests/lib/install-targets.test.js for coverage.

## Notes

- Treat this as a scaffold, not a hard-coded script.
- Update the command if the workflow evolves materially.