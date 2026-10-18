# agents-kit

Portable, role-based configuration for AI coding agents. One set of role definitions, adapters generated for each tool.

## Roles

| Role | What it does |
|---|---|
| `system-analyst` | Clarifies requirements and edge cases, evaluates technologies and scalability, writes a spec and splits work into tasks |
| `qa-engineer` | Linters, formatters, pre-commit, test runner, CI |
| `developer` | Implements features and fixes |
| `tester` | Tests for changes, regression tests for bugs |
| `reviewer` | Read-only code review with prioritised findings |
| `doc-writer` | README, docs, changelog, ADRs, AGENTS.md |
| `refactor-scout` | Read-only report on duplication, missing abstractions, dead code, hotspots |

Workflow, scalability baseline and ground rules are in [AGENTS.md](AGENTS.md).

## Supported tools

| Tool | Instructions | Subagents |
|---|---|---|
| Claude Code | `CLAUDE.md` → `AGENTS.md` | `.claude/agents/` |
| OpenCode | `AGENTS.md` | `.opencode/agents/` |
| GitHub Copilot | `AGENTS.md` | `.github/agents/` |
| Codex CLI, Cursor, Aider, others | `AGENTS.md` | roles by file: "act as reviewer per `.agents/roles/reviewer.md`" |
| Gemini CLI | `.gemini/settings.json` → `AGENTS.md` | roles by file |

## Install into a project

```bash
git clone https://github.com/<you>/agents-kit /tmp/agents-kit
rsync -a --ignore-existing --exclude .git --exclude README.md --exclude .github/workflows /tmp/agents-kit/ /path/to/project/
```

`--ignore-existing` keeps your existing `AGENTS.md`/`CLAUDE.md`; merge them by hand if present. Then fill in the Project, Commands, Structure and Conventions sections of `AGENTS.md`.

## Change a role

1. Edit `.agents/roles/<role>.md` (frontmatter: `name`, `description`, `capabilities: [read, edit, shell, web]`).
2. Run `python3 scripts/sync_agents.py` to regenerate adapters.
3. `python3 scripts/sync_agents.py --check` fails if adapters are stale — run it in CI.

Never edit files in `.claude/agents/`, `.opencode/agents/` or `.github/agents/` by hand.
