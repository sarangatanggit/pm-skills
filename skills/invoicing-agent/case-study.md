# Case Study: Invoicing / Compensation Extraction Agent

## The problem

White-glove client onboarding at a fintech platform required reviewing 100+ contracts, offer
letters, and compensation schedules per client and converting them into a strict import
spreadsheet — one row per payout event, ready to load into the platform.

The work was repetitive and error-prone. Comp schedules varied wildly: some members had a
single one-time bonus; others had layered monthly retainers, quarterly bonuses, and ad hoc
incentives described in inconsistent language across multiple documents. A recurring payment
described as "quarterly" in one document and "every three months starting Q2" in another
required human interpretation to expand correctly. Getting these rows wrong meant incorrect
payouts for real people — so every row had to be right.

Slow prep compounded the problem. Multi-day manual entry per client delayed go-live dates
and pushed out first payout runs, which created friction in the relationship from day one.
The account management team was spending high-judgment hours on low-judgment work.

## What I built

An extraction agent — built with Claude and deployed as a Claude skill — that ingests a
folder of mixed compensation documents and produces a platform-ready import file plus an
exceptions report.

The critical design decision came before writing a single prompt: I encoded the platform's
import schema, payout expansion rules, and validation requirements so the agent could reason
about them accurately. Getting these rules wrong silently was worse than the agent failing
loudly. So the agent was designed to flag ambiguity rather than resolve it, and to hold rows
out of the import file whenever a required field was missing or two documents conflicted.

The output of each run was two things: a clean import file (valid rows only) and an
exceptions report (everything that needed a human decision). The team's role shifted from
reading every document to making judgment calls on a small set of flagged items.

## Design decisions

**Encode the rules before prompting, not during.** The first version of this had rule logic
embedded in the prompt as natural language. It was brittle — the model would occasionally
interpret edge cases differently across runs. Moving the rules into a structured, explicit
encoding (schema field by field, expansion logic case by case) made the agent consistent and
auditable. The rules became an artifact that could be read, reviewed, and updated.

**Never guess; always flag.** Early in development, the agent would fill in plausible values
for missing fields — rounding amounts, inferring dates from context, defaulting to USD.
These filled-in values were hard to spot in a 1,200-row spreadsheet. The agent was
redesigned to hold out any row with a missing required field and write it to the exceptions
report instead. The exceptions report became the primary quality control surface.

**One row per payout event, not one row per person.** The platform's import schema required
expansion — a member with a 12-month monthly retainer needed 12 rows, not one. This was
the step that most surprised manual reviewers: a folder of 100 documents could produce
1,200+ import rows. The agent's expansion logic had to be explicit about what "recurring"
meant for each frequency type, what to do with open-ended schedules, and how to handle
edge cases like "monthly on the last business day."

**The encoded rules outlived the agent.** The payout expansion logic and validation rules
built for this agent were later used directly in the platform's self-service onboarding
design. Because the rules were explicit and documented (not buried in a prompt), they could
be handed to the engineering team as a spec. This was not planned — it was a consequence of
encoding the rules well from the start.

## Inputs

- Folder of compensation documents (PDF, Word, Excel — mixed formats)
- Platform import schema (exact column names, required fields, data types, formatting rules)
- Payout expansion horizon (end date for open-ended recurring schedules)
- Currency default (assumed currency when not stated in a document)
- Exceptions handling preference (hold flagged rows entirely, or include as partials)

## Decision logic

1. Survey the document folder; classify each document by type
2. Extract the canonical member record for each person (resolve conflicts across documents)
3. For each compensation line item, classify the payout type: one-time, recurring, or ad hoc
4. Expand recurring schedules into one row per payout event
5. Validate each row against the import schema
6. Write valid rows to the import file; write invalid or ambiguous rows to the exceptions report
7. Log all assumptions made (expansion horizon used, currency defaults applied, etc.)

## Outputs

- **Import file** — structured CSV/XLSX, one row per payout event, schema-compliant
- **Exceptions report** — flagged rows grouped by issue type, with the action required
- **Assumptions log** — every assumption the agent made that didn't rise to an exception
- **Document manifest** — the full list of ingested documents with their classified types

## Outcome

Onboarding prep collapsed from multiple days to a few hours per client. The exceptions
report made quality control faster and more reliable than reading every row manually —
reviewers knew exactly what needed attention and why.

1,200+ payout rows were produced per client batch. Error rates dropped because the agent
flagged ambiguities rather than guessing through them. Time to first payout shortened, and
the account management team's role shifted from data entry to judgment on flagged exceptions
— a meaningful step up in how they spent their time.

The encoded rules and data model built for this agent were later used by the engineering
team as a spec for the platform's self-service onboarding flow. The agent's output artifacts
became design artifacts.

## What the runbook skill encodes that I apply here

**Pattern Extraction:** Before writing prompts, identify the three payout types (one-time,
recurring, ad hoc) and their distinct expansion rules. These are the "doc types" of this
domain — each requires different logic to expand correctly.

**State Modeling:** Each member moves through a pipeline of states: document ingested →
member record extracted → compensation classified → rows expanded → rows validated → import
file or exceptions report. The agent's decision logic tracks this state explicitly for every
member and every row.

**Decision Compression:** The key non-obvious decisions are: flag vs. fill (never fill),
row per event vs. row per person (always expand), and rules-as-artifact vs. rules-in-prompt
(encode externally). These decisions aren't obvious until you've run a batch and found the
failure modes they prevent.
