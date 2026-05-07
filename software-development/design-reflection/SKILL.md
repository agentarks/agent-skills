---
name: design-reflection
description: Use when reviewing a breadboard or system design against its implementation to find design smells, sync documentation to code, and surface hidden complexity.
version: 1.0.0
author: rjs/shaping-skills adapted for Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [system-design, code-review, breadboarding, refactoring, design-smells]
    related_skills: [system-design-breadboarding, solution-shaping, code-review]
---

# Design Reflection

Reflect on a breadboard or system design by syncing it to the implementation, then finding and fixing design smells. Works on existing breadboards built with the `system-design-breadboarding` skill.

A breadboard should be a **legible explanation of how the system produces its effects**. When you look at it, you should be able to account for every behavior — not just see that an effect happens, but understand *how* it happens through the wiring. When someone's mental model doesn't match what the breadboard shows, either their model is wrong or the breadboard is incomplete.

## When to Use

- You have a breadboard that may have drifted from the actual codebase
- You're about to refactor a system and want to understand its real structure first
- The code has accumulated complexity and you want to surface hidden coupling or missing abstractions
- You're reviewing a design document against the implementation for accuracy
- A new developer (or you, months later) needs to understand how the system actually works

## When Not to Use

- The system is trivial (fewer than ~5 affordances)
- You're in the middle of active feature development and can't pause to audit
- The breadboard was never created in the first place (use `system-design-breadboarding` first)

## Core Loop

Reflection is a **two-phase loop**: **SEE**, then **REFLECT**. Always in this order.

```
SEE (sync to code) → REFLECT (find smells) → FIX → REPEAT
```

## Phase 1: SEE — Sync Breadboard to Implementation

The code is ground truth. The breadboard may have drifted or been built from a conceptual shape that doesn't match what was actually implemented. Before looking for design problems, make the breadboard accurate.

### Steps

1. **Read the implementation.** Find the relevant source files. Don't rely on the breadboard's current description of what the code does.
2. **Inspect the seams.** Walk the code using the checklist below — module boundaries, module-level definitions, function signatures, full call chains, decorators, state co-access.
3. **Update the breadboard to match.** Add missing nodes, stores, and places. Fix wrong wiring. Remove stale affordances. The goal is: the breadboard now accurately reflects the code's actual structure — even if that structure has problems.

After Phase 1, the breadboard shows **what IS**, not what should be.

### Reading Seams from the Implementation

#### 1. Module boundaries are seams

Each file or module is a boundary the code has already chosen. Check what crosses it — that's a public interface, and the breadboard should show it. If a module exists (e.g., `llm.py` separate from `app.py`), the breadboard shouldn't reach through it to grab internals. The module's public function is the affordance; what it calls internally is behind the boundary.

#### 2. Module-level definitions are data stores

Constants, configurations, and templates at the top of a module — `TOOLS`, `SYSTEM_PROMPT`, `DEFAULTS` — are static state that shapes behavior. If code reads them to produce effects, they belong in the breadboard as stores. They're easy to miss because they don't change at runtime, but they are still inputs to the system's behavior.

#### 3. Function signatures are affordance boundaries

What a function takes and returns is its contract. The breadboard should show this, not the internal implementation. If a function signature changed since the breadboard was written, update the breadboard.

#### 4. Decorators and middleware are affordances too

Auth decorators, logging middleware, rate limiters — they intercept calls and change behavior. They should appear in the breadboard as code affordances that sit between caller and callee.

#### 5. State co-access reveals place boundaries

If two affordances read and write the same state, they likely belong in the same Place. If an affordance needs state from two unrelated Places, that's a seam to investigate — the boundary may be wrong.

## Phase 2: REFLECT — Find and Fix Design Smells

Phase 1 is preparation. Phase 2 is the point. **Do not skip it.**

Now that the breadboard is accurate, inspect it for design problems. The code's names might be wrong. The split of responsibilities might not be ideal. The wiring might reveal unnecessary coupling or missing abstractions.

### The Checks

Run through each check in order. Record findings as you go.

#### 1. Trace User Stories Through the Wiring

Pick a concrete user story ("user creates a job", "admin bans a user") and trace it through the breadboard from start to finish.

- Does the path tell a coherent story?
- Can you explain every behavior **without hidden steps**?
- Are there gaps where something "just happens" with no affordance responsible?

A smell: you say "and then the job appears in the queue" but the breadboard shows no affordance that enqueues it.

#### 2. Run the Naming Test on Every Affordance

For each affordance:

- **Who is the caller?** (UI user? Another affordance? External system?)
- **What is the step-level effect?** (What changes in the world after this runs?)
- **Can you name it with one idiomatic verb?**

Flag any affordance that resists naming. Common naming-smell patterns:

| Bad Name | Smell | Better Name |
|----------|-------|-------------|
| `processData` | No idea what it does | `filterCandidates`, `normalizeScores` |
| `handleClick` | Event, not effect | `openJobModal`, `submitApplication` |
| `doStuff` | Dumping ground | Split into named affordances |
| `utils.py:helper()` | No coherent responsibility | Move to the place where it's used, give it a real name |

If you can't name an affordance in one verb, it may have multiple responsibilities. Consider splitting it.

#### 3. Check for Hidden Data Stores

For every module-level constant, config object, or template that an affordance reads: is it in the Data Stores table?

If code reads it to produce effects, it's a store — even if it's static. Ask: *"What does this affordance need to know in order to do its job?"* If the answer references something not in the breadboard, it's missing.

Common hidden stores:
- Hardcoded lists or dictionaries (`VALID_STATUSES`, `FEATURE_FLAGS`)
- Template strings stored as module constants
- Environment variable reads scattered in code
- Implicit state in closures or class attributes

#### 4. Question the Places

For each Place:

- Does everything in it share a single responsibility?
- Do state and affordances within it co-access each other?
- Do they stay isolated from other Places?

If a cluster of affordances, state, and UI elements all serve one job (e.g., "turn natural language into structured commands"), they might deserve their own Place — even if the code doesn't separate them yet.

A smell: an affordance in Place A frequently reads state from Place B. The boundary may be wrong.

#### 5. Check for Coupling Smells

Look at the wiring diagram:

- **Spaghetti wiring** — one affordance wires out to 5+ others
- **Back-and-forth** — two affordances call each other repeatedly
- **Orphan affordances** — no Wires Out or Returns To
- **Wrong-direction data flow** — UI affordance directly calling another UI affordance across Places without code in between

#### 6. Review the Smells Table

Compile findings into a structured table:

```markdown
## Design Smells

| # | Smell | Location | Severity | Fix Approach |
|---|-------|----------|----------|--------------|
| 1 | Hidden store | C3 reads `FEATURE_FLAGS` not in table | Medium | Add D4, audit all flag reads |
| 2 | Naming failure | `processData` in P2 | High | Split into `validateJob` + `createJob` |
| 3 | Wrong place | `sendEmail` in P1 (Dashboard) | Medium | Move to P3 (Notifications) |
| 4 | Spaghetti wiring | C5 wires to 7 other affordances | High | Introduce intermediate coordinator |
```

### Fixing Smells

For each smell, decide:

- **Split** — One affordance has multiple responsibilities → make two affordances
- **Merge** — Two affordances are always called together and share state → combine them
- **Rename** — The name doesn't match the effect → give it a truthful name
- **Rewire** — The wiring reveals coupling → introduce boundaries, move affordances between places
- **Extract** — A cluster of affordances deserves its own place → create a new place

After fixing, update the breadboard. Then optionally update the code to match.

## Output Format

Produce a reflection document alongside the breadboard:

```markdown
# Reflection: [System Name]

## Phase 1: SEE — Sync to Code

- Files read: [list]
- Changes made to breadboard: [list]
- New affordances added: [list]
- Removed stale affordances: [list]
- Added missing stores: [list]

## Phase 2: REFLECT — Design Smells

### Naming Test Results

| Affordance | Caller | Step Effect | Verdict |
|------------|--------|-------------|---------|
| ...        | ...    | ...         | ✅ / ❌ |

### Hidden Stores Found

| Store | Read By | Action |
|-------|---------|--------|
| ...   | ...     | Added to D# |

### Place Boundary Review

| Place | Responsibility | Isolation | Verdict |
|-------|----------------|-----------|---------|
| ...   | ...            | Good/Poor | ✅ / ❌ |

### Coupling Check

| Affordance | Wires Out Count | Pattern | Verdict |
|------------|-----------------|---------|---------|
| ...        | ...             | ...     | ✅ / ❌ |

### Smells Table

| # | Smell | Location | Severity | Fix |
|---|-------|----------|----------|-----|
| ... | ... | ... | ... | ... |

## Fixes Applied

- [ ] Split `processData` into `validateJob` + `createJob`
- [ ] Moved `sendEmail` from P1 to P3
- [ ] Added D4: `FEATURE_FLAGS` store
- [ ] Introduced C6: `notificationCoordinator` to reduce spaghetti wiring
```

## Common Pitfalls

1. **Skipping Phase 1 and jumping to design critique.** If the breadboard doesn't match the code, your reflection critiques a fiction, not the actual system.

2. **Treating naming as cosmetic.** Bad names often reveal wrong boundaries. If you can't name an affordance cleanly, the responsibility split is probably wrong.

3. **Forgetting static data stores.** Constants and configs shape behavior as much as runtime state. They're easy to miss because they don't change, but they are still system inputs.

4. **Only reflecting on UI affordances.** Code affordances often carry the real complexity. The naming test and store audit must cover both tables.

5. **Stopping after finding smells without fixing.** The value is in the fix. Update the breadboard, then optionally update the code.

6. **Reflecting on a system you're actively building.** Reflection assumes the code is mostly stable. If you're mid-feature, the breadboard will keep shifting. Wait for a natural boundary (feature complete, refactor point, pre-release).

## Quick-Start Recipe

### Scenario: auditing a notification system before adding SMS support

```
1. Phase 1: SEE
   - Read the notification module(s): notifications.py, email.py, push.py
   - Walk module boundaries: what's public? what's internal?
   - Check for hidden stores: template strings, config dicts, feature flags
   - Update the breadboard to match current code

2. Phase 2: REFLECT
   - Trace story: "user triggers notification → routed to channel → delivered"
   - Naming test: does `sendNotification()` describe email, push, or both?
   - Hidden stores: is `SMS_GATEWAY_URL` in a config or hardcoded?
   - Place boundaries: do email templates live with email logic or in a shared template store?
   - Coupling: does `sendNotification()` call 3 different channel handlers directly?

3. Fix smells BEFORE adding SMS
   - Split `sendNotification` into `routeNotification` + channel-specific senders
   - Extract config into a named Data Store
   - Consider a new Place for "Channel Configuration" if routing logic grows

4. Now add SMS — the system is cleaner and the new integration point is obvious
```

## Verification Checklist

- [ ] Phase 1 completed: breadboard synced to current code
- [ ] All relevant source files were read, not just the breadboard
- [ ] At least one user story traced end-to-end through the wiring
- [ ] Naming test run on every affordance (UI and code)
- [ ] Hidden data stores surfaced and added to the breadboard
- [ ] Place boundaries reviewed for responsibility and isolation
- [ ] Coupling patterns checked (spaghetti, back-and-forth, orphans)
- [ ] Smells table compiled with severity and fix approach
- [ ] Breadboard updated with fixes
- [ ] Code optionally refactored to match improved breadboard
