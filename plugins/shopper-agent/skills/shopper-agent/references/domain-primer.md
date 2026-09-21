# Domain Primer

How to become competent in a product category before interviewing the user.

## Why this phase exists

Without it you ask uninformed questions, the user answers them in good faith, and you discover mid-research that their answers describe a product that cannot exist. The primer moves that discovery to before the interview, where it is cheap and useful instead of late and embarrassing.

The primer also *is* a deliverable. The user asked for help partly because they lack this knowledge. Give it to them.

## Research recipe

Spend 3 to 5 searches on the **category**, not on products. Useful query shapes:

- `how to choose a <category> buying guide what matters`
- `<category> types explained which one for <use case>`
- `best <category> <destination-language>` for market-specific brands and retailers
- `<category> problems after 6 months` / `<category> reddit disappointed`
- `<category> <spec> vs <spec> tradeoff`

Read buying guides and owner complaints, not product listings. You are building a model of the category, not a shortlist.

One or two long-term owner videos ("after 6 months", "1 year later") are worth the same as several articles for failure modes and usage guidance. Captions and comments only; see `video-evidence.md`.

## Required outputs

### 1. Form factors and who each suits

Enumerate explicitly. This defines the search space, and it is the most common blind spot: anchoring on the form factor the user happened to name loses the option that actually fits.

Record for each: typical capacity range, typical size range, price band, and the use case it serves.

### 2. Differentiating specs

The 3 to 5 attributes that predict satisfaction. Everything else is tie-breaking.

### 3. Marketing noise

Specs that look decisive and are not. Naming these lets you refuse to gate on them, and lets you reassure the user when a cheap unit "loses" on a meaningless number.

### 4. Nominal-vs-effective spec traps

Where the advertised figure overstates the usable one. Record the conversion factor. **Gate on the effective figure.** Every category has these:

| Category | Nominal | Effective |
|---|---|---|
| Blenders | "2 L jug" | ~1.5 L working fill line |
| Storage | advertised GB | formatted capacity |
| Batteries | mAh | Wh, then real runtime under load |
| Backpacks | litres | usable litres excluding pockets |
| Monitors | "4K" | actual panel resolution and chroma subsampling |
| Speakers | peak watts | RMS watts |

### 5. Physical and engineering tradeoffs

Which specs fight each other, and at what exchange rate. This is the input to the Phase 3 feasibility check. Write it as a relationship you can compute with, not prose.

### 6. Price bands

What genuinely changes between entry, mid, and premium. Lets you tell the user whether their budget is the binding constraint or is already generous.

### 7. Owner failure modes

What breaks or annoys after months of use. These become soft constraints and usage guidance, and they often matter more than any spec.

### 8. Category usage guidance

The technique notes a knowledgeable owner gives a beginner. These go into the final report.

## Worked example: countertop blenders

Condensed from a real session, to show the shape and the payoff.

**Form factors**

| Form factor | Capacity | Height | Suits |
|---|---|---|---|
| Full jug | 1.5-2 L working | 40-48 cm | Families, batch smoothies, soups |
| Inverted cup ("bullet") | 0.5-1 L | 30-36 cm | Single servings, blend-and-go, minimal washing |
| Personal cordless | 0.4-0.6 L | 27-30 cm | Travel, desk use |
| Immersion / stick | jug-independent | stores flat | Soups in-pot, tight storage, weaker on hard frozen |

**Differentiating specs:** motor wattage *with* rpm, effective jug capacity, blade assembly detachability, lid opening for mid-blend additions, jug material.

**Marketing noise:** peak wattage quoted without rpm, blade count, "titanium coating", preset program count.

**Nominal-vs-effective:** a "2 L" jug has a 1.5 L working fill line. A "1.5 L" jug gives roughly 1.2 L. Confirmed on manufacturers' own documentation.

**Tradeoff, the one that mattered:** assembled height is roughly (jug height + base height). A 2 L jug is 24-26 cm alone; a frozen-capable base is 15-20 cm. So **capacity and total height trade off at roughly 1 L per 6-8 cm**, and any height ceiling under ~38 cm forces you out of the jug form factor entirely.

That single line is what a feasibility check consumes. In the real session it was derived only *after* the interview, and a request for 1.5 L under 35 cm turned out to be geometrically impossible. Had the primer run first, the interview would have opened with "your height limit caps capacity near 1 L, which matters more?" instead of discovering it ten research rounds later.

**Owner failure modes:** thermal cut-outs on consecutive batches; blade assemblies that cannot be detached, trapping residue; powder caking above the blades; jug scratching.

**Usage guidance:** liquid goes in first, frozen fruit last; add protein powder through the lid cap after the fruit is already moving, or it packs against the blades; never run frozen solids dry.

## Turning the primer into questions

Ask only what the primer could not answer and what changes your next action. Good questions after a primer look like:

- A **tradeoff choice**, because the primer found a conflict: "your ceiling caps capacity near 1 L. Hold the ceiling, or tell me the real clearance?"
- A **use-case quantifier**, so you can compute a spec: "how many servings, what size, meal or snack?"
- A **scope resolution**: "is that budget landed at your door, or the sticker price?"

Bad questions after a primer are ones the primer already answered, or ones asking the user to supply a spec you should be deriving.
