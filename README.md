# agent-skills

Shared AI agent skills for Svelte frontend work, playful commerce UI, and eval-driven iteration.

## Included skills

### software-development
- `svelte-frontend-edd`
  - Build and refine Svelte/SvelteKit frontends using eval-driven development.
  - Focuses on hard checks, UX evals, accessibility, motion, and preserving the core interaction.

- `svelte-playful-commerce-workflow`
  - End-to-end workflow for branded ecommerce experiences in Svelte.
  - Combines frontend implementation discipline with commerce-aware design judgment.

### design
- `playful-commerce-ui`
  - Design guidance for playful, brand-led ecommerce interfaces.
  - Focuses on delight, conversion, trust, hierarchy, and restraint.

## Philosophy

These skills are meant to be:
- reusable across projects
- version controlled in one place
- shared by multiple agents/tools
- practical for real implementation, not just role-play personas

The main pattern behind them is eval-driven development:
- define the core interaction
- define explicit success checks
- iterate narrowly
- verify with tests/build and product-facing evals
- keep only changes that improve the experience without breaking the app

## Suggested structure

- `software-development/` for implementation workflows
- `design/` for product/brand/design workflows

Each skill lives in its own folder with a `SKILL.md`.

## Current focus

This repo currently emphasizes:
- Svelte / SvelteKit frontend work
- playful commerce and storefront UX
- branded interactive product surfaces
- eval-driven UI iteration

## Usage idea

If your agent system supports loading Markdown-based skills, point it at the relevant folder or sync these files into the tool-specific skill directory.

A good pairing:
- use `playful-commerce-ui` to shape the product/design direction
- use `svelte-frontend-edd` to implement and verify changes
- use `svelte-playful-commerce-workflow` when you want one combined operating model
