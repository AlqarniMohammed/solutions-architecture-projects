# Role
For Outputs 1 and 2 you are writing as people inside a fictional company that needs a technical solution. None of them are AWS engineers. Outputs 3 and 4 are written outside that persona — see the role switches marked there.

# Task
Generate the raw client input for a Solutions Architecture practice project, plus three sealed artifacts.

# Step 0 — Select the domain and the level yourself (I never choose, to avoid bias)

Read `.project-context.md` in this repository — a snapshot of the framework hub's projects log, listing every previous project's domain, industry, level, score, per-pillar scores, raised dimension, and evaluation recommendation.

**Revisit rows are not projects.** `.project-context.md` may contain rows with Status `revisit` or `revisit done`. Each re-scores an *earlier* project against the same sealed reference, so for this rule they do not exist: the *previous score* that drives the ramp is the last **non-revisit** row's, and a revisit consumes no domain in the rotation. Read them for context if you like — a revisit delta says whether a weakness is actually closing — but never ramp off one, or a re-score of project 1 ends up setting project 6's difficulty.

**Level.** Project 1 is L1. From project 2, apply the ramp mechanically — this is not a judgment call:

| Previous score | This project |
|----------------|--------------|
| ≥ 85 | one level up |
| 70–84 | same level, next domain in rotation, weak pillar loaded |
| < 70 | same level, next domain in rotation, weak pillar loaded |

**The anti-plateau floor:** if this project is at the same level as the last one, you MUST raise at least one difficulty dimension — failure domain, constraint count, integration, or scale — and state which. Never deliver two consecutive projects at the same level with the same difficulty profile.

**Domain.** Rotate for breadth: no domain repeats until all six have been used ({Serverless | Containers | Data & Analytics | Networking | Migration | Hybrid}). The weakness from the previous evaluation does **not** choose the domain — it decides which aspect of the next domain in rotation gets loaded. A weak Security score produces the next domain with security-heavy requirements, not a third security project.

Also read the **Recurring weaknesses** section of `.project-context.md`: a pillar that has been lowest repeatedly gets loaded harder than one weak result would justify.

**Industry.** Do not reuse an industry, scenario, or problem shape listed in the Industry column of `.project-context.md`.

Open `requirements.md` with a clearly fenced meta block — it is framework bookkeeping, not part of the client's document:

```
> **Framework meta** — Domain: {…} | Level: L{n} | Industry: {…}
> Selected because: {…}. Raised dimension vs. last project: {…}.
```

# Output 1 — `requirements/requirements.md`

**Critical: do not pre-classify anything.** There is no section titled "Constraints". There is no section titled "Success Criteria". There is no "Functional Requirements" list and no "Non-Functional Requirements" table. Sorting the raw input into those categories is the architect's job in Stage 2 — delivering it pre-sorted deletes the exercise this project exists for.

Requirements arrive the way real requirements arrive, and how raw that is scales with the level:

- **L1** — a single stakeholder brief (a long email or a project-brief memo). Unstructured prose, but internally consistent and containing everything needed.
- **L2** — a brief **plus an email thread** with two or three voices. Numbers appear only as business statements ("we lose about thirty orders an hour when checkout is down"; "Black Friday runs roughly twelve times a normal Tuesday") — never as a table of figures. The architect derives the actual numbers.
- **L3** — **sources that disagree.** A meeting transcript where stakeholders hold conflicting priorities (a CTO who wants it fast, a CFO who wants it cheap, a Head of Ops who wants it quiet), plus a legacy system document and a compliance memo. Some requirements are only implied. One stakeholder is confidently **wrong** about how their own system currently works.
- **L4** — as L3, plus information that is missing and never volunteered, so the clarification interview is mandatory to reach a designable state — and at least one requirement stated in a document that is **contradicted** by the transcript.

Whatever the format, the raw input must contain (woven into the prose, never labelled as such): a realistic and imperfect company with legacy constraints and an unevenly skilled team; a business problem described in business terms — lost revenue, slow launches, outages, compliance pressure; enough detail to derive baseline and peak traffic, latency expectations, data volumes and growth; a realistic approximate monthly budget that is not a suspiciously round number; compliance needs, region(s), and RTO/RPO where disaster recovery matters; and measurable outcomes the business would call success.

# Quality rules

- **The obvious answer must be illegal.** At least one stated constraint must make the *most obvious* architecture for this problem non-viable. Do not flag this in `requirements.md` — record which approach was ruled out, and by which constraint, in Output 3.
- **Constraint budget by level:** L1 ≥2 binding constraints, L2 ≥3, L3 ≥4, L4 ≥5 including at least one pair in **direct conflict** (both cannot be fully satisfied; the architect must choose and defend). "Binding" means it eliminates at least one otherwise-reasonable design. A constraint that rules nothing out is set dressing, not a constraint.
- **From L2, plant at least one red herring** — a stated concern that turns out not to matter architecturally. Real requirement documents are full of them, and deciding what to ignore is a judgment skill.
- **From L2, at least one stakeholder proposes a solution instead of stating a need** — "we need a bigger database server", "our developer says we should be using Kubernetes", "can't we just put it all in one place". These must be **wrong or clearly suboptimal** for the actual problem, so they add noise to see through rather than hints to follow. Translating a proposed solution back into the underlying need is core architect work.
- **Never name a specific AWS service in Outputs 1 or 2.** Generic technology categories and non-AWS products are allowed in the stakeholder-proposal case above; an AWS service name is not, and nothing named may appear in Output 3.
- At least two genuinely viable architectures must exist, and choosing between them must involve a real trade-off.
- Leave 2–3 points ambiguous or underspecified — points that materially affect architecture decisions, so they must surface during the clarification interview.

# Output 2 — `requirements/.acceptance-tests.md` (BLACK-BOX, **SEALED until Stage 5**)

A validation plan derived from the **true** success criteria — including the ones the architect will have to derive rather than read. For each:

- What must be proven
- How to measure it (load level, response time threshold, data correctness check…)
- The pass/fail condition, with a number wherever possible

Include at least one resilience scenario in business terms ("if any single component fails, the service stays available and recovers within the stated recovery target").

**Strict rules.** This file names no AWS service and assumes no specific architecture — it describes WHAT to prove, never HOW the system is built. And it is **sealed**: clean acceptance tests sitting alongside deliberately messy requirements would let the success criteria be reverse-engineered straight out of them. It opens at Stage 5. If the architect misread the requirements, a test will prove something they never designed for — that is the intended lesson, not a flaw.

# Output 3 — `requirements/.reference-solution.md` (**SEALED until Stage 6**)

Drop the client persona and switch roles: write as an expert AWS solutions architect. Produce the complete reference architecture, containing:

- Selected services, each with a justification and at least one rejected alternative
- A written data-flow description
- How the design addresses each of the six Well-Architected pillars
- An estimated monthly cost breakdown
- Your resolution of each deliberately ambiguous point — the assumption a strong architect would most likely land on
- **The trap, named:** which obvious architecture the constraints rule out, and which constraint kills it
- **The red herring, named** (L2+): which stated concern does not actually affect the design
- **The full set of success criteria**, including any the architect would have to derive rather than read directly

# Output 4 — `requirements/.change-request.md` (**SEALED until Stage 4c**)

Still outside the client persona. Write the curveball: a realistic mid-flight change that arrives *after* the design is finished. Pick one that genuinely stresses this specific design — a budget cut of roughly 40%, a tenfold traffic projection, a new compliance regime, an acquisition forcing a second region, or the loss of the one engineer who could operate a chosen technology.

Write it as the client would send it — a short message, in business terms, apologetic but firm. Then, below a clear separator, add reviewer notes (not shown to the architect until Stage 6): what a strong response looks like, which parts of a reasonable design should absorb this change without modification, and which parts should have to change.

**Sealing rule for Outputs 2, 3 and 4:** after writing these files, never quote them, reference them, hint at their contents, or let them influence anything you say until the stage that opens each one. This includes soft leaks — steering the architect toward the reference design, or hedging in a way that signals they are off track.
