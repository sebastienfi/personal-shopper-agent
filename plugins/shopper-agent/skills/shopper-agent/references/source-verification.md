# Source Verification

How to decide whether a number is true. This is the difference between a useful recommendation and an expensive mistake.

## Source tiers

Rank every figure you record. Store the tier alongside the value.

| Tier | Source | Trust |
|---|---|---|
| 1 | Manufacturer spec sheet, manual, or technical PDF | Authoritative |
| 2 | Manufacturer marketing page, or the manufacturer's own video channel | Good, but marketing rounds figures |
| 3 | Measured independent review, written or video | Good, and often the only source for real-world figures |
| 4 | Retailer spec field | **Unreliable. Never let it decide alone.** |
| 5 | Retailer marketing copy, or a video reviewer talking without measuring | Weak |
| 6 | Search result snippet | Not a source. A lead only. |

**Gating rule.** A hard constraint may never be decided by tier 4 or below on its own. Promote it to tier 1-3 or mark the spec `UNKNOWN`.

**Video.** A video earns tier 3 only where the reviewer states a measurement they took, and the number is cross-checked, because auto-captions garble figures. Sponsored, gifted, and affiliate videos are demoted and flagged. Video comments are owner reports: a lead and a failure-mode signal, never a gating value. Procedure: `video-evidence.md`.

## Conflict resolution protocol

When two sources disagree on a gating spec:

1. Do not average, do not pick the convenient one, do not proceed.
2. Prefer the higher tier.
3. Apply a plausibility check (below). Implausible figures are usually corrupt, not remarkable.
4. If a tier 1-3 source cannot be reached, the spec is `UNKNOWN`, and `UNKNOWN` excludes.
5. Record the conflict in `unverified` even after resolving, and disclose it if the candidate is recommended.

## Documented failure catalogue

Real errors from live sessions. Each would have produced a wrong purchase.

**Transposed axes.** A blender listed as `H 26.5 / W 34.6 / D 17.6 cm`. A blender cannot be wider than it is tall. Real height was 47.7 cm, a 21 cm error, and the listing was carton dimensions in scrambled order. Caught by plausibility, not by a better source.

**Height and width swapped.** `H: 15.5 x W: 31.5 x D: 15.5 cm` for a bullet blender. The 31.5 was the height; the square 15.5 footprint gave it away.

**Retailer contradicting the manufacturer on a feature.** A retailer advertised "4 lames amovibles" (4 removable blades). The manufacturer's own site named the image asset `ProEdgeBladesNonDetachable`. The blades do not detach. The feature was a gating constraint, so this single check changed the recommendation.

**Retailer contradicting the manufacturer on capacity.** Retailer said a 2 L jug; the manufacturer said 1.5 L.

**Carton dimensions presented as product dimensions.** `33.9 x 26.1 x 47.4 cm` for a handheld appliance weighing 999 g. That is the box.

**Manual covering a product family.** A manual returned 310 mm for a "900 series"; the actual Pro 900 was 376 mm per the manufacturer and 405 mm as measured in a review. The manual described a smaller sibling.

**Order-of-magnitude typo.** The same blender listed at both "2500 RPM" and "25000 RPM".

**A page that is not what its URL claims.** A retailer "reviews" URL rendered a customer-support contact form. Zero reviews were extractable; the correct conclusion was `UNKNOWN`, not "no reviews exist".

**Auto-captions garbling the words that matter.** A video review captioned the brand "Aerogogo" as "arog goo" and "motor" as "mortar". Numbers and units fail the same way. A figure heard only in auto-captions is a lead until a second source agrees.

**An edited video understating a duration.** A reviewer pressed the inflate button and was describing the finished couch twelve seconds of runtime later, while quoting the maker's "within 4 minutes". The wait was cut. A duration read off video timestamps is a lower bound.

## Evidence that is not about this exact unit

The tier ladder ranks *who* said a figure. These rules cover figures that are about something close to the candidate, or measured in a way that cannot be compared. Each one decided a recommendation in a live session.

**Borrowed figures.** A value measured on a sibling model, a shared chip, another variant, a product-family manual, or a "typical" default for the category is `borrowed`. Record what it was borrowed from.

- A borrowed figure can at best support `PASS_MARGINAL`, never `PASS`.
- In any interactive tool, use the conservative end of what it supports, not the convenient one.
- Disclose it on the card and in the confidence disclosure.

Observed: a mini PC's gaming figure came from a more powerful sibling, and the text said to expect 45-55 fps while the report's tool used 55 against a 45 threshold. One chip-level benchmark was copied to six different machines. A headset transmit power of 5.6 dBm, from a single data point, was used as the default for fifteen models that did not publish one.

**Figures without their conditions.** A measurement is only comparable with the conditions it was taken in: noise with its distance and load, fps with its game, settings and resolution, battery life with its volume and codec, range with its obstacles. When the conditions are missing, the figure cannot decide a threshold: treat it as `UNKNOWN`, or `PASS_MARGINAL` at best when it passes by a wide margin.

Normalise where the source allows it. Noise shown as dB above the room's background, with the distance stated, compared reviewers that measured differently.

**Prices that look like errors.** A price far below the same product at other merchants, or the same price for every variant of a range (64 GB and 128 GB both at 1,999.99 EUR, where 128 GB sells for 3,299.99 EUR elsewhere), is a suspected site error. It is `UNKNOWN` until confirmed in a cart or at a second merchant. Never pass a budget constraint on it, and never on a margin of a few cents.

**Conversions you invented.** A conversion factor you set yourself ("10 m advertised is about 5 m in real use") is a `derived` assumption, not a fact. State it as a judgement call with its reasoning, keep it consistent with your own calculations elsewhere in the report, and do not let it alone exclude a candidate your other evidence favours: put that candidate in the near-miss panel with the assumption named. In one session such a proxy pushed out the headset with the second-highest published transmit power, contradicting the report's own link-budget analysis.

**Risks no source can settle.** Some constraints cannot be verified by anyone: electronics in a sauna, a garment's long-term shape, a seller's future service. When the user knowingly accepts the risk, record it as `accepted_risk`. It is neither gated nor hidden: show it as its own chip ("not verifiable"), give what is known (for example published maximum operating temperatures, where manufacturers publish them), and turn it into usage guidance (let it cool before charging). Do not count it as a `PASS`.

## Merchant trust

When the candidate is a single product page, especially one the user linked or an unknown brand, check the seller before the product. Signals that a page is a reseller or dropshipping front:

- No legal entity, registration number or postal address in the legal notice.
- A phone number or company in another country than the one the site addresses.
- A crossed-out "before" price far above what the product sells for elsewhere.
- Delivery times of one to two weeks for a product shipped "from France".
- Vague composition instead of a precise one (for cosmetics, a real INCI list; for electronics, a model number and a declaration of conformity).
- A product URL slug ending in `-copy` or similar duplicates.
- A price two or three times that of an established equivalent.

Fetch the page with a prompt that asks for each of these explicitly, then search for independent reviews of the seller. Report the signals found, not a verdict on the seller's honesty.

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
- A video review in which the reviewer states a measurement they took counts as a measured review. **A manufacturer's own video is not a review**, whatever its title says.
- Video view and like counts are not a rating. Zero extracted comments is `UNKNOWN`, not an absence of complaints.
- A candidate excluded solely on unverifiable reviews belongs in the near-miss panel. The user may reasonably accept that risk, but only if they can see the option.

## Stock, price, and freshness

- **In stock at a named merchant that ships to the destination** is a hard constraint. Out of stock is `FAIL`, not a footnote.
- Timestamp every price and state that prices drift.
- Record the merchant per price. A price nobody will honour is not a price.
- Verify the **destination-market SKU**. Suffixes matter: a US model and its EU sibling differ in voltage, plug, accessories, and sometimes capacity, and their reviews do not transfer.
