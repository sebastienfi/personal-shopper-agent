# personnal-shopper-agent

## Status

Greenfield. No commits yet on `master`, no source code, no build tooling. The only
content is the agent prompt at `.claude/skills/shopper-agent/SKILL.md`.

Nothing below describes existing code. Update this file once a stack is chosen and
the first commits land (build/test/lint commands, module layout, conventions).

## What this project is

An autonomous research-and-shopping agent. It converses with a user to collect
purchasing requirements, runs multi-step web research to build a candidate product
pool, verifies each candidate against hard constraints, and renders the survivors as
a self-contained interactive HTML shopping UI.

The behavioural spec lives in `.claude/skills/shopper-agent/SKILL.md` (invoked via
`/shopper-agent`). Read it before changing agent behaviour. Key invariants it
defines:

- **Constraint audit is deterministic and gating.** Every candidate is scored per
  constraint as `PASS | FAIL | UNKNOWN`; only all-`PASS` candidates may be shown.
  `UNKNOWN` excludes, same as `FAIL`.
- **Zero hallucination.** Specs, prices, and availability must come from an actual
  tool call, never from model recall or a search snippet alone.
- **Delta processing over re-research.** When the user changes a constraint, re-audit
  the existing `candidate_pool` first; only search again for the missing criteria, and
  only if fewer than 3 verified solutions remain. Relaxing a constraint re-audits
  previously failed candidates rather than discarding them.
- **Output is a single self-contained HTML block** (inline CSS/JS, no external assets).

## Conventions

- The skill file is the source of truth for agent behaviour. Change it there, not by
  duplicating rules elsewhere.
- Keep the shopping UI single-file and dependency-free so it renders standalone.
