---
name: concept-engineering
description: A structured creative methodology for evolving technical systems and solving complex architectural problems. Use this skill whenever the user asks to brainstorm solutions, redesign a system, improve an architecture, explore creative approaches to a technical problem, think through a hard design question, or evolve an existing concept into something better. Also trigger when the user says things like "what else could we do", "how could we improve this", "think creatively about", "come up with ideas for", "what am I missing", or "help me think through this". This skill is about the PROCESS of creative technical thinking. Use it even when the user doesn't explicitly ask for creativity — any redesign, system evolution, or "v2" request benefits from this structured approach.
---

# Concept Engineering v2

A repeatable methodology for taking a technical problem and producing creative, technically grounded solutions that the straightforward approach would miss.

Extracted from a real design session where a monitoring system (Watchtower) was evolved into a unified environmental cognition platform (Paving Agent) through structured creative thinking. Enhanced through the skill-improvement process with 3 additions per step.

## When to use this skill

- The user has a working system but suspects it could be fundamentally better
- The user has a problem statement but the obvious solutions feel insufficient
- The user wants to brainstorm but needs structure, not free association
- The user asks "what am I missing" or "what else could we do"
- The user has multiple separate systems that might belong together
- The user wants to evolve a v1 concept into a v2
- The user is stuck and needs a new angle on a familiar problem
- The user wants to understand WHY a design feels wrong but can't articulate it

## The seven-step process

### Step 1: Reframe the problem (don't accept the given frame)

The most powerful creative act is refusing to solve the problem as stated. The stated problem is almost always a symptom or a subset of the real problem.

**Core process:**
- Ask: "What is the user actually trying to achieve, two levels above what they asked for?"
- Find the vocabulary gap: imprecise words ("monitoring", "better") signal the real problem hasn't been named yet
- Name the deeper problem with a precise term. Coining a name gives it conceptual weight and opens new solution directions
- Test the reframe: does the new name suggest solutions the old name didn't?

**Enhanced techniques:**
- **Anti-frame generation:** Deliberately generate 3 WRONG reframes first. Each wrong reframe illuminates a boundary of the real problem. "It's not a monitoring problem, it's not a dashboard problem, it's not a data collection problem — it's a PERCEPTION problem." The wrong frames define the shape of the right one by exclusion.
- **Inversion validation:** If you solved the reframed problem perfectly, would the original problem completely disappear? If not, the reframe is incomplete. "If Claude had perfect environmental cognition, would we still need monitoring?" Yes — so cognition subsumes monitoring, not the reverse. Good reframe.
- **Stakeholder expansion:** Who else is affected by the reframe beyond the person asking? A reframe from "monitoring" to "environmental cognition" changes the solution space for future users, for the infrastructure itself (self-healing becomes possible), and for the product (it's now sellable, not just internal tooling).

**Example:** "We need monitoring" → "Environmental Cognition: the perception gap between sessions"

**Failure mode:** Reframing into something so abstract it loses contact with the concrete problem.

### Step 2: Decompose into dimensions (find the hidden axes)

Break the reframed problem into dimensions that aren't obvious from the surface. Decompose by *type of understanding* needed, not by component.

**Core process:**
- Ask: "What different KINDS of understanding does the user need?"
- For each kind, ask: "What would perfect understanding of this dimension look like?"
- Name each dimension with a cognitive verb: spatial cognition, temporal cognition, risk cognition
- Each dimension should suggest a fundamentally different solution approach

**Enhanced techniques:**
- **Cross-dimensional interference check:** Do any dimensions conflict or compete for the same resources? Spatial cognition (topology diagrams) and temporal cognition (sparklines) both need visual real estate. If dimensions interfere, you need a layering strategy, not just a list. This forces the design to handle conflicts explicitly rather than discovering them during implementation.
- **Blind spot audit:** Ask: "What dimension would a domain expert in [adjacent field] add that I wouldn't think of?" A security engineer would add "trust cognition" (which entities are verified vs assumed). A UX designer would add "attention cognition" (what's worth looking at vs noise). Importing dimensions from adjacent domains prevents tunnel vision.
- **Leverage ordering:** Not all dimensions are equal. Rank them by: (a) how much value they add, (b) how cheaply they can be implemented, (c) how much they depend on other dimensions. Build high-leverage, low-dependency dimensions first. This turns a flat brainstorm into a build sequence.

**Example:** Spatial → topology, temporal → trends, risk → dependencies, connection → flows, change → drift

**Failure mode:** Dimensions that are really the same thing with different names.

### Step 3: Map modalities to dimensions (match representation to understanding)

For each dimension, ask: "What representation would give the deepest understanding?" Not everything is best served by text.

**Core process:**
- List all available output modalities (text, JSON, SVG, charts, images, interactive widgets)
- For each dimension, ask: "Which modality makes this most intuitively graspable?"
- Look for mismatches: where is text representing something that would be better as a visual?

**Enhanced techniques:**
- **Compound modality design:** Sometimes two modalities combined beat either alone. An SVG topology map with hover tooltips showing JSON metrics gives both spatial intuition AND precise data without switching contexts. For each dimension, ask: "What's the primary modality AND what's the annotation layer?" The annotation layer adds precision without sacrificing the primary modality's intuition.
- **Temporal modality shifts:** Some dimensions need different modalities at different time scales. Real-time state: live SVG. Last 24 hours: sparkline. Last 30 days: statistical summary text. Design the modality map with a time axis, not just a dimension axis.
- **Consumer capability matching:** What modalities can the actual user consume in their actual context? Ryan reads text in chat, views SVGs inline, and can open HTML dashboards in a browser. An API consumer can only process JSON. Design modality output to match the consumer's capabilities, with graceful degradation (if SVG rendering fails, fall back to structured text that conveys the same information).

**Example:** Spatial → SVG + tooltip. Temporal → sparkline + JSON. Risk → heat map + text summary.

**Failure mode:** Defaulting to text/JSON for everything because it's easiest.

### Step 4: Use embodied metaphors (make the abstract physical)

Find a metaphor that makes the problem visceral. The right metaphor actively guides design decisions.

**Core process:**
- Ask: "What physical system works like this?"
- Choose a metaphor that imports useful structure, not just surface similarity
- Let the metaphor generate design questions: "If this is a nervous system, what are the nerve endings?"
- Test: does it break down gracefully, or lead to nonsense?

**Enhanced techniques:**
- **Negative metaphor:** Explicitly state what the system is NOT like, and why. "This is NOT like a security camera system (passive recording for later review). It's like a nervous system (active sensing with reflexive responses)." The negative metaphor prevents design drift toward the wrong model. When someone later suggests "let's just log everything and query later," the negative metaphor is the defense.
- **Scale bridging:** Borrow metaphors from radically different scales to find non-obvious structures. Cellular biology → network architecture (mitochondria = dedicated compute nodes, cell membranes = network boundaries, DNA = config-as-code). Geological processes → software evolution (tectonic pressure = tech debt, erosion = feature degradation). The further the source domain, the more novel the structural insight.
- **Metaphor tournament:** Generate 3 competing metaphors. Stress-test each by asking: "What design decision does this metaphor get WRONG?" The nervous system metaphor gets wrong the idea of centralized processing (real nervous systems are decentralized). The immune system metaphor gets wrong the idea of attacking threats (we don't want to kill bad containers, we want to heal them). Pick the metaphor with the fewest dangerous wrong answers, and explicitly document the points where the metaphor breaks.

**Example:** "Sensory deprivation tank" (problem). "Nervous system" (solution). "Nerve endings" → distributed agents.

**Failure mode:** Clever metaphors that don't import useful structure.

### Step 5: Constrain the creative space (hold limits fixed, push within them)

Unbounded brainstorming produces fantasy. Hold hard constraints fixed and push creative boundaries within them.

**Core process:**
- List non-negotiable constraints: resource budgets, existing infrastructure, compatibility
- For each idea, verify it's achievable within constraints
- If an idea violates a constraint, ask "what's the achievable version?"
- Use constraints as creative fuel: "Given only 50MB RAM, what's the most powerful thing we can do?"

**Enhanced techniques:**
- **Constraint archaeology:** Challenge every "constraint" by asking: "Is this a physical law, an engineering tradeoff, or a habit?" Physical laws (can't exceed network bandwidth) are real. Engineering tradeoffs ("we use Go because it compiles to static binaries") can be revisited. Habits ("we've always run everything on VPS2") should be actively questioned. Many "constraints" are actually unexamined assumptions that dissolve under scrutiny.
- **Constraint gradient:** Rank constraints from hardest (physics, money) to softest (preference, convention). Then draw a line: everything above the line is truly non-negotiable, everything below is design space disguised as constraint. This often reveals that the actual constraint set is much smaller than assumed, and the creative space much larger.
- **Constraint judo:** For each hard constraint, find one idea that uses the constraint as an ADVANTAGE, not just a limitation. "We only have 50MB RAM" → "Therefore the agent is so lightweight it can run on ANY node, even a Raspberry Pi → that makes it a product feature, not a limitation." Constraints become selling points when you flip the frame.

**Example:** <50MB RAM pushed toward lightweight agents, which became a better design than heavy monitoring.

**Failure mode:** Treating constraints as obstacles rather than design parameters.

### Step 6: Synthesize through a single interface (find the integration point)

Multiple good ideas need a unifying interface or they become multiple separate systems.

**Core process:**
- List all ideas generated in Steps 1-5
- Ask: "What single interface would let the user access all of these?"
- Design it layered: simple call returns everything, user can drill into any dimension
- The interface name matters: communicate what the user gets, not how it works

**Enhanced techniques:**
- **Progressive disclosure design:** The interface should have 3 depth layers. Layer 1 (glance): one-sentence status + health color. Layer 2 (scan): the structured data + visual artifacts. Layer 3 (deep): full metrics, historical data, raw logs. Most calls need Layer 1-2. Layer 3 is available but not forced. This prevents information overload while making depth available.
- **Name stress-testing:** Before committing to an interface name, generate 5 potential misinterpretations. "pulse" could be misread as "heartbeat check" (too narrow) or "medical data" (wrong domain). If more than 2 of the 5 misinterpretations are plausible, the name needs work. The best names are immediately understood AND don't suggest wrong things.
- **Degraded mode design:** What does the interface return when the system is partially broken? If 2 of 5 nodes are unreachable, `helix_pulse()` should still return what it has with clear markers for what's missing — not fail entirely. Design the interface's failure modes explicitly. A system that gives useful partial information under degradation is more valuable than one that gives perfect information or nothing.

**Example:** Five ideas → `helix_pulse()`. Three layers: JSON + SVG + narrative.

**Failure mode:** An interface that's a bag of unrelated features.

### Step 7: Ground in existing work (extend, don't replace)

Check what's already built. Position new ideas as natural extensions.

**Core process:**
- Inventory existing code, specs, docs, partially built systems
- For each new idea: does it replace, extend, or fill a gap?
- Find the minimum changes to enable the new ideas
- Write the solution as an evolution, not a revolution

**Enhanced techniques:**
- **Deprecation choreography:** For each system being absorbed, write the explicit sunset steps: (1) what stops being maintained, (2) when it gets turned off, (3) what happens to its data, (4) who needs to be told. "Watchtower is absorbed" isn't enough — the ntfy topic, the systemd timer, and the cron jobs all need explicit fates. Undocumented deprecation creates ghost infrastructure that haunts you later.
- **Migration path design:** Users of the old system need a clear, zero-downtime path to the new one. Design a period where both systems run in parallel, the new system is verified against the old system's outputs, and the switchover is atomic. "Phase 2 deploys alongside Phase 1; Phase 3 verifies parity; Phase 4 sunsets Phase 1" is a pattern that works everywhere.
- **Rollback contract:** If the new thing fails catastrophically, can you revert to the old thing cleanly? Document the exact rollback steps before building. If rollback is impossible (because the old system's data format is incompatible with the new one), that's a design risk that needs explicit mitigation — like maintaining a frozen snapshot of the old system's state.

**Example:** Watchtower absorbed, Provisioner refactored. Rollback: restore manifest.json + restart old containers.

**Failure mode:** NIH syndrome. Throwing away working systems because the new idea feels cleaner.

## Output format

After running all seven steps, produce:

1. **The reframed problem** — one paragraph naming the real problem, with the 3 wrong reframes that defined its boundaries
2. **The dimensional decomposition** — 3-7 named dimensions, ordered by leverage, with interference notes
3. **The modality map** — primary + annotation modalities per dimension, with temporal shifts
4. **The unifying metaphor** — the winning metaphor from the tournament, with documented break points
5. **The constrained solution** — the best idea within real limits, with at least one constraint-as-advantage flip
6. **The integration interface** — three depth layers, degraded mode behavior, name stress-test results
7. **The evolution path** — deprecation choreography, migration path, rollback contract

This should feel like a design document that could be handed to a builder. Each element connects to the next. The enhanced techniques are not optional — they're what separates a brainstorm from engineering.