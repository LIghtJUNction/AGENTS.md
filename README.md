# AGENTS.md

A concise Codex policy for a strict three-agent pipeline:

**Planner → Worker → Reviewer → Final**

The Planner defines the task contract, the Worker makes the smallest authorized change, and the Reviewer independently verifies correctness and scope. Repository state remains controlled throughout.

User-facing communication follows the user's language. Internal work, code, identifiers, and comments default to English, except when a language-specific technical ecosystem—such as WeChat Mini Programs—makes another language the practical standard.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/AGENTS.md
```
