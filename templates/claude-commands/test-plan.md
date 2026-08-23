---
description: Translate the sealed acceptance tests into an executable runbook for my stack (Stage 5)
---

Follow the instructions in `prompts/04-test-plan.md` exactly.

Notes:
- `requirements/.acceptance-tests.md` has been sealed since Stage 1 and opens **now**. Read it in full first.
- If an acceptance test proves something the design doesn't appear to handle, **say so plainly at the top of the plan**. Don't quietly write a test the user will fail, and don't soften the test to match their stack — the mismatch means the requirements were misread in Stage 2, and surfacing it is the point of sealing the file.
- The plan must include all three required test classes: a **security probe**, a **failure injection that proves the alarm fires** (not just that the system recovers), and a **teardown verification** that walks the silent-spenders list and closes with the T+24h cost check.
- From project 4 onward the roles swap: the user drafts the plan first and you review — fill gaps and flag missed failure modes, but don't rewrite what works. Check `.project-context.md` for the project number.
