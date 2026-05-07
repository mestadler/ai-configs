# OpenCode Default Agent Instructions

## Overview

This is the default AGENTS.md for new OpenCode workspaces. It provides guidelines for agent behavior, workflow, and best practices.

## File Operations

- **Always use the specialized tools** for file operations (Read, Write, Edit, Glob, Grep) rather than shell commands like `cat`, `echo`, or `sed`
- **Prefer Edit over Write** when modifying existing files to preserve context
- **Use Glob** for finding files by pattern
- **Use Grep** for searching file contents
- **Never assume file existence** — always verify before editing

## Issue Workflow

1. **Before making changes**, understand the issue or task
2. **Search the codebase** for relevant patterns and existing implementations
3. **Plan the approach** before writing code
4. **Make changes** in small, reviewable increments
5. **Verify** the changes work correctly

## Commit Workflow

Follow the Git workflow:

1. Check status with `git status`
2. Review changes with `git diff`
3. Stage relevant files with `git add`
4. Write a clear, concise commit message that explains **why**, not just **what**
5. Push to remote after commit

Use the `git_acp()` function available in the shell for an interactive add-commit-push workflow.

### Commit Message Guidelines

- Use present tense: "Add feature" not "Added feature"
- Keep first line under 72 characters
- Reference issues when applicable: "Closes #123" or "Fixes #456"

## Testing

- **Run tests** before considering changes complete
- **Verify lint/typecheck** pass (check for npm scripts like `lint`, `typecheck`, `format`)
- If no test command exists, check README or AGENTS.md for testing instructions

## Code Style

- **Follow existing conventions** — match the codebase's style, naming, and patterns
- **Never add comments** unless explicitly requested
- **Keep responses concise** — answer the question directly without unnecessary preamble

## Security

- **Never commit secrets, keys, or credentials** to the repository
- Use environment variables or secrets management for sensitive data
- Review files before adding to git to ensure no secrets are included

## Asking for Help

- If stuck, ask clarifying questions rather than guessing
- Provide relevant context when requesting user input
- Suggest options when the path forward is unclear