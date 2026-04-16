# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**ecc-ai-plugin** is a Claude Code plugin for the ECC Agile platform. It bundles skills, commands, agents, and hooks to extend Claude Code with ECC-specific workflows.

- Marketplace: `.claude-plugin/marketplace.json`
- Plugin manifest: `.claude-plugin/plugin.json`
- Repository: https://github.com/ShunquanWang/cc-plugin

## Plugin Development Commands

```bash
# Load plugin for current session (local dev)
claude --plugin-dir .

# Validate plugin structure
claude plugin validate .

# Debug plugin loading
claude --debug
```

## Component Locations

| Type | Path | Invocation |
|------|------|-----------|
| Skills | `skills/<name>/SKILL.md` | `/name` |
| Commands (legacy flat) | `commands/<name>.md` | `/name` |
| Agents | `agents/<name>.md` | Referenced by skills/commands |
| Hooks | `hooks/hooks.json` | Automatic lifecycle events |

## Current Components

### Skills
- **`skills/analyze-story`** — Analyzes ECC user stories/requirements; outputs AC (Given/When/Then), task breakdown with complexity (S/M/L), technical suggestions, and risks.

### Commands
- **`commands/code-review.md`** — Security and quality review of local uncommitted changes or GitHub PRs (pass PR number/URL as argument). Posts review to GitHub via `gh` CLI. Saves review artifacts to `.claude/PRPs/reviews/`.
- **`commands/e2e.md`** — Legacy shim that delegates to the `e2e-testing` skill (Playwright-based E2E test generation and execution).

## Architecture

### Two ways to define slash commands

1. **Skills** (`skills/<name>/SKILL.md`) — preferred for complex workflows; supports subdirectories with helper scripts.
2. **Commands** (`commands/<name>.md`) — flat files; legacy approach still supported via `"commands": ["./commands/"]` in `plugin.json`.

### Path variables available in hooks and MCP configs
- `${CLAUDE_PLUGIN_ROOT}` — absolute path to the plugin directory
- `${CLAUDE_PLUGIN_DATA}` — persistent data dir (`~/.claude/plugins/data/ecc-ai-plugin/`)

### plugin.json key fields
```json
{
  "skills": "./skills/",
  "agents": "./agents/",
  "hooks": "./hooks/hooks.json",
  "commands": ["./commands/"]
}
```

Claude Code auto-discovers components from these paths even without explicit manifest entries.

### code-review command phases
Local mode: GATHER → REVIEW → REPORT  
PR mode: FETCH → CONTEXT → REVIEW → VALIDATE → DECIDE → REPORT → PUBLISH → OUTPUT

The PR review reads full files (not just diffs), runs available validators (`npm`, `cargo`, `go`, `pytest`), and posts inline GitHub comments via `gh api`.
