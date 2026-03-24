# SDK Apps – Cursor AI Standards

This repository contains Cursor AI configuration for SDK Apps development.

It includes:

- Rules (persistent behavioral constraints)
- Skills (reusable AI capabilities)
- Commands (user-triggered workflows)

These standards are shared across the engineering team and apply only to SDK Apps code.

---

## 🎯 Purpose

This repository defines how AI behaves when working on SDK Apps.

Goals:

- Enforce consistent code and testing standards
- Ensure deterministic AI outputs
- Prevent weak test generation
- Improve MCP-based app analysis
- Provide reusable workflows
- Enable safe scaling of AI usage across projects

This is AI governance, not general tooling.

---

## 📂 Repository Structure
- rules/
- skills/
- commands/
- CHANGELOG.md
- README.md


### rules/
Persistent constraints applied to projects.

### skills/
Reusable capabilities invoked internally by commands.


### commands/
User-triggered workflows.

---

## 🔌 How to Use in Cursor

Currently the rules, skills and commands must be copy & pasted manually from gitHub files to local Cursor settings if you want to use them with Make App Editor plugin directly on the files in Temp.
See https://make.atlassian.net/wiki/x/NQFqpQ

### Generate Unit test
- It will check current unit test, improve them and add new tests
- uses Rule `01-apps-unit-tests.mdc` and command `/generate-unit-test` 
- Add the code.js with function and its test.js files to the prompt window and type `/generate-unit-test`


### Export app's components
- It will export all components and their endpoints to csv that can be imported to googlesheet - usefull for API updates
- uses Skill `make-mcp-app-scanner` and command `/export-make-app-components` 
- you need to specify the apps name (not label) for example, `/export-make-app-components pipedrive` 

### Review an app
- reviews the app for code best practice and UX Guidines. Uses Make MCP server and Atlasian MCP server (you need to connect them manually and update the `app-reviewer` skill with the names you are using for the MCP servers). On top of that there is a rule for unit tests and handling mappable parameter for dates without time.
Outputs a summary of errors, suggestions, and OKs.
- uses skill `app-reviewer` and command `review-app`
- you need to specify the app's name and title case / sentence case, for example `/review-app simplybook title case`

---

## 🏗 Scope

These standards apply to:

- SDK Apps code only
- MCP-based Make custom app development
- Associated test files

They do NOT apply to:
- Frontend projects
- Non-SDK internal tooling
- External repositories

---

## 📦 Versioning

This repository follows semantic versioning:

MAJOR.MINOR.PATCH

- MAJOR → Breaking AI behavior changes
- MINOR → New rules/commands/skills
- PATCH → Clarifications or non-breaking improvements

See CHANGELOG.md for details.

---

## 🔒 Governance

- All changes must go through Pull Request review.
- Direct commits to main branch are discouraged.
- AI behavior changes are treated as architectural changes.

When editing:

- Ensure correct `.mdc` frontmatter
- Do not mix experimental rules with stable rules
- Clearly document changes in CHANGELOG.md

---

## ⚠ Conflict Policy

If local project rules conflict with this repository:

This repository takes precedence for SDK Apps projects.

Avoid duplicating testing rules in local repositories.

---

## 🧠 Design Principles

1. Determinism over creativity
2. Strong assertions over vague checks
3. Explicit behavior over implicit assumptions
4. Reusable skills over repeated logic
5. Clear separation of rule vs command responsibilities

---

## 👥 Ownership

Maintained by: SDK Engineering Team

For questions or improvements:
Open a Pull Request or contact the maintainers.

---

## 🚀 Future Expansion

---

This repository defines how AI works with SDK Apps.

Treat it as infrastructure.
