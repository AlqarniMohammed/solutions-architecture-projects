# Solutions Architecture Projects

A self-training framework for AWS Solutions Architecture: **one realistic client project per week**, end to end — messy requirements, analysis, design, a Well-Architected review, an adversarial design review, a console build, CDK, deployment, validation, strict evaluation, and a public write-up.

Each weekly project lives in **its own public repo**. This repository is the framework itself: the methodology, the prompts, the templates, and the cross-project memory.

## The rule that makes it training rather than output

> **The AI plays the client, renders, challenges, and grades. Every architecture decision is mine.**

That boundary is the whole design. AI can produce a plausible architecture in seconds — which is exactly why handing it the interesting part would make the portfolio grow while the architect doesn't. So the framework spends its effort making the work *harder*, not faster:

- **Requirements arrive unstructured.** No "Constraints" section, no "Success Criteria" list. Sorting a stakeholder brief, an email thread, or a meeting transcript full of conflicting priorities into a real specification is the first exercise, not a preamble to it.
- **The obvious answer is illegal.** Every project has at least one constraint that makes the textbook architecture non-viable, so no week can be completed on autopilot.
- **The AI picks the domain and difficulty** from the previous week's weakest area — so the projects attack what I'm worst at, not what I enjoy.
- **Three files stay sealed.** The reference solution, the acceptance tests, and a mid-project change request are all written up front and opened only at their stage. Misread the requirements and a sealed test proves it at deploy time.
- **Code I can't explain doesn't get deployed.** Before deploying, I write a walkthrough of the stack from memory, then diff it against the real code.
- **Once it works, it gets broken.** The AI changes one thing in the running stack without saying what, and I diagnose it from logs and metrics alone — because deploy-test-destroy never asks anyone to diagnose anything, and the gap it exposes is usually a signal I never built.
- **Difficulty ratchets mechanically.** Score ≥85 and the next project moves up a level. Score lower and it stays — but a difficulty dimension still has to rise. Two identical weeks in a row is a bug.

## What it builds

Each mechanism above exists to produce a specific capability. The left column is what the framework makes you do; the right is what you can do afterwards that you couldn't before.

| The rep | The capability |
|---------|----------------|
| Read an unstructured brief and write `requirements-restated.md` in your own words — scored against the true requirements | Turn a rambling stakeholder conversation into a defensible specification, and know what you missed rather than not knowing |
| Interview a client who volunteers nothing and is sometimes wrong about their own system | Ask the question that surfaces the thing nobody mentioned — the single highest-leverage habit in the role |
| Sort every statement into Qualifier, Constraint, assumption, or **not a requirement** | Decide what to *ignore*. A design that treats every sentence as binding is over-constrained and over-priced |
| Justify every service with a rejected alternative, against constraints that make the obvious answer illegal | Compare options under real pressure instead of reaching for the familiar one |
| Survive `/defend` — a principal architect and a CFO attacking the design before it's built | Hold a design up in a room and defend it. This is most of what an interview and a client meeting actually are |
| Absorb a sealed change request after the design is finished | Design for change, and learn which of your decisions were load-bearing — while it's still cheap to find out |
| Build it in the console first, with evidence, before any code | Know what the knobs actually do. No amount of reading generated CDK substitutes for this |
| Write `walkthrough.md` from memory, closed book, before the deploy is allowed | Explain your own system without the code in front of you — the whiteboard skill, made non-optional |
| Clear cdk-nag, or justify every suppression in writing | Find the misconfigurations you didn't know to look for, and produce the documented-accepted-risk artifact that real teams run on |
| Prove the alarm fired, with a screenshot, not just that the system recovered | Make an operational claim you can evidence. Most people have never watched an alarm go to ALARM |
| Diagnose a fault the AI injected, from telemetry alone | Work backwards from a symptom under uncertainty — and discover which signal you never built |
| Record estimate → actual → variance every week | Develop real cost intuition, from the gap between what you predicted and what AWS charged |
| Publish a README where every claim has evidence behind it | Show the work to someone who wasn't there — a recruiter, an engineer, a client |

And because the log tracks per-pillar scores rather than a single total, a weakness that survives four weeks becomes visible instead of invisible — which is what makes twelve projects a curriculum rather than twelve disconnected weeks.

## How a week runs

```
/kickoff <name>       (hub)      → new public repo, fully scaffolded
/new-project          (project)  → raw requirements + 3 sealed artifacts
  → restate & classify (me) → /client interview → design + decisions (me) → /diagram
  → /defend  (survive the design review) → open the change request → adapt or defend
  → console build (me) → CDK (Claude Code, gated by my review) → cdk-nag → walkthrough from memory
  → deploy, test, prove the alarm fires → /fault-drill (it breaks something, I diagnose)
  → destroy → check the real bill 24h later
  → /evaluate → README → /linkedin → retrospective
/log-project          (hub)      → level, per-pillar scores, next-week focus recorded
```

Full narrative in [WORKFLOW.md](WORKFLOW.md); the methodology behind every step is in [setup.md](setup.md).

## What's in this repo

| File / folder | What it is |
|---------------|------------|
| [setup.md](setup.md) | The framework — stages, the difficulty ladder, the Selection Rule, the Well-Architected self-check questions, the scorecard |
| [WORKFLOW.md](WORKFLOW.md) | Step-by-step walkthrough, from one-time AWS setup to the LinkedIn post |
| [projects-log.md](projects-log.md) | One row per project: domain, level, per-pillar scores, recurring weaknesses — the input to the Selection Rule |
| [prompts/](prompts/) | The 8 AI prompts used every week (source of truth) |
| [templates/](templates/) | The files copied into every project repo at kickoff |
| [templates/CLAUDE-md-template.md](templates/CLAUDE-md-template.md) | The AI boundaries, installed into every project so they load automatically |
| `.claude/commands/` | Hub commands: `/kickoff`, `/log-project` |

## The difficulty ladder

Difficulty comes from constraints, not from diagram size — a four-service design under a hard compliance boundary is harder than a twelve-service greenfield sprawl.

| | Scope | Failure domain | Constraints | Requirements arrive as |
|---|-------|----------------|-------------|------------------------|
| **L1** | greenfield, ≤5 services | single region | ≥2 binding, no compliance | one unstructured brief |
| **L2** | greenfield, 5–8 services | multi-AZ, stated RTO/RPO | ≥3 binding, 1 compliance regime | brief + email thread; numbers must be derived |
| **L3** | brownfield, 8–12 services | multi-region or strict DR | ≥4 binding + an organizational constraint | sources that disagree; one stakeholder is wrong |
| **L4** | as L3 | as L3 | ≥5, including a pair in **direct conflict** | as L3, plus missing and contradictory information |

## What it deliberately doesn't cover

Being clear about this matters as much as the rest — a training framework that quietly implies it makes you complete is worse than one with a stated scope.

- **Operating a system over time.** Every stack is built and destroyed inside a week. The fault drill covers diagnosis, but nothing here covers running something for six months, watching it degrade, or living with a decision long enough to regret it.
- **Working in a team.** No peer review, no shared ownership, no handover, no inheriting someone else's design. Real architecture is negotiated; here it's solitary.
- **Organizational scope.** Multi-account structures, landing zones, and governance across an org are out of range for a one-week project.
- **Actual scale.** You reason about 10x load and defend the answer; you don't generate it. The numbers are realistic, the traffic isn't.
- **Consequences.** The client is simulated and nothing is at stake. Judgment under real pressure is a different thing, and no self-training loop can manufacture it.

What it does cover, it covers with evidence. Beyond that line, this framework builds readiness — not experience.

## Projects

See [projects-log.md](projects-log.md) for completed projects, their repos, and their scores.
