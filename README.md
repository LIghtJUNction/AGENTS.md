# Empty AGENTS.md

This repository intentionally keeps `AGENTS.md` empty and tracked by Git. No `.gitkeep` is needed because Git already tracks the file itself.

## Result

For the tested coding workload, no user-supplied global prompt was the best default:

- **Lowest fixed prompt cost:** 0 custom tokens.
- **Best paired total time:** 237.9 seconds across the short and noisy tasks.
- **Best repeated noisy-task median:** 134.8 seconds across three clean runs.
- **No quality loss:** every zero-preset run passed all 16 independent checks, the repository test suite, lint, and `git diff --check`.

Additional global instructions did not produce a repeatable quality improvement. Some single runs were faster or smaller, but those gains did not survive broader or repeated comparison.

## Method

The benchmark used a real package, `proxy-from-env@1.1.0`, and one behavior-changing task: add dependency-free IPv4 CIDR support to `NO_PROXY`, update focused tests and documentation, and preserve existing behavior.

Every run:

- used GPT-5.6 Terra at medium reasoning;
- started from the same clean source;
- could edit only `index.js`, `test.js`, and `README.md`;
- was completed by one coding agent with no nested delegation;
- was checked by the repository test suite, lint, `git diff --check`, and a separate 16-case evaluator.

Two input shapes were tested:

- **Short:** compact, authoritative requirements.
- **Noisy:** the final requirements embedded in a 178 KB, 2,426-line incident bundle with repetition and superseded proposals.

“Zero preset” means no user-supplied `AGENTS.md`. Runtime system and developer instructions still exist.

## Paired short and noisy runs

| Global preset | Estimated policy tokens* | Short time | Noisy time | Combined time | Independent checks |
| --- | ---: | ---: | ---: | ---: | ---: |
| None | **0** | **87.6 s** | 150.3 s | **237.9 s** | 32/32 |
| Broad context policy | 721 | 111.9 s | 183.9 s | 295.8 s | 32/32 |
| Focused context policy | 176 | 146.3 s | **110.5 s** | 256.8 s | 32/32 |
| Minimal conditional policy | 59 | 127.7 s | 125.9 s | 253.6 s | 32/32 |

The 176-token policy produced the fastest single noisy run, but it regressed the short task enough to lose overall. All four configurations had the same hidden-test result.

## Repeated noisy-input confirmation

| Global preset | Runs | Times | Median | Public test counts | Changed lines | Independent checks |
| --- | ---: | --- | ---: | --- | --- | --- |
| None | 3 | 150.3, 125.1, 134.8 s | **134.8 s** | 165, 171, 168 | 96, 86, 91 | 16/16 each |
| 25-token reread limit | 2 | 155.2, 158.6 s | 156.9 s | 168, 167 | 83, 77 | 16/16 each |
| 42-token search-first policy | 1 | 186.8 s | 186.8 s | 168 | 92 | 16/16 |
| 44-token working-set policy | 1 | 149.6 s | 149.6 s | 168 | 97 | 16/16 |

The shortest tested rule was still 16.4% slower by median than zero preset. Its smaller diff did not come with a hidden-test advantage.

## Decision

Keep the global `AGENTS.md` empty. Add repository- or task-specific instructions only when they correct a measured failure that the base agent does not already handle.

This avoids paying prompt tokens and workflow overhead on every task for behavior already supplied by the model and runtime.

## Use

Download the tracked empty file:

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/.codex/AGENTS.md
```

Leaving the global file absent has the same prompting effect.

\*Policy token counts are local tokenizer estimates, not billed-token telemetry. Wall-clock measurements include model and runtime variance; the repeated runs reduce this uncertainty but do not establish a universal result for every repository or model.
