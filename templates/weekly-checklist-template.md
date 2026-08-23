# Weekly Checklist — {Project Name}

> **How to use this file** — Placed in the project repo at kickoff as `checklist.md`, and it drives the whole week.
> Work strictly top to bottom; each box gates the next. If a box can't be checked, that IS the current task — don't skip ahead.
> Done when: every box is checked. That is the definition of a finished project in this repository.

## One-Time Setup — project 1 only

> None of this repeats. All of it blocks week 1 if skipped — `cdk deploy` fails without a bootstrapped account, and `/diagram` fails without graphviz.

**AWS account**
- [ ] MFA enabled on the root user; root credentials put away and not used again
- [ ] Admin identity created (IAM Identity Center, or an IAM user) and `aws configure` run against it
- [ ] One region pinned for all projects — record it here: `________`. Multi-region becomes a deliberate L3 design decision, never an accident
- [ ] AWS Budgets alert enabled at a fixed monthly threshold (e.g. $10)
- [ ] `Project` activated as a **cost-allocation tag** in Billing → Cost allocation tags. *Do this now: activation takes ~24h to appear, so it must precede the first deploy*

**Toolchain**
- [ ] Node LTS installed, then `npm i -g aws-cdk`
- [ ] Python 3 + `pip install diagrams` + graphviz installed (`sudo apt install graphviz` on WSL2) — needed by `/diagram`
- [ ] `gh auth status` passes
- [ ] `cdk bootstrap aws://<account-id>/<region>` — once per account+region. This provisions the `CDKToolkit` stack: the assets bucket, the ECR repo, and the deployment roles CDK needs. Worth reading the created stack rather than treating it as a magic command

## Kickoff
- [ ] This repo was created by `/kickoff` from the framework hub: standard structure present, templates copied in, `CLAUDE.md` and `.project-context.md` in place
- [ ] Run `/new-project` — the AI selects domain + level per the Selection Rule; I never choose
- [ ] Confirm all four outputs exist, and that the three sealed files stay closed: `.acceptance-tests.md`, `.reference-solution.md`, `.change-request.md`
- [ ] Note the level and the raised difficulty dimension from the meta block at the top of `requirements.md`

## Stage 2 — Analysis

> The requirements arrived unstructured and nothing is pre-classified. Sorting that out is the job.

- [ ] Fill `requirements-restated.md` — the raw input converted into a structured spec **in my own words**, before classifying anything
- [ ] **L3+ only:** fill `analysis/current-state.md` — the estate, and above all the dependencies in both directions. On L1–L2 delete this file and `disposition-7rs.md`
- [ ] Fill `qualifiers-constraints.md` — every statement lands as a Qualifier, a Constraint, an assumption, or **not a requirement** (with a reason)
- [ ] Write `assumptions.md` (before the interview, not after)
- [ ] Run `/client`: conduct the interview, **show the client my restatement and have them react to it**, close with "requirements locked", paste the summary into the interview log
- [ ] Mark which assumptions got validated
- [ ] Revise `requirements-restated.md` from what the interview revealed — Part 7 records what I had misread
- [ ] Fill the trade-offs table's "Business priority" column from the interview answers

## Stages 3–4 — Design
- [ ] **L3+ only:** fill `analysis/disposition-7rs.md` — one disposition per workload with a rejected alternative, plus sequencing that respects the dependency graph
- [ ] Fill `decisions.md` Part 1: every service justified, each with a rejected alternative, tied to a requirement
- [ ] Fill `decisions.md` Part 2: every Well-Architected self-check question answered in writing, each claim pointing at a mechanism
- [ ] Run `/diagram`, confirm the render matches my design exactly, save `architecture.png`
- [ ] Pricing Calculator estimate (link + line items) recorded in `decisions.md` Part 2 — is it inside the budget constraint?

## Stage 4b — Design Review
- [ ] Run `/defend`: survive the principal architect round, then the cost stakeholder round
- [ ] Log every challenge, my response, and the verdict in `design-review.md` Part 1
- [ ] Write down honestly what I could **not** defend
- [ ] Any revisions reflected back into `decisions.md` and the diagram

## Stage 4c — The Change Request
- [ ] Open `requirements/.change-request.md` — *only now, with the design finished and reviewed*
- [ ] Adapt the design, or defend in writing why it already absorbs the change — recorded in `design-review.md` Part 2
- [ ] `decisions.md` and the diagram updated if the design changed

## Stage 5 — Build & Validate
- [ ] **Pass 1:** build the stack manually in the console, sanity-check the core flow
- [ ] Screenshots of the console build saved to `iac/evidence/console/` — this layer leaves no trace if skipped
- [ ] **Pass 2:** Claude Code writes the CDK from `decisions.md` + the console build
- [ ] `Tags.of(app).add('Project', '{Project Name}')` applied — no tag, no cost attribution
- [ ] cdk-nag wired in: `Validations.of(app).addPlugins(new AwsSolutionsChecks(app))`
- [ ] `cdk synth` produces **zero unsuppressed errors**; every acknowledgement has a written reason and a row in `decisions.md` Part 3
- [ ] **Comprehension gate:** `iac/walkthrough.md` written **from memory**, closed book — then diffed against the real code, with what I got wrong recorded
- [ ] Commit the synthesized `template.yaml`
- [ ] Open `requirements/.acceptance-tests.md` (sealed since Stage 1) and run `/test-plan`; review the runbook before deploying
- [ ] If a test proves something I never designed for — that is the Stage 2 lesson arriving. Record it; don't quietly soften the test
- [ ] Destroy the console build, deploy fresh from CDK
- [ ] Execute the tests in order: smoke → **security probe** → **failure injection**, recording each result inline
- [ ] Failure injection proves **both** recovery within RTO **and** the alarm reaching ALARM state — screenshot of the alarm firing saved to `iac/evidence/`
- [ ] Run `/fault-drill` — the AI breaks one thing; diagnose it from logs, metrics and alarms alone. Record time-to-diagnosis and, more importantly, **which signal was missing** that would have made it fast
- [ ] Fault reverted and stack health re-verified with a real smoke test (not just asserted) before teardown
- [ ] `cdk destroy` + walk the teardown verification checklist against the silent-spenders list
- [ ] **T+24h:** Cost Explorer filtered to `Project={Project Name}`; actual recorded against the estimate in `decisions.md` Part 4, with why they differ

## Stages 6–8 — Evaluate, Publish, Reflect
- [ ] Run `/evaluate` — read the Step 1 independent evaluation, then give the go-ahead for the comparison, and answer the comprehension quiz honestly
- [ ] Open `.reference-solution.md` and study every difference — including whether I took the bait on the obvious-but-illegal architecture
- [ ] Find one published AWS reference architecture or Well-Architected Lab for the same problem shape; note two differences in README section 11
- [ ] Fill the README from the template
- [ ] Run `/linkedin`, **rewrite the chosen variant in my own words**, publish
- [ ] Retrospective: fill `retrospective.md` including the time table; commit any methodology change to `setup.md` in the hub
- [ ] Back in the framework hub: run `/log-project` so the projects log records the level, per-pillar scores, and next-focus

## Stack Log
> Per the Cost Guardrails hard rule: any stack still alive at the end of a working day gets a row here, with its teardown date.

| Date | Stack | Why it's still up | Planned teardown |
|------|-------|-------------------|------------------|
