---
name: concept-engineering
description: A structured creative methodology for evolving technical systems and solving complex architectural problems. Use this skill whenever the user asks to brainstorm solutions, redesign a system, improve an architecture, explore creative approaches to a technical problem, think through a hard design question, or evolve an existing concept into something better. Also trigger when the user says things like "what else could we do", "how could we improve this", "think creatively about", "come up with ideas for", "what am I missing", or "help me think through this". This skill is about the PROCESS of creative technical thinking.
---

# Concept Engineering

A repeatable methodology for taking a technical problem and producing creative, technically grounded solutions that the straightforward approach would miss.

Extracted from a real design session where a monitoring system (Watchtower) was evolved into a unified environmental cognition platform (Paving Agent) through structured creative thinking.

## When to use this skill

- The user has a working system but suspects it could be fundamentally better
- The user has a problem statement but the obvious solutions feel insufficient
- The user wants to brainstorm but needs structure, not free association
- The user asks "what am I missing" or "what else could we do"
- The user has multiple separate systems that might belong together
- The user wants to evolve a v1 concept into a v2

## The seven-step process

### Step 1: Reframe the problem (don't accept the given frame)

The most powerful creative act is refusing to solve the problem as stated. The stated problem is almost always a symptom or a subset of the real problem.

**Process:**
- Ask: "What is the user actually trying to achieve, two levels above what they asked for?"
- Find the vocabulary gap: imprecise words ("monitoring", "better") signal the real problem hasn't been named yet
- Name the deeper problem with a precise term. Coining a name gives it conceptual weight and opens new solution directions
- Test the reframe: does the new name suggest solutions the old name didn't?

**Example:** "We need monitoring" -> "Environmental Cognition: the perception gap between sessions"

**Failure mode:** Reframing into something so abstract it loses contact with the concrete problem.

### Step 2: Decompose into dimensions (find the hidden axes)

Break the reframed problem into dimensions that aren't obvious from the surface. Decompose by *type of understanding* needed, not by component.

**Process:**
- Ask: "What different KINDS of understanding does the user need?"
- For each kind, ask: "What would perfect understanding of this dimension look like?"
- Name each dimension with a cognitive verb: spatial cognition, temporal cognition, risk cognition
- Each dimension should suggest a fundamentally different solution approach

**Example:** Spatial cognition (topology), temporal cognition (trends), risk cognition (dependencies), connection cognition (network flows), change cognition (drift detection)

**Failure mode:** Dimensions that are really the same thing with different names.

### Step 3: Map modalities to dimensions (match representation to understanding)

For each dimension, ask: "What representation would give the deepest understanding?" Not everything is best served by text.

**Process:**
- List all available output modalities (text, JSON, SVG, charts, images, interactive widgets)
- For each dimension, ask: "Which modality makes this most intuitively graspable?"
- Look for mismatches: where is text representing something that would be better as a visual?

**Example:** Spatial -> SVG topology diagram. Temporal -> sparkline charts. Risk -> heat map.

**Failure mode:** Defaulting to text/JSON for everything because it's easiest.

### Step 4: Use embodied metaphors (make the abstract physical)

Find a metaphor that makes the problem visceral. The right metaphor actively guides design decisions.

**Process:**
- Ask: "What physical system works like this?"
- Choose a metaphor that imports useful structure, not just surface similarity
- Let the metaphor generate design questions: "If this is a nervous system, what are the nerve endings?"
- Test: does it break down gracefully, or lead to nonsense?

**Example:** "Sensory deprivation tank" for the problem. "Nervous system" for the agent. "Nerve endings" led to distributed agents instead of central polling.

**Failure mode:** Clever metaphors that don't import useful structure.

### Step 5: Constrain the creative space (hold limits fixed, push within them)

Unbounded brainstorming produces fantasy. Hold hard constraints fixed and push creative boundaries within them.

**Process:**
- List non-negotiable constraints: resource budgets, existing infrastructure, compatibility
- For each idea, verify it's achievable within constraints
- If an idea violates a constraint, ask "what's the achievable version?"
- Use constraints as creative fuel: "Given only 50MB RAM, what's the most powerful thing we can do?"

**Example:** Go binary <50MB RAM pushed toward lightweight agents, which was actually a better design than a heavy monitoring stack.

**Failure mode:** Treating constraints as obstacles rather than design parameters.

### Step 6: Synthesize through a single interface (find the integration point)

Multiple good ideas need a unifying interface or they become multiple separate systems.

**Process:**
- List all ideas generated in Steps 1-5
- Ask: "What single interface would let the user access all of these?"
- Design it layered: simple call returns everything, user can drill into any dimension
- The interface name matters: communicate what the user gets, not how it works internally

**Example:** Five ideas unified through `helix_pulse()`. Three layers: JSON + SVG URLs + narrative text. Name communicates "heartbeat, current state."

**Failure mode:** An interface that's a bag of unrelated features.

### Step 7: Ground in existing work (extend, don't replace)

Check what's already built. Position new ideas as natural extensions.

**Process:**
- Inventory existing code, specs, docs, partially built systems
- For each new idea: does it replace, extend, or fill a gap?
- Find the minimum changes to enable the new ideas
- Write the solution as an evolution, not a revolution

**Example:** Watchtower absorbed into Paving Agent as features. Provisioner refactored from host to router.

**Failure mode:** NIH syndrome. Throwing away working systems because the new idea feels cleaner.

## Output format

After running all seven steps, produce:

1. **The reframed problem** — one paragraph naming the real problem
2. **The dimensional decomposition** — 3-7 named dimensions
3. **The modality map** — which representation serves each dimension
4. **The unifying metaphor** — and 2-3 design decisions it generates
5. **The constrained solution** — the best idea within real limits
6. **The integration interface** — the single point of access
7. **The evolution path** — how this extends what exists

This should feel like a design document, not a brainstorm dump. Each element connects to the next.