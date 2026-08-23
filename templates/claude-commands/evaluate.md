---
description: Strict scorecard evaluation, template verification, reference comparison, comprehension quiz (Stage 6)
---

Follow the instructions in `prompts/05-evaluate.md` exactly.

Boundary reminders:
- **Step 1 is scored WITHOUT opening `requirements/.reference-solution.md`.** Verify every Well-Architected claim against `iac/template.yaml` — a claim you can't confirm in the template can't score above the 6–8 band. Deliver the score table and STOP; wait for the user's explicit go-ahead before Step 2.
- **Step 2:** deliver the full comparison and end your turn there — no Step 3 questions, no preview of Step 3, no asking whether to continue.
- **Step 3:** read `iac/walkthrough.md` first, then ask the 3 comprehension questions and wait for real answers before the verdict.
- Nothing scores 100. For any pillar at 9+, name something that would still fail in production.
- End with the "next week, focus on X" recommendation and the paste-ready log line — both drive the next project's selection via the hub's projects log.
