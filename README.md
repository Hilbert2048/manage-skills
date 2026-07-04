# manage-skills

Portable shared skill management for AI coding agents.

`manage-skills` helps AI agents install reusable skills into one shared hub, then
link those skills into supported coding agents such as Codex and Claude Code.

## Install Or Update Skills With AI

This is the preferred interface. Paste one of these prompts into Codex, Claude
Code, or another coding agent on a machine where `manage-skills` is installed.

Install one skill:

```text
Please install this reusable skill following the manage-skills convention:
https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Update one skill:

```text
Please update this reusable skill following the manage-skills convention:
https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Install every skill from a collection repository:

```text
Please install all reusable skills from this repository following the manage-skills convention:
https://github.com/Hilbert2048/skills
```

The agent should inspect the source, install or update the canonical copy under
`~/.ai-skills/<skill-name>`, then link supported agents to that shared copy. The
scripts are atomic helper tools for the agent, not the main user experience.

## Command Shortcuts

Use these when you prefer a terminal command or want to give an agent a precise
helper command.

Install from a GitHub tree URL:

```bash
~/.ai-skills/bin/manage-skills https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Update an installed skill:

```bash
~/.ai-skills/bin/manage-skills update https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Install all skills from a collection repository:

```bash
~/.ai-skills/bin/manage-skills https://github.com/Hilbert2048/skills
```

The lower-level command is also available:

```bash
~/.ai-skills/bin/install-skill https://github.com/Hilbert2048/skills/tree/main/root-cause-first
~/.ai-skills/bin/install-skill --replace https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

If a source repository has an unusual layout, let the agent inspect it and use
`install-skill`, `link-skill`, or a safe manual copy while preserving the same
shared-hub rule.

## Install manage-skills

Most users can paste this repository URL into Codex or Claude Code and ask:

```text
Please install https://github.com/Hilbert2048/manage-skills for Codex and Claude.
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

## How It Works

The core rule is simple: keep reusable skills in a shared hub first, then use
thin links or adapters for each coding agent.

Default shared hub:

```text
~/.ai-skills/<skill-name>/SKILL.md
```

Agent links:

```text
~/.codex/skills/<skill-name>  -> ~/.ai-skills/<skill-name>
~/.claude/skills/<skill-name> -> ~/.ai-skills/<skill-name>
```

Project-specific skills should live with the project, such as:

```text
<project>/.ai-skills/<skill-name>/SKILL.md
```

Agent-specific files should normally stay thin and point back to the shared
source. Do not copy reusable skills directly into only one agent-private
directory unless the user explicitly wants a tool-specific install.

## Authoring And Linking

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
