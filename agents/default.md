# OpenCode Default Agent Instructions

## Overview

This is the default AGENTS.md for new OpenCode workspaces. It provides guidelines for agent behavior, workflow, and best practices.

## Build Commands

Always document clear build/run commands in your AGENTS.md:

- **Build**: `npm run build`, `bun build`, `go build`, `make build`
- **Dev server**: `npm run dev`, `bun run index.ts`
- **Test**: `npm test`, `bun test`, `go test ./...`
- **Lint/Typecheck**: `npm run lint`, `bun run typecheck`, `npm run typecheck`

## Dependencies

List dependencies explicitly with installation method:

- **npm/bun**: `npm install`, `bun add <package>`
- **Python**: `pip install`, `uv pip install -r requirements.txt`
- **Go**: `go get`, `go mod tidy`
- Check package.json/requirements.txt before adding new deps

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

- **Run tests** before considering changes complete: `npm test`, `bun test`, `go test`
- **Verify lint/typecheck** pass: `npm run lint`, `bun run typecheck`, `npm run typecheck`
- If no test command exists, check README or AGENTS.md for testing instructions
- **Watch mode** for development: `npm test -- --watch`, `bun test --watch`

## Verification Checklist

Before submitting/pushing:

1. Run syntax check on shell files: `bash -n <file>`
2. Run tests: `npm test` or equivalent
3. Run typecheck: `npm run typecheck` or `tsc --noEmit`
4. Run lint: `npm run lint` or `bun run lint`
5. Check `git status` for unintended changes

## Code Style

- **Follow existing conventions** — match the codebase's style, naming, and patterns
- **Never add comments** unless explicitly requested
- **Keep responses concise** — answer the question directly without unnecessary preamble
- **Naming**: PascalCase (components), camelCase (variables/functions), UPPER_CASE (constants)
- **Imports**: Order imports: stdlib → external → internal → styles

## Source of Truth

Distinguish between **source files** (editable) and **generated artifacts** (from build tools):

- Generated files (build outputs, compiled binaries, CAD exports) should not be hand-edited
- Edit the source first, then regenerate
- If regenerated output differs from checked-in files, the regenerated output is authoritative

## Security

- **Never commit secrets, keys, or credentials** to the repository
- Use environment variables or secrets management for sensitive data
- Review files before adding to git to ensure no secrets are included
- The repo uses `.gitignore` for common secret patterns (`.env`, `*.key`, `credentials.json`)

## Asking for Help

- If stuck, ask clarifying questions rather than guessing
- Provide relevant context when requesting user input
- Suggest options when the path forward is unclear