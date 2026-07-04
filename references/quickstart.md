# Manage Skills Quickstart

`manage-skills` is a portable skill for organizing AI coding-agent skills across global shared, project
shared, agent-specific, and vendor layers.

## Portable Hub Layout

The default local hub is:

```text
~/.ai-skills
```

The scripts also support any hub path:

```bash
export AI_SKILLS_HOME="$HOME/.ai-skills"
# or pass --hub /path/to/skills-hub
```

Global shared skills live at:

```text
<skills-hub>/<skill-name>/SKILL.md
```

Agent directories should normally be links:

```text
~/.codex/skills/<skill-name>  -> <skills-hub>/<skill-name>
~/.claude/skills/<skill-name> -> <skills-hub>/<skill-name>
```

## Compatibility

Current automated support:

| Agent | Status |
| --- | --- |
| Codex | Supported automatically by `scripts/install` and `scripts/link-skill` |
| Claude Code | Supported automatically by `scripts/install` and `scripts/link-skill` |
| Cursor | Supported through project `.cursor/rules/*.mdc` adapters |

High-priority future adapters:

- Gemini / Gemini CLI and Google Antigravity, once their active local instruction paths are known.
- GLM / Zhipu and Kimi / Moonshot coding agents, especially for Chinese users, once their instruction or skill
  adapter paths are known.
- GitHub Copilot coding agent / Chat through `AGENTS.md` and repo docs when available.

Lower-priority developer-tool adapters can be added for Windsurf, Cline / Roo Code, Continue, or Aider when
the user actually uses those tools.

The compatibility rule is: share the canonical skill content once, then add thin adapters for each agent.

## Install This Skill On Another Machine

For AI-assisted installation, give the agent the URL or folder for this skill and ask:

```text
Install https://github.com/Hilbert2048/manage-skills for Codex and Claude.
```

The agent should clone or download the folder, then run:

```bash
bash manage-skills/scripts/install
```

Manual one-command install after cloning:

```bash
tmp="$(mktemp -d)" && \
  git clone https://github.com/Hilbert2048/manage-skills.git "$tmp/manage-skills" && \
  bash "$tmp/manage-skills/scripts/install"
```

The installer:

- copies `manage-skills` into the skills hub;
- links it into Codex and Claude;
- creates `bin/manage-skills`, `bin/install-skill`, `bin/new-skill`, `bin/link-skill`, and
  `bin/migrate-skill`;
- appends global shared-skill rules to Codex and Claude entry files when missing;
- avoids overwriting existing real skill directories unless `--replace` is passed.

Install into a custom hub or selected agents:

```bash
bash manage-skills/scripts/install --hub /path/to/skills-hub
bash manage-skills/scripts/install --agents codex,claude
```

## AI-Assisted Skill Install And Update

The preferred user experience is to ask an AI coding agent. The agent should inspect the source and use the
scripts as atomic helpers, not treat the user as the script runner.

Install one skill:

```text
请按 manage-skills 规范安装这个可复用 skill:
https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Update one skill:

```text
请按 manage-skills 规范更新这个可复用 skill:
https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Install all skills from a collection repo:

```text
请按 manage-skills 规范安装这个仓库里的所有可复用 skills:
https://github.com/Hilbert2048/skills
```

When a user asks an agent `/manage-skills <url>` or "install/update this skill URL", the agent should run the
same shared-hub flow: inspect the source, install or update the canonical copy under
`~/.ai-skills/<skill-name>`, then link it into supported agents.

## Command Shortcuts

Install a skill from a GitHub tree URL:

```bash
~/.ai-skills/bin/install-skill https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

The shorter dispatcher form is:

```bash
~/.ai-skills/bin/manage-skills https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Update an already installed skill:

```bash
~/.ai-skills/bin/install-skill --replace https://github.com/Hilbert2048/skills/tree/main/root-cause-first
```

Use these commands as reliable shortcuts. If a source repo has an unusual layout, the agent should inspect it
and use `install-skill`, `link-skill`, or a safe manual copy as needed.

## Authoring And Linking

Create and link a new global shared skill:

```bash
~/.ai-skills/manage-skills/scripts/new-skill flutter-widget-goldens "Validate Flutter golden and screenshot tests"
```

Link an existing global shared skill:

```bash
~/.ai-skills/manage-skills/scripts/link-skill flutter-ui-testing
```

Migrate an existing agent-private skill into the shared hub:

```bash
~/.ai-skills/manage-skills/scripts/migrate-skill --from claude root-cause-first
```

Use a non-default hub:

```bash
~/.ai-skills/manage-skills/scripts/link-skill --hub /path/to/skills-hub flutter-ui-testing
```

## Confirmation Rule

If a reusable skill would be installed directly into only one agent-specific directory, ask the user first.
Direct per-agent installs are acceptable only for tool-specific skills or adapters.
