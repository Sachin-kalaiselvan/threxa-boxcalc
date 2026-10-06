# threxa-boxcalc

Free corrugated box costing calculator. Standalone static site, no build step, no
dependencies, no backend. One HTML file plus config.

Live at **boxcalc.theingredientlist.co**

---

## Files

| File | What it is |
|---|---|
| `index.html` | The whole calculator — markup, CSS, JS, print stylesheet. Self-contained. |
| `og-boxcalc.png` | 1200×630 WhatsApp / LinkedIn link preview image. |
| `threxa-logo.png` | **You must add this.** Copy from the ERP repo (`src/assets/threxa-wordmark.png`). White wordmark — the nav sits on a light background, so use the dark version if you have one, otherwise the white one will be invisible. |
| `vercel.json` | Redirects, cache and security headers. |
| `robots.txt` / `sitemap.xml` | Indexing. |

Nothing typed into the calculator is transmitted or stored. All computation is
client-side. There is no analytics script — add one deliberately if you want it.

---

## Deploy

1. New GitHub repo `threxa-boxcalc`, push these files to `main`.
2. Vercel → Add New Project → import the repo.
3. Framework Preset: **Other**. Build Command: leave empty. Output Directory: leave empty.
4. Deploy.
5. Project → Settings → Domains → add `boxcalc.theingredientlist.co`.
6. At your DNS host, add the CNAME Vercel shows you.

Deliberately a separate repo from `threxa`, which has a `vercel.json` routing
bug that serves `index.html` for every non-root path. Nothing here touches that.

---

## Changing the domain

The domain appears in five places. Search and replace
`boxcalc.theingredientlist.co` across:

- `index.html` — `<link rel="canonical">`, `og:url`, `og:image`, `twitter:image`,
  the JSON-LD `"url"`, and the footer line inside the `sheet()` function
- `robots.txt` — the Sitemap line
- `sitemap.xml` — the `<loc>`

If you instead move this to a path on the main site, set the canonical to that
path, not here, or the two URLs will compete for the same search result.

---

## Features

**Costing.** Cutting length and deckle for RSC / HSC / FOL, combined GSM with
flute take-up applied to the fluting medium only, board weight per box, wastage
divided rather than added, grade-wise paper purchase quantities, conversion,
overhead, margin, and GST shown separately at the bottom.

**Share by link.** Every input — box size, ply, each layer's GSM / BF / rate,
allowances, rates, plant name — is encoded into the URL. "Copy link" gives a URL
that reopens the exact costing on any device. This is the distribution mechanism:
one estimator sends a link to his partner, the partner lands on the tool with the
numbers already in it.

**Send to WhatsApp.** Opens WhatsApp with the costing written out as text, with
the share link on the last line.

**Print / Save PDF.** Builds a clean A4 costing sheet headed with the plant's own
name and the customer's name, and calls the browser print dialog. On a phone this
is "Save as PDF" — so the output is something an owner can forward to a customer.

**Download CSV.** Full breakdown including per-layer weights, for Excel.

---

## Known gaps

- Bursting strength is estimated from GSM and BF. It is not a Mullen test and
  says so on the page.
- Flute take-up factors are the common defaults (B 1.36, C 1.45, A 1.55, E 1.27).
  They are editable per calculation but not per plant.
- English only. The ERP has Kannada, Hindi and Tamil; this does not.
- The costing math here and `QuoteCalculator.tsx` in `threxa-erp` are two separate
  implementations. The ERP one has the flute take-up applied to the whole board
  GSM and wastage multiplied instead of divided. **They will disagree in front of
  a customer until the ERP one is fixed.** Fix the ERP to match this, not the
  other way round.
