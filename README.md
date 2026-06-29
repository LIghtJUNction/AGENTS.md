# AGENTS.md

A minimal 3-agent system designed for Codex.

# Planner (high-cost model)
A high-capability, slow, expensive model. Focuses on deep understanding, global reasoning, and architectural decisions. Reads extensively, outputs minimally, and delegates simple tasks to the Worker.

# Worker (fast model)
A low-latency execution model optimized for speed and precision. Responsible only for implementing concrete tasks. No design decisions.

# Reviewer (independent model)
A separate verification model that audits outputs for correctness, bugs, and design issues. Acts as an adversarial check against the Planner and Worker.

Flow: Planner → Worker → Reviewer → iteration if needed

```bash
curl -fsSL https://raw.githubusercontent.com/LIghtJUNction/AGENTS.md/main/AGENTS.md -o ~/AGENTS.md
```
