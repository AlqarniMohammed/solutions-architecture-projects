# Qualifiers & Constraints — {Project Name}

> **How to use this file** — Stage 2, **after** `requirements-restated.md`, before the interview.
> Work from my restatement, checking back against the raw source. Every statement must land in one of four places: a **Qualifier** (it shapes which architectures are viable), a **Constraint** (a hard boundary the design cannot cross), an **assumption** (the source doesn't actually state it and I'm inferring — goes to assumptions.md), or **not a requirement at all**.
> That fourth bucket is deliberate. Not everything a stakeholder says is load-bearing: some of it is preference, some is legacy habit, some is a solution proposed in place of a need, and from L2 at least one item is a planted red herring that does not affect the architecture. Deciding what to **ignore** is a judgment skill, and a design that treats every sentence as binding is over-constrained and over-priced.
> The trade-offs table is filled in two passes: before the interview, name each tension and my expected direction, leaving "Business priority" blank; after the interview, fill that column from the client's answers.
> Done when: every statement in the source is accounted for in one of the four tables, and every expected tension is named. Fully done when the business-priority column is completed after the interview.

## Qualifiers
| # | Requirement | Why it shapes the design |
|---|-------------|--------------------------|

## Constraints
| # | Constraint | Type (budget / compliance / region / RTO-RPO / organizational / other) | Design impact |
|---|-----------|------------------------------------------------------------------------|---------------|

## Not A Requirement
> Preferences, legacy habits, solutions proposed instead of needs, and red herrings. Each one needs a reason — "I ignored this" is only a defensible position if I can say why.

| # | What was said | Why it isn't a requirement | If I'm wrong, what breaks? |
|---|---------------|----------------------------|----------------------------|

## Trade-offs Identified
| Trade-off | Business priority (from the client) | My direction |
|-----------|--------------------------------------|--------------|
