# SDK Apps – Cursor AI Standards

This repository contains the official Cursor AI configuration for SDK Apps development.

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

Examples:
- Testing conventions
- Assertion standards
- SDK architecture rules

### skills/
Reusable capabilities invoked internally by commands.

Examples:
- Make MCP App Scanner
- Test Strength Analyzer
- Endpoint Extractor

### commands/
User-triggered workflows.

Examples:
- /generate-unit-test
- /export-make-app-components
- /review-tests

---

## 🔌 How to Use in Cursor

1. Open Cursor
2. Go to:
   Settings → Rules for AI → Project Rules
3. Click:
   Add Rule → Remote Rule (GitHub)
4. Paste this repository URL
5. Enable it

The rules and commands will sync automatically.

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

Planned enhancements may include:

- Automated rule testing
- Multi-app analysis tooling
- AI mutation-style test strengthening
- API duplication detection across apps

---

This repository defines how AI works with SDK Apps.

Treat it as infrastructure.
