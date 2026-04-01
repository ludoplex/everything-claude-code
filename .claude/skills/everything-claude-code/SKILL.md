```markdown
# everything-claude-code Development Patterns

> Auto-generated skill from repository analysis

## Overview

This skill teaches you how to contribute effectively to the `everything-claude-code` JavaScript codebase. You'll learn the project's coding conventions, commit patterns, and the step-by-step workflows for adding features, skills, agents, install targets, commands, hooks, and documentation. This guide also covers testing patterns and provides quick-reference commands for common development tasks.

---

## Coding Conventions

**File Naming**
- Use `camelCase` for JavaScript files and scripts.
  - Example: `installTarget.js`, `sessionManager.js`
- Markdown documentation is named descriptively, e.g., `SKILL.md`, `AGENTS.md`.

**Import Style**
- Use relative imports.
  ```js
  // Good
  const utils = require('./utils');
  // Not used
  // const utils = require('utils');
  ```

**Export Style**
- Mixed: both CommonJS (`module.exports`) and ES6 (`export`) styles may be present.
  ```js
  // CommonJS
  module.exports = function doSomething() { ... };

  // ES6
  export function doSomethingElse() { ... }
  ```

**Commit Messages**
- Use conventional prefixes: `fix:`, `feat:`, `docs:`, `chore:`
- Average commit message length: ~58 characters.
  ```
  feat: add new agent registration to manifest
  fix: correct install script path resolution
  docs: update troubleshooting guide for ECC
  ```

---

## Workflows

### Add New Skill or Agent
**Trigger:** When introducing a new skill or agent for ECC or related harnesses  
**Command:** `/add-skill`

1. Create a new `SKILL.md` (for skills) or `.md` file (for agents) in `skills/` or `agents/`.
2. Add supporting files as needed (e.g., scripts, prompts).
3. Update `manifests/install-modules.json` to register the new skill/agent.
4. Update `AGENTS.md` and/or `README.md` to document the addition.
5. Optionally, add or update tests for the new skill/agent.

**Example:**
```bash
# Add a new skill
mkdir skills/myNewSkill
touch skills/myNewSkill/SKILL.md
# Register in manifest
vim manifests/install-modules.json
# Update documentation
vim AGENTS.md
```

---

### Add or Update Install Target
**Trigger:** When supporting a new install target or updating an existing one  
**Command:** `/add-install-target`

1. Create or update install scripts (`install.js`, `install.sh`, etc.) in the appropriate directory.
2. Update schemas (e.g., `ecc-install-config.schema.json`) as needed.
3. Register the target in `manifests/install-modules.json`.
4. Update or create logic in `scripts/lib/install-targets/*.js`.
5. Update tests in `tests/lib/install-targets.test.js`.
6. Update documentation (`README.md`).

**Example:**
```bash
# Add a new install target script
touch scripts/lib/install-targets/myIDE.js
# Register in manifest and schema
vim manifests/install-modules.json
vim schemas/ecc-install-config.schema.json
# Add test
vim tests/lib/install-targets.test.js
```

---

### Add or Update Command
**Trigger:** When introducing or improving a workflow command  
**Command:** `/add-command`

1. Create or update command `.md` files in `commands/`.
2. Add or update supporting scripts/orchestrators as needed.
3. Update documentation or references.
4. Address review feedback and iterate.

**Example:**
```bash
# Add a new command documentation
touch commands/review.md
# Add supporting script
touch scripts/review.js
```

---

### Dependency Update via Dependabot
**Trigger:** When a dependency update is available or required  
**Command:** `/update-dependency`

1. Update dependency versions in `package.json`, `yarn.lock`, or workflow YAML.
2. Update lockfiles as needed.
3. Commit with a message indicating the dependency and version.
4. Optionally update `.github/dependabot.yml`.

**Example:**
```bash
npm install some-package@latest
git commit -am "chore: bump some-package to 2.3.4"
```

---

### Refactor or Enhance Hook Logic
**Trigger:** When fixing, optimizing, or extending hook behavior  
**Command:** `/update-hook`

1. Edit or create `scripts/hooks/*.js` or `.sh` files.
2. Update `hooks/hooks.json` to register/configure the hook.
3. Add or update tests in `tests/hooks/` or `tests/scripts/`.
4. Iterate based on review feedback.

**Example:**
```bash
vim scripts/hooks/sessionManager.js
vim hooks/hooks.json
vim tests/hooks/sessionManager.test.js
```

---

### Feature Addition with Tests and Docs
**Trigger:** When developing a new feature or significant enhancement  
**Command:** `/add-feature`

1. Implement the feature in relevant source files (`scripts/`, `.opencode/`, `skills/`).
2. Add or update test files for coverage.
3. Update documentation (`README.md`, `AGENTS.md`, or `docs/`).
4. Update configuration or manifest files if needed.

**Example:**
```bash
vim scripts/newFeature.js
vim tests/newFeature.test.js
vim README.md
```

---

### Documentation Update
**Trigger:** When clarifying, adding, or updating documentation  
**Command:** `/update-docs`

1. Edit or create markdown files in `docs/` or top-level context files.
2. Update `WORKING-CONTEXT.md`, `TROUBLESHOOTING.md`, or related docs.
3. Commit with a `docs:` prefix.

**Example:**
```bash
vim docs/architecture.md
git commit -am "docs: expand architecture section"
```

---

## Testing Patterns

- Test files use the pattern `*.test.js`.
- Testing framework is not explicitly specified; likely uses a standard Node.js test runner (e.g., Jest, Mocha).
- Place tests alongside the code they cover, e.g., `tests/lib/install-targets.test.js`, `tests/hooks/*.js`.
- Example test file structure:
  ```js
  // tests/lib/install-targets.test.js
  const installTarget = require('../../scripts/lib/install-targets/myIDE');

  describe('installTarget', () => {
    it('should install correctly', () => {
      // test logic here
    });
  });
  ```

---

## Commands

| Command            | Purpose                                                      |
|--------------------|--------------------------------------------------------------|
| /add-skill         | Add a new skill or agent, register, and document it          |
| /add-install-target| Add or update an install target and related scripts/tests     |
| /add-command       | Add or update a workflow command and supporting scripts      |
| /update-dependency | Update dependencies and lockfiles                            |
| /update-hook       | Refactor or enhance hook logic and configuration             |
| /add-feature       | Add a new feature with tests and documentation               |
| /update-docs       | Add or improve documentation and context files               |
```
