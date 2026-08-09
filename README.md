# Minimal AGENTS.md

A one-line global policy that produced the best observed balance of speed, change size, and correctness in the repository benchmark:

```text
Inspect first. Make the smallest correct change. Verify it, then stop.
```

The policy is approximately 15 tokens. It does not repeat built-in safety, Git, testing, or coding behavior.

## Current result

Nine clean paired runs compared the one-line policy with no user-supplied `AGENTS.md` across three behavior-changing tasks in real npm packages.

- **Correctness:** both configurations passed 144/144 independent hidden checks; all repository tests and `git diff --check` runs passed.
- **Median wall time across all runs:** 45.0 seconds with the policy versus 61.1 seconds empty, a 26.4% reduction.
- **Aggregate wall time:** 504.9 seconds with the policy versus 553.2 seconds empty, an 8.7% reduction.
- **Changed lines:** 464 with the policy versus 586 empty, a 20.8% reduction. The policy produced a smaller diff in eight pairs and tied once.
- **Paired speed:** the policy was faster in seven pairs and slower in two.

The speed result is promising, not universal proof. Nine runs are still a small sample, wall time includes service variance, and the 7-2 speed split has a one-sided sign-test p-value of about 0.09. The diff-size result was more consistent.

## Method

Every run:

- used GPT-5.6 Terra at medium reasoning;
- started from the same clean package source;
- received the same task requirements within its pair;
- used one coding agent with no nested delegation;
- was evaluated by the original test suite, `git diff --check`, and a separate 16-case hidden evaluator.

The candidate condition added only the one-line global policy. The empty condition had no user-supplied `AGENTS.md`; runtime system and developer instructions still existed.

## New paired benchmark

| Package and task | Configuration | Times | Median | Median changed lines | Hidden checks |
| --- | --- | --- | ---: | ---: | ---: |
| `json-stable-stringify@1.0.2`: array replacer | Empty | 63.7, 42.7, 61.1 s | 61.1 s | 62 | 48/48 |
|  | One-line policy | 44.4, 63.1, 45.0 s | **45.0 s** | **48** | 48/48 |
| `wordwrap@1.0.0`: Unicode code points | Empty | 49.7, 51.9, 80.7 s | 51.9 s | 50 | 48/48 |
|  | One-line policy | 43.3, 40.0, 41.4 s | **41.4 s** | **30** | 48/48 |
| `proxy-from-env@1.1.0`: IPv4 CIDR | Empty | 52.4, 76.9, 74.2 s | 74.2 s | 97 | 48/48 |
|  | One-line policy | 46.7, 111.8, 69.2 s | **69.2 s** | **68** | 48/48 |

The second `proxy-from-env` policy run is the clearest warning against reading too much into one sample: it took 111.8 seconds while its paired empty run took 76.9 seconds. Repetition changed the conclusion from a single-run claim to a median-based result.

## Earlier rejected policies

An earlier benchmark used a compact and a 178 KB noisy version of the same `proxy-from-env` task. Every configuration passed its hidden checks, but the empty preset had the best paired total time.

| Global preset | Estimated tokens | Short time | Noisy time | Combined time |
| --- | ---: | ---: | ---: | ---: |
| Empty | **0** | **87.6 s** | 150.3 s | **237.9 s** |
| Broad context policy | 721 | 111.9 s | 183.9 s | 295.8 s |
| Focused context policy | 176 | 146.3 s | **110.5 s** | 256.8 s |
| Minimal conditional policy | 59 | 127.7 s | 125.9 s | 253.6 s |

Repeated noisy-input runs also rejected three shorter policies:

| Global preset | Runs | Times | Median | Hidden checks |
| --- | ---: | --- | ---: | ---: |
| Empty | 3 | 150.3, 125.1, 134.8 s | **134.8 s** | 16/16 each |
| 25-token reread limit | 2 | 155.2, 158.6 s | 156.9 s | 16/16 each |
| 42-token search-first policy | 1 | 186.8 s | 186.8 s | 16/16 |
| 44-token working-set policy | 1 | 149.6 s | 149.6 s | 16/16 |

Those failures are why the current rule contains no context-management process, delegation policy, or general workflow. It only reinforces inspection, minimality, verification, and stopping.

## Decision

The one-line policy is the current benchmark champion and is now stored in `AGENTS.md`.

It costs about 15 fixed input tokens rather than zero. Actual billed-token telemetry was unavailable, so the repository does not claim a measured total-token reduction. Keep the file empty when absolute minimum fixed prompt cost matters more than the observed speed and diff-size gains.

## Install

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/.codex/AGENTS.md
```

Token counts are local estimates. Wall-clock results include model, runtime, and service variance; results may change with other models, repositories, tools, or task distributions.
