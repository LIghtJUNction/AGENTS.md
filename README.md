# AGENTS.md

A compact global policy for coding agents. It keeps routine work fast while preserving stronger review for changes that actually carry risk.

## Design

- **Proportional workflow:** small, well-scoped changes can be handled directly; ambiguous or cross-cutting work gets more planning.
- **Risk-based delegation:** subagents and independent reviewers are used when they materially improve quality or latency.
- **Controlled scope:** preserve unrelated work and avoid unrequested dependency, configuration, API, or architecture changes.
- **Evidence before completion:** run relevant checks, inspect the final diff, and report validation and residual risk honestly.
- **Git and security safety:** destructive operations, external writes, branch changes, and secret handling require explicit authority.

Project- or directory-level `AGENTS.md` files remain the right place for repository layout, build commands, test commands, and local conventions.

## Benchmark

The policy was refined through four clean runs of the same real code task against `proxy-from-env@1.1.0`: adding dependency-free IPv4 CIDR support to `NO_PROXY` with public tests, lint, and a separate 16-case evaluator.

| Version | Policy tokens* | Agent turns | Time | Independent checks |
| --- | ---: | ---: | ---: | ---: |
| Original strict pipeline | 3,028 | 5 | 417 s | 16/16 after review follow-up |
| Optimized policy | 689 | 1 | 96 s | 16/16 |

\*Local tokenizer estimate; this is not billed-token telemetry.

## Install

### Codex

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/.codex/AGENTS.md
```

### Claude Code

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/.claude/AGENTS.md
grep -qxF '@AGENTS.md' ~/.claude/CLAUDE.md || echo '@AGENTS.md' >> ~/.claude/CLAUDE.md
```

### Per project

Place the file at the repository root as `AGENTS.md`. For Claude Code, reference it from `CLAUDE.md`:

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o AGENTS.md
grep -qxF '@AGENTS.md' CLAUDE.md || echo '@AGENTS.md' >> CLAUDE.md
```
