# Image Sourcing

Every candidate in the report needs a lead image, so the user can see what they are buying. Images are also a verification source.

## The hard constraint: no external images

The Artifact CSP blocks all external hosts except Google Fonts. Hotlinked product photos **will not render**. Every image must be embedded as a `data:` URI in the HTML.

Consequence: you must download, verify, compress, and inline each image. Budget for it.

## Acquisition ladder

Retailer product pages are the obvious source and the worst one: they are JS-rendered, bot-protected, or return shells. Observed failures include Darty, Fnac, idealo, Cdiscount, Kaufland and Conforama all yielding nothing useful, and Cdiscount returning literally the word "Cdiscount".

Descend this ladder:

1. **Manufacturer DAM / CDN.** The reliable source. Fetch the manufacturer product page with `curl` and a browser user-agent, then grep the HTML for image URLs. Known-good hosts seen in practice:
   - `dam.groupeseb.com` (Moulinex, Tefal, Krups, Rowenta)
   - `media3.bsh-group.com` (Bosch, Siemens, Neff)
   - `images.philips.com` (Philips)
   - `assets.sharkninja.com` (Ninja, Shark)
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
