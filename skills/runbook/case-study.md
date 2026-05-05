# Case Study: Runbook / Knowledge Transfer Skill

## The problem

In senior PM and product ops roles, a disproportionate amount of time goes toward knowledge transfer — explaining the same workflows, product decisions, and company context to new hires, support staff, and engineers who don't yet have enough background to operate independently.

The bottleneck isn't willingness to share knowledge. It's that the knowledge lives in one person's head in a form that's hard to write down: it's contextual, judgment-heavy, and built up through years of pattern recognition. Standard documentation approaches (Notion pages, Confluence wikis, recorded Looms) capture *what* but rarely capture *why* — which is the part that actually enables someone to think independently.

## What I built

A Claude skill that encodes a three-part meta-skill for knowledge transfer:

1. **Pattern Extraction** — identify what type of knowledge this is and what form it should take
2. **State Modeling** — describe the world (user, product, org) at each relevant stage
3. **Decision Compression** — surface the tradeoffs and reasoning so the reader understands not just what to do, but why

The skill supports six doc types, each with its own structure and narrative spine:
- Org & product history
- User workflows
- Business model
- Product decisions
- Role & responsibilities
- Internal processes

## Design decisions

**Workflow-first, not context-first.** When I onboard someone, I start with what a user actually does — the sequence of events — then layer in context as it becomes relevant. The skill reflects this: it leads with the user or process journey, not background slides.

**Narrative over bullets for context.** Most documentation defaults to bullet points because they're fast to write. But history, decisions, and workflows have causality and sequence that bullets destroy. The skill explicitly calls for prose in background and decision sections, and reserves bullets for takeaways and checklists.

**Six doc types, not one.** The original version of this skill was built specifically for payment incident runbooks (state-reading, tool-specific, ops-focused). The broader rewrite recognized that knowledge transfer covers much more than incident response: it includes why things were built, how users move through products, and what a new hire needs to understand to be effective. One template can't serve all of these well.

**Living wiki, not static documentation.** Every doc produced includes a "what's still evolving" section and an owner field. This is intentional — docs that pretend everything is decided age badly and mislead new hires. The skill is designed to produce docs that stay honest about uncertainty.

## Who it's for

New hires in any function — engineering, ops, support, business development — at early-stage companies where institutional knowledge is concentrated in a small number of people and documentation is perpetually behind.

## What it replaces

- Ad-hoc Slack explanations that disappear
- Long Loom recordings that nobody watches twice
- Bullet-point Notion pages that capture what but not why
- Repeated 1:1 onboarding conversations for the same topics

## Limitations and next steps

- The skill works best when the expert (the person using it) can provide detailed input. It asks good questions, but the quality of the output depends on the quality of the answers.
- It doesn't yet integrate with actual wikis (Notion, Confluence) — output is markdown that needs to be pasted manually.
- A future version could include a "doc review" mode: given an existing doc, identify what's missing, outdated, or unclear.
