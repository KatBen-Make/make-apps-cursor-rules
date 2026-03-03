# Make Apps SDK – Cursor Agent

This agent loads and activates Make SDK Apps Rules, Skills, and Commands
from this repository into Cursor.

It is designed for development and auditing of Make SDK custom apps using:
- Make Apps SDK
- Make API MCP server
- Cursor IDE

---

## 🔹 Rule Loading

Load all rule files from:

.rules/

Rules:
- Apply globally unless explicitly scoped.
- May enforce coding standards, UX guidelines, naming conventions, and SDK best practices.
- May define safety constraints.
- Must not assume command-specific arguments unless explicitly defined inside the rule.

Rules should guide behavior — not override explicit command instructions.

---

## 🔹 Skill Loading

Load all skills from:

.skills/

Skills:
- Represent reusable capabilities.
- May perform analysis, validation, inspection, or structured updates.
- May be used by commands or directly by the agent when appropriate.
- Should define their own input expectations.
- Must not assume required arguments unless explicitly defined in the skill.

When interacting with Make apps:
- Prefer using the Make API MCP server.
- Do not simulate app data if MCP access is available.

---

## 🔹 Command Loading

Load all commands from:

.commands/

Commands:
- Are explicitly invoked using `/command-name`.
- Define their own arguments.
- May have required arguments, optional arguments, or no arguments.
- Define their own output format.
- May operate in read-only or modification mode depending on their specification.

The agent must:
- Respect each command’s defined interface.
- Not enforce a universal argument pattern.
- Not enforce a universal output structure.
- Follow the behavior explicitly defined inside the command.

If a command includes optional fix instructions:
- Default to read-only unless the command specifies otherwise.

---

## 🔹 MCP Usage Policy

When a task requires interaction with a Make custom app:

- Use the Make API MCP server if available.
- Fetch the latest state before performing analysis.
- After modifications, confirm changes.
- If MCP access is unavailable, clearly state assumptions.

---

## 🔹 Safety & Modification Principles

Unless a command explicitly allows modifications:

- Default to analysis mode.
- Do not apply speculative fixes.
- Do not modify unrelated modules.
- If uncertain, report findings instead of guessing.

Modification scope must always be:
- Limited to the instruction provided.
- Limited to the relevant module(s).
- Consistent with Make Apps SDK best practices.

---

## 🔹 Output Behavior

There is no enforced global output structure.

Each:
- Rule
- Skill
- Command

may define its own output format.

However, outputs should generally be:
- Clear
- Structured when appropriate
- Explicit about actions taken (if any)
- Explicit about assumptions (if any)

---

## 🔹 Scope

This agent supports:

- Make SDK custom app development
- SDK auditing
- UX validation
- Parameter verification
- Structured AI-assisted app improvements
- Cursor-based workflow automation

---

End of agent configuration.