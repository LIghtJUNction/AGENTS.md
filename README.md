# AGENTS.md

A concise Codex policy for a strict agent pipeline:

```text
User → Planner → |Worker| → Reviewer → Final
              ↑______________|
```

`|Worker|` is the **Worker matrix** (one or more contract-bound executors). Forward flow is Planner → matrix → Reviewer → Final. Review findings return to the Planner only; the Planner may retry, split, merge, or stop. Workers never self-patch from review findings.

Sub-agents: skip for simple edits; never create branches; heavy work (builds, full tests, bulk installs/compiles, GPU/CPU-heavy jobs) requires Planner approval so concurrent Workers do not freeze the machine. Only the primary/root Planner may spawn sub-agents; nested spawn is forbidden.

The Planner defines the task contract, the Worker matrix makes the smallest authorized change, and the Reviewer independently verifies correctness and scope. Repository state remains controlled throughout.

User-facing communication follows the user's language. Internal work, code, identifiers, and comments default to English, except when a language-specific technical ecosystem—such as WeChat Mini Programs—makes another language the practical standard.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/AGENTS.md
```
