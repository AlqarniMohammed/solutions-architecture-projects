# Current State Assessment — {Project Name}

> **How to use this file** — L3+ projects only, during Stage 2 alongside the restatement. **If this project is greenfield (L1–L2), delete this file.**
> Brownfield work starts with a different question than greenfield: not "what should exist" but "what already exists, what does it cost, who depends on it, and what is safe to touch." Getting this wrong is how migrations fail — not through bad target architecture, but through an undiscovered dependency.
> Fill it from the source material and the interview. Where the client is *wrong* about their own estate (L3 requirements plant this deliberately), record both what they said and what you concluded, because that gap is the finding.
> Done when: every workload has a row, every integration is named, and nothing in the target design depends on something whose current behaviour is still a guess.

## 1. The estate

| # | Workload / system | What it does | Where it runs | Owner | Business criticality |
|---|-------------------|--------------|---------------|-------|----------------------|

## 2. Technical profile

| # | Workload | Stack / runtime | Data store & volume | Traffic profile | Availability today |
|---|----------|-----------------|---------------------|-----------------|--------------------|

## 3. Dependencies — the part that kills migrations

> Both directions. A workload nobody depends on is easy; a workload with an undocumented upstream is where a cutover fails at 2 AM.

| # | Workload | Depends on | Depended on by | Integration style (sync / batch / file / queue) | Can it be broken temporarily? |
|---|----------|------------|----------------|--------------------------------------------------|-------------------------------|

## 4. Constraints carried over from the existing estate

> Licensing, vendor contracts, compliance boundaries the current system already sits inside, hardware end-of-life dates, a team that operates it a particular way. These are constraints on the *target* even though they describe the *source*.

| # | Constraint | Where it came from | Effect on the target design |
|---|-----------|--------------------|-----------------------------|

## 5. What the client believes that I could not confirm

> L3 requirements deliberately include a stakeholder who is wrong about their own system. Record the gap rather than silently correcting it — how you found it out matters as much as the correction.

| # | What they said | What I concluded | How I found out | Impact if they were right after all |
|---|----------------|------------------|-----------------|--------------------------------------|

## 6. What I could not discover

> In real engagements this list is never empty, and pretending otherwise is the mistake. Anything here becomes an assumption with a risk rating, or a discovery task before cutover.

| # | Unknown | Why it matters | How I would find out with more access |
|---|---------|----------------|----------------------------------------|
