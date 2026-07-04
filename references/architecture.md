# Skill Architecture

This architecture is agent-neutral and applies across Codex, Claude, Cursor, and future coding agents.

## Goals

- Maintain reusable skill knowledge once.
- Let multiple coding agents discover the same skill behavior.
- Keep project-specific behavior versioned with the project.
- Keep agent-specific files as thin adapters, not competing sources of truth.
- Ask the user before installing a reusable skill into only one agent's private directory.

## Layers

## Supported Agents

### Currently Supported

| Agent | Support level | Mechanism |
| --- | --- | --- |
| Codex | First-class | Symlink shared skills into `~/.codex/skills/<skill-name>`; global policy in `~/.codex/AGENTS.md` |
| Claude Code | First-class | Symlink shared skills into `~/.claude/skills/<skill-name>`; global policy in `~/.claude/CLAUDE.md` |
| Cursor | Adapter-supported | Use project `.cursor/rules/*.mdc` as thin adapters that point to global or project shared skills/docs |

`scripts/install`, `scripts/link-skill`, `scripts/new-skill`, and `scripts/migrate-skill` currently automate
Codex and Claude. Cursor support is intentionally adapter-based because Cursor does not consume `SKILL.md` in
the same way.

### High-Priority Future Adapters

Prioritize tools by real user adoption and whether they expose stable local rule or skill-discovery surfaces.

| Agent / Tool | Likely integration | Priority note |
| --- | --- |
| Gemini / Gemini CLI | Add global or project adapters once the user's active Gemini surface and discovery path are known | High |
| Google Antigravity | Add adapter files once its local rules / agent instructions path is stable | High |
| GitHub Copilot coding agent / Chat | Use `AGENTS.md` and repository docs when available; add a dedicated adapter only if its local instruction surface is stable | Medium |
| GLM / Zhipu coding agents | Add a Chinese/English adapter once local instruction or skill paths are known | High for China |
| Kimi / Moonshot coding agents | Add a Chinese/English adapter once local instruction or skill paths are known | High for China |
| Cursor | Already adapter-supported via `.cursor/rules/*.mdc`; keep this path thin and project-scoped | Supported |

### Lower-Priority / Developer-Tool Adapters

| Agent / Tool | Likely integration |
| --- | --- |
| Windsurf | Add a thin rule-file adapter if the user actively uses it and its local rules path is known |
| Cline / Roo Code | Add a thin rules adapter when the project uses VS Code agent workflows |
| Continue | Add adapter instructions in `.continue` config if the user uses it |
| Aider | Add conventions through its supported context files for terminal-first workflows |

Future support should follow the same rule: do not duplicate full skill content. Add the smallest adapter that
points to Layer 1 or Layer 2.

### Layer 1: Global Shared Skills

Canonical location:

```text
<skills-hub>/<skill-name>/SKILL.md
```

Default local hub:

```text
~/.ai-skills/<skill-name>/SKILL.md
```

Use for reusable, cross-project workflows:

- Flutter UI testing
- root-cause-first debugging
- Flutter golden/screenshot validation
- release QA
- API integration patterns
- general architecture review workflows

Agent installation should be symlinks or thin adapters:

```text
~/.codex/skills/<skill-name>  -> <skills-hub>/<skill-name>
~/.claude/skills/<skill-name> -> <skills-hub>/<skill-name>
```

Default rule: if a skill could reasonably help more than one project or more than one coding agent, install it
here first.

### Layer 2: Project Shared Skills

Canonical location:

```text
<project>/.ai-skills/<skill-name>/SKILL.md
```

Use for project-specific workflows that should travel with the repository:

- project data model rules;
- product-specific UI language;
- project-specific QA flows;
- local architecture or sync protocols;
- domain-specific fixtures and runbooks.

Project shared skills should be referenced by project entry files:

```text
<project>/AGENTS.md
<project>/CLAUDE.md
<project>/.cursor/rules/*.mdc
<project>/docs/*.md
```

Do not promote a project skill to the global hub unless it has become genuinely cross-project.

### Layer 3: Agent-Specific Skills Or Adapters

Canonical locations depend on the agent:

```text
~/.codex/skills/<skill-name>
~/.claude/skills/<skill-name>
<project>/.cursor/rules/*.mdc
```

Use only when the skill depends on one agent's unique capabilities, syntax, plugin system, UI metadata, or
runtime APIs.

Agent-specific files should usually be thin adapters that point back to Layer 1 or Layer 2. They should not
duplicate long policy text from shared skills or project docs.

Before installing a reusable skill directly into an agent-specific directory, ask the user to confirm that the
skill is intentionally tool-specific.

### Layer 4: Vendor/Bundled Skills

Examples:

```text
~/.codex/skills/.system/*
plugin-provided skills
agent-bundled skills
```

Treat these as read-only upstream capabilities. If local behavior needs to differ, create a shared wrapper or
local skill in Layer 1 or Layer 2 rather than editing bundled files.

## Default Placement Decision

When creating or installing a skill, choose the first matching layer:

1. **Global shared**: reusable across projects or agents.
2. **Project shared**: specific to one repository, product, or domain.
3. **Agent-specific**: depends on one agent's unique runtime or UI.
4. **Vendor/bundled**: read-only; do not modify.

This is the install and maintenance order. It answers: "Where should the canonical skill live?"

## Runtime Precedence

Execution precedence is different from install location. When instructions conflict while working inside a
project, use this order:

1. System/developer/user instructions for the current session.
2. Project rules and project shared skills.
3. Relevant global shared skills.
4. Agent-specific adapters for mechanics only.
5. Model defaults.

Project rules outrank global shared skills because they are more specific. Agent-specific adapters should not
override shared policy; they should explain how the current agent applies the shared policy.

## Adapter Rules

Cursor does not consume `SKILL.md` in the same way as Codex or Claude. Use `.cursor/rules/*.mdc` as a thin
adapter that links to Layer 1 or Layer 2.

Cursor rules should summarize only the trigger and the most important constraints. The canonical long-form
workflow should remain in the global hub, project `.ai-skills`, or project `docs/`.

Apply the same pattern to any future agent with its own rule format: keep the adapter short, point to the
canonical skill or docs, and avoid copying long policy text.
