# Source Verification

How to decide whether a number is true. This is the difference between a useful recommendation and an expensive mistake.

## Source tiers

Rank every figure you record. Store the tier alongside the value.

| Tier | Source | Trust |
|---|---|---|
| 1 | Manufacturer spec sheet, manual, or technical PDF | Authoritative |
| 2 | Manufacturer marketing page | Good, but marketing rounds figures |
| 3 | Measured independent review | Good, and often the only source for real-world figures |
| 4 | Retailer spec field | **Unreliable. Never let it decide alone.** |
| 5 | Retailer marketing copy | Weak |
| 6 | Search result snippet | Not a source. A lead only. |

**Gating rule.** A hard constraint may never be decided by tier 4 or below on its own. Promote it to tier 1-3 or mark the spec `UNKNOWN`.

## Conflict resolution protocol

When two sources disagree on a gating spec:

1. Do not average, do not pick the convenient one, do not proceed.
2. Prefer the higher tier.
3. Apply a plausibility check (below). Implausible figures are usually corrupt, not remarkable.
4. If a tier 1-3 source cannot be reached, the spec is `UNKNOWN`, and `UNKNOWN` excludes.
5. Record the conflict in `unverified` even after resolving, and disclose it if the candidate is recommended.

## Documented failure catalogue

Real errors from one session. Each would have produced a wrong purchase.

**Transposed axes.** A blender listed as `H 26.5 / W 34.6 / D 17.6 cm`. A blender cannot be wider than it is tall. Real height was 47.7 cm, a 21 cm error, and the listing was carton dimensions in scrambled order. Caught by plausibility, not by a better source.

**Height and width swapped.** `H: 15.5 x W: 31.5 x D: 15.5 cm` for a bullet blender. The 31.5 was the height; the square 15.5 footprint gave it away.

**Retailer contradicting the manufacturer on a feature.** A retailer advertised "4 lames amovibles" (4 removable blades). The manufacturer's own site named the image asset `ProEdgeBladesNonDetachable`. The blades do not detach. The feature was a gating constraint, so this single check changed the recommendation.

**Retailer contradicting the manufacturer on capacity.** Retailer said a 2 L jug; the manufacturer said 1.5 L.

**Carton dimensions presented as product dimensions.** `33.9 x 26.1 x 47.4 cm` for a handheld appliance weighing 999 g. That is the box.

**Manual covering a product family.** A manual returned 310 mm for a "900 series"; the actual Pro 900 was 376 mm per the manufacturer and 405 mm as measured in a review. The manual described a smaller sibling.

**Order-of-magnitude typo.** The same blender listed at both "2500 RPM" and "25000 RPM".

**A page that is not what its URL claims.** A retailer "reviews" URL rendered a customer-support contact form. Zero reviews were extractable; the correct conclusion was `UNKNOWN`, not "no reviews exist".

## Plausibility checks

Run these on every gating figure before trusting it.

- **Aspect ratio.** Does the shape make sense for the object class? Taller-than-wide for most countertop appliances.
- **Sum of parts.** Does assembled height match component heights plus overlap?
- **Volume against dimensions.** Compute it. A cylinder of diameter *d* and liquid depth *h* holds `pi*(d/2)^2*h`. Use this to decide whether a requested combination is *possible* before concluding it is merely unavailable.
- **Performance against price.** A figure far outside its price band is usually peak-rated, fabricated, or a different unit.
- **Weight against size.** Catches carton dimensions.
- **Unit sanity.** Watch for factor-of-ten and unit-conversion errors, and for inches quoted as centimetres.

When a check fails, the figure is corrupt until a tier 1-3 source says otherwise.

## Using geometry to bound the search, not just to reject

Plausibility maths cuts both ways. In one session the question was whether a 1.5 L jug could fit under 35 cm. The calculation: a 15 cm-diameter jar needs only 8.5 cm of liquid depth for 1.5 L, so a jar around 20 cm plus a 14 cm base would total ~34 cm. **Geometrically possible**, therefore worth continuing to search. The search eventually showed nobody builds it, but the maths correctly distinguished "impossible" from "unavailable", which are different findings for the user.

## Margin and tolerance

Binary pass/fail discards decision-critical information on threshold constraints.

- Compute and record the **margin** for every threshold constraint.
- `PASS_MARGINAL` when the margin is within measurement error, under ~5% of the threshold, or under ~10 mm on a physical fit.
- Report the margin number in the output, and tell the user to verify physically.

Worked case: a unit measuring exactly 35.0 cm against a 35 cm clearance technically passes and will not physically slide in. Another at 34.3 cm has 7 mm. Both are "PASS" to a naive comparator and they are not equivalent purchases.

## Reviews and reputation rubric

Evaluate in this order and **state which limb you used**:

1. **Model-level rating and count** from a named source. Prefer a destination-market retailer, since another market's SKU may differ. Roughly 20+ reviews before treating the average as meaningful.
2. **Brand-level evidence** when model-level is thin: editorial recommendation rates, category reputation, repairability and parts commitments, warranty length.
3. **Cross-market reputation**, when the user permits it. Strong standing outside the destination market counts if they said so.

Rules:

- **Absence of reviews is `UNKNOWN`, never negative.** And `UNKNOWN` excludes.
- A brand with mixed, product-dependent quality does not satisfy a "good reputation" limb. Say that plainly.
- Watch for review counts pooled across variants or colours.
- A candidate excluded solely on unverifiable reviews belongs in the near-miss panel. The user may reasonably accept that risk, but only if they can see the option.

## Stock, price, and freshness

- **In stock at a named merchant that ships to the destination** is a hard constraint. Out of stock is `FAIL`, not a footnote.
- Timestamp every price and state that prices drift.
- Record the merchant per price. A price nobody will honour is not a price.
- Verify the **destination-market SKU**. Suffixes matter: a US model and its EU sibling differ in voltage, plug, accessories, and sometimes capacity, and their reviews do not transfer.
