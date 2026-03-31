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

5. Prefer autoresearch-style loops for polish work
When the task is exploratory, run repeated small experiments instead of one giant redesign.
Treat each UI change like an experiment:
- propose one idea
- implement it narrowly
- evaluate it
- keep or discard it

## Autoresearch-style frontend loop

Use this mode when the user asks for things like:
- "make it better"
- "do one more iteration"
- "make it feel more premium"
- "make it less technical"
- "keep improving until it feels right"

The point is not to blindly loop forever.
The point is to use the same discipline as autoresearch:
- small deltas
- explicit scoreboards
- keep winners
- reject regressions

### The frontend experiment cycle

1. Establish baseline
- identify the core interaction
- identify current pain points
- define hard checks
- define 3-5 product evals

2. Pick one hypothesis
Examples:
- hide metrics by default
- add one ambient motion system
- strengthen CTA hierarchy
- improve trust cues near product cards

3. Implement the smallest viable change
- prefer one file or one surface when possible
- avoid wide refactors during exploratory polish

4. Run the evals
- hard checks
- product eval checklist
- quick visual sanity review

5. Keep or discard
- keep if the page is clearly better and the core interaction still works
- discard or simplify if the result is noisier, less clear, or less trustworthy

### Frontend experiment log

When doing multiple iterations, maintain a short log in your notes or final summary.
Use this format:

```md
Iteration: ambient-buzz
Hypothesis:
- subtle idle motion will improve first impression

Change:
- added low-amplitude bee drift and wing flutter

Hard checks:
- npm test ✅
- npm run build ✅

Product eval:
- delight stronger ✅
- readability preserved ✅
- CTA clarity unchanged ✅

Decision:
- keep
```

### Keep / discard heuristic

Keep the change if:
- hard checks pass
- the core interaction feels as good or better
- the page is more understandable, more delightful, or more conversion-ready

Discard or simplify if:
- build/test quality regresses
- the main interaction becomes less stable
- the UI feels busier but not better
- the change only sounds clever without improving the experience

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

### Prefer reactive derivation for UI state
Use derived state for layout/computation, keep base state minimal.

Good examples:
- base state: bee positions, selected preset, toggles
- derived state: clamped positions, animated positions, computed layout lines

### Keep interaction math in `.ts` utilities when complex
If geometry/layout logic grows, move it out of `.svelte` into utilities.
This keeps components readable and testable.

Good split:
- `.svelte`: rendering, user input, composition
- `.ts`: layout math, collision, slotting, ranking, formatting helpers

### Use Svelte for presentation, not for hiding broken logic
Do not rely on reactive magic to cover up unclear state transitions.
If drag/motion logic is confusing, simplify the state model.

### Prefer accessible interaction surfaces
If using SVG or custom drag targets:
- give interactive groups sensible roles/labels where needed
- keep semantics clean
- avoid shipping known Svelte accessibility warnings

## Suggested file structure patterns

### Simple marketing surface
- `src/App.svelte`
- `src/style.css`
- `src/data.ts`
- `src/ui-utils.ts`

### SvelteKit landing page
- `src/routes/+page.svelte`
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
