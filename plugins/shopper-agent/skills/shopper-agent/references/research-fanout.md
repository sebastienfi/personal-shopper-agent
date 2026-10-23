# Research Fan-out

How to run Phase 4 with several research agents in parallel without losing the audit's discipline. Every multi-candidate session so far used this shape, and it produced large, sourced pools quickly. The failures came from what the briefs did not say.

## When to fan out

- The pool will exceed about ten candidates, or spans several brands, families or form factors.
- The purchase is a kit of several items (one agent per item).
- The `capabilities` probe found a tool to launch agents. Without one, run the same brief yourself, family by family.

Do not fan out before the user has seen the brief and the feasibility verdict (`SKILL.md`, Phases 2 and 3). An amendment to a running agent costs more than a correction before launch.

## Split

Give each agent a distinct, non-overlapping slice: by manufacturer or group (Renault-Dacia, Stellantis, VW group), by family (Strix Halo boxes, mid-range APUs), or by kit item (coat, trousers, knit, shoes). Give one agent the fallbacks: the cheaper class, the other form factor, the likely near-misses.

A **category physics** agent is worth adding when one technical question decides the field. In the headset session one agent studied radio range through glass and bodies alone, and its finding (the headset-to-phone return link is the weak side) reframed every candidate.

## The shared brief

Write one brief file in the working directory and tell each agent: "Read the brief first and follow it exactly", plus its slice. One file keeps every agent on the same rules and makes amendments a single edit.

The brief contains:

1. **The buyer, in a few lines.** Use case, destination, language of user-facing text.
2. **Constraints, numbered,** with their provenance tag and exact test: `H3 volume <= 3.0 L, computed from mm, not from the litre figure on the page`.
3. **Evidence rules.** The tier ladder from `source-verification.md`; a search snippet is a lead, not a source; never fill a value from memory: write `UNKNOWN` and what was tried; flag conflicts between sources; mark borrowed figures and missing measurement conditions.
4. **Capabilities.** What the Phase 0 probe found, stated as fact: "the shell has no network access; do not use curl; fetch pages with web fetch". No agent should spend searches rediscovering a closed path.
5. **Stop rules.** "If a candidate is clearly over budget or fails a hard constraint on a tier 1-3 source, record it and stop digging."
6. **Search cap** (below).
7. **Output schema** (below), and where to write it.

## Search cap

Web search quotas can be shared by every agent launched in the same turn. Two sessions hit a shared ceiling of about 200 searches, and agents stopped mid-work. The figure was observed, not documented, and may differ elsewhere.

Give every agent an explicit cap that leaves room for the gap-closing pass: roughly the shared ceiling, minus a reserve of about a quarter for yourself, divided by the number of agents. With four agents that is about 35 to 40 each; in practice they used less.

## Output schema

One record per candidate, in a file per agent (`r_<slice>.json` or `.md`), never only in the agent's final message:

- model and destination-market SKU
- every gating spec: value, unit, source URL, tier, conditions, `borrowed_from` when relevant
- price, merchant, stock, date, and the product page URL
- verdict per constraint (`PASS | PASS_MARGINAL | FAIL | UNKNOWN`) and the binding constraint
- the image URL found, if any
- `unverified`: what could not be confirmed and what was tried

Keep **user-facing text separate from codes**: a `binding_reason` field written in plain words in the user's language, next to the constraint id. In one session the ledger showed `C1` and `T5` to the user because the agents wrote only codes.

## While agents run

- **Amend, do not restart.** When the user changes a constraint, edit the brief and send each running agent a short amendment naming the change. Two sessions did this mid-flight and kept the work already done.
- **One line per returning agent.** Tell the user what each slice found as it lands: the best candidate and its catch. The user can redirect early.
- **Merge into one ledger** as results arrive, with the slice recorded per row.

## After the agents

Run a **gap-closing pass** yourself on the finalists and near-misses: targeted fetches for every `UNKNOWN` that could change a verdict. Then the audit (Phase 5), then the lineage check on finalists and near-misses (`lineage-check.md`).
