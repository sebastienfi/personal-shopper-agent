# Lineage Check

Before a candidate is recommended, look at its family at the same manufacturer: the model that replaced it, the model above it, and the siblings beside it. Then decide whether any of them is a better purchase for this user at an acceptable price difference.

## Why this phase exists

Candidate pools are built from search results, and search results lag the catalogue. A model can look like the best option and still be the previous generation, about to leave the shelves, or sitting next to a cheaper variant that does the same job. The user would find out after buying.

The check also re-opens the price and stock of every candidate a second time, hours after the first pass. In a live session that second look caught more than the lineage itself (see the worked example).

## When to run it

- **Phase 5, after the audit**, on every `verified_solution` and every `near_miss`. Not on rejected candidates: a successor of a product that fails two constraints rarely fixes both.
- **Phase 7**, when a constraint change promotes a candidate that never had a lineage check.

## What to look for, per candidate

| Relation | Question | Example |
|---|---|---|
| Successor | Has the same line been replaced? | Evolve2 75 -> Evolve3 75 |
| Step-up | What is the next tier in the range? | Headphone (1) -> Headphone (1) Pro |
| Step-down / variant | Is there a cheaper sibling that keeps the gating specs? | Zone Wireless 2 -> Zone Wireless 2 ES (Bluetooth only) |
| Predecessor | Is the previous generation still sold, cheaper, and good enough? | INZONE H9 still listed next to the H9 II |

Establish the lineage from the **manufacturer's range or catalogue page** first (release date, what it replaces, whether the start model is still listed). Press and retailers are the fallback, and say so.

## How to judge it

Compare **only on the gating specs and the user's ranked criteria**, never on the spec sheet as a whole. A newer model is not better for this user unless it moves a spec they ranked.

For each relation found, record:

- the delta on each gating spec, with source tier (an unchanged figure is a finding too)
- the price delta, at a named merchant with stock, same rules as Phase 5
- a one-line verdict: worth the difference for this use or not, and why

Verdict rules:

- **Promote** a relative into `candidate_pool` whenever it plausibly passes every hard constraint. It is then audited like any other candidate. Never recommend a relative that has not been audited.
- **Flag lifecycle risk** when the start model has been replaced: stock may dry up, firmware support may end, and the price may move either way. Put it in the card's weakest point.
- **Re-verify the start model's price and stock.** A conflict between the first pass and the lineage pass is a source conflict: resolve it at the merchant page before deciding.
- A relative whose gating spec is unpublished is `UNKNOWN`, same as anywhere else.

## Budget

One lineage per candidate, two or three fetches each, manufacturer pages first. Web search quotas can be shared across parallel subagents in one turn (a session hit a 200-search ceiling with five agents), so give each lineage agent an explicit search cap.

## Report

Add a lineage panel to the report: one row per finalist, with the relative checked, what it changes for this user, its price, and a verdict pill (over budget, nothing more, promoted, stock at risk, written in the user's language). Add a `Lineage:` line to each solution card. See `report-template.md`.

## Worked example: ANC headphones, sauna and travel, France, October 2026

Twelve lineages checked for 22 audited candidates, binding criteria being Bluetooth range through a glass door, headset transmit power, and a 300 EUR ceiling.

- **No successor improved a gating spec.** Evolve3 75 kept 30 m, Momentum 5 kept the same 10 mW radio, WH-1000XM6 kept 10 m with slightly lower declared power, Bose QC 2nd gen kept 9 m. Each cost 100 to 215 EUR more. Verdict: not worth it, stated with the reason.
- **A near-miss was promoted.** The INZONE H9 II had been held out because stock was unconfirmed at 199 EUR. The lineage pass for its predecessor found it in stock at 279.99 EUR at a second merchant, under the ceiling. It moved into the verified set.
- **A price conflict was caught and resolved.** A comparator showed 284.99 EUR for a finalist recorded at 189.99 EUR. The merchant page confirmed 189.99 EUR as a promotion on the last unit; the card now says so.
- **A cheaper sibling surfaced.** A Bluetooth-only variant of a finalist at 169.99 EUR, unknown to the first pass. Its stock in France could not be verified, so it was disclosed rather than recommended.
- **Lifecycle risk was flagged** on two finalists that had been replaced in the catalogue.

None of these changed the top recommendation. All of them changed what the user saw, and one changed the verified set.
