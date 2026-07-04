# manage-skills

Portable shared skill management for AI coding agents.

`manage-skills` installs a global shared skill hub, links shared skills into supported
AI coding agents, and helps avoid copying the same skill into every agent's private
directory.

## Install

Most users can paste this repository URL into Codex or Claude Code and ask:

```text
Install https://github.com/Hilbert2048/manage-skills for Codex and Claude.
```

Manual install:

```bash
tmp="$(mktemp -d)" && \
  git clone https://github.com/Hilbert2048/manage-skills.git "$tmp/manage-skills" && \
  bash "$tmp/manage-skills/scripts/install"
```

Install into a custom skills hub:

```bash
tmp="$(mktemp -d)" && \
  git clone https://github.com/Hilbert2048/manage-skills.git "$tmp/manage-skills" && \
  bash "$tmp/manage-skills/scripts/install" --hub "$HOME/.ai-skills"
```

The installer is idempotent. It will not overwrite an existing real
`manage-skills` directory unless you pass `--replace`.

## What It Sets Up

By default, installation creates:

```text
~/.ai-skills/manage-skills
~/.ai-skills/bin/manage-skills
~/.ai-skills/bin/install-skill
~/.ai-skills/bin/new-skill
~/.ai-skills/bin/link-skill
~/.ai-skills/bin/migrate-skill
```

It also links this skill into supported agents:

```text
~/.codex/skills/manage-skills  -> ~/.ai-skills/manage-skills
~/.claude/skills/manage-skills -> ~/.ai-skills/manage-skills
```

And it appends shared-skill installation rules to the global Codex and Claude
instruction files when they are missing.

## Supported Agents

Current automated support:

| Agent | Status |
| --- | --- |
| Codex | Installed and linked automatically |
| Claude Code | Installed and linked automatically |
| Cursor | Supported through project `.cursor/rules/*.mdc` adapters |

High-priority future adapters include Gemini / Gemini CLI, Google Antigravity,
GitHub Copilot coding agent or Chat, GLM / Zhipu coding agents, and Kimi /
Moonshot coding agents.

## Commands

Install a skill from a GitHub tree URL:

```bash
~/.ai-skills/bin/install-skill https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

The shorthand dispatcher works too:

```bash
~/.ai-skills/bin/manage-skills https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Update an already installed skill:

```bash
~/.ai-skills/bin/install-skill --replace https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Create and link a new global shared skill:

```bash
~/.ai-skills/bin/new-skill flutter-widget-goldens "Validate Flutter golden and screenshot tests"
```

Link an existing shared skill into supported agents:

```bash
~/.ai-skills/bin/link-skill flutter-ui-testing
```

Migrate an existing agent-private skill into the shared hub:

```bash
~/.ai-skills/bin/migrate-skill --from claude root-cause-first
```

## Skill Layout

The canonical skill entry is `SKILL.md`.

Additional documentation lives in:

```text
references/architecture.md
references/quickstart.md
```

Scripts live in:

```text
scripts/install
scripts/install-skill
scripts/manage-skills
scripts/new-skill
scripts/link-skill
scripts/migrate-skill
```

The core rule is simple: keep reusable skills in a shared hub first, then use
thin links or adapters for each coding agent.
