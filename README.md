# AGENTS.md

A host-neutral agent policy for a strict agent pipeline — works with **Codex**, **Claude Code**, and any runtime that loads `AGENTS.md` / `CLAUDE.md`:

```text
User → Planner → |Worker| → Reviewer → Final
              ↑______________|
```

`|Worker|` is the **Worker matrix** (one or more contract-bound executors). Forward flow is Planner → matrix → Reviewer → Final. Review findings return to the Planner only; the Planner may retry, split, merge, or stop. Workers never self-patch from review findings.

Roles are bound to responsibilities, not model names: the Planner is whatever primary/root agent the host starts (the root Codex agent, the main Claude Code session, …), and the Worker matrix is a capability ladder of that host's available executors or sub-agents.

Sub-agents: skip for simple edits; never create branches; heavy work (builds, full tests, bulk installs/compiles, GPU/CPU-heavy jobs) requires Planner approval so concurrent Workers do not freeze the machine. Only the primary/root Planner may spawn sub-agents; nested spawn is forbidden.

The Planner defines the task contract, the Worker matrix makes the smallest authorized change, and the Reviewer independently verifies correctness and scope. Repository state remains controlled throughout.

User-facing communication follows the user's language. Internal work, code, identifiers, and comments default to English, except when a language-specific technical ecosystem—such as WeChat Mini Programs—makes another language the practical standard.

## Install

### Codex

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/.codex/AGENTS.md
```

### Claude Code

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/.claude/AGENTS.md
# then reference it from your global CLAUDE.md:
grep -qxF '@AGENTS.md' ~/.claude/CLAUDE.md || echo '@AGENTS.md' >> ~/.claude/CLAUDE.md
```

### Per project

Drop the file at the repo root as `AGENTS.md` (Codex) and/or import it from `CLAUDE.md` (Claude Code):

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o AGENTS.md
echo '@AGENTS.md' >> CLAUDE.md
```
