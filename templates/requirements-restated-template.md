# Requirements — Restated — {Project Name}

> **How to use this file** — Stage 2, the **first** thing written, before any classification.
> The client sent raw input: prose, an email thread, a meeting transcript. This file is me converting it into a structured specification **in my own words** — the document I would send back saying "here is what I heard, correct me."
> Restating before classifying is deliberate. It forces a real reading of the source, and it separates "what did they ask for" from "how does that shape the design".
> Take it into the `/client` interview and have the client react to it. Then come back and revise it — the revised version is what the rest of the week is built on.
> Done when: everything in the source material is accounted for here in my own words, every number I need is either derived or flagged as missing, and the client has reacted to it.
> **This file is scored.** The evaluator knows the true underlying requirements, so anything I failed to extract shows up as a measured miss rather than an invisible one.

> Source material: {which files / how the requirements arrived} | Restated: {date} | Revised after interview: {date}

## 1. The business problem, in one paragraph
(In my words, not theirs. What is actually costing them money or sleep?)

## 2. Functional requirements
(What the system must do. Each traced to where in the source it came from.)

| # | Requirement | Where it came from | Explicit or inferred? |
|---|-------------|--------------------|-----------------------|

## 3. Non-functional requirements
(The numbers. Most of these will be **derived** from business statements rather than stated — show the derivation, because a wrong derivation here poisons the whole design.)

| # | Requirement | Number | Derived from | Confidence |
|---|-------------|--------|--------------|------------|

## 4. Constraints
(Hard boundaries the design cannot cross: budget, compliance, region, RTO/RPO, organizational.)

| # | Constraint | Type | Stated or implied? |
|---|-----------|------|--------------------|

## 5. Success criteria
(Measurable and testable. If the source never stated one, derive it and mark it — this is exactly what the sealed acceptance tests will check.)

| # | Criterion | How it would be measured | Stated or derived? |
|---|-----------|--------------------------|--------------------|

## 6. What I could not determine
(Gaps that need the interview. These are my interview agenda alongside the high-risk assumptions.)

| # | What is missing | Why the design needs it |
|---|-----------------|-------------------------|

## 7. Revisions after the interview
(What the client corrected, contradicted, or noticed was absent. Be specific — this is the record of what I misread, and it is the most useful thing in this file at retrospective time.)

| # | What I had | What it actually is | How I found out |
|---|-----------|---------------------|-----------------|
