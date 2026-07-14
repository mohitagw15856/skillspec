# RFC-0003: Runtime-specific frontmatter keys — tolerate, don't reject

- **Status:** draft
- **Affects:** schema | process

## Summary
Formalize what the reference validator already does in practice: unknown top-level frontmatter keys do not fail validation. Runtimes MAY define additional keys (OpenClaw's `homepage`, `user-invocable`, `command-dispatch`; future runtimes will invent more); validators MUST NOT reject skills for carrying them, and SHOULD surface them as informational under `--strict`.

## Motivation
SkillSpec 1.0 §2 says frontmatter contains "exactly the required fields and optionally the reserved ones." Read literally, that makes every OpenClaw skill with a top-level `homepage` non-conformant — while the reference validator happily grades them (verified 2026-07-14: an OpenClaw-dialect skill with `homepage`, `user-invocable`, and a `metadata.openclaw` block validates at L3). The ecosystems are converging on SKILL.md; the standard should describe the convergence, not fight it. The `metadata` open object (reserved in 1.0, "runtimes MUST ignore keys they don't understand") already embodies the right philosophy — this RFC extends it to top-level keys.

## The change
§2 sentence, before → after:

> **Before:** `SKILL.md` MUST begin with YAML frontmatter containing exactly the required fields and optionally the reserved ones:
>
> **After:** `SKILL.md` MUST begin with YAML frontmatter containing the required fields, optionally the reserved ones, and MAY carry additional runtime-specific keys. Runtimes MUST ignore keys they don't understand; validators MUST NOT fail on unknown keys (and SHOULD list them under strict mode). Runtime-specific keys SHOULD prefer a namespaced home inside `metadata` (e.g. `metadata.openclaw`) so top-level growth stays bounded.

## What breaks
Nothing measured: the reference validator's behavior is unchanged, so every currently-graded skill keeps its level. Skills previously non-conformant *on paper* (OpenClaw dialect) become conformant in text as they already were in practice.

## Alternatives considered
- **Do nothing** — leaves the spec text contradicting the validator and brands a major SKILL.md ecosystem non-conformant on a technicality.
- **Enumerate every runtime's keys as reserved** — an unwinnable game of catch-up; namespacing inside `metadata` is the scalable path.
- **Reject unknown keys strictly** — would require OpenClaw authors to strip fields their runtime needs; conformance that costs functionality won't be adopted.
