# Contributing

Thanks for contributing to `agent-skills`.

This repository is the shared source of truth for reusable agent skills, so changes should be easy to review, easy to reason about, and safe for other agents to consume.

## Core rule

Never push directly to `main`.

Always use this workflow:
1. create a branch
2. make the change
3. commit
4. push the branch
5. open a pull request
6. merge through PR review/approval

## Branch naming

Use clear branch names:
- `docs/...` for documentation
- `skill/...` for new skills
- `fix/...` for corrections
- `refactor/...` for structural cleanups

Examples:
- `skill/add-nextjs-frontend-edd`
- `docs/update-contribution-guide`
- `fix/clarify-svelte-workflow`

## Skill structure

Each skill should live in its own folder and include a `SKILL.md`.

Example:

```text
software-development/
  svelte-frontend-edd/
    SKILL.md

design/
  playful-commerce-ui/
    SKILL.md
```

## What makes a good skill

A good skill should be:
- reusable across multiple tasks
- operational, not just persona fluff
- explicit about workflow
- explicit about verification or evals
- scoped to a real type of work

Prefer skills that include:
- when to use the skill
- the core workflow
- concrete checks or evals
- common failure modes
- clear output/reporting expectations

## Preferred editing approach

When updating a skill:
1. keep the scope focused
2. prefer improving clarity over adding hype
3. preserve what already works
4. document real workflows, not generic role descriptions
5. if the skill changes behavior significantly, explain why in the PR

## Pull request expectations

A PR should include:
- what changed
- why it changed
- whether this is a new skill or an update to an existing one
- any impact on related skills

Suggested PR template:

```md
## Summary
- add/update/remove [skill name]
- clarify workflow for [...]

## Why
- explain the need or gap

## Notes
- mention any related skills or expected follow-up work
```

## Review checklist

Before opening a PR, check:
- [ ] no direct push to `main`
- [ ] skill path and folder structure are clean
- [ ] `SKILL.md` is present
- [ ] wording is operational and specific
- [ ] examples are relevant
- [ ] workflow and evals are clear
- [ ] related docs updated if needed

## Philosophy

This repo exists so multiple agents can share the same skill source.
That means consistency matters.

Optimize for:
- clarity
- reuse
- maintainability
- easy review
- real-world usefulness
