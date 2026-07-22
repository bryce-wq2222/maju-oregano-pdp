# Maju Oregano PDP — Intelligems Challenger

Mobile-first (390px reference) product page for **Oregano Oil + Black Seed Capsules (60ct)**, built around the verified gut-microbiome-comparison angle. Self-contained: one HTML file, inline CSS, ~90 lines of vanilla JS, no frameworks, no third-party scripts above the fold.

## Preview

Open `index.html` directly in a browser, or serve the folder:

```bash
npx --yes http-server "C:\Users\bryce\maju-oregano-pdp" -p 8087
```

Add-to-cart runs in "preview mode" (console log + button feedback) until the real Shopify IDs are filled in.

## Placeholder map — do NOT launch until these are resolved

| Placeholder | Where | Needs |
|---|---|---|
| `[CARVACROL_%_PENDING_COA]` | Hero chip + lab-proof module | GC potency assay on the **capsule** oil. The ARL COAs on hand are microbial panels for the 1oz/2oz liquid — no carvacrol %, wrong SKU. |
| `[PLACEHOLDER:BATCH_NO]` | Lab-proof module | Current batch number from the COA. |
| `[PLACEHOLDER:COA_PDF_URL]` | Lab-proof module | Hosted capsule-SKU ARL COA PDF. |
| `[PLACEHOLDER:ORDER_COUNT]` | Social-proof strip | "1,000,000+ orders since 2015" matches the homepage claim — confirm substantiation on file. |
| `[UGC_CLIP_1..6]` | UGC wall | Edited, rights-cleared Tribe/TikTok clips; keep `#ad` on incentivized creators. |
| `[FOUNDER_NAME]` / `[FOUNDER_PHOTO]` | Founder note | Real founder only (FTC — no synthetic persona). |
| `[PLACEHOLDER:GALLERY_PHOTOS]` | Gallery | Currently uses live-PDP CDN images; swap AI frames for real photography when delivered. |
| ~~`SHOPIFY_CONFIG`~~ | `<script>` at bottom | ✅ **DONE** — real IDs pulled from the live storefront JSON: product `8408719917242`, variants `45944784126138` (Buy 1) / `45945500172474` (B2G1) / `45945500205242` (B3G2), Recharge selling plan `4483580090` ("Delivery every 30 days"). Variant prices are renewal prices; the $29/$49/$74 intro price is a store-side automatic discount at checkout — verify it applies to carts created from this page (it's price-level config, not page-level). ATC goes direct to `/checkout`. |

Also pending (Junip / Jenny): delete the spam review (Sandra K "Dr Obudu"), hide the 3 disease-claim reviews (H P — covid; Alex B — "antibiotic substitute"; George M — parasites), import Amazon/TikTok reviews, enable post-purchase review requests. Review count on the page is the honest **4.7 / 23** until the import lands. LegitScript status may gate Meta scaling — confirm separately.

## Section order (per brief §4)

Above fold: sticky header → proof strip → swipe gallery → rating/headline/chips → offer chips (B2G1 preselected) → ATC → Andrea S testimonial → trust strip → sticky ATC bar on scroll.
Below fold (all `content-visibility:auto`): UGC wall → testimonial row → **interactive ARL lab-proof module** → vs-probiotics/garlic/papaya/black-walnut comparison → press row → benefits (S/F only) → how to use → sourcing → founder note → 60-day guarantee → FAQ → final CTA.

Removed from the old page per brief: 90-Day Journey before/after, TQ explainer, "Parasitic Defense" chip, "32,354 reviews" badge, us-vs-typical-oregano table.

## Compliance self-audit (brief §6 + MAJU_Oregano_Oil_Compliance_Guide_v2)

- Zero instances of: parasite, kill/eliminate/destroy, cure/treat, named diseases, "clinically proven" (product-level), covid, weight-loss framing, "[X]× more powerful", before/after imagery, fake urgency/countdowns.
- All benefit copy is structure/function ("supports," "helps maintain"); FDA disclaimer appears next to the benefit claims, next to the lab module, and in the footer.
- "Carvacrol has been studied in clinical research" is the approved compound-level phrasing (guide §6).
- Guarantee is money-back only — never tied to a health outcome.
- All testimonials are real Junip reviews quoted with light typo cleanup only; "results may vary" accompanies each block; `#ad` on incentivized UGC.
- No second-person health diagnosis anywhere (Meta personal-attributes rule). Headline A/B variant C ("It's not your probiotic's job — it's this") references the reader's *product*, not their health state — still, prefer variants A/B if Meta review is strict.

## Performance

- Budget: LCP < 2.5s, INP < 500ms, CLS ≈ 0 (current page: 4.9s LCP).
- Hero image: Shopify CDN with `width=800`, `fetchpriority=high`; all other images lazy. Every image has explicit dimensions or `aspect-ratio` — nothing shifts.
- Fonts: Poppins 400/600/700 only, `display=swap`, preconnect.
- No render-blocking third-party JS; below-fold sections use `content-visibility:auto`. Interactions are class-toggles — no re-renders (INP-safe).
- **Run Lighthouse (mobile) against the deployed Shopify URL** — a local run doesn't reflect Shopify's CDN/theme overhead. Record results here.

## Shopify / Intelligems integration — READY

The theme export (`maju-live-version`, 21 JUL 2026) is unpacked in `theme/` with two files added:

- `sections/oregano-challenger.liquid` — the whole challenger page as one section. Placeholders are **theme-customizer settings** (carvacrol %, batch no, COA link, rating/review count, social-proof number, founder name/photo) — fill them in Customize, no code edits.
- `templates/product.oregano-challenger.json` — renders just that section.

Variant/selling-plan IDs are resolved dynamically in Liquid from the product (falls back to the live IDs pulled 7/21). Gallery pulls `product.images` (≤8). ATC posts `{{ routes.cart_add_url }}.js` with `selling_plan`, then goes to `/checkout` — intro pricing ($29/$49/$74) comes from the store's automatic discount, same as the current page.

**Launch steps:**
1. Upload `maju-challenger-theme.zip` as a new **draft theme** (Online Store → Themes → Add theme → Upload zip). Or just add the two new files to the live theme via the code editor — the challenger is only reachable at `?view=oregano-challenger`, so it's invisible until linked.
2. Preview: `/products/oregano-oil-capsules-60ct?view=oregano-challenger` (exact same mechanism as the existing `?view=bso-oregano-gut-health` variants).
3. In Customize, open the template and fill the section settings (carvacrol % stays `[CARVACROL_%_PENDING_COA]` until the GC assay lands — do not launch with the placeholder visible).
4. Note: `theme.liquid` wraps every template with the standard header/footer groups. The challenger has its own sticky mini-header; if the standard header shows too, hide it for this template in Customize.
5. Intelligems: **Split URL test** — control `/products/oregano-oil-capsules-60ct`, challenger the `?view=oregano-challenger` URL, 50/50, primary metric CVR, read mobile separately. Recurring subscription orders excluded by default (desired). Verify the checkout automatic discount fires on challenger-originated carts during QA.
6. Swap the static review row for the lazy-loaded Junip embed once the review import is done.
7. Keep the FDA disclaimer blocks — Meta reviews the LP as the ad destination.

## COA status (checked 7/21)

The three ARL COAs on hand (`coa/` folder) are **microbial panels only** — Lot 2501012 (1oz), 2501013 (2oz), 2503046 (2oz): TPC/Coliforms/Yeast/Mold "None Detected", E. coli/Staph/Salmonella/Pseudomonas "Absent", ISO/IEC 17025:2017 lab #77504. **No carvacrol % exists in any of them, and they're the liquid SKU, not the capsules.** The hero number requires ordering a GC (gas chromatography) potency assay on the capsule oil from ARL. The page's contaminant rows now mirror the real COA wording exactly.
