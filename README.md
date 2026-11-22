# threxa-boxcalc

free corrugated box costing calculator. one static html file, no build, no deps, no backend.

live: **https://boxcalc.theingredientlist.co**

## files

everything sits at the repo root, no folders. vercel serves `index.html` from root so nesting anything breaks it.

- `index.html` - the whole calculator. markup, css, js, print stylesheet
- `threxa-logo.png` - nav wordmark, 520×148. **this is the light-background version** (THREXA letters recoloured to ink, X + tagline still violet). the one in the `threxa` repo is white on transparent and disappears here. don't swap them
- `threxa-icon.png` - favicon, 260×260
- `og-boxcalc.png` - 1200×630 link preview for whatsapp / linkedin
- `vercel.json` - redirects, cache + security headers
- `robots.txt` - allows indexing, points at the sitemap
- `sitemap.xml` - submit this in search console
- `LICENSE.txt` - proprietary, all rights reserved. not open source, on purpose

nothing you type gets sent or stored, all the maths runs in the browser. no analytics either, add one deliberately if you want it.

## hosting

vercel, `main`, auto-deploy on push. preset **other**, no build command, no output dir.

`boxcalc.theingredientlist.co` is a CNAME at namecheap pointing at the target on the vercel domain card, SSL is automatic.

separate repo from `threxa` on purpose: that one's `vercel.json` serves `index.html` for every non-root path. nothing here touches it.

after a push, ctrl+shift+R. pngs are cached hard (`max-age=31536000, immutable`) so a new logo looks broken until you hard refresh.

## changing the domain

`boxcalc.theingredientlist.co` shows up in 5 places:

- `index.html` - canonical, `og:url`, `og:image`, `twitter:image`, JSON-LD `"url"`, the footer line in `sheet()`
- `robots.txt` - sitemap line
- `sitemap.xml` - `<loc>`

if this moves to a path on the main site, point the canonical at that path, otherwise both urls fight for the same search result.

## what it does

**costing.** cutting length + deckle for RSC / HSC / FOL, combined GSM with flute take-up on the fluting medium only, board weight per box, wastage divided (not added), paper purchase qty per grade, conversion, overhead, margin, GST shown separately at the bottom.

**share by link.** every input (box size, ply, each layer's GSM / BF / rate, allowances, rates, plant name, customer name) goes into the URL. "copy link" reopens the exact costing anywhere. that's the whole distribution play: one estimator sends a link to his partner, partner opens it with the numbers already filled in.

**whatsapp.** opens whatsapp with the costing as text, share link on the last line.

**print / PDF.** clean A4 costing sheet with the plant's name and the customer's name on top, then the print dialog. on a phone that's "save as PDF", so it's something an owner can forward straight to a customer, threxa credit in the footer.

**CSV.** full breakdown incl. per-layer weights, for excel.

## known gaps

- bursting strength is estimated from GSM + BF. not a mullen test, and the page says so
- flute take-up uses the usual defaults (B 1.36, C 1.45, A 1.55, E 1.27). editable per calc, not saved per plant
- english only. the ERP has kannada, hindi, tamil, this doesn't
- **the costing maths here and `QuoteCalculator.tsx` in `threxa-erp` are two separate implementations and they don't agree.** the ERP applies flute take-up to the whole board GSM instead of just the fluting, and multiplies wastage instead of dividing. this one's correct. fix the ERP to match this, not the other way round, before demoing both to the same customer
