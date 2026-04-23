---
name: process-extraction
description: A meta-skill for observing a workflow in progress and extracting a repeatable, teachable skill from it. Use this skill whenever the user says "turn this into a skill", "can we skillify this", "capture this process", "save what we just did as a template", "map the steps we took", or "create a skill from this conversation". Also trigger when the user describes a process they repeat frequently and wants to formalize, or when you notice a conversation naturally following a pattern that could be extracted and reused. This is the skill that creates other skills.
---

# Process Extraction

The meta-skill for turning observed workflows into repeatable, teachable skills. This is how skills are born — not from abstract design, but from watching real work happen and recognizing the reusable pattern underneath.

## When to use this skill

- The user says "turn this into a skill" or "can we skillify this process"
- You notice a conversation following a pattern that's appeared before
- The user describes a multi-step workflow they want to formalize
- A complex task just completed and the user wants to capture how it was done
- You've done something creative or effective and the user wants to understand and replicate the process

## The extraction process

### Phase 1: Observe without interfering

While working on a task, maintain a silent process log. Don't narrate the logging — just track:

**For each significant action, record:**
- What was the action? (search, analyze, create, compare, validate, synthesize)
- What input triggered it? (user request, result of previous action, pattern recognition)
- What output did it produce? (data, decision, artifact, insight)
- Was it a CREATIVE step (generated something new) or a MECHANICAL step (applied a known pattern)?
- Did the user course-correct after this step? (if yes, the original approach was wrong — capture the correction as a rule)

**Track the shape of the process:**
- What was the overall sequence? (research → inventory → map → synthesize → plan)
- Where were the decision points? (moments where the next step depended on judgment)
- What was iterated vs. done once? (iterations reveal the core loop)
- What could have been done in parallel? (reveals which steps are independent)
- What was the user's emotional arc? (excited → skeptical → convinced tells you which step was the convincing one)

### Phase 2: Identify the reusable pattern

Not every conversation is a skill. A skill exists when:

**Test 1 — Recurrence:** Would this process be useful in a different context with different inputs? If the process is specific to one project and can't generalize, it's a playbook, not a skill.

**Test 2 — Teachability:** Could you explain this process to someone who hasn't seen it before and have them execute it? If the process requires tacit knowledge that can't be written down, it's expertise, not a skill.

**Test 3 — Value:** Does formalizing this process save significant time or improve quality on future instances? If the process is faster to do ad-hoc than to follow a template, a skill adds overhead, not value.

If all three tests pass, proceed to extraction.

### Phase 3: Extract the skeleton

**Strip the specifics. Keep the structure.**

The Concept Engineering skill wasn't "how to redesign the Watchtower into the Paving Agent." It was "how to creatively evolve any technical system" — with the Watchtower/Paving Agent session as the example. The specifics become examples; the structure becomes the skill.

**Process:**
1. List every step from the process log
2. For each step, ask: "What is this step ACTUALLY doing, abstracted from the specific topic?"
   - "I searched for ZeroID" → "Research the external landscape for existing solutions"
   - "I mapped 5 nodes to their revenue proximity" → "Score each asset by proximity to the goal"
   - "I found that Stripe integration serves 3 revenue streams" → "Identify shared capabilities that serve multiple objectives"
3. Group related steps into phases (typically 4-8 phases per skill)
4. Name each phase with a verb + object: "Inventory assets", "Map modalities", "Constrain the space"
5. For each phase, write: the process (what to do), an example (from the original session), and a failure mode (what goes wrong if done badly)

### Phase 4: Add the enhanced layer

The skeleton is the v1. The enhanced layer makes it v2. For each phase in the skeleton:

**Ask three questions:**
1. "What would a domain expert from an adjacent field add to this step?" → imports techniques from other disciplines
2. "What's the most common way this step fails, and what's the specific countermeasure?" → turns failure modes into prevention rules
3. "What's the non-obvious output this step should produce besides the obvious one?" → finds secondary value (e.g., the asset inventory also reveals which assets you should STOP maintaining)

Add 2-3 enhanced techniques per phase. These are what separate a checklist from a methodology.

### Phase 5: Write the skill document

Follow the SKILL.md format:

```
---
name: [verb-noun, hyphenated]
description: [When to trigger + what it does. Be specific and slightly pushy — 
  include trigger phrases. Max 3 sentences.]
---

# [Skill Name]

[One paragraph: what this skill does and where it was extracted from]

## When to use this skill
[Bullet list of 5-8 trigger conditions]

## The [N]-step process
### Step 1: [Verb + object]
[Process, example, failure mode, enhanced techniques]
...

## Output format
[What the skill produces when complete]
```

**Key principles for skill writing:**
- Every step must be actionable by someone who hasn't seen the original conversation
- Examples are from the origin session but the process is general
- Failure modes are specific and prevention-oriented, not vague warnings
- The output format defines what "done" looks like

### Phase 6: Validate by testing

Before calling the skill complete, apply it to a DIFFERENT problem than the one it was extracted from. If the skill was extracted from revenue planning, test it on a different builder's portfolio. If it was extracted from system design, test it on a different system.

**Validation criteria:**
- Does every step produce meaningful output on the new problem?
- Are there steps that feel forced or irrelevant? (remove them)
- Are there gaps where you need to improvise? (add a step)
- Does the output format capture what's actually valuable?

### Phase 7: Ship and version

Save the skill to the skills repository with a commit message that includes:
- The skill name and version
- Where it was extracted from (origin session/conversation)
- What it was tested on (validation problem)

Skills evolve. v1 is always rough. After using the skill 3-5 times, extract the improvements and publish v2. The enhanced techniques from Phase 4 are often discovered during real usage, not during initial extraction.

## Recognizing skill-worthy patterns

Not every repeated action is a skill. Here's a quick filter:

| Pattern | Skill-worthy? | Why |
|---------|:---:|---|
| Multi-step creative process with consistent structure | ✅ | Concept Engineering, Revenue Architecture |
| Technical procedure with exact steps | ❌ | This is a runbook/playbook, not a skill. Save as documentation. |
| Decision framework used across domains | ✅ | The structure transfers; the domain-specific content is the input |
| One-off complex task | ❌ | No recurrence = no skill value |
| A process you course-corrected mid-stream | ✅ | The corrections ARE the skill — they encode the non-obvious rules |
| Something you do automatically without thinking | ✅✅ | The hardest to extract but most valuable — tacit knowledge made explicit |

## The two-skill pattern

When extracting skills, you often find TWO skills hiding in one process:

1. **The domain skill** — the actual methodology for doing the thing (Revenue Architecture, Concept Engineering)
2. **The meta-skill** — the process of extracting and improving skills itself (this document)

Always look for both. The domain skill solves today's problem. The meta-skill solves tomorrow's.

## Output format

After extraction, produce:

1. **The process log** — raw record of what was observed
2. **The skill skeleton** — abstracted steps without specific content
3. **The SKILL.md document** — complete, formatted, ready to commit
4. **The validation result** — how the skill performed on a different problem
5. **The v2 roadmap** — what to improve after 3-5 real uses