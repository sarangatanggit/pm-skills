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
| [runbook](./runbook/) | Generates structured knowledge docs for onboarding and knowledge transfer |
