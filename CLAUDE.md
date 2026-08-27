# personnal-shopper-agent

## Status

Greenfield. No source code, no build tooling. The only content is the agent prompt at
`.claude/skills/shopper-agent/`.

Nothing below describes existing code. Update this file once a stack is chosen and
the first commits land (build/test/lint commands, module layout, conventions).

## What this project is

An autonomous research-and-shopping agent. It researches the *product category* until
it is competent in it, interviews the user with questions informed by that competence,
runs verified multi-step web research to build a candidate pool, audits each candidate
against hard constraints, and renders the survivors as a self-contained interactive
HTML shopping report with product photos.

## Where the behaviour lives

```
.claude/skills/shopper-agent/
├── SKILL.md                          # phase-gated behavioural spec, source of truth
└── references/
    ├── domain-primer.md              # how to become competent in a category first
    ├── source-verification.md        # source tiers, conflict resolution, margins
    ├── image-sourcing.md             # lead images: acquisition, CSP, verification
    └── report-template.md            # HTML report anatomy and interaction patterns
```

`SKILL.md` is invoked via `/shopper-agent` and holds the authoritative 7-phase
sequence. The reference files hold the detailed playbooks and are read on demand
(progressive disclosure) - `SKILL.md` points to each at the relevant phase.

Read `SKILL.md` before changing agent behaviour.

## Key invariants

- **Domain primer precedes the interview.** Never ask the user a specification
  question from a position of ignorance about the category. The primer establishes
  form factors, differentiating specs, marketing noise, nominal-vs-effective spec
  traps, and the physical tradeoffs between specs. It is also reported to the user,
  because the missing expertise is why they asked.
- **Feasibility is checked before the candidate pool is built.** Constraint sets can
  be jointly unsatisfiable. Detect `OVER_DETERMINED` early, name the binding
  constraint, and report it as a finding rather than searching indefinitely.
- **Constraint provenance is tracked.** Each constraint is tagged
  `stated_hard | stated_soft | user_estimate | derived`. A `user_estimate` (the user
  guessed - question marks, "maybe", "or") is **never gated on**; re-derive it from
  the use case, show the arithmetic, and confirm. Requirements are derived from the
  use case, not requested as specs.
- **Constraint audit is deterministic and gating.** Every candidate is scored per
  constraint as `PASS | PASS_MARGINAL | FAIL | UNKNOWN`. Only candidates whose every
  hard constraint passes enter `verified_solutions`. `UNKNOWN` excludes, same as
  `FAIL`. Threshold constraints also carry a computed margin; a technically-passing
  zero-margin fit is `PASS_MARGINAL`, not `PASS`.
- **Zero hallucination, ranked by source tier.** Specs, prices, and availability come
  from a tool call against an acceptable source. Manufacturer spec sheet or manual >
  manufacturer page > measured review > retailer spec field > marketing copy > search
  snippet. A hard constraint may never be decided by a retailer spec field alone -
  those were wrong four times in one session. On conflict, resolve before deciding; if
  unresolvable, the spec is `UNKNOWN`.
- **Near-misses are surfaced, not hidden.** Products failing a hard constraint stay
  out of `verified_solutions`, but a candidate that beats the field and fails one rule
  belongs in a visually distinct near-miss panel with its binding reason stated. The
  user may overrule a constraint; they cannot overrule what was concealed. This
  supersedes any blanket "never display a failing product" reading.
- **The rejected-candidate ledger is a user-facing deliverable**, not an internal log.
  It proves coverage and enables overruling.
- **Delta processing over re-research.** When the user changes a constraint, re-audit
  the existing `candidate_pool` first and emit an explicit constraint diff and impact
  statement (what changed, what now fails, what is promoted). Only search again for
  the newly binding criteria, and only if fewer than 3 verified solutions remain.
  Relaxing a constraint re-audits previously failed candidates rather than discarding
  them; the pool is cumulative.
- **Output is a single self-contained HTML artifact** (inline CSS/JS, images as
  `data:` URIs, no external assets except Google Fonts), republished to the **same
  artifact URL** across revisions.
- **Every candidate carries a lead image**, verified by looking at it. Images are an
  evidence source, not decoration: they have confirmed physical features that text did
  not mention.

## Conventions

- The skill is the source of truth for agent behaviour. Change it there, not by
  duplicating rules elsewhere. Put detailed procedure in `references/`, keep
  `SKILL.md` scannable.
- Keep the shopping report single-file and dependency-free so it renders standalone.
- Never use the em dash. Use a plain dash.
