# Weekly Solutions Architecture Projects — Framework

This document defines the process every weekly project follows, from receiving a client's messy requirements to publishing the result.

**The standing principle:** the architect classifies, derives, and decides. Anywhere this framework hands over a pre-digested answer, that is a bug — not a convenience. Requirements arrive the way they arrive in real work: unstructured, incomplete, and occasionally wrong.

## Repository Standards

- **Repository model:** this repository is the *framework hub* — it holds `prompts/`, `templates/`, the [WORKFLOW.md](WORKFLOW.md) walkthrough, and [projects-log.md](projects-log.md). **Each weekly project lives in its own public GitHub repo**, created and scaffolded from the hub via `/kickoff`. `projects-log.md` is the cross-project memory the Selection Rule reads — a snapshot is copied into every new project repo as `.project-context.md`, and `/log-project` writes results back when a project finishes
- Every project is time-boxed to one week — no exceptions
- Projects rotate across domains (Serverless, Containers, Data & Analytics, Networking, Migration, Hybrid) so coverage is broad, not repetitive
- **I never pick the domain or the difficulty** — see the Selection Rule below. Exactly like real client work: the client never asks what I would prefer to build

### The Difficulty Ladder

"Beginner / Intermediate / Advanced" are vibes, not levels. Difficulty here comes from **constraints**, not from diagram size — a four-service design under a hard compliance boundary is harder than a twelve-service greenfield sprawl. Four levels, defined by observable properties:

| | Scope | Failure domain | Constraints | Integration |
|---|-------|----------------|-------------|-------------|
| **L1** | greenfield, ≤5 services | single region, AZ loss tolerable | generous budget, no compliance regime | none |
| **L2** | greenfield, 5–8 services | multi-AZ required, stated RTO/RPO | binding budget, 1 compliance regime | one external system |
| **L3** | brownfield, 8–12 services | multi-region or strict DR | 2+ compliance regimes, plus a hard organizational constraint ("the team cannot operate X") | migrate from / coexist with a live system |
| **L4** | as L3 | as L3 | **requirements that cannot all be satisfied at once** | as L3 |

L4 is the top of the ladder: some requirement has to lose, and the work is choosing which one and defending the choice. Most architects never practise this deliberately.

### Selection Rule

I never choose the domain or the level. Project 1 is **L1** — enough to learn the loop end to end. From project 2 the rule runs mechanically, with no discretion:

| Last score | Next project |
|------------|--------------|
| **≥ 85** | one level up |
| **70–84** | same level, next domain, weak pillar loaded |
| **< 70** | same level, next domain, weak pillar loaded — and the retrospective must name the study gap |

**The anti-plateau floor:** never two consecutive projects at the same level without raising at least one difficulty dimension (failure domain, constraint count, integration, or scale). The raised dimension is recorded in `projects-log.md`. A level repeated without a raised dimension is a wasted week.

**Breadth and weakness both get served.** These pull against each other, so the rule separates them:

> The **domain** rotates for breadth — no domain repeats until all six have been used. The **weakness** decides which aspect of that domain gets loaded. A weak Security score does not produce another security project; it produces the next domain in rotation *with* security-heavy requirements.

### Revisit Weeks

**Every 5th week is a revisit — no new project.** Re-architect the project from four weeks ago, working from its original `requirements.md`, on a `revisit-wN` branch **in that project's own repo** — so the comparison is a literal `git diff` of `design/decisions.md`.

Do not re-read the original decisions until the new design is finished. Then re-run `/evaluate` against the same sealed reference and the same scorecard. **The score delta is the measurement** — it is the only direct evidence in this framework that judgment actually improved, rather than just accumulating projects.

**A revisit still gets a row in `projects-log.md`.** `/kickoff` writes it on detecting the week; `/log-project` completes it with the re-score. It earns its place twice over: the row is what advances the project numbering — without it, `/kickoff` recomputes the same multiple of 5 every week and the ladder deadlocks — and the delta belongs in the cross-project memory alongside everything else.

**But a revisit row is not a project.** It is excluded from the Selection Rule's ramp (the *previous score* is always the last non-revisit row's) and from the domain rotation (it re-architects an existing project, so it consumes no new domain). Folding it in would let a re-score of project 1 decide project 6's difficulty, and would burn a domain slot on a domain already used.

### When A Week Runs Short

No hour budgets — the week is measured by what gets finished, not by time spent. But when something has to give, the order is fixed:

> **Cut in this order, and never past the line:** project scope first (fewer requirements, simpler failure injection) → the LinkedIn post → console-build depth.
>
> **Never cut:** the comprehension gate, executing the test plan with evidence, the change-request response, `/evaluate`, the retrospective, `/log-project`.

A week that will not fit means a **smaller project next week** — not a skipped evaluation. The stages most tempting to drop under pressure are precisely the ones that make the following week better.

**No parallel projects.** An unfinished project is finished before a new one is kicked off.

### Project Repository Structure

Every project repo starts from this exact structure, created by `/kickoff` — no file is improvised mid-project. Every template opens with a docstring explaining when to fill it, how, and what "done" looks like:

```
project-XX-{name}/                 (its own public GitHub repo)
├── CLAUDE.md                      (the AI boundaries — loaded automatically every session)
├── .gitignore                     (CDK / Python / Node ignores)
├── .project-context.md            (snapshot of the hub's projects-log.md — Selection Rule input)
├── prompts/                       (copied from the hub, so the repo is self-contained)
├── .claude/commands/              (/new-project /client /diagram /defend /test-plan /fault-drill /evaluate /linkedin)
├── checklist.md                   (from template — drives the whole week)
├── requirements/
│   ├── requirements.md            (generated — prompt 01; unstructured, NOT pre-classified)
│   ├── .acceptance-tests.md       (generated — prompt 01; SEALED until Stage 5)
│   ├── .reference-solution.md     (generated — prompt 01; SEALED until Stage 6)
│   └── .change-request.md         (generated — prompt 01; SEALED until Stage 4c)
├── analysis/
│   ├── requirements-restated.md   (from template — MY structured reading of the raw input)
│   ├── current-state.md           (from template — L3+ only; deleted on greenfield projects)
│   ├── disposition-7rs.md         (from template — L3+ only; deleted on greenfield projects)
│   ├── qualifiers-constraints.md  (from template)
│   ├── assumptions.md             (from template)
│   └── clarification-interview.md (from template)
├── design/
│   ├── decisions.md               (from template — decisions + Well-Architected answers)
│   ├── design-review.md           (from template — /defend challenges + change-request response)
│   ├── diagram.py                 (generated — prompt 03)
│   └── architecture.png           (rendered from diagram.py)
├── iac/
│   ├── ...                        (CDK app — written by Claude Code, gated by my review)
│   ├── template.yaml              (synthesized via cdk synth)
│   ├── walkthrough.md             (written from memory BEFORE deploy — the comprehension gate)
│   ├── .fault-drill.md            (sealed — what the AI broke; opened at the debrief)
│   ├── test-plan.md               (generated — prompt 04; results recorded inline during execution)
│   └── evidence/
│       ├── console/               (proof the Pass 1 console build actually happened)
│       └── ...                    (screenshot checkpoints from the test plan)
├── retrospective.md               (from template)
└── README.md                      (from template)
```

**Three sealed artifacts.** `.acceptance-tests.md`, `.reference-solution.md`, and `.change-request.md` are all written in Stage 1 and all stay closed until their stage. Opening one early does not shortcut the week — it deletes the exercise.

### Cost Guardrails

- Before the very first deploy: enable **AWS Budgets** with an alert at a fixed monthly threshold (e.g., $10), and **activate `Project` as a cost-allocation tag** in Billing. Tag activation takes ~24h to appear, so it must happen during one-time setup, not on deploy day
- **Every stack tags itself.** One line in the CDK app: `Tags.of(app).add('Project', 'project-XX-<name>')`. Non-negotiable, exactly like teardown — without it, Cost Explorer cannot answer "what did project 02 actually cost?" and the estimate-vs-actual loop below is impossible
- **Estimate → actual → variance.** The Pricing Calculator estimate goes in `decisions.md` before the build. **24 hours after teardown**, check Cost Explorer filtered to `Project=project-XX` and record the actual alongside it, with a written note on *why* they differ. The variance is the lesson — it is where you discover you forgot NAT hourly charges, cross-AZ data transfer, or per-request pricing
- **Silent spenders watch list** — services that quietly bill when forgotten: NAT Gateways, running RDS/Aurora instances, unattached Elastic IPs, orphaned EBS volumes, CloudWatch Logs without a retention policy, and cross-AZ data transfer. The teardown checklist walks this list explicitly
- **Hard rule:** no deploy day ends without the stack either fully destroyed, or its continued existence documented with a teardown date — as a row in the Stack Log at the bottom of `checklist.md`

## Division of Labor — AI Boundaries

Every helper in this framework exists to make me do *more* architect work, not less.

**The boundary list lives in [templates/CLAUDE-md-template.md](templates/CLAUDE-md-template.md)**, which `/kickoff` installs as `CLAUDE.md` at the root of every project repo — so it loads into every Claude Code session in that repo automatically, instead of sitting in a document the session may never read. It is not restated here; a rule kept in two places drifts into two different rules.

The short version: **the AI plays the client, renders, challenges, and grades. Every architecture decision is mine.** Requirements analysis, service selection, rejected alternatives, Well-Architected answers, and the Pass 1 console build are mine and cannot be delegated, even casually, mid-session.

## Stage 1: Business Requirements

Every project starts with a fictional company in a realistic scenario — and their requirements arrive **the way real requirements arrive**, not as a tidy specification.

**Nothing is pre-classified.** There is no section headed "Constraints" and none headed "Success Criteria". Functional and non-functional requirements are mixed together in prose. Numbers show up as business statements ("we lose about thirty orders an hour when it's down") rather than as an NFR table. Sorting all of that out is Stage 2 — it is the job, and handing it over pre-sorted would delete the exercise.

How raw the input is scales with the level:

| | How the requirements arrive |
|---|---|
| **L1** | A single stakeholder brief. Unstructured, but internally consistent and complete |
| **L2** | Brief plus an email thread — multiple voices; the numbers must be derived from business statements |
| **L3** | Sources that **disagree**: a meeting transcript with conflicting stakeholder priorities, a legacy system doc, a compliance memo. Some requirements are only implied, and one stakeholder is wrong about their own system |
| **L4** | As L3, plus information that is missing and never volunteered — the interview becomes mandatory to reach a designable state — and at least one requirement contradicted between sources |

Two rules keep the problem honest:

- **The obvious answer must be illegal.** At least one stated constraint makes the most obvious architecture for the problem non-viable. Which obvious approach was ruled out, and by which constraint, is recorded in the sealed reference — so at Stage 6 I find out whether I took the bait
- **Constraint budget by level:** L1 ≥2 binding constraints, L2 ≥3, L3 ≥4, L4 ≥5 including at least one pair in direct conflict. "Binding" means it eliminates at least one otherwise-reasonable design. From L2 the requirements also contain at least one deliberate red herring — a stated concern that turns out not to matter architecturally

Stage 1 produces four files. Three are sealed:

- `requirements/requirements.md` — the raw client input
- `requirements/.acceptance-tests.md` — **SEALED until Stage 5.** A black-box validation plan derived from the true success criteria, naming no AWS service. It is sealed because clean acceptance tests alongside messy requirements would let the success criteria be reverse-engineered straight out of them. The consequence is deliberate: **misread the requirements and I find out at Stage 5, when a test proves something I never designed for**
- `requirements/.reference-solution.md` — **SEALED until Stage 6.** The expected reference architecture. Seeing it early biases the design and defeats the whole purpose
- `requirements/.change-request.md` — **SEALED until Stage 4c.** The curveball (see Stage 4c)

## Stage 2: Requirements Analysis

Four artifacts, in this order. The order matters — restating before classifying is what forces a real reading.

1. **`analysis/requirements-restated.md`** — I convert the raw stakeholder input into a structured specification **in my own words**: functional requirements, non-functional requirements with numbers I derived, constraints, and measurable success criteria. This is a real consulting deliverable, not an exercise — it is what you send a client to say "here is what I heard." It is also scored: the evaluator knows the true underlying requirements, so anything I failed to extract is a measured miss rather than an invisible one
2. **`analysis/qualifiers-constraints.md`** — classify every statement: a **Qualifier** (shapes which architectures are viable), a **Constraint** (a hard boundary), an **assumption** (I am inferring it — goes to assumptions.md), or **not a requirement at all** (noise, a preference, a solution in disguise, a red herring). That fourth bucket is deliberate: not everything a stakeholder says is load-bearing, and deciding what to *ignore* is a judgment skill
3. **`analysis/assumptions.md`** — written **before** the interview. The high-risk rows are the interview agenda
4. **`analysis/clarification-interview.md`** — the full Q&A log, conducted with the AI in the client role via `/client`. The interview validates the restatement directly: "here is what I understood — correct me." Close with **"requirements locked"** and paste the [NEW INFO] summary

**From L3 the project is brownfield**, which starts from a different question than greenfield: not "what should exist" but "what already exists, what depends on it, and what is safe to touch." Two further artifacts apply, and only at L3+:

- `analysis/current-state.md` — the existing estate: workloads, technical profile, **dependencies in both directions**, constraints inherited from the source, and what the client believes that I could not confirm. Migrations fail on undiscovered dependencies far more often than on bad target architecture
- `analysis/disposition-7rs.md` — one disposition per workload (rehost / replatform / repurchase / refactor / retire / retain / relocate), each with a justification and a rejected alternative, plus the sequencing that respects the dependency graph. "Migrate it" is not a disposition, and refactoring everything because it is more interesting is the absence of a decision

On L1–L2 projects both files are deleted at kickoff.

Then return to the restatement and revise it from what the interview revealed. By the end of this stage I have a clear and complete picture — one I built, not one I was given.

## Stage 3: Initial Design

I select the services and sketch the shape of the stack:

- Every selected service gets a row in `design/decisions.md` Part 1: why this service, what were the alternatives, why were they rejected, and which requirement does it serve. A decision without a rejected alternative is not a decision — it is a habit
- The architecture diagram is rendered from **my** decisions via `/diagram`, using official AWS service icons. The AI draws exactly what I designed — it never adds, removes, or "improves" a component

## Stage 4: Well-Architected Review

Listing pillars is theory. Each pillar comes with fixed **self-check questions**, and my written answers go into `design/decisions.md` Part 2. A pillar counts as applied only when I can answer its questions in writing, pointing at specific mechanisms in my design. **"Yes" is not an answer.** The evaluator scores each pillar against these same questions — and verifies my claims against the synthesized template, not just against my prose.

### Operational Excellence
- If this breaks at 3 AM, how do I find out before the client does? Which alarms, on which metrics? **And prove it — the failure-injection test in Stage 5 must show that alarm actually firing.**
- Can I answer at any moment: what is deployed, is it healthy, and what changed last?
- Is everything reproducible through IaC — zero manual console changes surviving into the final stack?

### Security
- Can I justify every IAM permission? Any `*` in actions or resources fails this pillar.
- Is data encrypted at rest and in transit — everywhere it lives and everywhere it moves?
- Is everything private by default, with public exposure only where a requirement demands it?
- Are secrets in a secrets store — never in code, env files, or the template?

### Reliability
- Walk the diagram component by component: if this one dies, does the system stay up?
- Does recovery meet the stated RTO/RPO?
- Does it absorb the stated peak traffic without manual intervention?

### Performance Efficiency
- Did I size and choose each resource from the stated numbers (traffic, latency, data volume) — or from habit?
- Where is the first bottleneck at 10x the stated load, and why is that acceptable?

### Cost Optimization
- Does the Pricing Calculator estimate fit inside the stated budget constraint? (Record calculator link plus line items here; Stage 5 adds the actual, and Stage 7 copies both into README section 6)
- What am I paying for while the system is idle — and can any of it scale to zero?
- Is anything over-provisioned "just in case" without a requirement behind it?

### Sustainability
- Does anything run at fixed capacity while its utilization is low? (This is the core sustainability smell)
- Could a managed or serverless option that scales to zero replace an always-on component?
- Is there a lifecycle policy for data (tiering / expiry), or does everything sit in hot storage forever?

## Stage 4b: Design Review — Defense

Before a single resource is built, the design gets attacked. `/defend` puts two adversaries in front of me in sequence: a principal architect (single points of failure, scaling ceilings, operational burden, security holes) and a cost-focused stakeholder (why are we paying for this, what scales to zero, what is the cheaper design and what do we lose).

**The reviewer challenges; it never proposes a replacement architecture and never answers its own challenge.** I defend or I revise. Every exchange is logged in `design/design-review.md`: the challenge, my response, whether I revised, and what changed.

This exists because defending a live design under pressure is most of what the job actually is — and because the evaluator's version of this at Stage 6 arrives too late to change anything. Challenges I could not answer convincingly go into the README's lessons learned.

## Stage 4c: The Change Request

Now open `requirements/.change-request.md`.

The client has changed their mind. A budget cut, a tenfold traffic projection, a new compliance regime, an acquisition that forces multi-region, the one engineer who could operate the container platform handing in their notice. Requirements being "locked" and never disturbed again is the least realistic thing this framework could do — real clients break the lock, usually right after the design is finished.

I either **adapt the design** and record the delta in `decisions.md`, or **defend in writing** why the design already absorbs it. Both go in `design/design-review.md`, and the evaluator scores the response.

At L1–L2 the change request lands here: after the design, before the build — it costs a redesign, not a rebuild. **From L3 it may instead land after deployment**, which is where evolutionary architecture is actually learned.

## Stage 5: Implementation & Deployment

A two-pass build — console first, code second.

**Pass 1 — Console build (my hands).** I build the stack manually in the AWS console first. Clicking through every service, setting, and connection is where an architect learns what the knobs actually do — deeper than any amount of reading generated code. **Capture screenshots into `iac/evidence/console/`**: this is the layer that leaves no trace if skipped, and it is the easiest thing to quietly drop when the week gets tight, so it gets evidence like everything else.

**Pass 2 — Codify (Claude Code's hands, my review).** Claude Code writes the CDK from `design/decisions.md` and the console build. `cdk synth` generates the CloudFormation template, so nothing is written twice.

**The comprehension gate — closed book, before deploy.** Code I cannot explain does not get deployed, and I do not get to grade that claim while looking at the code:

> Write `iac/walkthrough.md` **from memory** — construct by construct, each mapped to the decision it implements. *Then* diff it against the actual code and record what I got wrong. The diff is the learning artifact, and the evaluator reads it.

**Automated review before deploy.** Wire `cdk-nag`'s AWS Solutions rule pack into the app. Self-check questions can only catch what I already know to look for; cdk-nag surfaces the rest, and every finding carries a rule ID I can go read:

```ts
import { App, Validations } from 'aws-cdk-lib';
import { AwsSolutionsChecks } from 'cdk-nag';

const app = new App();
new MyStack(app, 'project-XX');
Validations.of(app).addPlugins(new AwsSolutionsChecks(app));
```

`cdk synth` must produce **zero unsuppressed errors** before `cdk deploy`. Anything acknowledged needs a written reason and a row in `decisions.md`:

```ts
Validations.of(bucket).acknowledge({
  id: 'AwsSolutions-S1',
  reason: 'Access logging omitted — no requirement, and the log bucket would exceed the budget constraint.',
});
```

An acknowledgement with a real justification is a documented accepted risk, which is the production practice. An acknowledgement to make the build go green is a lie to my future self.

**Deploy & validate.** Testing is never improvised. Now open `requirements/.acceptance-tests.md` — sealed since Stage 1 — and run `/test-plan` to translate it into `iac/test-plan.md`, a runbook matched to my actual stack. If a test proves something I never designed for, that is the requirements-reading lesson arriving, and it is cheaper to learn here than in front of a client. Then:

- Destroy the console-built stack and deploy fresh from CDK — reproducing the system from code onto a clean slate and passing the same tests is the proof that the code implements the design
- Execute the test plan in order: smoke tests proving the success criteria, then a **security probe** (can private resources be reached from the internet? do public endpoints expose only what a requirement demands? can any role exceed its intended permission?), then at least one **failure-injection test** proving resilience — which must show both recovery within the stated RTO **and the alarm firing**, captured as a screenshot of the alarm in ALARM state
- Record every result inline in `test-plan.md` and save every screenshot checkpoint to `iac/evidence/` — the executed file with its results is what the evaluator reads
- **Fault drill** (`/fault-drill`) — with the stack still deployed and healthy, the AI breaks one thing without telling me what, and I diagnose it from logs, metrics and alarms alone. Config-level faults only, revert recorded before the fault is applied, and `cdk deploy` as the backstop since the stack is defined in code. Deploy → test → destroy never asks me to *diagnose* anything; this is the only place in the week that does, and what it usually exposes is not a knowledge gap but a missing signal I never built
- **Full teardown** (`cdk destroy`), then walk the teardown verification checklist against the silent-spenders list
- **24 hours later:** check Cost Explorer filtered to `Project=project-XX`, record the actual cost against the estimate, and write down why they differ

## Stage 6: Evaluation & Comparison

- The AI evaluates my work using a **fixed scorecard** applied identically to every project (defined in [prompts/05-evaluate.md](prompts/05-evaluate.md)) — fixed criteria make progress measurable week over week
- **Claims are checked against the template, not the prose.** A pillar answer that says "encrypted at rest" must point at the property in `template.yaml`. Well-written but unverifiable claims cannot score well
- **The three representations get reconciled.** The diagram, `decisions.md`, and `template.yaml` are three views of one architecture; the evaluator checks they agree and reports any divergence
- A **code comprehension check** quizzes me on my own CDK before the verdict, against the walkthrough I wrote from memory
- Only then is `.reference-solution.md` opened and compared. **The reference is a comparison point, not an answer key** — every difference gets argued: whose decision is stronger, and why? Sometimes mine is, and that gets recorded
- Document the gaps and lessons learned explicitly

## Stage 7: Documentation & Publishing

Every project gets a README with a fixed structure ([templates/README-template.md](templates/README-template.md)): the problem, the requirements and my analysis, the diagram, the decisions and rejected alternatives, the Well-Architected review, estimated *and actual* cost, deployment steps, the change request and how the design absorbed it, the scorecard, and lessons learned. Every claim carries evidence — a diagram, a table, a screenshot, or a score.

**One external anchor.** This framework is otherwise a closed loop: the AI writes the requirements, writes the reference, and grades the result. Find one *published* AWS reference architecture or Well-Architected Lab covering the same problem shape, and note two differences from my design. Ground truth from outside the loop, once a week.

Since the goal includes personal branding, every project produces a **ready-to-publish LinkedIn post**. The AI drafts the structure; the words that go out are rewritten in my own voice.

## Stage 8: Retrospective (10 minutes)

Three questions, answered in writing in `retrospective.md`, plus a rough note of time spent per stage — measurement, not a budget; it is the data that shows whether the loop is sustainable.

1. What slowed me down this week?
2. What knowledge gap surfaced that I need to study?
3. What should change in this methodology itself?

Any change to the methodology becomes a standalone commit to this file. After 3–4 projects, the framework will have evolved from real experience rather than guesswork.

---

# Prompts

Source of truth is `prompts/` — these files are copied into every project repo at kickoff and exposed as slash commands under `.claude/commands/`. **They are not reproduced here.** A prompt kept in two places becomes two different prompts.

| Prompt | Command | What it does |
|--------|---------|--------------|
| [01-generate-requirements.md](prompts/01-generate-requirements.md) | `/new-project` | Selects domain + level per the Selection Rule; writes the raw requirements and the three sealed artifacts |
| [02-client-persona.md](prompts/02-client-persona.md) | `/client` | Role-plays the client for the clarification interview and validates my restatement |
| [03-diagram.md](prompts/03-diagram.md) | `/diagram` | Renders my decisions into `diagram.py` / `architecture.png` — renderer, not designer |
| [07-design-review.md](prompts/07-design-review.md) | `/defend` | Attacks my design before the build: principal architect, then cost stakeholder |
| [04-test-plan.md](prompts/04-test-plan.md) | `/test-plan` | Translates the sealed acceptance tests into an executable runbook for my stack |
| [05-evaluate.md](prompts/05-evaluate.md) | `/evaluate` | Scorecard evaluation, template verification, reference comparison, comprehension quiz |
| [08-fault-drill.md](prompts/08-fault-drill.md) | `/fault-drill` | Breaks one thing in the running stack; I diagnose it from telemetry alone |
| [06-linkedin-post.md](prompts/06-linkedin-post.md) | `/linkedin` | Drafts two LinkedIn post structures from the README |

The hub itself has two more: `/kickoff` (create the next project repo) and `/log-project` (record a finished project's results).

# Templates

Source of truth is `templates/` — copied into each project repo at kickoff. Every template opens with a docstring: when to fill it, how, and what "done" looks like. **They are not reproduced here**, for the same reason.

| Template | Becomes | Stage |
|----------|---------|-------|
| [CLAUDE-md-template.md](templates/CLAUDE-md-template.md) | `CLAUDE.md` | all — the AI boundaries |
| [weekly-checklist-template.md](templates/weekly-checklist-template.md) | `checklist.md` | all — drives the week |
| [requirements-restated-template.md](templates/requirements-restated-template.md) | `analysis/requirements-restated.md` | 2 |
| [qualifiers-constraints-template.md](templates/qualifiers-constraints-template.md) | `analysis/qualifiers-constraints.md` | 2 |
| [assumptions-template.md](templates/assumptions-template.md) | `analysis/assumptions.md` | 2 |
| [clarification-interview-template.md](templates/clarification-interview-template.md) | `analysis/clarification-interview.md` | 2 |
| [decisions-template.md](templates/decisions-template.md) | `design/decisions.md` | 3–4 |
| [design-review-template.md](templates/design-review-template.md) | `design/design-review.md` | 4b–4c |
| [current-state-template.md](templates/current-state-template.md) | `analysis/current-state.md` | 2 — **L3+ only** |
| [disposition-7rs-template.md](templates/disposition-7rs-template.md) | `analysis/disposition-7rs.md` | 3 — **L3+ only** |
| [README-template.md](templates/README-template.md) | `README.md` | 7 |
| [retrospective-template.md](templates/retrospective-template.md) | `retrospective.md` | 8 |
| [revisit-template.md](templates/revisit-template.md) | `revisit.md` (in the revisited repo) | revisit weeks |
