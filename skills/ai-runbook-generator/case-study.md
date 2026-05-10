# Case Study: AI Runbook Generator

**Context**: New employee onboarding
**Role**: PM, team lead, and builder of the agent workflow
**Team**: Solo build, validated with new hires
**Stack**: Claude Code, Notion, GitHub, Loom

---

## The problem

New hires ramped on the product through ad hoc Loom recordings and scattered Notion pages.
Critical context lived in one person's head. Videos went stale fast. And existing docs
explained *what* the product did — but never *why* it worked that way.

The bottleneck wasn't documentation volume. It was that the knowledge worth transferring
— decision rationale, tradeoff history, the reasoning behind non-obvious choices — never
made it from the Loom into the doc. Someone watched the recording, understood the demo,
and still had to Slack the same person to understand why.

Three systems held pieces of the answer: the Loom (what was explained verbally), Notion
(what was written down), and GitHub (what was actually built). No single doc connected them.
Every new hire was on their own to triangulate.

---

## What I built

A Claude Code skill that acts as a knowledge synthesis agent. It ingests three sources —
a Loom transcript, the relevant Notion pages, and the linked GitHub repository — and
produces a structured runbook that preserves product behavior, decision rationale, and
code references in a single doc.

The agent doesn't overwrite existing documentation. It merges: it identifies what's already
covered, what's missing, and what's outdated, then adds and updates rather than duplicating.
Institutional knowledge compounds with each pass instead of resetting.

The skill encodes three extraction passes, one per source:
1. **Transcript pass** — extract concepts, workflows, decision rationale, and anticipated
   questions from what the speaker said
2. **Codebase pass** — locate the implementation of each extracted concept; build a code map
   that orients a reader without requiring them to read the full repo
3. **Notion audit** — compare extracted content against existing docs to produce a merge
   plan: what to add, what to update, what to link instead of restate

---

## Design decisions

**Transcript-first, not code-first.** The Loom is the most information-dense input because
it's the only place where the speaker's reasoning is captured verbally — things they'd never
bother writing down. Starting with the transcript ensures the "why" gets extracted before
the "what" takes over.

**Merge, don't overwrite.** The single most destructive pattern in documentation is treating
every update as a fresh start. Existing docs have accumulated context that isn't always
visible — a section that looks thin might be intentionally brief because the detail is
covered in a linked doc. The skill audits before writing, and marks its additions clearly so
owners can review rather than recover.

**Code map over code dump.** New hires don't need a code walkthrough in a runbook — they
need orientation: the right file, the right function, a one-sentence description of what
it does. A code map gives engineers a starting point without making the doc unreadable for
non-engineers.

**FAQ seeded from the recording.** Every Loom contains a speaker who anticipates questions —
"you might be wondering why..." or "the thing people always ask about is..." These are the
real questions new hires will ask. Capturing them at extraction time means the FAQ section
is useful from day one instead of being written after someone has already Slacked you.

---

## Outcomes

- **80% reduction in onboarding repetition** — new hires answer their own questions from
  runbooks instead of pinging on Slack
- **24/7 self-serve access** — the answer exists before the question is asked
- **3 systems unified** — Loom, Notion, and GitHub connected in one doc instead of three
  separate artifacts a new hire has to triangulate manually
- **1 reusable skill** — works across any product area or codebase; not specific to the
  domain it was first applied to

Decision rationale is preserved alongside implementation details as institutional knowledge
that survives personnel changes — the answer to "why does it work this way?" is in the doc,
not in someone's head or in a two-year-old Slack thread.

---

## What this replaces

- Ad hoc Loom recordings watched once and forgotten
- Scattered Notion pages that captured what but not why
- Repeated 1:1 explanations of the same product decisions
- Knowledge that disappears when the person who holds it leaves

---

## Limitations

- **Transcript quality matters.** The skill extracts decision rationale from what the speaker
  says. If the Loom is a screen recording with no narration, the output will be thinner.
  A transcript with verbal reasoning produces a much richer runbook than one that only
  describes what's on screen.
- **Merge quality requires good existing docs.** If existing Notion pages are disorganized
  or inconsistently structured, the audit pass is harder and the merge is more manual.
- **Code references require navigable repos.** The skill locates concepts in the codebase by
  searching for relevant function and class names. Repos with unclear naming conventions or
  minimal structure will produce less precise code maps.
- **Does not write directly to Notion.** Output is markdown. A future version could use the
  Notion API to push directly to the right page and maintain merge history automatically.
