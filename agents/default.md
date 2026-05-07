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

## Installation & Setup

Many projects have fresh-machine setup workflows:

### Common Setup Patterns

- Run setup scripts first: `./setup.sh`, `./bootstrap.sh`, `./install.sh`
- Check for required tools: `which <tool>`, `command -v <tool>`
- Install dependencies: `npm install`, `pip install -r requirements.txt`
- Verify installation: `<tool> --version`

### Package Managers

Each language has preferred package managers:

- **Node.js**: npm, bun (prefer bun for speed)
- **Python**: pip, uv (prefer uv for speed), poetry
- **Go**: go get, go mod tidy
- **Rust**: cargo
- **Shell**: apt, pacman, dnf

### Fresh Machine Installation

Typical fresh-machine workflow:
```bash
# 1. Install system packages
sudo apt install -y git gh curl wget

# 2. Install language runtimes
curl -fsSL https://bun.sh/install | bash

# 3. Install dependencies
npm install
pip install -r requirements.txt

# 4. Verify setup
<tool> --version
./scripts/setup-password-store.sh
```

### LSP Servers

For editor support, install language servers:

- **Python**: `npm install -g pyright`
- **TypeScript**: `npm install -g typescript-language-server`
- **Go**: `go install golang.org/x/tools/gopls@latest`
- **Bash**: `npm install -g bash-language-server`

## Documentation Guidelines

### README vs AGENTS.md

- **README.md**: User-facing documentation, setup instructions, usage examples
- **AGENTS.md**: Agent instructions, workflow guidance, build commands
- **CLAUDE.md**: Claude Code specific guidance (for Claude Code compatibility)

Keep documentation in sync with changes. Update docs when:
- New features are added
- Dependencies change
- Workflows are modified
- Configuration options are added

### Project Notes

For project-specific notes that don't fit in main docs:
- `PROJECT.md` - project-specific context
- `NOTES.md` -临时 notes
- `TODO.md` - deferred work

## CI/CD & GitHub Actions

Many projects use GitHub Actions for automation:

### Common Workflows

```yaml
# .github/workflows/test.yml
name: Test
on: [push, pull_request]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install
      - run: npm test
      - run: npm run lint
```

### Verify CI Locally

Before pushing, run equivalent of CI locally:
```bash
npm test        # Run tests
npm run lint   # Run linter
npm run build  # Verify build
```

## Additional Debug Patterns

Many tools support debug output:

- **Environment variable**: `DEBUG=1 <command>`
- **Flag**: `<command> --debug` or `<command> -d`
- **Verbose**: `<command> --verbose` or `<command> -v`
- **Trace**: `<command> --trace`

Example:
```bash
DEBUG=1 npm run dev    # Enable debug output
curl -vvv <url>      # Verbose HTTP debugging
kubectl -v=6 apply   # Kubernetes verbose output
```

### Log Levels

Common log levels (low to high):
- DEBUG - Detailed diagnostic info
- INFO - General informational messages
- WARN - Warning messages
- ERROR - Error messages
- SUCCESS - Success messages (custom)

### Verbose Logging

For network/API debugging:
```bash
curl -vvv http://example.com    # Very verbose HTTP
wget --debug <url>          # Debug wget
python -v -v <script.py>      # Verbose Python
```

## Quick Reference

Many repos have quick reference sections with common commands:

### Standard Commands Pattern

```bash
# Build
make build

# Run
make run

# Test
make test

# Lint/check
make lint
make check

# Full quality gate
make check   # runs lint + test
```

### Makefile Patterns

Standard make targets:
- `build` - Build the binary/package
- `run` - Run locally
- `test` - Run tests
- `lint` - Run linter
- `check` - Quality gate (lint + test)
- `clean` - Clean build artifacts

## Definition of Done

Each project should define what "done" means:

- Code builds successfully
- Tests pass
- Lint/typecheck passes
- Documentation updated
- Changes are committed

For deployment projects, add:
- Deployed to target environment
- Validation checks pass
- Smoke tests pass
- Run evidence written

## Safety Guardrails

Important safety practices:

- **Never commit secrets** or credentials
- **Never run destructive operations** unless explicitly requested
- **Fail fast** on validation errors
- **Verify** before executing destructive commands
- **Review** changes before pushing to production

## Environment Variables

Common env var patterns:
- `DEBUG=1` - Enable debug output
- `VERBOSE=1` - Enable verbose logging
- `DRY_RUN=1` - Preview without executing
- `ENV=development` - Set environment

Check for required env vars:
```bash
# Check if var is set
if [ -z "${VAR_NAME}" ]; then
    echo "VAR_NAME is not set"
    exit 1
fi