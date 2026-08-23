---
description: Record a finished project's domain, level, per-pillar scores, and next-week focus in projects-log.md (also closes out revisit weeks)
argument-hint: [project repo name, e.g. project-01-serverless-orders — optional if only one is open]
---

Close out a finished weekly project by completing its row in `projects-log.md` — or close out a revisit week, which has its own path at the bottom of this file.

## Steps

1. **Locate the work being closed out.** Use `$ARGUMENTS` if given; otherwise take the most recent incomplete row in `projects-log.md` — Status `kicked off` **or** Status `revisit`.

   **If it is a `revisit` row, stop and follow the Revisit close-out section at the bottom instead.** A revisit week has no repo of its own and none of the steps below apply to it.

   Otherwise the repo is a sibling directory of this hub (`../project-XX-name`). If the directory doesn't exist locally, ask the user where it is.

2. **Extract the results.** The evaluator's Step 4 verdict ends with a paste-ready line in exactly this format — find it first, since it carries most of what you need:

   ```
   Pillars: OE{n} SE{n} RE{n} PE{n} CO{n} SU{n} | Total: {n} | Beat-reference: {n} | Focus: {…}
   ```

   If it's missing, fall back to `README.md` section 10 (the scorecard table) and the evaluation output, and tell the user the evaluator didn't emit the log line. Also collect:
   - **Domain, Level, Industry** — from the `> **Framework meta**` block at the top of `requirements/requirements.md`
   - **Raised dimension** — from that same meta block

3. **Update the project's row:** fill Domain, Industry, Level, Score, Pillars, Raised dimension, Beat ref, Next-week focus; set Status to `done`.

4. **Refresh the Recurring Weaknesses section.** Recompute across every completed project: for each pillar, how many times it was the lowest-scoring one, its average, and whether it's improving or not. Update the **Current standing weakness** line. This is what prompt 01 reads to decide how hard to load the next project — a stale table quietly breaks the Selection Rule.

   **Count projects only — skip `revisit` rows.** A revisit re-scores an earlier project against the same sealed reference, so folding it in would count that project's pillars twice and pull the averages toward whichever project happened to be revisited.

5. **Refresh the Domain Rotation table** with this project's domain. **A `revisit` row never touches this table** — it re-architects a project whose domain is already recorded, so it consumes no new domain.

6. **Check what the next project will be, and warn if the hub isn't ready.** Apply the ramp rule to the last **non-revisit** row's score (≥85 → level up; 70–84 or below → same level, next domain, raised dimension required):
   - **If the next project will be L3**, it is brownfield by definition — remind the user that `analysis/current-state.md` and `analysis/disposition-7rs.md` become live artifacts for the first time, and that discovery work (dependencies in both directions) now precedes the design.
   - **If the next row number N is a multiple of 5**, remind the user it's a **revisit week**: no new project, re-architect project N-4 on a `revisit-w{N}` branch in its own repo using `templates/revisit-template.md`. `/kickoff` handles this and writes the revisit row itself. Note that row numbers are week numbers, so they carry a gap at every revisit — there is no `project-05`.

7. **Sanity checks — warn about anything unfinished:**
   - Unchecked boxes left in the project's `checklist.md`
   - Any Stack Log row without a completed teardown (cost guardrail)
   - `decisions.md` Part 4 missing the T+24h actual cost — the estimate-vs-actual loop is the point, and a blank actual means it never closed
   - `retrospective.md` question 3: if it names a methodology change, remind the user it must become a standalone commit to `setup.md` here in the hub

8. **Commit the hub change** as `chore: log project XX results`.

The Next-week focus and the per-pillar scores recorded here drive the next `/kickoff` + `/new-project` selection. Never leave them blank on a done project.

## Revisit close-out

Every 5th week is a revisit, and `/kickoff` has already written its row. There is no project repo — the work sits on the `revisit-w{N}` branch of project N-4.

1. **Read `revisit.md` on that branch**: the original score, the re-score, and the delta.
2. **Complete the revisit row:** Score = the re-score, Pillars = the re-scored pillars, Next-week focus = what the revisit records as *still* not visible. Status `revisit done`. Leave Raised dimension and Beat ref as `—` — neither applies to a re-architecture.
3. **Do not touch Recurring Weaknesses or Domain Rotation**, per steps 4 and 5. A revisit is not a project.
4. **Report the delta plainly** — up, down, or flat. It is the only direct evidence in this framework that judgment improved rather than just accumulating projects, so it gets said out loud, not buried in a table.
5. **Say which row drives the next project.** The ramp still reads the last non-revisit row, so name it explicitly — the next `/kickoff` will be project N+1 at whatever level that row implies.
6. **Commit the hub change** as `chore: log revisit week N`.
