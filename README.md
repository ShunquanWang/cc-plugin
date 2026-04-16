# ecc-ai-plugin

Claude Code plugin for the ECC Agile platform — brings ECC-specific skills, commands, and agents directly into your Claude Code workflow.

## Installation

### Option 1: Via ECC Marketplace (Recommended)

```bash
# Add the ECC marketplace
claude plugin marketplace add https://github.com/ShunquanWang/ecc-ai-plugin

# Install the plugin
claude plugin install ecc-ai-plugin@ecc-ai-plugin
```

### Option 2: From Local Directory

```bash
# Clone the repo
git clone https://github.com/ShunquanWang/ecc-ai-plugin.git

# Install from local path
claude plugin install ecc-ai-plugin@local --plugin-dir ./ecc-ai-plugin
```

Or load for a single session only:

```bash
claude --plugin-dir ./ecc-ai-plugin
```

### Option 3: Self-Hosted Marketplace

If your team hosts a private Claude Code marketplace, add it and install from there:

```bash
# Add your team's marketplace (git repo containing marketplace.json)
claude plugin marketplace add https://github.com/your-org/your-marketplace

# Install the plugin from that marketplace
claude plugin install ecc-ai-plugin@your-marketplace
```

To set up your own marketplace, create a `marketplace.json` at `.claude-plugin/marketplace.json` in a git repo:

```json
{
  "name": "your-marketplace",
  "owner": { "name": "Your Team", "email": "team@example.com" },
  "plugins": [
    {
      "name": "ecc-ai-plugin",
      "source": "./",
      "description": "Claude Code plugin for ECC Agile platform",
      "version": "1.10.0"
    }
  ]
}
```

Then push the repo and share the URL with your team.

### Installation Scope

By default, plugins are installed for the current user. To share with your team via the project repo:

```bash
claude plugin install ecc-ai-plugin@ecc-ai-plugin --scope project
```

This writes to `.claude/settings.json` which can be committed to version control.

## Available Commands

| Command | Description |
|---------|-------------|
| `/analyze-story` | Analyze ECC user stories — outputs AC, task breakdown, technical suggestions, and risks |
| `/code-review [pr-number\|pr-url]` | Security and quality review of local changes or a GitHub PR |
| `/e2e` | Generate and run Playwright E2E tests for user flows |

## Requirements

- [Claude Code](https://claude.ai/code) CLI installed
- `gh` CLI (for `/code-review` PR mode and GitHub integration)
