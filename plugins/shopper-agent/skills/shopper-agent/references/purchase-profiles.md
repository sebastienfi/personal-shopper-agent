# Purchase Profiles

The seven phases in `SKILL.md` are written for the default case: find one physical product that meets a set of requirements. Sessions also brought four other shapes of request, and each one bent the rules in the same places. Identify the profile at the start; it changes what each phase gathers and what the report shows. The invariants do not change: verified sources, `UNKNOWN` excludes, near-misses surfaced, judgement calls disclosed.

| Profile | Recognise it by | Main change |
|---|---|---|
| Find a product (default) | "find me", "what should I buy" | None |
| Verdict on a given product | a link or model name, "is this good?" | Verdict first, full search offered after |
| Kit of several items | a wardrobe, a setup, "everything I need for" | Taste first, budget and fit per item |
| Financed offer | lease, subscription, credit, monthly price | Cost over time replaces sticker price |
| Used listing | second-hand, a classified ad, "occasion" | Each listing is a unit with its own evidence |

A request can combine profiles (the car session went from lease offers to used cars on credit). Apply each profile to the candidates it covers.

## Verdict on a given product

The user wants an answer about *this* product. In the beard-dye session the verdict was given, but under a full primer and followed by four interview questions, and the user did not reply. Answer the question first.

1. **Fetch the page first.** This fetch doubles as the Phase 0 fetch probe; run the rest of the probe only if the user takes up the full search. Use a prompt that asks explicitly for the composition or model number, the legal entity and address, the delivery terms, the price history shown, and the negative reviews. Run the merchant-trust checklist in `source-verification.md`.
2. **Run a short primer**, only what is needed to judge it: the category's sub-types, which one this product is, and the trap that matters for it.
3. **Reply verdict first**: a one-line verdict, the two to four reasons with their sources, and the claim-against-reality gap if there is one ("lasts six weeks" is true of the dyed hair, while regrowth shows in one to three weeks).
4. **Then offer the full search**, with at most two questions and a default the user can accept in one word. If the product fails, name one or two established alternatives as leads, labelled as not yet audited.

A published report is only needed once the user takes up the full search.

## Kit of several items

Several items bought together for one purpose: a work wardrobe, a desk setup. Each item is audited as its own product, and the kit is judged as a whole.

**Interview.**

- **Taste and fit first** (`SKILL.md`, Phase 2): the look they want, what they refuse, the brands they already buy and why.
- **Measurements by guide, not by question.** Send a simple diagram with numbered points and how to take each (inseam on a pair of trousers they own, foot length in mm in the evening against a wall). One user sent all eight figures within twenty minutes.
- **Sanity-check outliers** against norms for the stated height and ask for a re-measure. One sleeve figure came back corrected from 50 to 58 cm; an unusually short inseam was confirmed and turned into a hemming requirement on every pair.
- **Budget per item and in total.** A cap on one source ("not everything from Uniqlo") becomes a share cap on the kit, stated as a judgement call.

**Research.** One agent per item (`research-fanout.md`).

**Sizing.**

- Map each measurement to each brand's own size chart.
- When the chart and a size the user already wears in that brand disagree, the size they wear wins.
- A chart on the brand's site is tier 1. A reseller's chart is tier 4, so fit is `UNKNOWN`: the item goes to the near-miss panel marked "size to confirm", unless the user accepts the fit risk (`accepted_risks`), for example when free returns or a try-on in store make a wrong size cheap.
- A chart published only as an image is `UNKNOWN` unless you can read it.

**Report.**

- A fit check with controls preset to the user's measurements, coloured per item (fits, borderline, does not fit).
- An outfit builder in the items' real colours.
- Practical notes: a nearby store where the item can be tried, whether to order two sizes and return one, return terms, and alterations needed.
- Each style or direction is its own artifact (`report-template.md`).

## Financed offer

A lease (LOA, LLD), a subscription, or a purchase on credit: the price is a schedule, not a number. The car session showed every place where the default rules do not fit.

**Price.**

- Model the full schedule: first payment, monthly payment, duration, and what happens at the end (buy-back price, return conditions).
- Gate on the figures the user stated: the monthly ceiling and the first payment they accept.

**Incentives.** Recompute each offer for this buyer's real eligibility (`SKILL.md`, Phase 2). Advertised offers often assume the most generous incentive tier, which most buyers do not get.

**Contract terms are gating specs.** Treat these like any other spec, with sources: mileage cap and excess-mileage price, duration, what is included (maintenance, insurance, assistance), deposit, and early-exit terms.

- A mileage cap below the user's stated mileage is a `FAIL`, not a footnote.
- When the price at the user's real mileage is not published, it is `UNKNOWN`: say what was tried (configurators, dealer pages).

**Sources.** The offer's legal notice ("mention légale") is the tier 1 source for its terms, often reposted verbatim by dealers. A configurator that does not render is a failed fetch, not a value.

**Freshness replaces stock.**

- Record each offer's end date and whether the price is national or dealer-specific.
- An expired offer is a `FAIL`.
- An offer ending within days is flagged on the card.

**Credit.**

- Take the rate from a dated scale at a named source, and show the legal maximum rate where one exists.
- Say whether borrower insurance is included.
- Recompute every payment by script before publishing.

**Compare ways to buy on one basis.** Owning at the end is not the same as returning the car. Show the total paid and what the user holds afterwards. Name what is excluded: insurance, energy, maintenance, resale value.

**Report.**

- A calculator: deposit, monthly ceiling, duration, rate, and incentive toggles. It re-audits leases, new cars on credit and used cars live.
- Each offer card carries its end date.

**Lineage** applies to the model generation, not to the offer. One image of the model serves all its offers.

## Used listing

Each listing is one unit with its own history. The model's spec sheet still applies, but the gating evidence is the unit's own.

**Record per listing:**

- the listing id and URL, and the date it was seen
- the seller type (a professional or a private seller), which changes the warranty and the incentives
- age, mileage or hours of use, and the date of first registration or first use

**Condition evidence.**

- "Declared" or "estimated" condition is the seller's claim, tier 5.
- A certificate or a measured report is tier 1 for that unit. For an electric car, that means a battery health certificate.
- Without one, condition is `UNKNOWN`: the listing goes to the near-miss panel, which says what to ask the seller for.

**Freshness.**

- Listings disappear, so stock becomes "listed on this date".
- Say plainly that the listing may be gone, and recommend acting quickly when it is the pick.

**Price.** Compare against similar listings (same model, age and mileage band), not against the new price.

**Lineage becomes a model-year check.** Did a revision change a gating spec (a battery, a charger, a known fault fixed)?

**Next step.** End with the questions to put to the seller: the certificate, the exact registration date, the service history, and who handles any incentive.
