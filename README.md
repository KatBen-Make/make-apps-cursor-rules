# Personal Cursor Rules / Skills / Commands

This repository holds **KatBen's personal** Cursor AI configuration: rules,
skills, and commands used day to day in Cursor.

> **Note:** This repo previously held a set of Make SDK Apps rules/skills/commands
> (unit test generation, app review, app component export). That content has
> been retired here — it's now covered by the **make-integration-engineering**
> Cursor plugin (marketplace), which provides more complete, actively
> maintained equivalents:
> - `sdk-app-review` skill — app review against coding practices and UX guidelines
> - `sdk-app-inventory` skill — component inventory / export / API-vs-vendor-docs checkup
> - the global "Apps Unit Testing Convention" user rule — deterministic unit test generation
>
> If you're looking for that content, install the `make-integration-engineering`
> plugin from the Make Cursor Marketplace instead of copying files from here.

## Purpose

A personal, versioned home for:

- **rules/** — persistent behavioral constraints for Cursor AI
- **skills/** — reusable AI capabilities
- **commands/** — user-triggered workflows

Content to be added.

## Structure

```
rules/
skills/
commands/
README.md
```

## How to use in Cursor

Copy the relevant files into your local Cursor config (e.g. `~/.cursor/rules`,
`~/.cursor/skills`, `~/.cursor/commands`), or reference this repo directly if
your workflow supports it.
