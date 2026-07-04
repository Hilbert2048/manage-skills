---
name: manage-skills
description: Manage AI coding-agent skills across global shared, project shared, agent-specific, and vendor layers. Use when creating, installing, linking, migrating, auditing, or deciding placement for reusable skills across Codex, Claude, Cursor, or other coding agents; when asked to avoid per-agent skill drift; or when a skill installer wants to write directly into one agent's private skill directory.
---

# Manage Skills

Use this skill to keep AI coding-agent skills portable, shareable, and consistent across tools.

## Core Policy

Use shared canonical sources before agent-specific installs:

1. Global shared skill: reusable across projects or agents.
2. Project shared skill: specific to one repository or product.
3. Agent-specific adapter: only for unique agent runtime, UI, or plugin mechanics.
4. Vendor/bundled skill: read-only upstream capability.

If a reusable skill would be installed directly into only `~/.codex/skills`, `~/.claude/skills`, or another
agent-private folder, ask the user to confirm that it is intentionally tool-specific.

## Compatibility

Current automated support: Codex and Claude Code. Current adapter support: Cursor through `.cursor/rules/*.mdc`.
For other coding agents, create thin adapters that point to the shared skill or project docs instead of copying
the full skill content.

## Workflow

1. Decide placement with `references/architecture.md`.
2. If installing this management skill on a new machine, run `scripts/install`.
3. For global shared skills, use `scripts/new-skill` or `scripts/link-skill`.
4. For existing agent-private skills, migrate the canonical copy into the shared hub before linking.
5. Keep agent-specific files as symlinks or thin adapters.
6. For Cursor, create `.cursor/rules/*.mdc` adapters that point to shared or project docs.

## Scripts

- `scripts/install`: one-command installer for the management skill itself.
- `scripts/new-skill`: scaffold a portable global shared skill and link it into supported agents.
- `scripts/link-skill`: link an existing shared skill into supported agents.
- `scripts/migrate-skill`: move an existing agent-private skill into the shared hub and replace agent copies
  with symlinks.

All scripts support `AI_SKILLS_HOME` and `--hub <path>` so the skill can be shared without depending on the
original author's local directory.

## References

- Read `references/architecture.md` when deciding skill placement, precedence, or layer boundaries.
- Read `references/quickstart.md` when installing this skill hub on another machine or explaining commands.
