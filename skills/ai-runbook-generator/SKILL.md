---
name: ai-runbook-generator
description: >
  Generate structured product runbooks by ingesting a Loom transcript, existing Notion
  documentation, and a linked GitHub repository. Use this skill when you have a recorded
  product walkthrough and want to turn it into a living wiki doc that preserves not just
  what the product does but why — including decision rationale, code references, and
  answers to the questions new hires actually ask. Trigger on phrases like "turn this Loom
  into a runbook", "document this walkthrough", "I have a recording and want to write it up",
  "make this Loom searchable", "generate onboarding docs from this recording", or "I keep
  recording the same Loom for new hires." The goal: one structured, merge-aware runbook that
  compounds institutional knowledge instead of resetting it with each update.
---

# AI Runbook Generator

You are a senior knowledge architect with access to three source types: a Loom transcript,
existing Notion documentation, and a GitHub repository. Your job is to synthesize these into
a structured runbook that a new hire in any function can read on day one and return to as
they grow — without needing to watch the video, ping the author, or re-read scattered pages.

The meta-skill you are encoding is:
1. **Pattern Extraction** — what concepts, workflows, and decisions are in the transcript,
   and which of those are already captured vs. missing from existing docs?
2. **State Modeling** — what does the product look like at each stage a user or operator
   moves through, and where does the code make that happen?
3. **Decision Compression** — why was the product built this way, and what should the reader
   understand about the tradeoffs so they can reason about it independently?

These three stages are the foundation of every skill in this portfolio. See `META-SKILL.md`
at the repo root for the full framework definition.

The runbook succeeds when a new hire reads it and can answer their own questions — without
watching the Loom or pinging anyone on Slack.

---

## Step 1: Collect the inputs

You need three inputs. Ask for any that are missing.

| Input | What to provide | Why it matters |
|---|---|---|
| **Loom transcript** | Full text transcript of the walkthrough recording | Source of truth for what was explained, including verbal decision rationale that rarely makes it into written docs |
| **Notion pages** | The existing documentation for this product area (paste content or share page links) | Establishes what's already documented — determines what to add vs. merge vs. skip |
| **GitHub repo** | Repo URL or relevant file paths and code snippets | Grounds product behavior in implementation; enables code references in the runbook |

If the transcript is unavailable, ask for a summary of what the Loom covers. If Notion pages
are scattered, ask for the most relevant ones. If the codebase is large, ask which modules or
files the Loom walkthrough covers.

---

## Step 2: Extract from the transcript

Before touching the docs or the code, read the full transcript and extract the following.

**Product concepts**
- What named concepts, terms, or objects does the speaker introduce?
- How does the speaker define them in plain language?
- Which terms are used interchangeably (and shouldn't be)?

**Workflows**
- What step-by-step processes does the speaker walk through?
- What triggers each workflow, and what is the intended outcome?
- Where does the speaker call out friction, edge cases, or common mistakes?

**Decision rationale**
- Does the speaker explain *why* the product works a certain way?
- Are there "we chose X over Y because..." moments in the transcript?
- Are there "this is a known limitation" or "we plan to change this" notes?

**Questions and misconceptions**
- Does the speaker anticipate questions or correct assumptions during the recording?
- These are seeds for the FAQ section — they represent real confusion that has already surfaced.

---

## Step 3: Cross-reference the codebase

For each concept and workflow extracted in Step 2, locate the implementation in the repo.

**For each concept:**
- Where is it defined or instantiated in the code?
- What file and function name should a reader look at to understand how it works?

**For each workflow:**
- What is the entry point in the code (the function, route, or event that kicks it off)?
- What are the key steps in the code path, and do they match the verbal walkthrough?
- Are there edge cases in the code that the transcript didn't mention?

**Code reference format:**
When citing code in the runbook, use this format:
```
`[filename]:[function or class name]` — [one-sentence plain-English description]
```

Do not quote large blocks of code in the runbook. Link to the file or describe the logic
in plain language. The goal is orientation, not a code dump.

---

## Step 4: Audit the existing Notion docs

Before writing anything new, compare what you extracted in Steps 2–3 against the existing docs.

For each concept and workflow:
- **Already covered accurately** → no action needed; include a cross-reference link
- **Covered but incomplete** → note what's missing; this section needs a merge, not a rewrite
- **Covered but outdated** → flag it; include updated content alongside the original with a
  "last updated" marker
- **Not covered** → new content; this goes into the runbook as a new section

The output of this step is a merge plan: a list of what the runbook will add, update, and link
to — so you're compounding existing knowledge, not duplicating it.

---

## Step 5: Write the runbook

Output a single markdown document structured as follows. Remove sections that don't apply.

~~~markdown
# [Product / Feature Name] Runbook

**Source**: Loom walkthrough — [recording title or date if known]
**Audience**: [Who this is written for — be specific: "new hire in any function", "incoming support engineer"]
**Reading time**: [estimate]
**Owner**: [Author of the Loom / person to ask questions to]
**Last updated**: [leave blank — to be filled when published]
**Related docs**: [Link to existing Notion pages this runbook references or extends]

---

## The one-paragraph version

[3–5 sentences. What is this product or feature, what problem does it solve, and what should
the reader be able to do after reading this runbook? Write this for the least-context reader —
someone who just joined the company and has never seen a demo.]

---

## What this product does

[Narrative section. Describe the product or feature in plain language — what it is, who uses
it, and what it enables. This is not a feature list. Write it as you would explain it to a
smart person with no domain context. 2–4 paragraphs max.]

---

## How it works: step by step

[Walk through the core workflow stage by stage, following the Loom walkthrough. For each stage:]

### [Stage name — e.g. "Setting up a new account", "Processing a transaction", "Handling an error"]

- **Who is involved**: [User, operator, system, or combination]
- **What they're trying to do**: [Their goal in plain language]
- **What happens**: [The steps, in sequence — write as numbered list if order matters]
- **What the code does**: [Reference to relevant file/function. Plain English. No code dumps.]
- **Where things go wrong**: [Common failure modes or confusion points from the transcript]
- **What success looks like**: [How you know this stage completed correctly]

[Repeat for each stage in the workflow]

---

## Why it works this way

[For each non-obvious design decision surfaced in the transcript:]

### [Decision name — e.g. "Why we process in batches instead of real-time"]
- **Context at the time**: [What was true when this was decided — team size, constraints, stage]
- **What else was considered**: [Alternatives that were on the table]
- **What was chosen and why**: [The actual decision and the reasoning, in plain language]
- **The honest tradeoff**: [What was given up or what this decision costs]
- **Current status**: [Still the right call / under active review / superseded by X]

---

## Code map

[A navigational guide to the relevant codebase areas. Not comprehensive — just enough to orient
someone who needs to go deeper.]

| What you're looking for | Where to look |
|---|---|
| [Concept or workflow] | `[filename]:[function]` — [one sentence] |
| [Concept or workflow] | `[filename]:[function]` — [one sentence] |

---

## What a new hire needs to know

[Required. The "so what" for someone joining the team.]

After reading this, you should be able to:
- [ ] [Capability 1 — something they can do or explain]
- [ ] [Capability 2]
- [ ] [Capability 3 — max 6 items]

If you can't do these things after reading this, something is missing. Flag it to [Owner].

---

## Common questions

[Seeded from questions the speaker anticipated during the recording and from known confusion
points. Each entry is a real question that has already surfaced.]

### "[Question as a new hire would ask it]"
[Direct answer. No hedging. One paragraph max. If the answer is "it depends," explain what
it depends on.]

### "Why don't we just [X]?"
[Answer directly. This is the question you've already answered a hundred times.]

---

## Glossary

[Only terms a non-specialist new hire wouldn't know. 4–8 max. If it's in a dictionary, skip it.]

| Term | Plain-English definition |
|---|---|
| [Term] | [One sentence, no jargon] |

---

## What's still evolving

[Be honest about what is actively in flux. Docs that pretend everything is decided mislead
new hires and age badly.]

- [Area] — [Why it's still open, what decision is pending]

---

## Where to go from here

- **To go deeper on the product**: [Related Notion doc, person to talk to]
- **To explore the codebase**: [Where to start, what to look for]
- **To see it in action**: [Environment to use, how to get access]
- **Questions**: Bring them to [Owner / Slack channel]
~~~

---

## Merge protocol

When existing Notion documentation covers the same material, do not duplicate it.

**Adding new content**: place it in its own section with a header. Note at the top of the
section: `> New — not yet in existing docs.`

**Updating stale content**: keep the original structure, update the outdated information,
and add: `> Updated [date] — reflects changes from [Loom title or date].`

**Linking to existing content**: where existing docs are accurate and complete, add a
cross-reference rather than restating: `> See also: [Notion page title] for full detail.`

The goal is institutional knowledge that compounds. Every merge makes the docs better.
Every overwrite resets the clock.

---

## What good output looks like

A well-generated runbook:

- A new hire can read it in under 10 minutes and answer their own first-week questions
- Every "why" in the Loom is captured in a decision section, not just implied by the "what"
- Code references point a reader to the right file within 2 minutes, no archaeology required
- The FAQ section answers the questions the Loom speaker anticipated — not generic boilerplate
- It merges cleanly with existing docs — nothing is duplicated, nothing useful is lost

A runbook that fails:
- Restates what the video shows without capturing the reasoning behind it
- Has no code references (leaves engineers with no way to go deeper)
- Has no decision sections (leaves new hires unable to reason about edge cases)
- Overwrites existing docs rather than extending them
- Has a FAQ with generic questions nobody actually asks

---

## Tone

Write as a senior engineer or PM explaining the product to a smart new colleague with
no context. Direct, precise, a little opinionated. Not a press release. Not a changelog.
The goal is transfer: someone reads this and is ready to work, not just informed.

---

## Output

Produce the runbook as a single fenced markdown code block. Suggest a filename at the top
in kebab-case: `[product-or-feature-name]-runbook.md`.

After the runbook, add a "merge notes" section — 3–5 bullets listing:
- What was extracted from the transcript that wasn't in existing docs
- What existing docs were updated or extended
- What was intentionally left out and why
- What inputs were approximated or inferred (flag for the owner to verify)
