# personnal-shopper-agent

## Status

No application source code. The content is the agent prompt at
`plugins/shopper-agent/skills/shopper-agent/`, packaged as a Claude Code plugin and
distributed through the marketplace manifest at `.claude-plugin/marketplace.json`.

Tooling is CI only: `.github/workflows/ci.yml` validates the manifests, and
`release.yml` builds `shopper-agent.zip` on a `v*` tag. There is no build or test
command for the skill itself.

## What this project is

An autonomous research-and-shopping agent. It researches the *product category* until
it is competent in it, interviews the user with questions informed by that competence,
runs verified multi-step web research to build a candidate pool, audits each candidate
against hard constraints, and renders the survivors as a self-contained interactive
HTML shopping report with product photos.

## Where the behaviour lives

```
plugins/shopper-agent/skills/shopper-agent/
├── SKILL.md                          # phase-gated behavioural spec, source of truth
└── references/
    ├── domain-primer.md              # how to become competent in a category first
    ├── source-verification.md        # source tiers, conflict resolution, margins
    ├── video-evidence.md             # YouTube as a source: captions, comments, tiering
    ├── lineage-check.md              # successor, step-up and sibling check before recommending
    ├── image-sourcing.md             # lead images: acquisition, CSP, verification
    └── report-template.md            # HTML report anatomy and interaction patterns
```

There is deliberately **no `.claude/skills/` copy or symlink in this repo**. It would load
as a project skill named `shopper-agent` while an installed plugin loads as
`shopper-agent:shopper-agent`; different names, so they never dedupe and both run.

To exercise the skill live, launch with `--plugin-dir`:

```
claude --plugin-dir <repo>/plugins/shopper-agent
```

That shadows any installed copy of the same plugin (verified: with both present, the
working tree wins and only one entry loads), so a plain `claude` still gets the released
snapshot from `~/.claude/plugins/cache/`.

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
  constraint, and report it as a finding rather than searching indefinitely. The
  verdict is always told to the user before research starts, `TIGHT` included, with
  what each relaxation would buy.
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
- **Finalists are lineage-checked before they are recommended.** For every verified
  solution and near-miss, check the successor, the next tier up and cheaper siblings at
  the same manufacturer, from its range page. Judge them on the gating specs and the
  user's ranked criteria only, with a price delta at a named merchant in stock. A relative
  that plausibly passes enters the candidate pool and is audited; a replaced start model
  carries a lifecycle warning. The pass also re-verifies the start model's price and stock.
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
  not mention. When the environment cannot download images, a labelled to-scale drawing
  from verified dimensions replaces it, and the lost image check is disclosed.
- **Capabilities are probed, not remembered.** Phase 0 tests search, fetch, shell
  network access, images, video, agents and rendering before anything else. The skill
  never lists sites that worked or failed before: access changes, and such a list
  biases source choice.

## Conventions

- The skill is the source of truth for agent behaviour. Change it there, not by
  duplicating rules elsewhere. Put detailed procedure in `references/`, keep
  `SKILL.md` scannable.
- Keep the shopping report single-file and dependency-free so it renders standalone.
- Never use the em dash. Use a plain dash.
