# SkillSpec — the conformance standard for Agent Skills

**[Read the spec → SKILLSPEC.md](SKILLSPEC.md)** · [JSON Schema](spec/skill.schema.json) · [Governance](GOVERNANCE.md) · [RFCs](docs/rfcs/)

SkillSpec grades any `SKILL.md` into three conformance levels, with a security scan at every level:

| Level | Requires |
|---|---|
| **L1 Loadable** | Valid frontmatter: kebab-case `name` matching the folder, a `description` with **"Use when …"** trigger conditions (that's what makes auto-discovery work) |
| **L2 Structured** | Declared inputs ("Required Inputs" / "What This Skill Produces") and an Output section |
| **L3 Trustworthy** | L2 + **Quality Checks** and **Anti-Patterns** — the skill can verify its own output |

The security scan bans prompt-injection phrasing, unvetted network calls, data-exfiltration instructions, and embedded credentials. Security findings are always errors.

## The validator: `skillspec-check`

The reference implementation lives in this repo (`cli.mjs`). Zero dependencies, Node ≥ 18.

```bash
npx skillspec-check                    # scan the current directory for SKILL.md files
npx skillspec-check skills/            # a specific tree
npx skillspec-check --min-level 2      # CI gate: every skill must be at least L2 Structured
npx skillspec-check --strict --json    # warnings fail; machine-readable output
```

Exit codes: `0` clean · `1` errors / below `--min-level` / warnings with `--strict` · `2` usage.

### CI in one line

```yaml
- run: npx skillspec-check --min-level 1   # add to any workflow; exits 1 on findings
```

### pre-commit

```yaml
repos:
  - repo: https://github.com/mohitagw15856/skillspec
    rev: v1.0.0
    hooks:
      - id: skillspec
```

## The badge

A live-graded shield for any public repo — the badge service fetches your SKILL.md files, grades them with this validator, and reports the **minimum** level across the repo:

```markdown
![SkillSpec](https://img.shields.io/endpoint?url=https%3A%2F%2Fpm-skills-mcp.pm-claude-skills.workers.dev%2Fbadge%3Frepo%3DYOURUSER%2FYOURREPO)
```

## Governance

Changes to conformance levels, security patterns, or the schema land as RFCs with a 14-day comment window ([template](docs/rfcs/0000-template.md)). Maintainership is earned by adoption — the two largest non-fork adopters found by each quarterly census hold standing invitations. See [GOVERNANCE.md](GOVERNANCE.md), and [RFC-0001](docs/rfcs/0001-skillspec-1.0-as-is.md), which ratifies SkillSpec 1.0 as-is.

## Adopters

Add yourself by PR to [GOVERNANCE.md](GOVERNANCE.md). Largest implementation: [pm-claude-skills](https://github.com/mohitagw15856/pm-claude-skills) — 466 skills held at L3 by a CI gate.
