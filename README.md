# PM Skills

A portfolio of skills built to automate and scale the work of a senior PM operating at the intersection of product, ops, and fintech infrastructure.

Each skill encodes a repeatable thinking pattern — the kind of judgment that usually lives in one person's head and creates bottlenecks. The goal is to make that thinking portable, teachable, and composable.

---

## The underlying framework

Every skill in this portfolio applies the same three-stage meta-skill:

**1. Pattern Extraction** — identify what kind of knowledge this is and what form it needs to take before trying to encode it

**2. State Modeling** — make explicit what the world (user, system, org, process) looks like at each meaningful moment, and what causes transitions between moments

**3. Decision Compression** — surface the tradeoffs the expert internalized so deeply they stopped noticing them, so a new practitioner can apply the same judgment to new situations

These stages chain: you can't model state until you've extracted the right pattern, and you can't compress decisions until you've modeled the states they operate on.

See [META-SKILL.md](./META-SKILL.md) for the full framework — including how to apply it when building new skills.

---

## What's here

| Skill | What it does |
|---|---|
| [ai-runbook-generator](./skills/ai-runbook-generator/) | Synthesizes Loom transcripts, Notion docs, and a GitHub repo into a structured runbook that preserves decision rationale — not just what the product does, but why |
| [document-extraction-agent](./skills/document-extraction-agent/) | Extracts structured, schema-ready data from a folder of mixed-format documents and produces a validated import file with an exceptions report for human review |
| [invoicing-agent](./skills/invoicing-agent/) | Generates client-accurate invoices from natural-language requests by loading per-client configs, applying stored billing rules, and writing new decisions back to config after each run |

---

## How skills are structured

Each skill lives in its own folder under `/skills` and contains:

```
skills/
└── skill-name/
    ├── SKILL.md          # The skill itself — instructions Claude follows
    └── case-study.md     # Why it was built, what problem it solves, how it performs
```

`SKILL.md` files are installable in Claude via the Skills feature. Drop the folder into your skills directory and Claude will pick it up automatically.

Every `SKILL.md` encodes Pattern Extraction → State Modeling → Decision Compression, even if the domain looks nothing like knowledge transfer.

---

## Status

Living portfolio. Skills get refined as they're used in the real world.
