# Skills Directory

This directory is for shared OpenCode skills that can be used across multiple workspaces.

## Usage

Skills follow the [Anthropic Agent Skills Spec](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview). Each skill is a directory containing a `SKILL.md` with YAML frontmatter:

```yaml
---
name: my-skill
description: A brief description of what this skill does
---

# My Skill

Instructions for the AI agent...
```

## Discovery

With the `opencode-agent-skills` plugin, skills are automatically discovered from:

1. `.opencode/skills/` (project)
2. `.claude/skills/` (project)
3. `~/.config/opencode/skills/` (user)
4. `~/.claude/skills/` (user)

## Notes

- This directory is pending skill development
- Add skills here for global availability across all workspaces
- Project-specific skills go in the workspace's own `.opencode/skills/`