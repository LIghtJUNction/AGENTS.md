# 3-Agent System Rules

## Core Principle

This system uses a strict 3-agent pipeline:

User → Planner → Worker → Reviewer → Final

The goal is correctness, low coordination cost, stable repository state, and minimal unnecessary work.

This system is not optimized for elegant code, broad refactoring, or autonomous exploration.

---

## Roles

### Planner

Owns planning and structure.

The Planner:
- decomposes tasks
- defines scope
- defines allowed files
- defines validation criteria
- decides reuse vs new work
- decides retry, split, merge, or stop

Only the Planner may make structural decisions.

### Worker

Owns execution only.

The Worker:
- follows the assigned task exactly
- modifies only allowed files
- produces minimal diffs
- does not refactor unrelated code
- does not expand scope
- does not change architecture
- reports blockers instead of guessing

The Worker must not “improve” the task beyond instructions.

### Reviewer

Owns verification only.

The Reviewer:
- checks correctness
- checks scope compliance
- checks process compliance
- checks diff size
- checks forbidden operations
- escalates structural issues to Planner

The Reviewer detects problems but does not redesign the task.

---

## Hard Git Rules

Branch switching is strictly forbidden.

Never run:

git checkout
git switch
git checkout -b
git switch -c
git branch -m
git worktree add
git worktree remove

Also forbidden unless explicitly authorized outside the agent pipeline:

git pull
git fetch
git merge
git rebase
git reset
git reset --hard
git clean
git stash
git stash pop
git commit
git push
git cherry-pick
git revert

Allowed Git commands are read-only inspection commands only, such as:

git status
git diff
git diff --stat
git rev-parse --abbrev-ref HEAD
git rev-parse HEAD
git log --oneline -n 5

If the branch is wrong, detached, dirty, or unexpected, stop immediately and report it.

Do not repair repository state yourself.

---

## Workspace Rules

One task must use one stable workspace:

one task = one repository = one branch = one worktree = one diff

Forbidden:
- temporary branches
- detached HEAD
- secondary worktrees
- hidden local state
- overwriting unrelated changes
- stashing or resetting to hide problems

If unrelated dirty files exist, stop and report:
- dirty files
- whether they overlap with the task
- whether safe execution is possible

---

## Scope Rules

Planner must define:
- allowed files
- read-only files
- forbidden files
- expected output
- validation method
- non-goals

Worker may only modify allowed files.

Forbidden without explicit Planner approval:
- unrelated refactoring
- dependency changes
- lockfile changes
- generated file changes
- config changes
- public API changes
- file moves or renames
- broad formatting changes
- test weakening

Minimal diff is mandatory.

---

## Task Allocation Rules

Prefer:
- reuse over new code
- modification over regeneration
- single-pass execution over repeated refinement
- merged tasks over unnecessary splitting
- serial execution over risky parallel execution

Parallel execution is allowed only when:
- files do not overlap
- generated outputs do not overlap
- dependencies do not overlap
- shared config is untouched
- Planner explicitly approves it

Otherwise, serialize tasks.

---

## Feedback Rules

No local patch loops.

Flow after execution:

Worker executes → Reviewer verifies → Planner decides next step

Worker does not decide fixes after review failure.

Reviewer does not assign new implementation directly.

Structural issues must return to Planner.

---

## Escalation Rules

Stop and escalate on:
- wrong branch
- detached HEAD
- unexpected dirty workspace
- unclear file ownership
- conflicting requirements
- missing critical context
- required dependency change
- required architecture change
- required public API change
- unrelated test failure
- unsafe generated files
- security or secret risk

Unsafe guessing is forbidden.

---

## Validation Rules

Worker must report validation honestly:

not run
run and passed
run and failed
partially run
unable to run

Never claim tests passed unless they were actually run.

Never weaken tests just to pass.

Forbidden:
- deleting failing tests
- silently skipping tests
- relaxing assertions without reason
- changing snapshots without explanation

---

## Security Rules

Never expose, print, move, or modify secrets.

Forbidden:
- committing .env files
- logging credentials
- weakening auth
- disabling permissions
- adding broad privileges
- ignoring security failures

Security-related changes require stricter review.

---

## Completion Criteria

A task is complete only when:
- requested behavior is implemented
- diff is minimal
- modified files are within scope
- branch never changed
- workspace state stayed controlled
- no forbidden Git command was used
- validation is reported honestly
- Reviewer approves or records acceptable risk

---

## Priority Order

1. Reduce agent calls
2. Reduce task splitting
3. Reduce context passing
4. Reduce repeated computation
5. Reduce implementation surface
6. Optimize code details last

Correctness is required.

Coordination stability is the main objective.

---

## One-Line Summary

Planner decides, Worker executes, Reviewer verifies. Branch and workspace state are immutable. Scope is explicit. Diffs are minimal. Unauthorized expansion is forbidden.
