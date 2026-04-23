---
name: skill-enhancer
description: A methodology for systematically improving an existing skill by deepening its steps, pruning low-value additions, and validating that each change improves output quality. Use this skill when the user wants to improve a skill they already have, deepen a methodology, add rigor to a process, go from v1 to v2 of any skill, or when a skill feels too thin or too bloated. Also trigger on "enhance this skill", "improve this process", "this skill needs more depth", "this skill is too long", or "make this skill better". This skill is about IMPROVING existing skills, not creating new ones (use process-extraction for creation).
---

# Skill Enhancer

A methodology for taking an existing skill from v1 to v2 (or vN to vN+1) by expanding what's thin, pruning what's bloated, and validating that every change earns its place. The key insight: enhancement is not just addition. It's addition + subtraction + validation.

Extracted from the process of enhancing the Concept Engineering skill from v1 to v2, then recognizing that the v2 had additions that added complexity without proportional value.

## When to use this skill

- A skill exists but feels too shallow (thin steps, missing edge cases)
- A skill exists but feels too long (bloated steps, academic additions, context window waste)
- A skill was just created and needs a quality pass before publishing
- A skill has been used 3-5 times and the user has feedback on what works and what doesn't
- The user says "make this skill better" or "this process could be improved"

## The four-phase process

### Phase 1: Audit the current skill

Before adding or removing anything, understand what's there and how it performs.

**Step 1.1 — Step-by-step inventory**

For each step in the skill, record:
- **What it produces:** What's the specific output of this step? (If you can't name a concrete output, the step is vague.)
- **How long it takes:** Is this step 30 seconds of thinking or 10 minutes of research? Steps that take disproportionate time relative to their value are candidates for pruning.
- **Dependency:** Does any later step require this step's output? If nothing downstream uses it, it might be unnecessary.
- **Failure frequency:** When using the skill, which steps most often produce weak results? Those need deepening.

**Step 1.2 — Output quality check**

Look at the skill's final output format. Ask:
- Does every output element trace back to a specific step that produces it? (Orphan outputs = missing steps. Steps with no output = bloat.)
- When the skill was last used, which output elements were most useful to the user? Which were ignored?
- Is the output format right-sized? Too many elements = overwhelming. Too few = incomplete.

**Step 1.3 — Context budget estimation**

Estimate how many tokens the skill consumes when fully executed:
- Skill instructions themselves: count lines, estimate ~4 tokens per word
- Typical output per step: how much text does each step generate?
- Total skill execution: instructions + all step outputs
- Compare to available context: a skill that consumes 30% of context window to execute leaves 70% for actual work. That's acceptable. A skill that consumes 60% is too heavy.

### Phase 2: Expand what's thin

Now add depth — but with discipline.

**Step 2.1 — Identify thin steps**

A step is "thin" if:
- It tells you WHAT to do but not HOW ("research the landscape" without search strategies)
- It has no failure mode documented (you don't know what going wrong looks like)
- It produces a vague output ("insights" instead of "a ranked list of X")
- When used in practice, the user had to improvise because the step didn't give enough guidance

**Step 2.2 — Generate candidate enhancements**

For each thin step, generate 3 candidate additions using these lenses:

**Lens A — Adjacent domain import:** What would an expert from a different field add to this step? A product manager's perspective on a technical step. A designer's perspective on a data step. An economist's perspective on a planning step. The best enhancements come from outside the skill's native domain.

**Lens B — Failure prevention:** What's the most common way this step fails, and what's a specific countermeasure? Not "be careful" — a concrete rule. "Before placing text in a box, calculate: chars × 8px = minimum width. If the box is smaller, the text WILL overflow." That's a failure prevention rule.

**Lens C — Non-obvious output:** What secondary value does this step produce that the skill currently ignores? The asset inventory step in Revenue Architecture doesn't just list assets — it also reveals which assets you should STOP maintaining. That secondary insight is often more valuable than the primary output.

Generate all 3 candidates per thin step. Do NOT add them yet — they go through the quality gate first.

### Phase 3: Prune what's bloated (THE CRITICAL STEP)

This is the step that was missing from our original process and the reason v2 skills get bloated. Every addition must pass the quality gate. Every existing step must also be re-evaluated.

**Step 3.1 — The value-density test**

For EVERY element in the skill (existing steps AND candidate additions), apply this test:

> **"If I removed this element, would the skill's output quality drop noticeably on the next use?"**

Three possible answers:
- **Yes, quality drops significantly** → KEEP. This element is load-bearing.
- **Quality drops slightly** → KEEP only if it's short. If it's more than 3 sentences, compress it to 1 sentence.
- **No noticeable quality drop** → CUT. This element is decoration.

**Step 3.2 — The usage frequency test**

For elements that pass the value-density test, apply:

> **"In the last 3-5 uses of this skill, was this element actually applied?"**

- **Applied every time** → CORE. This belongs in the main skill body.
- **Applied sometimes** → CONDITIONAL. Move to a "when applicable" subsection or a reference file.
- **Never applied** → CUT, regardless of how clever it is. If the metaphor tournament technique has never been used in practice, it doesn't belong in the skill.

**Step 3.3 — The compression pass**

For elements that survive both tests, ask:

> **"Can this be said in fewer words without losing meaning?"**

A 5-sentence explanation that can be a 1-sentence rule should be a 1-sentence rule. A 3-paragraph example that can be a 1-line example should be a 1-line example. The skill should be as short as possible while remaining as complete as necessary.

Specific compression targets:
- Examples: 1-2 sentences max. If the example needs a paragraph, it's a case study — put it in a reference file.
- Failure modes: 1 sentence each. "Failure mode: reframing into something so abstract it loses contact with the concrete problem."
- Process steps: imperative mood, short sentences. "Ask X. List Y. Score each by Z."
- Enhanced techniques: 2-3 sentences max. If a technique needs a paragraph, it's too complex to be an enhancement — it's a separate step.

**Step 3.4 — The context budget recheck**

After expansion + pruning, re-estimate the context budget. Compare to the Phase 1 estimate:
- If the skill got LONGER: did the value increase proportionally? If the skill grew 50% but output quality only improved 10%, you added too much.
- If the skill got SHORTER: good, but verify nothing essential was cut.
- Target: the enhanced skill should be no more than 30% longer than the original. If it's more than 30% longer, the pruning pass was too gentle.

### Phase 4: Validate and ship

**Step 4.1 — Side-by-side test**

Apply both the old and new versions of the skill to the same problem. Compare outputs:
- Is the new output more complete? (Expansion worked.)
- Is the new output more focused? (Pruning worked.)
- Is the new process faster to execute? (Compression worked.)
- If the new output isn't clearly better, the enhancement failed — revert and try a different approach.

**Step 4.2 — Changelog**

Document exactly what changed and why:
- What was added: list each new element with a 1-sentence justification
- What was removed: list each cut element with a 1-sentence reason
- What was compressed: list each shortened element
- Net change: +N elements, -M elements, X% length change

**Step 4.3 — Version and commit**

Save the enhanced skill with a version bump and a commit message that includes the changelog summary. The old version should remain accessible (git history) in case the enhancement turns out to be a regression.

## The quality gate — summary

Every element in the skill (existing or new) must pass ALL THREE:

```
┌──────────────────────────────────────────┐
│  1. VALUE-DENSITY TEST                    │
│  "If removed, does output quality drop?"  │
│  No → CUT    Slightly → COMPRESS    Yes → ▼ │
├──────────────────────────────────────────┤
│  2. USAGE FREQUENCY TEST                   │
│  "Was this applied in last 3-5 uses?"      │
│  Never → CUT   Sometimes → MOVE   Always → ▼│
├──────────────────────────────────────────┤
│  3. COMPRESSION PASS                       │
│  "Can this be shorter without losing value?"│
│  Compress to minimum viable length    → KEEP│
└──────────────────────────────────────────┘
```

The 30% length cap is the hard constraint. If the enhanced skill is more than 30% longer than the original, either the additions aren't dense enough or the pruning wasn't aggressive enough.

## Anti-patterns to watch for

| Anti-pattern | What it looks like | Fix |
|-------------|-------------------|-----|
| **Academic addition** | A technique that sounds sophisticated but is never used in practice | Cut. If it wasn't used in 3 real applications, it won't be used in the 4th. |
| **Example creep** | Examples that grow from 1 sentence to full paragraphs across versions | Compress examples to 1-2 sentences. Move detailed examples to reference files. |
| **Redundant safety** | Multiple failure modes that all say the same thing in different words | Merge into one failure mode with the most specific wording. |
| **Nesting bloat** | Enhanced techniques that contain their own sub-techniques | Flatten. If a technique needs sub-steps, it's a new step, not an enhancement. |
| **Perfectionist additions** | Edge cases that apply to <5% of uses but add 20% to skill length | Move to a "rare cases" appendix or cut entirely. |

## Output format

After enhancing a skill, produce:

1. **Audit summary** — what was thin, what was bloated, context budget before/after
2. **Changelog** — added, removed, compressed, with justifications
3. **The enhanced SKILL.md** — complete, ready to commit
4. **Quality gate results** — which candidates were cut and why
5. **Net metrics** — line count before/after, estimated context tokens before/after, % change