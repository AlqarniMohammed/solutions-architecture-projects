# Projects Log

> The cross-project memory of this framework. One row per weekly project, in order.
> `/kickoff` adds the row and snapshots this file into the new repo as `.project-context.md` (the Selection Rule's input); `/log-project` completes the row when the project finishes.
> Revisit weeks get a row too — see the Status note below. It is what advances the numbering past every 5th week.
> **Never leave a done project's row incomplete** — the Level, Score, and Next-week focus are what the next `/kickoff` reads to decide how hard the next project is.

## Projects

| # | Repo | Domain | Industry | Level | Score | Pillars (OE/SE/RE/PE/CO/SU) | Raised dimension | Beat ref | Next-week focus | Status |
|---|------|--------|----------|-------|-------|------------------------------|------------------|----------|-----------------|--------|

**Column notes**
- **Industry** — the scenario's sector. Prompt 01 reads this to avoid repeating a problem shape; without it the anti-repetition rule cannot be enforced.
- **Level** — L1–L4 per the difficulty ladder in [setup.md](setup.md#the-difficulty-ladder). Not a label — each level has defined constraint, failure-domain, and integration properties.
- **Pillars** — per-pillar scores as `OE8 SE6 RE5 PE7 CO9 SU7`. The total alone cannot show that Security has been weak for four straight weeks; this column can.
- **Raised dimension** — which difficulty dimension went up versus the previous project. The anti-plateau floor requires one whenever the level repeats. A blank here on a repeated level means a wasted week.
- **Beat ref** — how many decisions the evaluator judged stronger than the reference solution. A growth signal that would otherwise be discarded.
- **Status** — `kicked off` → `done` for a project. Every 5th row is a **revisit week** instead: `revisit` → `revisit done`, written by `/kickoff` and completed by `/log-project`, with the Repo cell pointing at project N-4's `revisit-w{N}` branch.

**Revisit rows are not projects.** They exist for two reasons: the row advances the numbering (without it `/kickoff` recomputes the same multiple of 5 forever and the ladder deadlocks), and the score delta belongs in the cross-project memory. But they are **excluded** from the Recurring Weaknesses recomputation, from the Domain Rotation table, and from the Selection Rule's ramp — which always reads the last non-revisit row. A revisit re-architects a project already counted; counting it twice would distort every average the next project's difficulty is chosen from.

## Recurring Weaknesses

> Maintained by `/log-project` across all completed projects. This is what makes the loop compound: one bad Security score is noise, four in a row is a curriculum item.
> Prompt 01 reads this section — a pillar that is repeatedly lowest gets loaded harder than a single weak result would justify.

| Pillar | Times lowest | Average | Trend |
|--------|--------------|---------|-------|

**Current standing weakness:** _(none yet — no projects completed)_

## Domain Rotation

> Breadth is guaranteed by rotation, not by the weakness recommendation: no domain repeats until all six have been used. The weakness decides which *aspect* of the next domain gets loaded.

| Domain | Used in project |
|--------|-----------------|
| Serverless | — |
| Containers | — |
| Data & Analytics | — |
| Networking | — |
| Migration | — |
| Hybrid | — |
