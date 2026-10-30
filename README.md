<div align="center">
  <h1>Minimal AGENTS.md</h1>
  <p><strong>One line for coding agents. Measured against saying nothing.</strong></p>

  [![Policy size](https://img.shields.io/badge/policy-71%20bytes-111111?style=flat-square)](AGENTS.md)
  [![Paired runs](https://img.shields.io/badge/paired%20runs-9-111111?style=flat-square)](#benchmark-result)
  [![License](https://img.shields.io/github/license/LIghtJUNction/AGENTS.md?style=flat-square)](LICENSE)
  [![Last commit](https://img.shields.io/github/last-commit/LIghtJUNction/AGENTS.md?style=flat-square)](https://github.com/LIghtJUNction/AGENTS.md/commits/main)

  [The line](#the-entire-policy) · [Install](#install) · [Result](#benchmark-result) · [Method](#method) · [Rejected policies](#why-not-write-more)
</div>

## The entire policy

```text
Inspect first. Make the smallest correct change. Verify it, then stop.
```

That is the complete [`AGENTS.md`](AGENTS.md): 71 bytes, approximately 15 tokens.

Its mechanism is deliberately small:

```text
inspect before guessing
        ↓
change only what the task requires
        ↓
verify the change
        ↓
stop instead of expanding scope
```

It does not restate built-in safety, Git, testing, style, delegation, context-management, or programming rules. The benchmark suggests that this small reminder can improve change focus without paying the fixed prompt cost of a large global policy.

## Install

For Codex global instructions:

```bash
mkdir -p ~/.codex
curl -fsSL \
  https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md \
  -o ~/.codex/AGENTS.md
```

Or copy the line into a repository-level `AGENTS.md` when it should apply only to that project.

Before overwriting an existing file, inspect it. This repository's result does not imply that project-specific build, safety, architecture, or contribution instructions should be deleted.

## Benchmark result

Nine clean paired runs compared the one-line policy with **no user-supplied `AGENTS.md`** across three behavior-changing tasks in real npm packages.

| Metric | Empty policy | One-line policy | Observed difference |
| --- | ---: | ---: | ---: |
| Independent hidden checks | 144/144 | 144/144 | tied |
| Median wall time across all runs | 61.1 s | **45.0 s** | **26.4% lower** |
| Aggregate wall time | 553.2 s | **504.9 s** | **8.7% lower** |
| Changed lines | 586 | **464** | **20.8% lower** |
| Faster paired runs | 2 | **7** | 7–2 split |

Every run also passed its repository tests and `git diff --check`.

The strongest repeated observation was smaller diffs: the policy produced fewer changed lines in eight pairs and tied once. The speed result is promising but not universal proof. Nine runs are a small sample, service latency is noisy, and the 7–2 speed split has a one-sided sign-test p-value of about 0.09.

## Per-task data

| Package and task | Configuration | Run times | Median | Median changed lines | Hidden checks |
| --- | --- | --- | ---: | ---: | ---: |
| `json-stable-stringify@1.0.2`: array replacer | Empty | 63.7, 42.7, 61.1 s | 61.1 s | 62 | 48/48 |
|  | One line | 44.4, 63.1, 45.0 s | **45.0 s** | **48** | 48/48 |
| `wordwrap@1.0.0`: Unicode code points | Empty | 49.7, 51.9, 80.7 s | 51.9 s | 50 | 48/48 |
|  | One line | 43.3, 40.0, 41.4 s | **41.4 s** | **30** | 48/48 |
| `proxy-from-env@1.1.0`: IPv4 CIDR | Empty | 52.4, 76.9, 74.2 s | 74.2 s | 97 | 48/48 |
|  | One line | 46.7, 111.8, 69.2 s | **69.2 s** | **68** | 48/48 |

The 111.8-second policy run is important evidence, not an outlier to hide. Its paired empty run took 76.9 seconds. Repetition changed the conclusion from “always faster” to the narrower and supportable claim: the policy had a better median in this sample and usually produced a smaller diff.

## Method

Each paired run:

- used GPT-5.6 Terra at medium reasoning;
- started from the same clean package source;
- received identical task requirements within the pair;
- used one coding agent with no nested delegation;
- was evaluated by the original repository tests;
- ran `git diff --check`;
- ran a separate 16-case hidden evaluator.

The only experimental difference was the user-supplied global file:

```text
candidate: the one-line AGENTS.md
control:   no user-supplied AGENTS.md
```

Runtime system and developer instructions still existed in both conditions. This is therefore a comparison of **one extra global line versus no extra user policy**, not “instructed model versus completely uninstructed model.”

## Why not write more

Earlier experiments tested broader and more procedural policies. All configurations remained correct on the measured hidden checks, but extra instruction often increased time.

| Global preset | Estimated tokens | Short task | Noisy task | Combined |
| --- | ---: | ---: | ---: | ---: |
| Empty | **0** | **87.6 s** | 150.3 s | **237.9 s** |
| Broad context policy | 721 | 111.9 s | 183.9 s | 295.8 s |
| Focused context policy | 176 | 146.3 s | **110.5 s** | 256.8 s |
| Minimal conditional policy | 59 | 127.7 s | 125.9 s | 253.6 s |

Repeated noisy-input tests also failed to justify several shorter context policies:

| Policy | Runs | Times | Median | Hidden checks |
| --- | ---: | --- | ---: | ---: |
| Empty | 3 | 150.3, 125.1, 134.8 s | **134.8 s** | 16/16 each |
| 25-token reread limit | 2 | 155.2, 158.6 s | 156.9 s | 16/16 each |
| 42-token search-first | 1 | 186.8 s | 186.8 s | 16/16 |
| 44-token working-set | 1 | 149.6 s | 149.6 s | 16/16 |

That is why the winning line contains no general context process, delegation policy, tool ritual, or long checklist. It reinforces four observable behaviors and then gets out of the way.

## What this does not prove

- It does not prove the line is optimal for every model, repository, runtime, or task distribution.
- It does not prove statistically significant speed improvement from nine pairs.
- It does not measure billed input/output tokens; the ~15-token size is a local estimate.
- It does not replace repository-specific requirements.
- It does not show that every longer `AGENTS.md` is harmful; some projects require detailed contracts.

Keep the global file empty when absolute minimum fixed prompt cost matters more than the observed focus and diff-size gains. Use project-local instructions for facts the agent cannot reliably infer from the repository.

## Decision

The one-line policy is the current benchmark champion in this repository and is stored verbatim in [`AGENTS.md`](AGENTS.md). Future changes should be evaluated with clean paired runs, independent correctness checks, full raw timings, and visible negative results—not intuition alone.

## License

[MIT](LICENSE)
