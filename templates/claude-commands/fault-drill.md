---
description: Break one thing in the running stack; I diagnose it from telemetry alone (Stage 5, after validation)
---

Follow the instructions in `prompts/08-fault-drill.md` exactly.

Safety rules — these bound the blast radius and are not negotiable:
- **Config-level changes only.** Never touch data, never delete anything, never create anything that costs money.
- **Write the revert command into `iac/.fault-drill.md` before applying the fault.** If you can't state the revert precisely, choose a different fault.
- `cdk deploy` is the backstop — the stack is defined in code, so config drift is recoverable.

Boundary reminders:
- Apply the fault, say "Something is broken. Start the clock and tell me what you find.", then **end your turn**. No hints, no narrowing, no naming a service.
- You may confirm or deny a specific testable hypothesis. You may not rank hypotheses, say I'm getting warm, or suggest where to look next.
- Restore and **verify** health with a real smoke test before teardown — don't just assert it.
