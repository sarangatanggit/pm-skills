# Case Study: Natural-Language Invoicing Agent

**Context**: Multi-tenant billing
**Role**: PM and builder, working directly with AP/AR team
**Team**: Solo build, deployed into a finance ops workflow
**Stack**: Claude Code, GitHub, Google Sheets, PDF export

---

## The problem

There was no dedicated invoicing platform. Every invoice was hand-built in Google Sheets —
each one requiring the person generating it to remember or look up client-specific rules:
formatting preferences, naming conventions, banking details, billing contacts.

Those rules lived in informal notes, email threads, and one person's head. Every client had
different conventions, and none of them were in one place. When an urgent invoice request
came in — often from someone with no spreadsheet expertise — it interrupted other work and
slowed cash collection while the right person tracked down the right details.

The bottleneck wasn't the invoice itself. It was the lookup cost before you could even start.
Ten minutes per invoice wasn't the filling-in time; it was the time spent remembering what
ACME's billing contact was, what number the last invoice was, and what format the filename
needed to be in.

---

## What I built

Before writing any automation, I mapped the client billing rules. Each client's preferences,
naming conventions, and banking details needed to be a real source of truth — not informal
notes. I structured these as versioned YAML configs in GitHub, one per client, covering
everything the invoice required: numbering formats, sequence tracking, banking details,
billing contacts, and any standing decisions (like "always include semester notes for
academic clients").

Then I built a Claude Code agent that:
1. Takes a natural-language request from any team member
2. Identifies the client and loads their YAML config
3. Resolves every field it can from config or request — without asking
4. Prompts only for information that is genuinely missing
5. Populates the existing Google Sheets template with the correct values
6. Writes any new decisions back to the YAML config, incrementing the sequence number

The config is the center of gravity. The agent doesn't hold knowledge — the YAML does.
That means every invoice is auditable, the history is in git, and any team member can
generate one without being trained on client-specific conventions.

---

## Design decisions

**Config-first, not template-first.** The instinct was to start with the Sheets template —
it was the visible artifact. But the real problem was unstructured client knowledge. Starting
with YAML configs meant the automation was building on a real source of truth, not papering
over the information gap with a prettier template.

**GitHub as the config store.** Client billing rules are sensitive and change over time.
Storing them in GitHub gives version history, auditability, and the ability to review changes
before they affect production invoices. A shared folder or spreadsheet would have worked for
storage but not for the change history that makes billing audits tractable.

**Ask once, write back.** The most important design rule was: any decision made during an
invoice generation gets written back to the config. This makes each invoice marginally faster
than the last — the agent learns the client's preferences by accumulating decisions rather
than requiring them to be manually configured upfront. This also means the config improves
with use rather than requiring a separate maintenance workflow.

**No new billing platform.** The evaluation included dedicated invoicing SaaS tools. The
decision against them came down to: the existing Google Sheets template was already
accepted by clients, switching formats would require re-educating AP/AR contacts at each
client, and the recurring cost wasn't justified when the real problem was knowledge
organization, not template capability. The agent solved the knowledge problem without
requiring a platform change.

---

## Outcomes

- **95% reduction in invoice prep time** — from 10 minutes to under 1 minute per invoice
- **Zero spreadsheet expertise required** — any team member can generate an invoice from
  a natural-language request
- **Consistent billing across all clients** — no more off-format filenames, wrong banking
  details, or missed numbering conventions
- **Faster cash collection** — invoices go out the same day requests come in instead of
  queuing behind whoever holds the knowledge
- **$0 in new tooling** — no SaaS subscription; existing stack (GitHub + Google Sheets)
  extended with automation

The compounding effect is real: the first invoice for a new client requires the most input;
invoices for established clients generate with a one-line request.

---

## What this replaces

- Manual lookup of client banking details, billing contacts, and naming conventions
- Informal notes and email threads as the source of truth for client billing rules
- Dependence on one person to generate invoices correctly
- Invoice prep as an interruption — it's now a background task any team member can handle

---

## Limitations

- **Config bootstrap requires human input.** The first invoice for a new client requires
  collecting all billing details upfront. This is one-time work, but it's manual.
- **Template changes require coordination.** If the Google Sheets invoice template structure
  changes (new fields, layout changes), the agent's field-mapping logic needs to be updated.
- **No direct Sheets API integration in v1.** The agent populates a template by providing
  filled values; a human still opens the sheet and confirms before PDF export. A future
  version could use the Sheets API to write directly and trigger PDF generation automatically.
- **YAML configs require access control.** Banking details are sensitive. The GitHub repo
  holding configs needs appropriate access restrictions — not a public or open-team repo.
