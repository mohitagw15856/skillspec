# RFC-0002: Machine-readable inputs

- **Status:** draft
- **Affects:** schema | conformance levels (additive)

## Summary
Add an OPTIONAL `inputs` frontmatter key so a skill can declare its required inputs as structured data — enabling runtimes to render real forms and field-by-field prompts instead of a blank text box — without breaking any existing skill.

## Motivation
Consumers today recover input structure by parsing the prose "Required Inputs" section with heuristics (bold label = field, "optional" keyword = optionality). The reference corpus (pm-claude-skills) does exactly this to render its playground forms and CLI prompts. It works, but every runtime must reimplement the same fragile parse, and authors can't express types or choices ("one of: internal / partner / public").

## The change
New OPTIONAL frontmatter key:

```yaml
inputs:
  - label: Feature or product name
    hint: as users would say it
    optional: false
    long: false          # render as multi-line
    choices: []          # optional enumeration
```

Rules:
- `inputs` is additive and OPTIONAL at every conformance level. A skill with a prose "Required Inputs" section and no `inputs` key remains fully conformant.
- If both are present, they MUST agree (validator warning on divergence).
- `label` is required per entry; all other fields optional with the defaults shown.
- Prose "Required Inputs" remains REQUIRED at L2 — frontmatter is for machines, the section is for readers and for models running the skill.

## What breaks
Nothing: purely additive. Validators that reject unknown frontmatter keys must add `inputs` to the reserved list — that is the compatibility cost, and the reason this needs an RFC rather than silent adoption.

## Alternatives considered
- **Do nothing (keep heuristic parsing)** — the status quo works for one implementation but doesn't compose into an ecosystem.
- **Replace the prose section entirely** — rejected: SKILL.md must stay self-sufficient for a runtime that reads only the body text.
- **JSON Schema per skill** — rejected as over-engineering; four fields cover the observed corpus.
