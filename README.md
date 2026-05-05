# PM Skills

A portfolio of skills built to automate and scale the work of a senior PM operating at the intersection of product, ops, and fintech infrastructure.

Each skill encodes a repeatable thinking pattern — the kind of judgment that usually lives in one person's head and creates bottlenecks. The goal is to make that thinking portable, teachable, and composable.

---

## What's here

| Skill | What it does | Meta-skill encoded |
|---|---|---|
| [runbook](./skills/runbook/) | Generates structured knowledge docs for onboarding and knowledge transfer | Pattern Extraction → State Modeling → Decision Compression |

More skills coming as they're built and refined.

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

---

## The thinking behind this

These skills were built to automate a specific kind of PM work: the parts that require pattern recognition and judgment, not just execution. Things like:

- Compressing institutional knowledge into docs a new hire can actually use
- Turning recurring pain into structured runbooks instead of tribal knowledge
- Capturing product decisions with their tradeoffs intact so the next person understands the *why*

Each skill is built by interviewing the expert (me), extracting the underlying reasoning pattern, and encoding it so Claude can apply that same pattern to new inputs.

---

## Status

This is a living portfolio. Skills get refined as they're used in the real world.
