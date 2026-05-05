# The Meta-Skill Framework

Every skill in this portfolio encodes a three-stage thinking pattern. Understanding the framework helps you build new skills that work the same way, apply it to domains the existing skills don't cover, and audit existing skills for gaps.

The three stages are not independent steps — they chain. You can't model state until you've extracted the right pattern. You can't compress decisions until you've modeled the states they operate on. Run them in order.

---

## Stage 1: Pattern Extraction

**The question:** What kind of problem is this, and what form does the knowledge take?

Before encoding any expertise, you have to identify what category of thinking is happening. Most experts don't know what they know — they just know. Pattern extraction is the act of surfacing the implicit structure.

This stage answers:
- What is the *type* of knowledge being applied? (diagnostic, procedural, historical, evaluative, generative)
- What *triggers* the need for this knowledge? (a new hire, an incident, a decision, a customer question)
- What *form* does the knowledge need to take to be usable? (a workflow, a decision tree, a narrative, a checklist)
- Who is the reader, and what can they already do without this knowledge?

**The failure mode:** Skipping this stage and jumping straight to writing produces docs that are thorough but wrong-shaped — detailed answers to questions nobody was asking.

**In the runbook skill:** The six doc types (org history, user workflow, business model, product decision, role, process) are the output of pattern extraction. Each one is a different shape of knowledge that requires a different structure.

---

## Stage 2: State Modeling

**The question:** What does the world look like at each meaningful moment, and what causes transitions between moments?

Most expertise is implicitly a state machine. An expert knows that "when the customer is here, do X; when they're there, do Y." What makes experts hard to replace is that they carry the state map in their head — they've internalized which state they're in and what it implies without consciously reasoning about it.

State modeling makes that map explicit. It asks:
- What are the distinct states (of the user, the system, the org, the product, the process)?
- What does each state look like from the outside — how would you know you're in it?
- What causes a transition from one state to the next?
- What can go wrong at each state, and what does failure look like?
- What does the actor (user, operator, system) need at each state to move forward?

**The failure mode:** Producing a flat list of steps instead of a model of states. Steps tell you what to do; states tell you *where you are*, which is what you need before you can decide what to do. Steps without states break down the moment the real world diverges from the expected path.

**In the runbook skill:** The "How it works" section of each doc type is a state model. Each stage in a user workflow is a state — with a who, a goal, what happens, and what success looks like.

---

## Stage 3: Decision Compression

**The question:** What tradeoffs did the expert internalize so deeply they stopped noticing them, and how do we make that reasoning legible?

The hardest part of expertise to transfer is not knowledge — it's judgment. Judgment is the accumulated weight of tradeoffs: "we chose A over B because of C, even though it costs us D." Novices don't know A, B, C, or D. Intermediate practitioners know A but not why it's not B. Experts know all four but can no longer explain the reasoning because it happens below the level of conscious thought.

Decision compression surfaces the internalized tradeoffs so a new practitioner can apply the same judgment to new situations — not just follow the same steps.

This stage answers:
- What were the live alternatives at the time of each key decision?
- What constraints (technical, regulatory, timing, resource) shaped the decision?
- What did the chosen option give up — what's the honest downside?
- What would have to change for the decision to be wrong?
- What is still open — where is the judgment still being exercised, not yet settled?

**The failure mode:** Documenting conclusions without context. "We use ACH" with no explanation of why not wire, why not card, what the regulatory constraints were, or what would change the calculus. This produces compliance — people follow the rule without understanding it — which breaks the moment the situation changes slightly.

**In the runbook skill:** The "Key decisions and tradeoffs" section is decision compression. So is the "Common questions and misconceptions" section — each FAQ is an implicit tradeoff that wasn't obvious enough to survive without explanation.

---

## How the stages chain

```
Pattern Extraction → State Modeling → Decision Compression
      ↓                    ↓                   ↓
  What shape         What's true at        Why it's true
  is this?           each moment?          and when it isn't
```

A skill that only does Pattern Extraction produces well-organized emptiness — the right categories with the wrong content.

A skill that does Pattern Extraction + State Modeling produces accurate, complete documentation that a practitioner can follow — but can't adapt when conditions change.

All three stages produce documentation a practitioner can *think with*, not just follow.

---

## Applying the framework to new skills

When building a new skill, run this checklist before writing the SKILL.md:

**Pattern Extraction**
- [ ] What is the domain — what category of expertise is being encoded?
- [ ] What triggers the need for this skill (what situation does the user find themselves in)?
- [ ] What are the distinct types of inputs this skill handles? (These become the skill's "modes" or "doc types")
- [ ] Who is the eventual reader or decision-maker, and what context do they arrive with?

**State Modeling**
- [ ] What are the distinct states or stages the domain operates through?
- [ ] What does each state look like from the outside?
- [ ] What transitions between states, and what can go wrong at each transition?
- [ ] What does the practitioner need at each state to move to the next?

**Decision Compression**
- [ ] What are the top 3–5 non-obvious decisions embedded in how this domain works?
- [ ] For each: what were the alternatives, what was chosen, and what was given up?
- [ ] What would cause those decisions to be revisited?
- [ ] What questions does every new practitioner ask — and what are the real answers?

If you can't answer these questions about the domain, you're not ready to write the skill. Go back to the expert.

---

## What this framework is not

It is not a documentation format. The framework is a thinking tool for skill authors. The output of each skill can look very different — a structured template, a socratic interview, a decision tree, a diagnostic protocol — as long as the underlying knowledge was extracted, modeled, and compressed using these three stages.

It is not a one-time process. As the domain evolves, the state model changes and decisions get revisited. Skills built on this framework should be treated as living artifacts, not finished products.
