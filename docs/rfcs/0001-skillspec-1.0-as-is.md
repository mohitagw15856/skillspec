# RFC-0001: SkillSpec 1.0 as-is

- **Status:** comment-window (until 2026-07-27)
- **Affects:** process

## Summary
Ratify the current text of `SKILLSPEC.md` (version 1.0, as carried over from its origin repo) as the standard this organization maintains. No normative changes. The org's first act is legitimacy, not change.

## Motivation
SkillSpec was written and battle-tested inside [pm-claude-skills](https://github.com/mohitagw15856/pm-claude-skills), where all 466 curated skills are held at L3 by a CI gate and the validator has graded thousands of external SKILL.md files via the quarterly census and the badge service. A standard capped by its address needed a neutral home; this RFC establishes that the home starts from the text as adopted, so every existing conformance claim (badges, CI gates, census grades) remains valid on day one.

## The change
None to the spec text. `SKILLSPEC.md` at commit of this repo's founding is declared **SkillSpec 1.0**. All future changes to conformance levels, the security pattern set, or `spec/skill.schema.json` require an RFC per `GOVERNANCE.md`.

## What breaks
Nothing. Every skill's level is unchanged by definition.

## Alternatives considered
- **Fold in pending tweaks at spin-out** — rejected: bundling changes with the move makes the move itself contestable.
- **Do nothing (no ratification)** — rejected: without a ratified baseline, "conformant to SkillSpec" has no fixed referent.
