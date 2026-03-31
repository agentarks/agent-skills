---
name: svelte-frontend-edd
description: Build and refine Svelte/SvelteKit frontends using eval-driven development (EDD) with explicit UX, interaction, accessibility, and build verification loops.
version: 1.1.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [svelte, sveltekit, frontend, ui, ux, eval-driven-development, accessibility, performance]
    related_skills: [writing-plans, systematic-debugging, test-driven-development]
---

# Svelte Frontend with Eval-Driven Development

Use this skill when building or refining Svelte/SvelteKit UI, especially interactive product surfaces, landing pages, dashboards, and branded experiences.

Core idea:
- Do not steer only by a spec like "make it nicer"
- Convert frontend work into explicit evals
- Keep one core interaction sacred
- Iterate in small steps and keep only changes that improve the experience without breaking the app

This skill is especially useful for:
- Svelte or SvelteKit apps
- Interactive SVG/canvas/DOM experiences
- Marketing pages with motion and branded delight
- UI polish passes
- Converting vague design asks into measurable acceptance checks

## Principles

1. Protect the core interaction
- Identify the one thing the screen must do
- Example: drag cards, animate bees, route text around shapes, complete checkout, edit form state
- All iterations must preserve this

2. Turn taste into evals
Translate vague requests into concrete checks.

Examples:
- "Make it feel less technical"
  - no engineering metrics visible by default
  - hero copy is customer-facing
  - primary CTA visible above the fold
- "Make it delightful"
  - one visible motion cue on load
  - hover/drag feedback exists
  - motion does not reduce readability
- "Make it ecommerce-ready"
  - featured product visible
  - clear CTA
  - at least one discovery section and one trust/social-proof section

3. Keep changes narrow
- Prefer one high-leverage iteration at a time
- Avoid rewriting everything unless architecture is actually blocking progress

4. Evaluate every iteration
Always verify with hard checks and product checks.

5. Stay stable and reusable
This skill should describe durable frontend practice.
Keep it focused on how to work well in Svelte, not on meta-process or orchestration.

If you use this skill in a broader evaluation setup, that is fine — but the skill itself should stay practical and implementation-focused.

## Hard checks (default)

Run after meaningful changes:

```bash
npm test
npm run build
```

If present, also run:

```bash
npm run check
npm run lint
npm run test:e2e
```

For Svelte projects, build should ideally include `svelte-check`.
If it does not, consider adding it when appropriate.

## Product eval checklist

Use this checklist after each frontend iteration.

### A. Core interaction
- Does the primary interaction still work?
- Does state update correctly?
- Does drag/hover/click behavior still feel coherent?
- Are there any obvious visual glitches or stuck states?

### B. UX clarity
- Can a new user understand the screen's purpose in 3-5 seconds?
- Is the primary CTA obvious?
- Are labels customer-facing instead of internal/technical?
- Is secondary information visually subordinate?

### C. Visual hierarchy
- Is there one clear focal point?
- Are spacing, type scale, and contrast coherent?
- Do decorative elements support rather than compete with content?

### D. Accessibility
- Keyboard interaction still works where relevant
- Interactive elements have role/label support
- Decorative motion respects readability
- Avoid introducing warnings from `svelte-check` or obvious a11y issues
- Prefer semantic HTML unless SVG/custom interaction requires otherwise

### E. Motion / delight
- Motion should clarify or charm, not distract
- Idle animation should be subtle
- Drag/hover should have immediate feedback
- If motion is central, expose a way to reduce/disable it when appropriate

### F. Responsive sanity
At minimum, inspect mentally or manually for:
- desktop
- tablet
- mobile

Check:
- no overflow
- controls don’t dominate mobile
- CTA remains visible
- text remains readable

## Workflow

### Step 1: Inspect the project
Identify:
- framework: Svelte or SvelteKit
- entry points: `src/App.svelte`, `src/routes/+page.svelte`, etc.
- styling approach: CSS, Tailwind, component styles
- build/test commands from `package.json`
- the current core interaction

Read first:
- `package.json`
- main route/component file(s)
- main style file(s)
- any interaction utility files

### Step 2: Define the evals before changing code
Write down:
- core interaction eval
- hard checks
- 3-5 product/UX evals for this task

Example:

```md
Task: Make hero feel more premium

Core interaction eval:
- draggable bees still reroute hero copy

Hard checks:
- npm test
- npm run build

Product evals:
- hero communicates product value above the fold
- technical metrics are hidden by default
- featured product card includes price + trust signal
- decorative elements do not reduce readability
```

### Step 3: Make one focused improvement
Good iteration examples:
- rewrite hero framing
- improve CTA hierarchy
- add one ambient motion system
- hide technical detail behind disclosure
- add one commerce section below hero

Phrase the change as a hypothesis whenever possible:
- "If we hide metrics by default, the page will feel less technical."
- "If we add ambient motion, the first impression will feel more alive."

Avoid bundling 8 unrelated ideas into one change.

### Step 4: Verify
Run hard checks.
Then review against the product eval checklist.

### Step 5: Keep or refine
If the change:
- passes the hard checks
- preserves the core interaction
- improves the product evals

keep it.
Otherwise simplify or revert.

When doing multiple polish passes, explicitly mark each pass as:
- keep
- discard
- keep with simplification


## Svelte-specific guidance

### Choose the smallest effective component boundary
Do not split components too early.
Start with one file when the surface is still simple, then extract only when a boundary becomes real.

Good reasons to extract a component:
- repeated markup
- a clearly separable visual section
- an interaction that has its own local state
- a reusable product card / modal / control group

Bad reasons to extract a component:
- the file feels emotionally large
- you want to feel "more componentized"
- the section is not actually reusable or conceptually separate

A strong default:
- keep page composition in `+page.svelte` or `App.svelte`
- extract leaf components for repeated or isolated UI units

### Prefer reactive derivation for UI state
Use derived state for layout/computation and keep base state minimal.

Good examples:
- base state: bee positions, selected preset, toggles
- derived state: clamped positions, animated positions, computed layout lines

Avoid duplicating state when one value can be derived from another.
If two values can drift out of sync, that is usually a sign the state model is wrong.

### Know when to use local state vs stores
Default to local component state first.
Use stores only when state truly spans multiple distant components or route layers.

Use local state for:
- toggles
- hover/drag state
- one-page controls
- temporary UI selections

Use stores for:
- cross-route user/session preferences
- shared cart state
- global overlays/toasts if they are genuinely app-wide
- shared product filters reused across multiple sections or routes

Do not introduce a store just because the app may grow later.

### Keep interaction math in `.ts` utilities when complex
If geometry/layout logic grows, move it out of `.svelte` into utilities.
This keeps components readable and testable.

Good split:
- `.svelte`: rendering, user input, composition
- `.ts`: layout math, collision, slotting, ranking, formatting helpers

A simple heuristic:
- if logic has multiple loops, collision math, ranking rules, or non-trivial branching, move it out of the component

### Use Svelte for presentation, not for hiding broken logic
Do not rely on reactive magic to cover up unclear state transitions.
If drag/motion logic is confusing, simplify the state model.

Before adding more reactivity, ask:
- what is the actual source of truth?
- which values are user-controlled?
- which values are computed?
- which values only exist for presentation?

### Be explicit about SvelteKit route/data boundaries
In SvelteKit projects, decide early what belongs in:
- `+page.ts` / `+page.server.ts`
- `+layout.ts` / `+layout.server.ts`
- component-local client state

Good defaults:
- route loaders fetch durable page data
- server loaders handle secrets/auth-sensitive data
- client-side local state handles immediate interaction
- avoid fetching the same data in both loaders and `onMount` unless there is a real live-refresh reason

If the task is mostly marketing/landing-page UI, keep the route layer thin and avoid unnecessary loader complexity.

### Prefer accessible interaction surfaces
If using SVG or custom drag targets:
- give interactive groups sensible roles/labels where needed
- keep semantics clean
- avoid shipping known Svelte accessibility warnings
- keep decorative SVG separate from core actions when possible

### Styling guidance for Svelte surfaces
Choose the lightest styling approach that fits the project.

Good defaults:
- existing project styling system first
- CSS files or scoped component styles for small/medium custom work
- utility classes only when the project already uses them coherently

Avoid mixing too many styling models in one feature unless the repo already does that.

## Suggested file structure patterns

### Simple marketing surface
- `src/App.svelte`
- `src/style.css`
- `src/data.ts`
- `src/ui-utils.ts`

### SvelteKit landing page
- `src/routes/+page.svelte`
- `src/routes/+page.ts` or `+page.server.ts` only if route data is truly needed
- `src/lib/data/*.ts`
- `src/lib/ui/*.ts`
- `src/lib/components/*.svelte`

### Interactive branded demo
- `src/App.svelte` or `src/routes/+page.svelte`
- `src/demo-data.ts`
- `src/layout-utils.ts`
- `src/*.test.ts`

## Default eval templates

### Template: branded landing page

Core interaction eval:
- primary hero interaction still works

Hard checks:
- tests pass
- build passes
- svelte-check passes

Product evals:
- hero value proposition visible above fold
- CTA visible and clear
- one trust signal present
- one discovery section below hero
- decorative motion does not harm readability

### Template: dashboard UI refinement

Core interaction eval:
- filters/tables/forms still function

Hard checks:
- tests/build pass

Product evals:
- hierarchy clearer than before
- dense areas easier to scan
- empty/loading/error states remain coherent
- no new console or type issues

### Template: playful interactive experience

Core interaction eval:
- interactive object manipulation still works
- layout/animation updates correctly

Hard checks:
- tests/build pass

Product evals:
- delight is noticeable within first 3 seconds
- idle state feels alive but not noisy
- drag feedback is immediate
- motion does not overpower content

## Good iteration examples

### Example 1: less technical hero
Before:
- metrics shown immediately
- controls dominate first screen

After:
- metrics hidden behind disclosure
- customer-facing hero copy
- product CTA above fold

Why this passes EDD:
- core interaction preserved
- product clarity improved
- technical complexity reduced from default view

### Example 2: ambient motion
Before:
- fun only after user interacts

After:
- subtle idle motion makes first impression stronger
- interaction still works when user takes control

Why this passes EDD:
- delight increased
- no loss of functionality
- motion has clear emotional purpose

### Example 3: homepage expansion
Before:
- only a cool hero

After:
- hero + product discovery + social proof + conversion band

Why this passes EDD:
- concept becomes believable as a product surface
- hero remains anchor
- conversion path is clearer

## Failure modes to avoid

1. Demo-ification
- too many metrics
- too many controls
- internal terminology exposed to users

2. Motion overload
- everything moves
- animation competes with CTA or text
- idle motion reduces calmness/readability

3. Design-only iteration with no verification
- changing visuals without running build/tests
- breaking interactions while polishing

4. Overengineering
- adding new dependencies for minor polish
- splitting into too many components too early

5. Loss of product focus
- the screen becomes a concept board, not a usable interface

## Recommended final report format

When done, summarize as:

```md
What changed
- [short bullets]

Core interaction preserved
- [yes/no + how verified]

Eval results
- Hard checks: [commands passed]
- Product checks: [key criteria satisfied]

Next best iteration
- [one focused recommendation]
```

## Use this skill when the user says things like
- "make this UI better"
- "make it feel more premium"
- "this feels too technical"
- "make it more delightful"
- "turn this into a real product page"
- "refine this Svelte frontend"

## Bottom line

In Svelte frontend work, do not just chase aesthetics.
Define success, preserve the core interaction, iterate narrowly, and keep only changes that pass both technical and UX evals.
