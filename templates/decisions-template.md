# Design Decisions — {Project Name}

> **How to use this file** — Stages 3–4. This is the core of the project; everything else exists around it.
> Part 1 (Stage 3): one row per service — and the "why" must reference a specific requirement or constraint, not a general best practice. A decision without a rejected alternative isn't a decision; it's a habit.
> Part 2 (Stage 4): answer every Well-Architected self-check question from the framework in writing, pointing at specific mechanisms in this design. "Yes" is not an answer — and the evaluator verifies every claim against `iac/template.yaml`, so a claim with no property behind it scores as if it were absent.
> Parts 3 and 4 are filled during Stage 5, after the stack exists.
> Done when: someone could rebuild my architecture from this file alone — and challenge any choice knowing exactly why I made it.

## Part 1 — Service Decisions
| Decision | Alternatives Considered | Rejected Because | Requirement It Serves |
|----------|------------------------|-------------------|------------------------|

> Revised after Stage 4b (`/defend`) or 4c (change request)? Note what changed and why, so the diff is visible.

## Part 2 — Well-Architected Self-Check Answers

> Answer the questions from the framework's Stage 4, each pointing at a specific mechanism. For every claim, the property in `template.yaml` that makes it true should be nameable.

### Operational Excellence

### Security

### Reliability

### Performance Efficiency

### Cost Optimization

> The Pricing Calculator estimate goes here before the build; the actual goes in Part 4 after teardown.

### Sustainability

## Part 3 — cdk-nag: Suppressions & Accepted Risks

> `cdk synth` must produce zero unsuppressed errors before `cdk deploy`. Everything acknowledged via `Validations.of(x).acknowledge({ id, reason })` gets a row here.
> An acknowledgement with a real justification is a documented accepted risk — the production practice. An acknowledgement written to make the build go green is a lie to my future self.

| Rule ID | Resource | Why it's accepted | What it trades against | Would I accept this in production? |
|---------|----------|-------------------|------------------------|-------------------------------------|

## Part 4 — Cost: Estimated vs. Actual

> Estimate from the Pricing Calculator before the build. Actual from Cost Explorer, filtered to the `Project={Project Name}` tag, **24 hours after teardown**.
> The variance is the lesson. This is where forgotten NAT hourly charges, cross-AZ data transfer, and per-request pricing get discovered — and where an architect stops guessing at cost.

| | Amount | Notes |
|---|--------|-------|
| Pricing Calculator estimate | | link: |
| Actual (Cost Explorer, T+24h) | | |
| Variance | | |

**Why they differ:**

**What I will estimate differently next time:**
