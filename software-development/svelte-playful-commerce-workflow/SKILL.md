---
name: svelte-playful-commerce-workflow
description: End-to-end workflow for designing and building playful ecommerce experiences in Svelte using eval-driven frontend iteration and brand-led commerce design.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [svelte, ecommerce, playful-ui, workflow, frontend, design, eval-driven-development, brand]
    related_skills: [svelte-frontend-edd, playful-commerce-ui, writing-plans]
---

# Svelte Playful Commerce Workflow

Use this skill when you need to create or refine a branded ecommerce surface in Svelte/SvelteKit that should feel delightful, distinctive, and conversion-aware.

This skill combines:
- `svelte-frontend-edd` for implementation and eval loops
- `playful-commerce-ui` for design direction and commerce judgment

It is the bridge between:
- “make this frontend work well”
and
- “make this storefront feel memorable and on-brand”

## When to use this skill

Use it for:
- interactive ecommerce landing pages
- branded storefront homepages
- product launch microsites
- DTC hero sections with motion or custom interaction
- gift/bundle/subscription commerce surfaces
- Svelte marketing sites with a strong visual metaphor

Examples:
- honey brand with draggable bees
- tea brand with drifting steam and ritual collections
- bakery shop with playful product storytelling
- candle brand with glow-based layout accents

## Core philosophy

1. Start from the brand toy
Pick one memorable, playful brand device.
Examples:
- bees
- ribbons
- steam curls
- stars
- petals

2. Protect the shopping path
The user must still quickly understand:
- what is being sold
- why it matters
- what to click next

3. Use eval-driven iteration
Do not just “polish until it feels good.”
Define explicit checks, make one focused change, verify, and keep only what improves the experience.

4. Keep the hero and the storefront in the same language
A magical hero attached to a boring generic product grid is a design failure.
The page below the fold should continue the same brand world.

## Combined workflow

### Phase 1: Discover the brand and the page’s job
Write down three things:

1. Brand toy
- What playful element makes this brand memorable?

2. Emotional promise
- “This storefront should feel like ______ while helping customers quickly ______.”

3. Core interaction
- What is the single most important interaction or dynamic behavior?

Example:
- Brand toy: bees shaping the page
- Emotional promise: warm illustrated gift shop + easy honey discovery
- Core interaction: draggable bees reroute hero copy

### Phase 2: Define evals before building
Use both technical and product evals.

#### Hard checks
At minimum:

```bash
npm test
npm run build
```

If available, also run:

```bash
npm run check
npm run lint
npm run test:e2e
```

#### Product evals
Always define:

1. Core interaction eval
- the signature interaction still works

2. Brand eval
- the page feels distinct from a generic template
- the brand metaphor is visible and coherent

3. Commerce eval
- value proposition visible above the fold
- primary CTA clear
- at least one obvious path into shopping

4. Trust eval
- products feel purchasable, not just decorative
- price, reviews, reassurance, or shipping cues exist where appropriate

5. Restraint eval
- decorative elements do not overpower reading or CTA clarity

### Phase 3: Build in the right order
Recommended order:

1. Hero and core interaction
2. Primary CTA and featured product
3. Discovery sections (collections/categories)
4. Social proof / reassurance
5. Lower-page conversion band
6. Optional backstage or technical detail, if relevant

This keeps the experience emotionally strong and commercially coherent.

### Phase 4: Iterate one layer at a time
Good focused iterations:
- make hero less technical
- improve CTA hierarchy
- add one ambient motion system
- add one collection section
- strengthen product cards
- improve testimonials / trust area
- add one sticky conversion band

Bad iterations:
- rewrite visual system, copy, motion, navigation, and commerce structure all at once

### Phase 5: Verify and keep/reject
Keep a change only if:
- hard checks pass
- core interaction remains strong
- brand feel improves
- shopping clarity improves or remains clear

Reject or simplify if:
- the UI becomes more confusing
- motion overwhelms the page
- the page feels more like a demo than a store
- the storefront below the hero does not match the hero’s voice

## Implementation split

### Use `playful-commerce-ui` for:
- defining the brand toy
- emotional promise
- section planning
- visual and copy heuristics
- deciding where whimsy belongs
- judging whether the page still converts

### Use `svelte-frontend-edd` for:
- Svelte/SvelteKit file inspection
- state and layout architecture
- utility extraction
- hard check loops
- accessibility and responsive sanity
- preserving core interaction through iterations

## Default page blueprint

For a playful commerce homepage, this structure usually works well:

1. Hero
- headline
- emotional value proposition
- signature playful element
- primary CTA
- supporting CTA
- featured product or trust cue

2. Interactive stage
- branded motion or interaction
- clear invitation to play
- not required to understand basic shopping path

3. Collection section
- by mood / ritual / use case / gifting intent

4. Featured products
- shoppable cards with price and quick action

5. Social proof or trust
- testimonials, reviews, shipping promises, gift note info

6. Conversion band
- gift box CTA, subscribe CTA, shop all CTA

## Common failure modes

### 1. The “concept art” problem
The hero is gorgeous, but the rest of the page doesn’t help anyone shop.

Fix:
- add collections
- add product cards
- add trust and CTA structure

### 2. The “demo panel” problem
Controls and metrics dominate the experience.

Fix:
- hide backstage details
- make shopper-facing content primary

### 3. The “generic lower page” problem
The hero is playful, but lower sections look like a standard template.

Fix:
- carry forward the same palette, motifs, copy voice, and interaction tone

### 4. The “too much whimsy” problem
Everything moves, everything sparkles, and nothing converts.

Fix:
- keep one strong signature behavior
- reduce decorative competition
- preserve focal hierarchy

### 5. The “frontend-only” problem
The code is solid, but the emotional promise is weak.

Fix:
- return to the emotional promise sentence
- rewrite copy and hierarchy before adding more code

## Suggested operating checklist

Before implementation:
- [ ] identify brand toy
- [ ] write emotional promise
- [ ] define core interaction
- [ ] define hard checks
- [ ] define 3-5 product evals

During implementation:
- [ ] keep one iteration focused
- [ ] preserve core interaction
- [ ] keep CTA clarity strong
- [ ] ensure the page still feels like a store

After implementation:
- [ ] run tests/build/checks
- [ ] verify product evals
- [ ] verify responsive sanity
- [ ] summarize what improved and what the next best iteration is

## Recommended summary format

```md
Brand toy
- [what playful element anchors the experience]

Emotional promise
- [one sentence]

Core interaction preserved
- [yes + how verified]

What changed
- [bullets]

Eval results
- Hard checks: [commands passed]
- Brand/commerce checks: [key criteria satisfied]

Next best iteration
- [one focused improvement]
```

## Bottom line

A strong Svelte playful-commerce experience is not just a well-built frontend and not just a pretty brand concept.
It is a controlled marriage of:
- memorable brand interaction
- clear shopping flow
- eval-driven implementation discipline

Use `playful-commerce-ui` to decide what the experience should feel like.
Use `svelte-frontend-edd` to build it without breaking the product.
