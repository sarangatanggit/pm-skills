# Skills

Each skill lives in its own folder and encodes a repeatable thinking pattern using the [Pattern Extraction → State Modeling → Decision Compression](../META-SKILL.md) framework.

## Structure

```
skills/
└── skill-name/
    ├── SKILL.md        # The skill itself — instructions Claude follows
    └── case-study.md   # Why it was built, what problem it solves, how it performs
```

`SKILL.md` files are installable in Claude via the Skills feature. Drop the folder into your skills directory and Claude will pick it up automatically.

## Index

| Skill | Description |
|---|---|
| [ai-runbook-generator](./ai-runbook-generator/) | Synthesizes Loom transcripts, Notion docs, and a GitHub repo into a structured runbook that preserves decision rationale — not just what the product does, but why |
| [document-extraction-agent](./document-extraction-agent/) | Extracts structured, schema-ready data from a folder of mixed-format documents and produces a validated import file with an exceptions report for human review |
| [invoicing-agent](./invoicing-agent/) | Generates client-accurate invoices from natural-language requests by loading per-client configs, applying stored billing rules, and writing new decisions back to config after each run |
