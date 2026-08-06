---
name: product-infographic
description: Turn a product URL or product photo into a clean listing infographic with feature callouts, like the graphics on top Amazon listings and product pages. Paste a link and the skill scrapes the product name, specs, and hero image automatically, then keeps the real product image and adds 4-5 labeled feature callouts with icons and leader lines on a clean white background. Use this skill when the user says "product infographic", "listing infographic", "listing image", "Amazon image", "feature callout image", "product feature graphic", "spec sheet image", pastes a product URL and wants a graphic, or drops a product photo and wants a graphic that sells the features. Powered by the Higgsfield MCP (Nano Banana Pro with the product photo as reference), never external image APIs.
---

# Product Infographic

Paste a product URL (or drop in a photo). Get back a listing-ready infographic: the real product, big and centered, with clean feature callouts pointing at the parts that matter.

## What this skill does (in one breath)

1. Takes a product URL and scrapes the name, specs, and hero image (or takes a photo + features directly)
2. Confirms the extracted details with the user
3. Imports the product image into Higgsfield as a reference
4. Generates the infographic with the real product preserved
5. Checks likeness and label spelling
6. Saves it to `outputs/product-infographics/<product-slug>/`

## Setup the user needs (check before generating)

- Higgsfield MCP connected. If `balance` returns an error, stop and tell them to connect it in settings.
- A product URL (best) or a clean product photo.
- 1-2 Higgsfield credits per image.

## THE LOCKED STYLE (copy into every prompt)

```
Clean e-commerce product infographic. Solid white to very light gray background (#F7F7F7).
The exact product from the reference image rendered large on the left side, photorealistic,
studio lighting, subtle soft shadow. On the right: a vertical column of feature callouts.
Each callout = a light gray circular badge with a simple thin-line black icon, next to it
the feature value in large bold dark text (example: "4K 120fps") with a smaller lighter
descriptor word below it (example: "Video"). Thin light gray leader lines connecting each
callout badge to the matching part of the product. At the bottom center: the brand and
product name in large dark modern sans-serif, with a short tagline below in lighter text.
Minimal, premium, lots of white space. No clutter, no decorative graphics.
```

This is the DJI-listing look. Do not improvise a different layout unless the user asks.

## Process

### Step 1: Get the product

**URL mode (default).** If the user gave a product URL (or says they have one), scrape the page:

- Use the best available web tool, in this order: Firecrawl scrape if connected, otherwise the built-in web fetch.
- Extract: brand, product name, the 5-8 strongest specs (value + short descriptor), any official tagline, and the hero image URL (the main product photo, largest and cleanest, white background preferred).
- If the page is JavaScript-heavy and the fetch comes back empty, fall back to browser tools or ask the user for a screenshot/photo instead.

**Photo mode.** If the user gave a photo instead, ask for: 4-5 features (value + one-word descriptor), brand + product name, and a tagline (offer to draft 3).

### Step 2: Confirm before generating (mandatory in URL mode)

Show the user what was scraped with AskUserQuestion in one batch:

1. **Features** — "I pulled these specs. Which 4-5 go on the graphic?" List the scraped specs as multi-select options.
2. **Tagline** — the official one from the page, plus 2 drafted alternatives.
3. **Shape** — Options: Square listing (1:1), Tall listing (4:5), Wide banner (16:9).

Never skip this. Scraped pages sometimes mix specs from bundles, variants, or accessories, and the user knows their product.

### Step 3: Lock the features (NO INVENTED SPECS)

- Every spec on the image must come from the product page or the user. Never round, never guess, never fill gaps.
- A wrong spec on a listing is a real business problem for the user.
- 4-5 callouts max. Six or more breaks the layout.

Write the final list to `outputs/product-infographics/<slug>/features.md` with the source URL.

### Step 4: Get the product image into Higgsfield

- URL mode: pass the scraped hero image URL to `media_import_url` and keep the returned media_id.
- Photo mode: upload via `media_upload` and keep the media_id.
- Look at the image with vision first so the callout leader lines point at real parts (lens, screen, battery area, ports).

### Step 5: Build the prompt

Structure:

```
[LOCKED STYLE BLOCK]
The product is the exact product from the reference image, angled three-quarter view.
Callout 1 (top): icon of [icon idea], bold text "[VALUE 1]", descriptor "[WORD 1]", leader line to [product part].
Callout 2: icon of [icon idea], bold text "[VALUE 2]", descriptor "[WORD 2]", leader line to [product part].
[...]
Bottom center: "[BRAND PRODUCT NAME]" in large dark text, tagline "[TAGLINE]" below.
All text spelled exactly as written.
```

### Step 6: Generate with Higgsfield

Call the Higgsfield MCP `generate_image` tool with model `nano_banana_pro`, passing the product image media_id as a reference. Aspect ratio from the brief. Resolution `2k`.

### Step 7: QA pass (mandatory)

Look at the result with vision (or walk the user through it if the sandbox can't fetch the output) and check:

- [ ] The product actually looks like the real product (logo, color, shape, buttons)
- [ ] Every feature value and descriptor spelled exactly right
- [ ] No extra invented specs or badges
- [ ] Leader lines point at sensible parts
- [ ] Brand name and tagline correct at the bottom

If the product drifted, regenerate with: "Do not change the product. Reproduce the reference product exactly." If text typos persist, reduce to 4 callouts and retry.

### Step 8: Deliver

```
outputs/product-infographics/<product-slug>/
  ├── features.md       ← scraped/confirmed features with source URL
  ├── prompt.md         ← the final prompt used
  └── infographic.png   ← the image (or download-urls.md if it stays in Higgsfield)
```

End with file links and ask: "Want a second angle, a different shape for another marketplace, or a version with different features?"

## Rules

- ALWAYS use Higgsfield via the MCP. Never call Gemini, OpenAI, or any external image API directly.
- ALWAYS check `balance` before generating. Below 5 credits, warn the user.
- ALWAYS confirm scraped specs with the user before they go on the image. Scraping is the convenience; confirmation is the safety net.
- ALWAYS pass the product image as a reference. Text-only product description is the fallback, and you must tell the user likeness may drift.
- Specs come from the product page or the user only. Never invent a number.
- 4-5 callouts max. White background. No em dashes anywhere.

## When NOT to use this skill

- The user wants a stats/data infographic about a topic. Use `explainer-infographic` instead.
- The user wants a UGC-style ad with a person. Use `product-to-ad` instead.
