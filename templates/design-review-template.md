# Design Review — {Project Name}

> **How to use this file** — Stages 4b and 4c, after the Well-Architected answers and **before** any building starts.
> Part 1 logs the `/defend` session: the design gets attacked by a principal architect and a cost stakeholder, and I defend or revise. Part 2 logs my response to the sealed change request.
> Log the exchanges as they happen — the value is in what I could *not* answer, and that is exactly what gets tidied away if this is written afterwards from memory.
> Done when: every challenge has a verdict, every revision is reflected in `decisions.md` and the diagram, and the unanswered challenges are written down honestly. Unanswered challenges go into README section 9 — a design review with no unresolved findings usually means the review was too polite.

## Part 1 — `/defend` (Stage 4b)

### Round 1 — Principal architect
| # | Challenge | My response | Verdict (held / partial / failed) | Did I revise? |
|---|-----------|-------------|-----------------------------------|---------------|

### Round 2 — Cost stakeholder
| # | Challenge | My response | Verdict (held / partial / failed) | Did I revise? |
|---|-----------|-------------|-----------------------------------|---------------|

### What I could not defend
(Plainly. These go into README section 9, and the evaluator will look for them.)

### Revisions made
| What changed in the design | Which challenge drove it | `decisions.md` updated? | Diagram updated? |
|----------------------------|--------------------------|-------------------------|------------------|

## Part 2 — The Change Request (Stage 4c)

> Opened `requirements/.change-request.md` **after** the design was complete and reviewed. Real clients break the lock — usually right after the design is finished.

### What the client changed

### My response
(One of two: **adapt** — what changes in the design and why — or **defend** — why the design already absorbs this without modification. Hand-waving is neither.)

### What this cost
| | |
|---|---|
| Did the design absorb it, or bend? | |
| Which decision turned out to be load-bearing? | |
| Which decision would I have made differently at Stage 3, knowing this was coming? | |

### If the change landed after deployment (L3+)
| | |
|---|---|
| What had to be rebuilt rather than reconfigured? | |
| What did that reveal about how evolvable the design actually was? | |
