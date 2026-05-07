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

### TODO.md Workflow

Many repositories use `TODO.md` to track deferred work:

1. **Create TODO.md** at repository root with deferred items
2. **Plan Agent** creates GitHub issues from TODO items
3. **Build Agent** works against the GitHub issues

Example TODO.md structure:
```markdown
# TODO

Backlog of work that has been identified but deliberately deferred.

---

- [ ] Item 1 - description
- [ ] Item 2 - description
```

When an item is completed, delete it from TODO.md in the same commit.

### GitHub Issues

Create GitHub issues for work that needs tracking beyond the repository:

1. **Use GitHub CLI** (`gh issue create`) to create issues
2. **Reference issues** in commits: "Closes #123" or "Fixes #456"
3. **Label appropriately**: bug, enhancement, documentation

Example issue creation:
```bash
gh issue create --title "Feature: Add X support" --body "Description..." --label enhancement
```

### Plan Agent / Build Agent Workflow

The OpenCode agent operates in two modes:

1. **Plan Mode (read-only)**: Research, analyze, propose solutions
   - Cannot make changes
   - Creates GitHub issues for the work
   - Provides detailed plans for review

2. **Build Mode**: Implements changes
   - Can make edits and run commands
   - Works against issues created in plan mode
   - Commits and pushes changes

When prompted for a task:
1. If in **plan mode** → research and create issues, don't implement
2. If in **build mode** → implement the solution directly

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

## Troubleshooting

When encountering issues:

1. **Check logs first**: Look for error messages in output
2. **Search existing issues**: Check TODO.md, README, or documentation
3. **Verify environment**: Check dependencies, environment variables
4. **Isolate the problem**: Test individual components separately

### Common Debug Patterns

- **Shell scripts**: Run with `bash -n <file>` to check syntax before executing
- **Container issues**: Check logs with `nerdctl logs` or `docker compose logs`
- **Kubernetes**: Use `kubectl describe` for detailed resource info
- **API errors**: Use verbose curl (`curl -vvv`) to see request/response

## Container & Kubernetes

Many projects use containerized workflows:

### nerdctl (containerd)
- `nerdctl ps` - List containers
- `nerdctl logs -f <container>` - Follow logs
- `nerdctl exec -it <container> sh` - Shell into container
- `nerdctl compose up -d` - Start compose services
- `nerdctl compose logs -f` - Follow compose logs

### kubectl
- `kubectl get pods/svc/deployments` - List resources
- `kubectl logs -f <pod>` - Follow pod logs
- `kubectl describe <resource>` - Detailed info
- `kubectl exec -it <pod> -- sh` - Shell into pod
- `kubectl apply -f <file>` - Apply YAML
- `kubectl delete -f <file>` - Delete resources

## Distribution Model

This repository uses a **hardcopy** distribution model:

- Copy configs to workspaces (not symlinks)
- Workspaces can diverge from defaults as needed
- This repo remains source of truth for future projects

For symlink-based repos (like dotfiles), changes take effect immediately on next shell source.