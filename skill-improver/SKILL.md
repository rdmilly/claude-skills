---
name: skill-improver
description: >
  Systematically improve any existing Claude skill through research-driven iteration.
  Use this skill when the user wants to upgrade, refine, or deepen a skill they already
  have. Triggers on: "improve this skill", "make this skill better", "upgrade the skill",
  "iterate on the skill", "deepen", "refine", "v2/v3/next version", "this skill could
  be better", or any request to enhance an existing SKILL.md. Also use when the user says
  "run it again" on a previously-improved skill — this skill is designed for repeated
  passes that go DEEPER each time, not just wider. Complements the skill-creator skill:
  skill-creator builds from scratch, skill-improver refines what exists.
---

# Skill Improver

Take any existing skill and make it measurably better through structured research,
element-by-element analysis, and tested implementation. Each pass goes deeper — not
just wider — so running this repeatedly on the same skill produces compounding quality.

---

## Core Principle: Depth Over Width

The failure mode of skill improvement is adding MORE sections without improving
existing ones. Each pass should:
- Drill deeper into the weakest elements (not add new topics)
- Add code patterns where only prose exists
- Add constraints where only suggestions exist
- Add anti-patterns where only positive guidance exists
- Replace vague language with specific decisions

Width (new sections) is only justified when an audit reveals a complete blind spot.

---

## The Improvement Cycle

Each run follows 6 phases. Track which phase you are in.

### Phase 1: Baseline Audit

Read the skill completely. Produce an element matrix:

| # | Element | Lines | Code? | Anti-patterns? | Specificity (1-5) | Gap Score (1-5) |
|---|---------|-------|-------|----------------|-------------------|-----------------|

Specificity: 1=vague advice, 3=code snippets but generic, 5=complete system with patterns+anti-patterns+constraints
Gap: 1=strong, 3=shallow prose only, 5=missing entirely

Identify the weakest 3 and strongest 3 elements.

### Phase 2: Define Evaluation Criteria

Before improving, define 4-6 scoring criteria (1-5 each).
Score the CURRENT version as baseline. Without a baseline, you cannot prove improvement.

### Phase 3: Research

Target the weakest 3 elements from Phase 1. For each:
1. Web search for current state of the art (2-3 searches)
2. Fetch actual implementations (1-2 site fetches)
3. Search past conversations for lessons learned
4. Identify top 3 improvements per element

Each improvement must be concrete and add a missing dimension:
- Missing code? Add code patterns.
- Has code but generic? Add opinionated defaults.
- Has defaults but no guardrails? Add anti-patterns.
- Has everything but shallow? Add depth/variants.

### Phase 4: Implement

1. Preserve strong elements unchanged
2. Deepen weak elements with researched improvements
3. Add blind spot sections only if identified in Phase 1
4. Version the name field
5. Produce a changelog (DEEPENED/ADDED/REMOVED + line/code counts)

### Phase 5: Test

Build test output(s) that exercise the improved skill.
Rules:
- Test in a DIFFERENT context than any previous test
- Exercise the IMPROVED elements specifically
- Score against the Phase 2 rubric
- If improvement < +2 points, return to Phase 3

### Phase 6: Save & Document

1. Save the improved skill
2. Save the audit alongside it
3. Save test outputs as reference
4. Identify the next 3 weakest elements for the next cycle

---

## Repeated Runs: Going Deeper

When the user says "run it again":
1. Read previous iteration audit and next-priorities
2. Skip structure audit, re-score element matrix
3. Target the 3 elements identified as next priorities
4. Run Phases 3-6

Convergence pattern:
- Run 1: Fix 3 worst gaps (biggest jumps)
- Run 2: Deepen next 3 weakest (medium jumps)
- Run 3: Edge cases, variants, polish (refinement)
- Run 4+: Diminishing returns — suggest stopping

If total improvement < +1 point between iterations, the skill has converged.

---

## Anti-Patterns

DONT: Rewrite everything each pass, add sections without researching, research broadly
instead of specifically, skip baseline scoring, test in same context twice, improve on
vibes instead of rubric, add more than 3 new sections per pass, make skill longer
without making it more specific.

DO: Score before AND after, research real implementations, search past conversations,
produce changelog every iteration, identify next starting point before closing.

---

## Quick Reference: One-Pass Summary

Fast version (no formal eval):
1. Read skill -> list weakest 3 elements
2. Web search each (2 searches per element)
3. Top 3 improvements per element -> implement
4. Build one test output -> eyeball quality
5. Save + identify next 3 priorities
