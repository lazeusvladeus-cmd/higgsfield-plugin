---
name: explainer-infographic
description: Generate a retro flat-vector explainer infographic image for any topic using Higgsfield (Nano Banana Pro). Produces a single polished infographic with sections, pictograms, donut charts, bar charts, and icon labels in a warm editorial style (cream background, brown/terracotta/navy palette). Use this skill when the user says "explainer infographic", "make an infographic", "infographic about", "stats infographic", "visual breakdown of", "turn this data into an infographic", "retro infographic", or any variation of wanting facts and stats turned into one shareable infographic image. Powered by the Higgsfield MCP, never external image APIs.
---

# Explainer Infographic

Give it a topic and a handful of facts. Get back one polished, retro-style infographic image ready to post or drop into a video.

## What this skill does (in one breath)

1. Collects the topic and the facts that go on the infographic
2. Plans 4-7 visual sections (charts, pictograms, icon grids)
3. Builds one detailed layout prompt with every label and number spelled out
4. Generates the image with Higgsfield (Nano Banana Pro)
5. Checks the output for typos and regenerates if needed
6. Saves it to `outputs/infographics/<topic-slug>/`

## Setup the user needs (check before generating)

- Higgsfield MCP connected. If `balance` returns an error, stop and tell them to connect it in settings.
- 1-2 Higgsfield credits per image.

## THE LOCKED STYLE (copy into every prompt)

```
Retro flat vector infographic poster. Solid cream paper background (#EAE4D6).
Title in a dark brown ribbon banner at the top, bold ALL-CAPS cream lettering.
Color palette: dark brown (#4A3428), terracotta orange (#E0622E), navy blue (#3D4E66),
muted teal (#6E9A96), soft beige (#C9BBA4). Flat 2D vector shapes only, no gradients,
no photorealism, no 3D. Section headers sit on beige label bars in bold ALL-CAPS dark text.
Clean editorial grid layout with generous spacing. Small repeating icon dividers between
sections. Pictogram rows, flat donut charts with callout lines, horizontal bar charts,
sized circle bubbles, flat silhouette illustrations. Vintage print texture feel.
```

This style is the product. Do not improvise a different look unless the user explicitly asks.

## Process

### Step 1: Brief the user with AskUserQuestion

Ask in one batch:

1. **Topic** — what is the infographic about?
2. **Facts source** — "Should I use facts you give me, or research and verify them myself?" Options: I'll paste the facts / Research it for me / Mix of both.
3. **Shape** — Options: Tall poster (3:4), Square (1:1), Vertical story (9:16).
4. **Audience or platform** — where is this going? (social post, video slide, blog, community post)

### Step 2: Lock the facts (ZERO FABRICATION)

This is the rule that overrides everything:

- Every number, percentage, ranking, and claim on the infographic must come from the user OR from a web search you ran and verified.
- NEVER let the image model fill in stats. If a number is not in your prompt, it must not be in the image.
- If researching, prefer official or primary sources. If a stat can't be verified, leave it off.
- Keep it to 5-9 total facts. Crowded infographics fail at a glance.

Write the final fact list to `outputs/infographics/<slug>/facts.md` with a source URL next to each fact.

### Step 3: Plan the sections

Map each fact to a visual block. Pick from:

| Block | Best for |
|---|---|
| Pictogram row (repeated icons) | Quantities per group ("cups per person") |
| Donut chart with callout lines | Share breakdowns (percentages) |
| Horizontal bar chart | Rankings and top-10 lists |
| Big stat + icon | One impressive number |
| Icon grid with labels | Types, categories, variants |
| Sized circle bubbles | Top-5 comparisons |
| Flat map with markers | Geographic data |

Layout rule: 4-7 blocks arranged in a 2-column editorial grid. One hero block (the most interesting stat) gets the largest area.

### Step 4: Build the prompt

Structure:

```
[LOCKED STYLE BLOCK]
Title banner text: "[TITLE IN CAPS]".
Section 1 (top left): [block type] showing [exact labels and exact numbers].
Section 2 (top right): [block type] showing [exact labels and exact numbers].
[...]
All text labels spelled exactly as written. Clean alignment, balanced composition.
```

Spell out EVERY label and number in quotes. The model renders what you write, so write everything.

### Step 5: Generate with Higgsfield

Call the Higgsfield MCP `generate_image` tool with model `nano_banana_2` (best text rendering). Aspect ratio from the brief. Resolution `2k`.

### Step 6: QA pass (mandatory)

Look at the generated image with vision and check:

- [ ] Every label spelled correctly (AI text rendering typos are common)
- [ ] Every number matches `facts.md`
- [ ] No invented extra stats snuck in
- [ ] Style matches the locked look (cream background, flat vector, ribbon title)

If anything fails, fix the prompt (often by simplifying to fewer sections) and regenerate. Tell the user how many credits the retry used.

### Step 7: Deliver

```
outputs/infographics/<topic-slug>/
  ├── facts.md          ← verified facts with sources
  ├── prompt.md         ← the final prompt used
  └── infographic.png   ← the image (or download-urls.md if it stays in Higgsfield)
```

End with the file links and ask: "Want a second variation, a different shape, or different facts swapped in?"

## Rules

- ALWAYS use Higgsfield via the MCP. Never call Gemini, OpenAI, or any external image API directly.
- ALWAYS check `balance` before generating. Below 5 credits, warn the user.
- Numbers in the image must exist in `facts.md`. No exceptions.
- 5th-grade reading level on all labels. Short words. ALL CAPS for headers.
- No em dashes anywhere.
- Max 9 facts per infographic. If the user brings 20, help them pick the best 7.

## When NOT to use this skill

- The user wants a product feature graphic for a listing. Use `product-infographic` instead.
- The user wants the dark cinematic 3D style used in video slides. That's a different locked style.
- The user wants an interactive or HTML infographic. This skill makes one flat image.
