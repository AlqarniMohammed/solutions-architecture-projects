# Weekly Project Workflow — Start to LinkedIn Post

The complete path for one weekly project, in order. Each step says **where** it happens (framework hub or the project repo) and **who** does it (me or the AI). The project's own `checklist.md` is the box-by-box version — this file is the narrative walkthrough.

The full methodology behind every step is in [setup.md](setup.md).

---

## Step 0 — One-time setup (before project 1 only)

**Where: AWS console + this machine**

None of this repeats, and all of it blocks week 1 if skipped.

**AWS account**
1. Enable **MFA on the root user**, then put root away and stop using it.
2. Create an admin identity (IAM Identity Center, or an IAM user) and run `aws configure` against it.
3. **Pin one region** for all projects and write it down. Multi-region becomes a deliberate L3 design decision, never an accident.
4. Enable **AWS Budgets** with a monthly alert threshold (e.g. $10) — the cost guardrail exists before the first deploy ever happens.
5. Activate **`Project` as a cost-allocation tag** in Billing → Cost allocation tags. Do this now: activation takes about 24 hours to appear, and every project's estimate-vs-actual check depends on it.

**Toolchain**
6. Node LTS, then `npm i -g aws-cdk`.
7. Python 3, `pip install diagrams`, and graphviz (`sudo apt install graphviz` on WSL2) — `/diagram` fails without it.
8. `! gh auth status` (and `! gh auth login` if it fails).
9. **`cdk bootstrap aws://<account-id>/<region>`** — once per account+region. `cdk deploy` fails without it. It provisions the `CDKToolkit` stack: an assets bucket, an ECR repo, and the deployment roles. Read that stack rather than treating the command as magic — it is the first piece of real AWS knowledge this framework gives you.

## Step 1 — Kick off the project

**Where: this hub, in Claude Code | Who: the AI**

1. Run `/kickoff <short-name>` (e.g. `/kickoff serverless-orders`).
2. The AI creates a **new public GitHub repo** `project-XX-<short-name>` as a sibling directory, scaffolds it with the templates, prompts, `CLAUDE.md`, and the `.project-context.md` snapshot, pushes it, and registers it in `projects-log.md`.
3. Close this session and open Claude Code **inside the new project directory**. Everything until Step 10 happens there.

> Every 5th week is a **revisit week** instead — no new project. `/kickoff` detects it, records the revisit row in `projects-log.md`, and points you at project N-4; `/log-project` closes that row out with the re-score. The row matters: it is what advances the numbering to project N+1.

## Step 2 — Receive the requirements (Stage 1)

**Where: project repo | Who: the AI**

1. Run `/new-project`. The AI selects the domain and level per the Selection Rule (I never choose), then produces:
   - `requirements/requirements.md` — the raw client input. **Unstructured and not pre-classified** — no "Constraints" section, no "Success Criteria" list. Sorting it out is my job
   - `requirements/.acceptance-tests.md` — **SEALED until Stage 5**
   - `requirements/.reference-solution.md` — **SEALED until Stage 6**
   - `requirements/.change-request.md` — **SEALED until Stage 4c**
2. Read the `> **Framework meta**` block at the top of `requirements.md`: the domain, the level, and which difficulty dimension went up this week.
3. Confirm all four files exist and the three sealed ones stay closed. Commit: `stage 1: requirements received`.

## Step 3 — Analyze the requirements (Stage 2)

**Where: project repo | Who: ME (the AI only plays the client)**

1. Fill `analysis/requirements-restated.md` — convert the raw input into a structured spec **in my own words**, deriving the numbers the client only implied. This comes first, before any classification.
2. **L3+ (brownfield) only:** fill `analysis/current-state.md` — the existing estate and, above all, its dependencies in both directions. `analysis/disposition-7rs.md` follows in Step 4. On L1–L2 projects, delete both files.
3. Fill `analysis/qualifiers-constraints.md` — every statement lands as a Qualifier, a Constraint, an assumption, or **not a requirement** (with a reason for ignoring it).
4. Fill `analysis/assumptions.md` — **before** the interview; the high-risk rows are the interview agenda.
5. Run `/client` and interview them. **Show them the restatement and let them react to it** — that is the highest-value part of the session. Log every Q&A live in `analysis/clarification-interview.md`. The client volunteers nothing, so a question I don't ask is information I don't get. Close with **"requirements locked"** and paste the [NEW INFO] summary.
6. Revise the restatement from what the interview revealed, mark which assumptions were validated, and fill the trade-offs "Business priority" column.
7. Commit: `stage 2: analysis complete`.

## Step 4 — Design (Stages 3–4)

**Where: project repo | Who: ME (the AI only renders)**

1. **L3+ only:** fill `analysis/disposition-7rs.md` — one disposition per workload (rehost / replatform / repurchase / refactor / retire / retain / relocate), each justified with a rejected alternative, sequenced to respect the dependency graph.
2. Fill `design/decisions.md` Part 1 — one row per service: alternatives, why rejected, which requirement it serves.
3. Fill `design/decisions.md` Part 2 — answer every Well-Architected self-check question in writing. "Yes" is not an answer, and the evaluator will check each claim against the synthesized template.
4. Run `/diagram` — the AI renders exactly what I designed; confirm the PNG matches, save as `design/architecture.png`. (From project 4, alternating weeks: draw it myself in draw.io and have the AI only verify it.)
5. Do a **Pricing Calculator** estimate; record the link + line items in Part 2 under Cost Optimization. It must fit the budget constraint.
6. Commit: `stages 3-4: design complete`.

## Step 5 — Defend the design (Stage 4b)

**Where: project repo | Who: ME defending, the AI attacking**

1. Run `/defend`. Round 1 is a principal architect; Round 2 is the person who signs the bill. The AI challenges but never proposes a replacement — I defend or I revise.
2. Log every challenge, my response, and the verdict in `design/design-review.md` Part 1.
3. Write down honestly what I **could not** defend. That goes in the README, and the evaluator looks for it.
4. Push any revisions back into `decisions.md` and the diagram. Commit: `stage 4b: design reviewed`.

## Step 6 — The change request (Stage 4c)

**Where: project repo | Who: ME**

1. Now open `requirements/.change-request.md`. The client has changed their mind, right after the design was finished — as they do.
2. Either adapt the design and record the delta, or defend in writing why it already absorbs the change. Both go in `design/design-review.md` Part 2.
3. Update `decisions.md` and the diagram if anything changed. Commit: `stage 4c: change request absorbed`.

## Step 7 — Build, deploy, validate (Stage 5)

**Where: AWS console + project repo | Who: ME first, then the AI codifies**

1. **Pass 1 — Console (my hands):** build the stack manually, sanity-check the core flow, and **save screenshots to `iac/evidence/console/`**.
2. **Pass 2 — Codify:** Claude Code writes the CDK in `iac/` from `decisions.md` + the console build. Apply the `Project` tag and wire in cdk-nag.
3. `cdk synth` must be **cdk-nag clean** — zero unsuppressed errors. Every acknowledgement needs a written reason and a row in `decisions.md` Part 3.
4. **Comprehension gate:** write `iac/walkthrough.md` **from memory**, closed book — construct by construct, mapped to decisions — then diff it against the real code and record what I got wrong. Code I can't explain doesn't get deployed.
5. Commit the synthesized `template.yaml`.
6. Open `requirements/.acceptance-tests.md` (sealed since Stage 1) and run `/test-plan`. If a test proves something I never designed for, that is the Stage 2 lesson arriving — record it, don't soften the test.
7. **Destroy the console-built stack**, then `cdk deploy` fresh from code.
8. Execute the test plan in order: smoke → **security probe** → **failure injection**. The failure injection must prove both recovery within RTO *and* the alarm reaching ALARM state. Record every result inline; save every screenshot to `iac/evidence/`.
9. Run `/fault-drill` while the stack is still up and healthy: the AI breaks one thing without saying what, and I diagnose it from telemetry alone. The drill's real output isn't the diagnosis — it's discovering which alarm, log or metric I never built. Revert, re-verify health with a real smoke test, then proceed.
10. `cdk destroy` + walk the teardown verification checklist against the silent-spenders list. **No day ends with an undocumented running stack.**
11. **24 hours later:** Cost Explorer filtered to `Project=project-XX`; record the actual against the estimate in `decisions.md` Part 4, and write down why they differ.
12. Commit: `stage 5: built, validated, torn down`.

## Step 8 — Evaluation (Stage 6)

**Where: project repo | Who: the AI grades, ME answering**

1. Run `/evaluate`:
   - Read the **Step 1 independent evaluation** — scored without the reference, with every pillar claim checked against `template.yaml` — then give the explicit go-ahead.
   - Read the **Step 2 comparison** against the now-opened reference, including whether I took the bait on the obvious-but-illegal architecture.
   - Answer the **3 code-comprehension questions** honestly.
   - Receive the verdict: top 3 gaps, lessons learned, the **"next week, focus on X"** recommendation, and the paste-ready log line.
2. Open `.reference-solution.md` myself and study every difference. Where my decision is stronger, say so in the README. Commit: `stage 6: evaluated`.

## Step 9 — Document & publish (Stage 7)

**Where: project repo | Who: ME assembling, the AI drafting**

1. Fill `README.md` from the template — sections assemble from files already written; if one is hard to fill, fix the gap upstream.
2. Find one **published** AWS reference architecture or Well-Architected Lab for the same problem shape; note two differences in section 11. External ground truth for an otherwise closed loop.
3. Run `/linkedin`, pick a variant, **rewrite it in my own words**, and post it with the public repo link.
4. Commit and push: `stage 7: documented and published`.

## Step 10 — Retrospective & close out (Stage 8)

**Where: project repo, then this hub | Who: ME**

1. Fill `retrospective.md` — the 3 questions ("nothing" is not an answer for 1 and 2) plus the rough time-per-stage table.
2. If question 3 names a methodology change: make it a **standalone commit to setup.md in this hub**.
3. Final commit + push of the project repo.
4. Back in the hub, run `/log-project` — it records the level, per-pillar scores, raised dimension, and "next week, focus on X", and refreshes the Recurring Weaknesses table that drives the next `/kickoff`.
5. Push the hub. Week done.

---

## Command reference

| Command | Where | What happens |
|---------|-------|--------------|
| `/kickoff <name>` | hub | Create + push the next public project repo, fully scaffolded |
| `/log-project` | hub | Record a finished project's level, scores, and next-focus |
| `/new-project` | project repo | Generate the raw requirements + three sealed artifacts (Selection Rule applied) |
| `/client` | project repo | Client role-play for the clarification interview |
| `/diagram` | project repo | Render decisions.md into diagram.py / architecture.png |
| `/defend` | project repo | Adversarial design review before the build |
| `/test-plan` | project repo | Translate the sealed acceptance tests into an executable runbook |
| `/fault-drill` | project repo | Break one thing in the running stack; I diagnose it from telemetry |
| `/evaluate` | project repo | Scorecard evaluation, template verification, comparison, comprehension quiz |
| `/linkedin` | project repo | Two LinkedIn post structures from the README |

Plain-language asks also work (e.g. "write the CDK from my decisions" — Stage 5, Pass 2), but the boundaries in each project's `CLAUDE.md` always apply: the AI will not classify requirements, restate them for me, make architecture decisions, answer Well-Architected questions, or do the console build.
