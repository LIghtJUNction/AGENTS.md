# AGENTS.md

A 59-token opt-in policy that teaches coding agents to filter large, repetitive context without adding a process to ordinary tasks.

It intentionally does not repeat the model's built-in coding, testing, Git, or safety behavior. Every permanent instruction consumes context on every task, so this file contains only the behavior being added.

## When to use it

Install it globally when work commonly includes long logs, repeated tool output, incident histories, or specifications with superseded decisions.

Use no global `AGENTS.md` when most tasks are short and the absolute lowest fixed input cost matters. The controlled benchmark below found that zero custom preset remained the best aggregate baseline; the 59-token policy helped specifically on noisy input.

## Benchmark

Each cell is one clean run of the same real task against `proxy-from-env@1.1.0`: add dependency-free IPv4 CIDR support to `NO_PROXY`, update tests and documentation, and pass lint plus a separate 16-case evaluator.

- **Short:** requirements were already compact and authoritative.
- **Noisy:** the same final requirements were embedded in a 178 KB, 2,426-line incident bundle containing repetition and superseded proposals.
- Every run used the same model and reasoning setting, started from the same source, and could edit only `index.js`, `test.js`, and `README.md`.
- **Zero preset** means no user-supplied `AGENTS.md`. Runtime system/developer instructions still exist and cannot be removed.

| Custom preset | Estimated tokens* | Short time | Short public tests | Short diff | Noisy time | Noisy public tests | Noisy diff |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| None (baseline) | 0 | **87.6 s** | 167 | +89/-3 | 150.3 s | 165 | +93/-3 |
| Broad context policy | 721 | 111.9 s | 167 | **+70/-2** | 183.9 s | **170** | +99/-5 |
| Focused context policy | 176 | 146.3 s | 166 | +143/-25 | **110.5 s** | 168 | +91/-4 |
| Minimal conditional policy (current) | 59 | 127.7 s | 167 | +111/-6 | 125.9 s | 166 | **+84/-3** |

All eight runs passed 16/16 independent checks, the repository test suite, lint, and `git diff --check`.

On the noisy workload, the current policy was 16.2% faster than no preset and produced a 9.4% smaller diff by changed lines. Across both single runs, no preset had the lowest total wall time. The 59-token version is therefore a targeted tradeoff, not a claim that more prompting always helps.

\*Token counts are local tokenizer estimates for the policy file, not billed-token telemetry. Wall-clock results are single-run observations and include sampling/runtime variance; repeat them before treating small differences as universal.

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
