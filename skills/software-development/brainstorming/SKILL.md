---
name: brainstorming
description: "Use when user presents an idea for a feature, component, or behavior change — before any implementation, code, or scaffolding."
version: 3.0.0
author: Hermes Agent (adapted from obra/superpowers)
license: MIT
metadata:
  hermes:
    tags: [design, requirements, brd, specification, ideation]
    related_skills: [systematic-debugging, test-driven-development]
---

# Brainstorming Ideas Into BRDs

Help turn ideas into complete, implementation-ready Business Requirements Documents through natural collaborative dialogue.

Start by understanding the current project context, then ask questions one at a time to refine the idea. Output directly to the full BRD format — every section, no exceptions.

<HARD-GATE>
Do NOT invoke any implementation skill, write any code, scaffold any project, or take any implementation action until a curated BRD is approved. This applies to EVERY project regardless of perceived simplicity.
</HARD-GATE>

## The Process

### 1. Explore project context

Check the current project state — existing BRDs, docs, recent commits. Understand what you're working with before asking questions.

### 2. Ask clarifying questions — one at a time

Understand purpose, constraints, and success criteria. One question per message. Prefer multiple choice when possible.

**All sections must be covered.** Use the checklist below to ensure nothing is skipped:

| Section | Must Ask / Confirm |
|---------|-------------------|
| **Overview** | What does this do? Why does it matter? |
| **User Stories** | Who is the user? What do they need? What's the benefit? |
| **Functional Requirements** | What must it do? What's in scope? What explicitly is NOT in scope? |
| **Non-Functional Requirements** | Any latency, availability, scale, or compliance targets? |
| **Observability** | What metrics should we emit? What log events? Any health endpoints needed? |
| **Feature Flag** | Does this need a flag? Server-only or dual (server + browser)? What's the flag name? |
| **Acceptance Criteria** | How do we know it's done? What specific behavior proves it works? |
| **Risks** | What could go wrong? What depends on external systems? |
| **Open Questions** | Anything unresolved that blocks implementation? |

**Observability questions (often skipped — ask explicitly):**
- What should we measure to know this feature is working?
- What counters should we emit? (e.g., `cp_<feature>_<action>_total`)
- What histograms should we track? (e.g., `cp_<feature>_duration_ms`)
- What log events do you want at key transitions?
- Do we need `/ready` or `/live` health endpoints?

**NFR questions (often skipped — ask if not volunteered):**
- Any latency targets?
- Any availability SLA?
- Any data retention or compliance requirements?

**Feature flag questions (ask for every feature):**
- Does this feature need a kill switch or gradual rollout?
- Server-side only, or both server and browser?
- What's the flag name? (format: `ff_enable_<feature>`)

### 3. Propose 2-3 approaches

With trade-offs and your recommendation. Present options conversationally — lead with your recommended option and explain why.

### 4. Present the BRD outline

Before writing, present a high-level outline and confirm scope. Ask:

> "Here's what I'm thinking for the BRD — does this scope look right? [outline]"

### 5. Write the full BRD

Save directly to `specs/<domain>/brd-XX-<slug>.md` using the template below.

**Filename format:** `brd-XX-<slug>.md` where XX is the next available BRD number and slug is a short descriptive name.

**BRD numbering:** Check STATUS.md for the next available BRD number.

### 6. Full section coverage check

After writing, verify every section has real content — not "TBD", not empty dashes, not "to be discussed":

- [ ] **Overview** — 2-3 sentences, clear purpose
- [ ] **User Stories** — at least one, with As a / I want / So that
- [ ] **Functional Requirements** — split Must/Should/Could, no placeholder bullets
- [ ] **Non-Functional Requirements** — specific targets, not vague goals
- [ ] **Observability** — at least one metric and one log event defined
- [ ] **Feature Flag** — name, type, default, and scope (server/browser/both)
- [ ] **Acceptance Criteria** — each AC must be specific and eval-able
- [ ] **Risks & Mitigations** — at least one risk if any exist
- [ ] **Open Questions** — explicitly noted if any, or marked "None"

**If any section is empty or TBD: fill it in or explicitly call it out as a follow-up before asking for user review.**

### 7. User reviews and approves

Ask the user to review the BRD before proceeding:

> "BRD written to `specs/<domain>/brd-XX-<slug>.md`. Full review — every section must have content. Please flag anything that needs changes."

Wait for their response. If they request changes, make them. Only proceed to curation once the user approves.

### 8. Submit for curation

After user approval:

> "BRD approved. Submitting to curation queue."

Log the BRD in STATUS.md under the Backlog table with status "In Curation."

---

## BRD Template

Every BRD uses this structure. Fill ALL sections — no exceptions.

```markdown
# BRD-XX: <Title>

> **Status:** Draft

---

## Overview

What does this feature do? Why does it matter? Keep it to 2-3 sentences.

---

## User Stories

| ID | As a | I want | So that |
|----|------|--------|---------|
| US-1 | ... | ... | ... |

---

## Functional Requirements

### Must Have

- ...

### Should Have

- ...

### Could Have

- ...

---

## Non-Functional Requirements

| Requirement | Target |
|-------------|--------|
| Latency | < Xms for ... |
| Availability | 99.9% uptime |
| Throughput | Supports N concurrent users |

---

## Observability

### Metrics to emit

- `cp_<feature>_<action>_total` — counter (description)
- `cp_<feature>_<action>_duration_ms` — histogram (description)

### Log events

- `<feature>.<action>.started` — when ...
- `<feature>.<action>.completed` — when ...
- `<feature>.<action>.failed` — when ..., error includes ...

### Health/readiness endpoints

- `GET /ready` — returns 200 when ...
- `GET /live` — returns 200 when ...

---

## Feature Flag

- **Flag name:** `ff_enable_<feature>`
- **Type:** boolean
- **Default:** `false`
- **Server env:** `FF_ENABLE_<FEATURE>` (omit if browser-only)
- **Browser env:** `VITE_FF_ENABLE_<FEATURE>` (omit if server-only)

---

## Acceptance Criteria

| ID | Criterion | Eval method |
|----|-----------|-------------|
| AC-1 | ... | ... |

---

## Risks & Mitigations

| Risk | Likelihood | Impact | Mitigation |
|------|------------|--------|------------|
| ... | ... | ... | ... |

---

## Open Questions

- ...
```

---

## Scope Decomposition

If the request describes multiple independent subsystems, flag this immediately. Help the user decompose into sub-projects first. Each sub-project gets its own BRD → curation → implementation cycle.

---

## Curation

Every BRD goes through curation before implementation:

1. **PM review** — confirms scope, priority, user stories, acceptance criteria
2. **Architect review** — confirms technical approach, ADRs, data flows, failure modes
3. **Refiner review** — catches gaps, inconsistencies, naming issues, observability gaps
4. **Curated package** — PM consolidates all input into `specs/curated/brd-XX-<name>.md`

The curated package is the canonical implementation reference. Engineers implement from the curated package, not the raw BRD.

---

## Key Principles

- **Every section, every time** — no skipped sections, no TBD placeholders
- **One question at a time** — Don't overwhelm
- **Multiple choice preferred** — Easier to answer than open-ended when possible
- **YAGNI ruthlessly** — Remove unnecessary features
- **Explore alternatives** — Always propose 2-3 approaches before settling
- **Be flexible** — Go back and clarify when something doesn't make sense

## Red Flags — STOP

If you catch yourself thinking:
- "This is simple enough to skip design"
- "Let me just start building"
- "Observability can be added later"
- "We'll figure out the flag later"
- "This section doesn't apply to our project"

**ALL of these mean: STOP. Every section applies. Fill it.**

## Related Skills

- **`systematic-debugging`** — Use when the task is fixing a bug, not creating something new
- **`test-driven-development`** — Use during implementation for individual tasks