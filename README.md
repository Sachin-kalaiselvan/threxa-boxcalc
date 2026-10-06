# threxa-boxcalc

Free corrugated box costing calculator. Standalone static site — no build step, no
dependencies, no backend. One HTML file plus assets and config.

Live at **https://boxcalc.theingredientlist.co**

---

## Files

All nine sit at the repo root. No folders — Vercel serves `index.html` from the
root, so anything nested breaks the site.

| File | What it is |
|---|---|
| `index.html` | The whole calculator — markup, CSS, JS, print stylesheet. Self-contained. |
| `threxa-logo.png` | Nav wordmark, 520×148. **Light-background variant** — the "THREXA" letterforms recoloured to ink, X mark and tagline left violet. The original on the `threxa` repo is white-on-transparent and is invisible here. Do not swap them. |
| `threxa-icon.png` | Favicon, 260×260. |
| `og-boxcalc.png` | 1200×630 WhatsApp / LinkedIn link preview image. |
| `vercel.json` | Redirects, cache and security headers. |
| `robots.txt` | Indexing permission, points at the sitemap. |
| `sitemap.xml` | Submit this in Search Console. |
| `LICENSE.txt` | Proprietary, all rights reserved. Not an open-source licence — deliberately. |
| `README.md` | This file. |

Nothing typed into the calculator is transmitted or stored. All computation is
client-side. There is no analytics script — add one deliberately if you want it.

---

## Hosting

Deployed on Vercel from this repo, `main` branch, auto-deploy on push.
Framework Preset **Other**, no build command, no output directory.

Domain `boxcalc.theingredientlist.co` is a CNAME at Namecheap pointing to the
target on the Vercel domain card. SSL is issued automatically.

Kept as a separate repo from `threxa` on purpose: that repo has a `vercel.json`
routing rule that serves `index.html` for every non-root path. Nothing here
touches it.

After any push, hard-refresh with Ctrl+Shift+R. Images cache aggressively
(`max-age=31536000, immutable` on `.png`), so a changed logo will look broken
until you do.

---

## Changing the domain

It appears in five places. Search and replace `boxcalc.theingredientlist.co`:

- `index.html` — `<link rel="canonical">`, `og:url`, `og:image`, `twitter:image`,
  the JSON-LD `"url"`, and the footer line inside the `sheet()` function
- `robots.txt` — the Sitemap line
- `sitemap.xml` — the `<loc>`

If you move this to a path on the main site instead, set the canonical to that
path, not to here, or the two URLs compete for the same search result.

---

## Features

**Costing.** Cutting length and deckle for RSC / HSC / FOL, combined GSM with
flute take-up applied to the fluting medium only, board weight per box, wastage
divided rather than added, grade-wise paper purchase quantities, conversion,
overhead, margin, and GST shown separately at the bottom.

**Share by link.** Every input — box size, ply, each layer's GSM / BF / rate,
allowances, rates, plant name, customer name — is encoded into the URL. "Copy
link" gives a URL that reopens the exact costing on any device. This is the
distribution mechanism: one estimator sends a link to his partner, the partner
lands on the tool with the numbers already in it.

**Send to WhatsApp.** Opens WhatsApp with the costing written out as text, share
link on the last line.

**Print / Save PDF.** Builds a clean A4 costing sheet headed with the plant's own
name and the customer's name, then calls the browser print dialog. On a phone
that is "Save as PDF" — so the output is something an owner forwards to a
customer, with a Threxa attribution line in the footer.

**Download CSV.** Full breakdown including per-layer weights, for Excel.

---

## Known gaps

- Bursting strength is estimated from GSM and BF. Not a Mullen test, and the page
  says so.
- Flute take-up factors are the common defaults (B 1.36, C 1.45, A 1.55, E 1.27),
  editable per calculation but not storable per plant.
- English only. The ERP has Kannada, Hindi and Tamil; this does not.
- **The costing maths here and `QuoteCalculator.tsx` in `threxa-erp` are two
  separate implementations and they disagree.** The ERP applies flute take-up to
  the whole board GSM instead of the fluting alone, and multiplies wastage instead
  of dividing. This file is the correct one. Fix the ERP to match it — not the
  other way round — before demoing both to the same customer.
