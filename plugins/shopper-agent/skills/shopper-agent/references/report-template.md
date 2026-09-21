# Report Template

The interactive HTML report is the deliverable. This is its required anatomy and the interaction patterns that earn their place.

Load the `artifact-design` skill before writing. Publish with the `Artifact` tool.

## Non-negotiables

- Single self-contained HTML file. Inline CSS and JS. Images as `data:` URIs. Google Fonts is the only permitted external host.
- Theme-aware across all three viewer states: bare `:root` for light, `@media (prefers-color-scheme: dark)` guarded as `:root:not([data-theme="light"])`, and `:root[data-theme="dark"]`. Define every colour as a token; never define a colour only inside a media or `[data-theme]` block.
- `body` sets an explicit token background.
- Wide content (tables, charts) scrolls inside its own `overflow-x: auto` container. The page body never scrolls sideways.
- `font-variant-numeric: tabular-nums` everywhere digits align.
- Format prices and decimals in the destination locale (`95,44 €` for France, not `95.44 EUR`).

## Structure

### 1. Masthead
Lead with the **headline finding**, not a greeting. If one constraint dominated the search, say so here. Include a compact digest of the brief: constraint values, budget, destination, and a verified-versus-audited count.

### 2. Constraint strip
The active constraint set as a compact row. On a revision, render it as a **diff** with removed / modified / added tags.

### 3. Interactive tool for the binding constraint
**The highest-value element in the report.** Build it whenever one constraint dominates.

The pattern: let the user drag the binding threshold and watch the entire field re-audit live. This converts "here is what qualifies" into "here is what your constraint costs you", which is the question they actually have.

Two shapes that worked:

- **Threshold sweep.** Every candidate drawn to scale against a draggable limit line, with a live pass/blocked tally. Used for a height ceiling: dragging it from 35 to 39 cm visibly tripled the qualifying field, which was the single most actionable fact in the report.
- **Requirement calculator.** Sliders for the use-case inputs, resolving to a computed requirement against a fixed product capability, with a pass/fail verdict box. Used for portion planning against a jug's working fill line.

Keep the maths visible and the verdict unambiguous. Provide a reset control back to the user's stated brief.

### 4. Verified solution cards
One per survivor. Each needs:

- **Lead image** on a light plate (see `image-sourcing.md`)
- Rank label ("best overall", "best value") and an all-pass badge
- Model name plus **destination-market SKU**
- Price with the merchant named
- Spec table with the gating specs, marking margin on threshold constraints and colouring good / marginal values
- A plain-language "why this one", including its **weakest point**. A card with no downside reads as advertising.
- Merchant links

Order cards by a stated ranking rationale, not by price alone.

**Citing a video-sourced figure.** Link to the second and state who said it, how, and the tier, so the user can hear it themselves:

> Inflation: about 90 s - [maker's video, 0:08](https://www.youtube.com/watch?v=ID&t=8s), spoken claim, tier 2, not independently timed

Mark sponsored, gifted, or affiliate sources on the citation itself. Link out only; an embedded player is an external asset and will not render.

### 5. Near-miss / override panel
Visually distinct from verified cards so it cannot be mistaken for a recommendation. For each entry: what it would have won on, the **exact unmet constraint**, and any secondary catch.

This exists because the most interesting finding in a search is often a product that beats the field and fails one rule. The user may rationally overrule that rule. They cannot overrule what you hid.

Rank by how few constraints are missed.

### 6. Audit ledger
Every candidate considered, with jug/spec summary, price, verdict, and binding reason. User-facing, sorted by the dominant spec.

Two jobs: it proves the search was thorough, and it lets the user overrule any single exclusion. Include previously-recommended products that a constraint change has since disqualified, so the history is legible.

### 7. Comparison modal
Side-by-side spec matrix across finalists plus the near-misses. Sticky row headers, colour-coded cells, a verdict row at the bottom.

### 8. Category usage guidance
Three or four practical notes from the domain primer. This is the shopper expertise the user lacked, and it belongs in the artifact rather than only in chat.

### 9. Confidence disclosure
An explicit list of what could not be verified, what was tried, and what the user should check themselves. Name the specific figure and its source tier. Example: a height sourced only from a retailer field with transposed axes, flagged medium-confidence with a recommendation to measure on arrival.

Name every **video-only figure** here: a value whose sole source is a video, with its method (spoken claim, stated measurement, or caption-timestamp lower bound) and any sponsorship. A duration taken from video timestamps is disclosed as a lower bound, since edits understate it.

### 10. Footer
Price timestamp with a drift caveat, source policy note, image attribution, and any category-wide exclusion (for example, all units of an incompatible mains voltage).

## Update in place

Republish to the **same artifact URL** on every revision, by passing the same file path in-session or the `url` parameter otherwise. The user keeps one link across the whole conversation.

Change the `<title>` and favicon only on a hard pivot in what the report is about. A constraint being removed such that the entire product class changes is a hard pivot; adding a candidate is not.

## Accompanying chat summary

The artifact is the reference; the chat message is the briefing. Keep it short and lead with the decision:

1. Headline finding, including any impossibility or binding constraint
2. Ranked recommendation with the deciding reason for each
3. Judgement calls made on the user's behalf, and how to reverse them
4. Anything you could not verify

Do not restate the whole report in chat.
