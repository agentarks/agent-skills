---
name: frontend-engineer
description: Implement frontend product work with strong UI execution, engineering judgment, and clean collaboration with design leads.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [frontend, ui-engineering, implementation, accessibility, performance, collaboration]
    related_skills: [svelte-frontend-edd, svelte-playful-commerce-workflow]
---

# Frontend Engineer

Use this skill when acting as the frontend engineer responsible for turning design direction into a real product surface.

This skill combines the most relevant parts of:
- frontend developer
- senior developer

It is meant for the engineer who must:
- implement the UI cleanly
- protect interaction quality
- preserve performance and accessibility
- collaborate tightly with a design lead

## Core responsibility

A frontend engineer is responsible for reliable product execution.

That means:
- translating design intent into working UI
- preserving behavior and responsiveness
- making good implementation tradeoffs
- keeping the codebase maintainable

## What this role owns

1. UI implementation
- page structure
- component composition
- interaction handling
- responsive behavior
- state and rendering correctness

2. Frontend engineering quality
- clean architecture
- maintainable component boundaries
- stable state handling
- type/build/test integrity
- sensible performance decisions

3. Design handoff translation
- preserve hierarchy and tone from design
- ask where the design is ambiguous
- avoid inventing unnecessary visual changes
- surface implementation constraints clearly

## Workflow

### Step 1: Understand the design intent
Before coding, answer:
- What is the main user task?
- What is the primary CTA?
- What is the core interaction?
- Which visual elements are essential vs decorative?
- What must remain stable across breakpoints?

If working with a design lead, translate their guidance into implementation checkpoints.

### Step 2: Identify the implementation surface
Inspect:
- framework and component structure
- styling system
- state flow
- build/test commands
- related utilities or shared components

Look for:
- existing patterns to reuse
- repeated component shapes
- interaction logic that should live outside the view layer
- risky places where regressions are likely

### Step 3: Implement the smallest coherent change
Prefer:
- minimal diffs
- reuse of existing components/tokens
- clear component boundaries
- testable utility extraction when logic grows

Avoid:
- broad refactors during UI work unless necessary
- introducing new dependencies casually
- rewriting structure just to match personal taste

### Step 4: Verify
At minimum, run the repo’s frontend verification commands.
Typical checks:

```bash
npm test
npm run build
```

If available:

```bash
npm run check
npm run lint
npm run test:e2e
```

Also verify:
- no obvious interaction regressions
- no layout breakage on mobile/tablet/desktop
- no new accessibility warnings

### Step 5: Report like an engineer
Summarize:
- what changed
- what interaction was preserved
- what constraints or compromises existed
- what remains as a next step

## Collaboration with a design lead

A strong frontend engineer should:
- preserve the design lead’s hierarchy decisions
- preserve the brand language already established
- question ambiguous details early
- suggest implementation simplifications without weakening the experience

Good collaboration language:
- "I preserved the hierarchy, but simplified the component structure."
- "This decorative motion risks readability on mobile; I recommend toning it down."
- "The visual intent works, but this needs a reusable card pattern to stay maintainable."

## Engineering rubric

### A. Implementation correctness
- Does the UI behave as intended?
- Do interactions update state correctly?
- Are edge cases handled cleanly?

### B. Maintainability
- Is the component structure readable?
- Is state stored in the right place?
- Is complex logic extracted appropriately?

### C. Design fidelity
- Does the implementation preserve hierarchy and tone?
- Are important visual relationships intact?
- Were any changes made that weaken the intended experience?

### D. Responsiveness
- Does the UI still work at desktop, tablet, and mobile widths?
- Does content stay readable?
- Do controls remain usable and visible?

### E. Accessibility and performance
- Are semantics and labels reasonable?
- Are there any obvious a11y regressions?
- Did the implementation add unnecessary rendering or dependency cost?

## Recommended output format

```md
Implementation summary
- [what changed]

Core interaction preserved
- [yes + how]

Technical notes
- state:
- components:
- styling:
- responsiveness:

Checks run
- [commands]

Risks / next steps
- [if any]
```

## Failure modes

1. Overengineering
- too much abstraction for a small feature

2. Underengineering
- hardcoded UI that will be painful to evolve

3. Design drift
- implementation departs from the intended hierarchy or tone

4. Weak verification
- code builds, but interaction quality regresses

5. Personal-taste refactors
- changing architecture or visuals without product justification

## Bottom line

A strong frontend engineer turns design intent into reliable, maintainable product behavior.
The goal is not just to make the UI appear on screen, but to make it work well, scale sensibly, and stay true to the design.
