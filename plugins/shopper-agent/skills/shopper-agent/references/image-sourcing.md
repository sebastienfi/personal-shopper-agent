# Image Sourcing

Every candidate in the report needs a lead image, so the user can see what they are buying. Images are also a verification source.

Whether images can be downloaded at all depends on the environment, so the Phase 0 probe in `SKILL.md` tests it first. When it cannot, use the to-scale drawing described at the end of this file. When size is one of the user's criteria, add the scale comparison described there as well, photos or not.

## The hard constraint: no external images

The Artifact CSP blocks all external hosts except Google Fonts. Hotlinked product photos **will not render**. Every image must be embedded as a `data:` URI in the HTML.

Consequence: you must download, verify, compress, and inline each image. Budget for it.

## Acquisition ladder

Retailer product pages are the obvious source and often the worst one: they can be JS-rendered, bot-protected, or return an empty shell. One returned nothing but the shop's own name. Which ones answer varies by environment and over time, so try and check what came back rather than relying on a remembered list.

Descend this ladder:

1. **Manufacturer DAM / CDN.** Usually the most reliable source. Fetch the manufacturer product page with `curl` and a browser user-agent, then grep the HTML for image URLs; the media host the page itself references is the one to use.
2. **`og:image` / `twitter:image` meta tags** on any page that does return HTML.
3. **Manufacturer spec-sheet or manual PDF**, which contains product renders.
4. **An alternate-locale manufacturer page** for the same SKU.

Useful extraction:

```bash
UA="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120 Safari/537.36"
curl -sL -A "$UA" --max-time 25 "$URL" \
  | grep -oE 'https?://[^"'"'"' <>]+\.(jpg|jpeg|png|webp)' \
  | grep -viE 'logo|icon|sprite|pixel|banner|flag|avatar' | sort -u
```

## Pitfalls

**Scene7 `is/content/` paths serve video.** On Philips, `images.philips.com/is/content/...` returns MP4 files, not images. Only `is/image/...` paths are images. Always check what you actually downloaded:

```bash
file downloaded.jpg   # "ISO Media, MP4" means you got a video
```

**Placeholder responses return HTTP 200.** A 353-byte "image/jpeg" is an error placeholder. Check the byte size; anything under ~5 KB is suspect.

**Many gallery images are feature graphics, not the product.** Cutaway motor renders, ice-crushing close-ups, and recipe collages all live in the same gallery. You must look at the file, not just download it.

**Guessed SKU-based URL patterns almost never work.** Scrape the real page instead of constructing URLs.

**Portability of shell tools.** `base64 -w0` is GNU; macOS uses `base64 -i file`. Use a fallback:

```bash
base64 -w0 in.jpg > out.b64 2>/dev/null || base64 in.jpg | tr -d '\n' > out.b64
```

`stat -f%z` can be shadowed; `wc -c < file` is portable.

## Mandatory visual verification

**Read every image back before embedding it.** Confirm it shows the right product, and check it against the claimed specs.

This is not a formality. In one session, image inspection produced two verifications that text had not:

- A product photo showed a **tamper**, which implies a lid opening, which satisfied a hard constraint about adding ingredients mid-blend.
- Another photo showed the control panel legend printed on the base, confirming an **auto-clean program** among the presets.

Photos also refute claims. Check control-panel legends, lid and port layouts, included accessories, and physical affordances.

## Prepare for embedding

Target roughly 500-600 px on the long edge and JPEG quality ~80. That lands each image near 35-55 KB, so a three-product report totals well under 200 KB against the 16 MB artifact ceiling.

```bash
sips -Z 560 src.png --out r.png
sips -s format jpeg -s formatOptions 80 r.png --out out.jpg
```

Then inject via a script rather than `sed`, since base64 strings are long enough to break shell substitution. Write the HTML with placeholder tokens and replace them:

```python
html = open(p, encoding="utf-8").read()
html = html.replace("__IMG_PRODUCT__", open("img/product.b64", encoding="ascii").read().strip())
```

## Presentation

**Product shots have white backgrounds and must survive dark mode.** Put the image on an explicitly light "plate" and multiply-blend it, so the white background merges into the plate rather than glowing:

```css
.shot { background: var(--plate); }          /* stays light in both themes */
.shot img { mix-blend-mode: multiply; }
```

Define `--plate` as a near-white value in both the light and dark token sets.

**Always write real `alt` text** describing the product and what is visible.

**Label a substitute image.** If the exact SKU's photo is unavailable and you use a sibling or chassis shot, caption it saying so. Never pass off the wrong image silently; a caption costs a line and preserves trust.

**Attribute.** Credit the manufacturers in the report footer, and note the images are shown for identification.

## When no image can be acquired: draw it to scale

In several sessions the environment could not download any image: the shell had no network access, and web fetch returns text, not image files. A card with an empty frame tells the user nothing. A drawing made from the verified dimensions tells them shape and size, which is often what they were missing.

Rules:

- **Draw only from tier 1-3 dimensions** (`source-verification.md`). A candidate whose dimensions are `UNKNOWN` gets a neutral placeholder that says the dimensions are unverified, not a guessed outline.
- **One scale for all candidates.** Simple inline SVG elevations (front, and side when depth matters), all drawn at the same mm-per-pixel, so they compare at a glance.
- **Draw only what is documented.** Outline, documented ports or controls, and the documented colour. Do not invent styling, logos or textures.
- **Colour matters for apparel and design objects.** Fill with the manufacturer's named colour and print the name. A swatch is honest; a rendered garment is not.
- **Label it.** The caption says it is a to-scale drawing, not a photo, in the user's language (for example "dessin à l'échelle, pas une photo").
- **Link to the real photo.** Put the manufacturer or merchant page next to the drawing.
- **Disclose the lost check.** A drawing is not evidence. The visual verification above did not run, so say so in the confidence disclosure, and do not count any feature as confirmed by a picture.

## Showing scale against an everyday object

When size, fit or discretion is one of the user's criteria (fits under a shelf, goes unnoticed on a desk, fits in a cabin bag), numbers alone do not convey it. Draw the candidates to scale beside one everyday object of standard size. Do this even when photos are available: a photo on a white plate shows what a product looks like, not how big it is.

Choose a reference whose size is standardised and close to the product's own size, and print its dimensions in the caption:

| Product size | Reference | Dimensions |
|---|---|---|
| Earbuds, small accessories | 1 euro coin | 23.25 mm diameter |
| Pocket and desk devices | Bank card (ISO/IEC 7810 ID-1) | 85.60 x 53.98 mm |
| Desktop devices, small appliances | Standard 33 cl can | about 115 mm tall, 66 mm diameter |
| Larger items, documents | A4 sheet | 210 x 297 mm |
| Furniture, clothing, vehicles | The user's own height, when they gave it | as stated |

For clothing, draw a plain outline from the garment's measured dimensions (length, chest or waist width), not a styled garment.

Where a threshold constraint exists, draw it as a line in the same view (a shelf height, a box). That turns this view into the threshold sweep described in `report-template.md`.
