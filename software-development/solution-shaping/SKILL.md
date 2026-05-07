---
name: solution-shaping
description: Use when collaboratively defining a problem and exploring solution options before implementation. Enforces structured requirements, shape options, fit checks, and consistency across shaping → slicing → implementation.
version: 1.0.0
author: Hermes Agent
metadata:
  hermes:
    tags: [product-design, system-design, planning, scoping, requirements, shape-up]
    related_skills: [system-design-breadboarding, design-reflection, writing-plans]
---

# Solution Shaping Methodology

A structured approach for defining problems and exploring solution options *before* committing to implementation. Works for software features, system architectures, product surfaces, and process designs.

## When to Use

- User asks "how should we build X?" or "what's the scope of X?"
- You need to evaluate multiple solution approaches before picking one
- A previous project suffered from scope creep or ambiguous requirements
- You want to convert a vague idea into an implementable, scoped plan
- The user says "let's shape this" or references Shape Up methodology

## When Not to Use

- Implementation is already underway and the architecture is settled
- The task is a trivial refactor or bug fix with obvious solution
- The user explicitly wants to skip analysis and start coding

## Starting a Session

Offer two entry points:

- **Start from R (Requirements)** — Describe the problem, pain points, or constraints. Build up requirements and let shapes emerge.
- **Start from S (Shapes)** — Sketch a solution already in mind. Capture it as a shape and extract requirements as you go.

There is no required order. Shaping is iterative — R and S inform each other throughout.

## Core Concepts

### R: Requirements

A numbered set defining the problem space.

- **R0, R1, R2...** are members of the requirements set
- Requirements are negotiated collaboratively — not filled in automatically
- Track status: Core goal, Undecided, Leaning yes/no, Must-have, Nice-to-have, Out
- Requirements extracted from fit checks should be made standalone (not dependent on any specific shape)
- **R states what's needed, not what's satisfied** — satisfaction is always shown in a fit check (R × S)
- **Chunking policy:** never have more than 9 top-level requirements. When R exceeds 9, group related requirements into chunks with sub-requirements (R3.1, R3.2, etc.) so the top level stays scannable and forces meaningful grouping

### S: Shapes (Solution Options)

Letters represent mutually exclusive solution approaches.

- **A, B, C...** are top-level shape options (you pick one)
- **C1, C2, C3...** are components/parts of Shape C (they combine)
- **C3-A, C3-B, C3-C...** are alternative approaches to component C3 (you pick one)

Always give shapes short descriptive titles that characterize the approach:

```markdown
## C: Two data sources with hybrid pagination

| Part | Mechanism |
|------|-----------|
| C1   | ...       |
```

Good: "C: Two data sources with hybrid pagination"  
Bad: "C: The solution" (too vague) or "C: Add search to widget-grid by swapping..." (too long)

### Notation Hierarchy

| Level | Notation | Meaning | Relationship |
|-------|----------|---------|--------------|
| Requirements | R0, R1, R2... | Problem constraints | Members of set R |
| Shapes | A, B, C... | Solution options | Pick one from S |
| Components | C1, C2, C3... | Parts of a shape | Combine within shape |
| Alternatives | C3-A, C3-B... | Approaches to a component | Pick one per component |

Keep notation persistent as an audit trail. When finalizing, compose new options by referencing prior components (e.g., "Shape E = C1 + C2 + C3-A").

## Phases

```
Shaping → Slicing
```

| Phase | Purpose | Output |
|-------|---------|--------|
| **Shaping** | Explore the problem and solution space | Shaping doc with R, shapes, fit check |
| **Slicing** | Cut the selected shape into implementable vertical slices | Slices doc with breadboards and work streams |

Never mix slice planning into the shaping phase. Shaping is about *what* and *why*; slicing is about *how* and *in what order*.

## The Fit Check

The central artifact for comparing shapes against requirements.

**Format:**

```markdown
### Fit Check

| Requirement | Shape A | Shape B | Shape C |
|-------------|---------|---------|---------|
| R0: Accept payment | ✅ | ✅ | ✅ |
| R1: Handle refunds | ⚠️ | ❌ | ✅ |
| R2: Support multi-currency | ❌ | ⚠️ | ✅ |
| R3: Subscription billing | ✅ | ✅ | ❌ |
```

**Legend:**
- ✅ Solved directly and cleanly
- ⚠️ Partially solved, or solved but with tradeoffs/edge cases
- ❌ Not solved
- ❔ Undecided / needs more exploration

### Working with an Existing Shaping Doc

When the shaping doc already has a selected shape:

1. **Display the fit check for the selected shape only** — Show R × [selected shape] (e.g., R × F), not all shapes
2. **Summarize what is unsolved** — Call out any requirements that are Undecided, or where the selected shape has ❌

This gives the user immediate context on where the shaping stands and what needs attention.

## Multi-Level Consistency (Critical)

Shaping produces documents at different levels of abstraction. **Truth must stay consistent across all levels.**

### The Document Hierarchy (high to low)

1. **Shaping doc** — ground truth for R's, shapes, parts, fit checks
2. **Slices doc** — ground truth for slice definitions, breadboards
3. **Individual slice plans** (V1-plan, etc.) — ground truth for implementation details

### The Principle

Each level summarizes or provides a view into the level(s) below it. Lower levels contain more detail; higher levels are designed views that acquire context quickly.

**Changes ripple in both directions:**

- **Change at high level → trickles down:** if you change the shaping doc's parts table, update the slices doc too
- **Change at low level → trickles up:** if a slice plan reveals a new mechanism or changes the scope of a slice, the Slices doc and shaping doc must reflect that

### The Practice

Whenever making a change:

1. **Identify which level you're touching**
2. **Ask: "Does this affect documents above or below?"**
3. **Update all affected levels in the same operation**
4. **Never let documents drift out of sync**

The system only works if the levels are consistent with each other.

## Shaping Document Structure

Maintain the shaping doc in a single markdown file with this structure:

```markdown
---
shaping: true
---

# Shaping: [Project Name]

## R: Requirements

| # | Requirement | Status |
|---|-------------|--------|
| R0 | ... | Must-have |
| R1 | ... | Must-have |
| R2 | ... | Core goal |
| R3 | ... | Undecided |
| R4 | ... | Nice-to-have |

## A: [Title]

| Part | Mechanism |
|------|-----------|
| A1   | ...       |
| A2   | ...       |

## B: [Title]

| Part | Mechanism |
|------|-----------|
| B1   | ...       |

## Fit Check

| Requirement | A | B | Selected |
|-------------|---|---|----------|
| R0 | ✅ | ⚠️ | ...       |

## Gaps

- [ ] R3 is undecided — need to resolve before slicing
- [ ] B doesn't solve R2

## Open Questions

- Can we get away with A2 and still satisfy R1?
- What latency is acceptable for the async path in C1?

## Parts

| Part | Mechanism | Shape(s) | Status |
|------|-----------|----------|--------|
| A1   | ...       | A        | Selected |
| B1   | ...       | B        | Rejected |
| C2-A | ...       | C        | Candidate |
```

### Sections Explained

- **R:** Problem definition. Never more than 9 top-level items.
- **A, B, C...:** Shape options with component parts tables.
- **Fit Check:** Requirements × shapes matrix. Selected column tracks the chosen path.
- **Gaps:** Unsolved requirements or known holes.
- **Open Questions by Part:** Questions tied to specific mechanisms.
- **Parts table:** A consolidated register of all mechanisms referenced across shapes, showing which shapes they belong to and their current status.

## Slicing

After a shape is selected, slice it into vertical, end-to-end pieces.

**A slice is a complete vertical cut through the system** that delivers a coherent piece of working functionality. It's not a layer (not "the data model" or "the API"). It's a user-visible or operator-visible chunk of behavior that works end to end.

### Slices Document Structure

```markdown
# Slices: [Project Name]

## S1: [Slice Name]

- **Scope:** What user story or behavior this slice delivers
- **Breadboard:** Reference (link or inline) to the breadboard for this slice
- **Work streams:** Parallel tracks if any (frontend, backend, infra)
- **Parts used:** Which shaping parts (A1, C2, etc.) this slice covers
- **Status:** Not started / In progress / Done

## S2: ...

## Work Stream Diagram

(Mermaid flow showing parallel tracks and dependencies between slices)
```

### Work Streams

When a slice has parallel tracks:

```markdown
### Work Streams for S2

| Stream | Owner | Tasks | Depends On |
|--------|-------|-------|------------|
| UI     | ...   | ...   | API contract from S1 |
| API    | ...   | ...   | — |
| Data   | ...   | ...   | Schema finalized in S1 |
```

Work streams are about *how* the team builds in parallel. They are NOT the same as parts or slices. Use a Mermaid diagram when dependencies are complex.

## Common Pitfalls

1. **Skipping the fit check and jumping to implementation.** The fit check is the core artifact. Without it, you don't know what you're solving for.

2. **Letting requirements depend on a specific shape.** Requirements should be standalone. If R2 only makes sense in the context of Shape B, extract the underlying need and rephrase it.

3. **Mixing "shapes" with "slices."** A shape is a candidate solution approach; a slice is a vertical implementation chunk of the *selected* shape. Don't list "slices" during shaping.

4. ** Updating only the shaping doc and not touching slices.** If you change a part mechanism, the slices doc and any slice plans must reflect that in the same operation.

5. **Forgetting to update the Parts table.** The Parts table is the single source of truth for mechanisms. If you add a new component to a shape, register it there.

6. **Drift between shaping doc and code.** If implementation reveals a new edge case or changes a mechanism, that change must flow back up — update the slices doc, then the shaping doc.

## Quick-Start Recipe

### Scenario: user says "we need to build a notification system"

```
1. Offer entry points: "Start from R (requirements) or S (a shape you already have in mind)?"

2. If R: enumerate requirements collaboratively
   - What notification types? (email, push, in-app, SMS)
   - Who triggers them? (system events, user actions, scheduled)
   - What controls does the user need? (mute, frequency, channels)

3. Capture R0…Rn in the Requirements table with status

4. Sketch shapes:
   - A: Event-driven queue with template engine
   - B: Simple direct API calls, no queue
   - C: Third-party service integration (SendGrid + OneSignal)

5. Run the fit check against all R's

6. Identify gaps and open questions

7. User selects a shape → proceed to slicing

8. Slices doc: vertical cuts (e.g., S1 = email notifications end-to-end,
   S2 = push notifications end-to-end, S3 = user preference controls)
```

## Verification Checklist

- [ ] Shaping doc has ≤ 9 top-level requirements
- [ ] Each requirement states a need, not a solution
- [ ] All shapes have short descriptive titles
- [ ] Fit check covers every R against every active S
- [ ] Selected shape is clear in the fit check
- [ ] Gaps and open questions are tracked explicitly
- [ ] Parts table is up to date
- [ ] When moving to slicing, shapes are NOT mixed with slices
- [ ] Changes at any level trigger updates to all affected levels
