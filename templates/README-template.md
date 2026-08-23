# {Project Name}

> **How to use this file** — Stage 7, filled last.
> Most sections are assembled from files already written during the week — that's by design: if a section is hard to fill, the gap is upstream; go fix it there.
> Written for a stranger (a recruiter, an engineer): they should follow problem → analysis → decision → proof without needing to ask me anything.
> Done when: every claim has evidence — a diagram, a table, a screenshot, or a score.

> Domain: {...} | Level: L{n} | Industry: {...} | Week: {...}

**Evidence:** [Architecture diagram](design/architecture.png) · [Design review](design/design-review.md) · [Executed test plan](iac/test-plan.md) · [Screenshots](iac/evidence/) · [Synthesized template](iac/template.yaml) · [Scorecard](#10-evaluation-result-scorecard)

## 1. The Problem & Scenario

## 2. Requirements & Analysis

> The client's input arrived unstructured. This is my reading of it — see [requirements-restated.md](analysis/requirements-restated.md) for the full derivation.

### What they actually asked for
### Qualifiers
### Constraints
### What I judged *not* to be a requirement, and why
### Assumptions
### Clarifying Questions & Client Answers

## 3. Architecture Diagram
![Architecture Diagram](design/architecture.png)

## 4. Architecture Decisions
| Decision | Alternatives Considered | Why |
|----------|------------------------|-----|

## 5. Design Review — What Held Up Under Challenge
(From [design-review.md](design/design-review.md): the challenges that landed, and what I changed because of them. Include the ones I could not answer — a review with no unresolved findings was a review that was too polite.)

## 6. The Change Request
(What the client changed after the design was finished, and whether the design absorbed it or had to bend.)

## 7. Well-Architected Review
(Pillar by pillar: answer the framework's self-check questions with the specific mechanisms used — each one traceable to a property in the template.)

## 8. Cost — Estimated vs. Actual
| | Amount |
|---|--------|
| Pricing Calculator estimate | |
| Actual (Cost Explorer, T+24h) | |
| Variance | |

**Why they differed:**

## 9. Deployment & Validation
```bash
cdk synth      # must be cdk-nag clean before deploying
cdk deploy
# execute iac/test-plan.md — smoke, security probe, failure injection
cdk destroy
```

Validation evidence: [test-plan.md](iac/test-plan.md) with inline results, screenshots in [iac/evidence/](iac/evidence/) including the alarm in ALARM state.

## 10. Evaluation Result (Scorecard)
| Category | Score |
|----------|-------|
| Operational Excellence | /10 |
| Security | /10 |
| Reliability | /10 |
| Performance Efficiency | /10 |
| Cost Optimization | /10 |
| Sustainability | /10 |
| Requirements analysis | /15 |
| Decision justifications | /10 |
| IaC quality | /15 |
| **Total** | **/100** |

## 11. How This Compares To A Published Reference
(One published AWS reference architecture or Well-Architected Lab for the same problem shape, and two differences from my design. External ground truth for an otherwise closed loop.)

## 12. Lessons Learned
