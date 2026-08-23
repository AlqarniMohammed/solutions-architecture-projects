# Migration Disposition (7 Rs) — {Project Name}

> **How to use this file** — L3+ projects only, Stage 3, after the current-state assessment and before the target design. **If this project is greenfield (L1–L2), delete this file.**
> Every workload gets exactly one disposition, and the disposition is a *decision* with rejected alternatives — the same standard as `decisions.md`. "Migrate it" is not a disposition.
> The most common failure here is refactoring everything because it is more interesting. The second most common is rehosting everything because it is faster. Both are the absence of a decision.
> Done when: every workload from `current-state.md` §1 appears exactly once, each with a justification tied to a requirement or constraint, a rejected alternative, and a sequence position.

## The seven dispositions

| R | What it means | Typically right when |
|---|---------------|----------------------|
| **Rehost** | Lift and shift, unchanged | Time pressure, a hard datacentre exit date, or a workload not worth investing in yet |
| **Replatform** | Lift and reshape — managed database, managed runtime — without changing the code | A clear operational win is available cheaply |
| **Repurchase** | Drop it and buy SaaS | The workload is undifferentiated (email, CRM, CI) |
| **Refactor** | Re-architect meaningfully | The current shape is the actual constraint and the business case justifies the spend |
| **Retire** | Turn it off | Nobody uses it — and there is more of this in every estate than anyone expects |
| **Retain** | Leave it where it is, for now | Compliance, latency to something local, or a contract that has not expired |
| **Relocate** | Move the hypervisor, not the workload | Bulk VMware-style moves where the unit of migration is infrastructure |

## Disposition per workload

| # | Workload | Disposition | Why this one | Alternative rejected, and why | Depends on (must move after) | Wave |
|---|----------|-------------|--------------|-------------------------------|------------------------------|------|

## Sequencing

> Derived from the dependency table in `current-state.md` §3. A wave that moves a workload before something it depends on is a wave that fails.

| Wave | Workloads | Why these together | What must be true before this wave starts |
|------|-----------|--------------------|-------------------------------------------|

## Retire list

> Called out separately because it is the cheapest win available and the easiest to skip. Every retired workload is migration effort and running cost that never has to be spent.

| # | Workload | Evidence nobody needs it | Who signs off |
|---|----------|---------------------------|---------------|

## Coexistence period

> While the migration runs, both estates are live. This is where the real complexity sits, and where a design that looked clean on the target diagram meets reality.

| Question | Answer |
|----------|--------|
| What is the source of truth for data during the transition? | |
| How do the two estates talk to each other? | |
| What is the rollback if a wave fails mid-flight? | |
| How long is coexistence expected to last, and what does it cost while it does? | |
