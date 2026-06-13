---
name: module-creation
description: Create automation modules for any process, workflow, or business function by decomposing it into a value chain and identifying friction at each stage. Use this skill whenever the user wants to brainstorm modules, break down a process into automatable steps, identify bottlenecks in a workflow, design a modular system, or figure out "what should I automate." Triggers on phrases like "create modules for," "what modules do we need," "how do I automate this," "brainstorm ideas for," "modularize this," "break this down," "decompose this process," "find the friction," or any request to systematically analyze a workflow and generate actionable components. Also use when the user describes a business process and wants to know where AI, automation, or tooling can be inserted. Also triggers automatically when a PRD skill completes — the PRD skill calls this skill to decompose each P-item into modules and tasks. Works for ANY domain — sales pipelines, content creation, infrastructure, client delivery, product development, personal workflows, or anything with sequential steps that produce an outcome. This is a universal module factory — give it any process, get back a complete set of buildable modules with a machine-readable task list.
---

# Module Creation v2

A universal framework for turning any process into a system of automatable modules. The core insight: every process that produces value follows a chain of transformations. Each transformation has friction. Each friction point is a module waiting to be built.

**v2 additions:** Task list generation per module, master task list compilation, JSON task manifest for controller tracking, P-item references, and PRD-linked output mode.

## When to use this skill

- Standalone: user wants to decompose a process into buildable modules
- **Linked mode (automatic):** called by the PRD skill after PRD completion — decomposes each P-item into modules + tasks
- Updating: user updates a PRD and specific P-items need re-decomposition

## The Framework (5 Steps)

### Step 1: Identify the Value Chain

Ask: "What is the sequence of steps that transforms *nothing* into the *desired outcome*?"

Every process has a chain. The user might describe it as a workflow, a pipeline, a funnel, or just "the way we do things." Your job is to extract the chain — the ordered sequence of stages where each stage transforms the output of the previous stage.

Don't accept the user's first description as complete. Probe for hidden stages. Most people skip the beginning (where do inputs come from?) and the end (what happens after delivery?). The full chain almost always has 6-10 stages.

**How to find stages:** Walk the process from left to right (input → output) and ask "what has to happen before this step can begin?" at each point. Each answer that introduces a new transformation is a stage.

**Example:**
- User says: "We find leads and call them"
- Actual chain: Discovery → Qualification → Intelligence → Outreach → Response Handling → Conversion → Delivery → Retention → Optimization
- The user collapsed 9 stages into 2 because the middle ones feel invisible

### Step 2: Name and Define Each Stage

Each stage needs:
- A **name** (1-2 words, verb-oriented when possible)
- A **transformation statement**: "This stage transforms [input state] into [output state]"
- An **output artifact**: What tangible thing does this stage produce?

The transformation statement is the most important part. If you can't articulate what changes between input and output, the stage is either not real or needs to be split.

**Example:**
- Stage: Qualification
- Transforms: "known business" into "scored, prioritized prospect"
- Output: Lead score + priority ranking + qualification reason

### Step 3: Apply the Three Friction Questions

At each stage, ask these three questions. Each answer is a candidate module.

**Question 1: "What data do I wish I had at this stage?"**
→ Generates **intelligence/data modules** (scrapers, enrichment, research tools)

**Question 2: "What action do I keep doing manually at this stage?"**
→ Generates **automation modules** (sequencers, classifiers, generators)

**Question 3: "What would change my outcome if I knew it before reaching this stage?"**
→ Generates **prediction/insight modules** (scorers, analyzers, optimizers)

Not every question produces a module at every stage. Some stages are already efficient. Some stages produce multiple modules from a single question. The questions are generators, not checklists.

### Step 4: Map Compound Chains

This is where the real leverage appears. Review all generated modules and ask: "Does this module's output become another module's input?"

Draw the dependency graph. You'll find that modules cluster into sub-pipelines within the larger chain. These clusters are your build priorities — a module that feeds 3 other modules is more valuable than a standalone module.

**Look for these patterns:**
- **Fan-out:** One module's output triggers multiple downstream modules (high-value — build first)
- **Fan-in:** Multiple modules feed into one synthesis module (the synthesis module is the bottleneck — build it well)
- **Sequential chains:** A → B → C with no branching (build as a unit, not separately)
- **Feedback loops:** Module output eventually feeds back to an earlier stage (these create learning systems — flag them as advanced)

### Step 5: Prioritize and Sequence

Score each module on three dimensions:
1. **Impact**: How much does this module improve the outcome of its stage? (High/Medium/Low)
2. **Dependency**: How many other modules depend on this one? (Count)
3. **Buildability**: How hard is it to build with available tools? (Easy/Medium/Hard)

Priority = (Impact × Dependency) / Buildability. Build high-impact, high-dependency, easy-to-build modules first.

Group modules into build phases:
- **Phase 1**: Foundation modules that other modules depend on
- **Phase 2**: High-impact modules that leverage Phase 1 outputs
- **Phase 3**: Optimization modules that improve the whole system

---

## Output Format (v2 — includes task list and controller manifest)

When delivering a Value Chain Decomposition, always produce ALL of the following:

### 1. The Chain Diagram
List all stages in order with transformation statements.

### 2. The Module Inventory
For each module:
- **Name**: Clear, descriptive (2-4 words)
- **Stage**: Which stage(s) it serves
- **P-item**: Which PRD P-item this module serves (e.g. P2-1) — required in linked mode
- **Type**: Intelligence / Automation / Prediction
- **Description**: One paragraph — what it does and why it matters
- **Input**: What data/events it needs
- **Output**: What it produces
- **Trigger**: When it runs (scheduled, event-driven, on-demand)
- **Compound connections**: What modules feed it, what modules it feeds

### 3. Task List Per Module
For every module, generate a concrete task list — atomic work units (30min–4h each).

Format:
```markdown
## Module: [Name]
**P-item:** [e.g. P2-1]
**Status:** TODO

### Tasks
- [ ] [Task 1 — specific, implementable]
- [ ] [Task 2]
- [ ] [Task 3]
- [ ] Tests
```

Task writing rules:
- Each task is a single, atomic, testable unit of work
- A developer or AI should be able to complete it in one sitting without ambiguity
- Tasks should be implementation-level ("Define ReconcileEngine struct with events channel field") not goal-level ("Build reconciler")
- Always include a Tests task at the end of each module
- 3–8 tasks per module is the target range

### 4. Master Task List
Flatten all module task lists into one sorted list with status and P-item references.

```markdown
# Master Task List — [Project Name]
Generated: [date]

## TODO
- [ ] [P2-1.1] [Task description] — Module: [Module Name]
- [ ] [P2-1.2] [Task description] — Module: [Module Name]
- [ ] [P2-2.1] [Task description] — Module: [Module Name]

## IN PROGRESS
(empty at generation — updated by controller on each commit)

## DONE
(empty at generation — updated by controller on each commit)
```

### 5. JSON Task Manifest
Machine-readable version for controller tracking. Always output this block — it's what the controller reads to auto-update PRD status.

```json
{
  "project": "[project-id]",
  "generated": "[ISO date]",
  "prd_version": "[version]",
  "modules": [
    {
      "id": "[module-id]",
      "name": "[Module Name]",
      "p_item": "[P2-1]",
      "status": "TODO",
      "tasks": [
        {
          "id": "[P2-1.1]",
          "description": "[Task description]",
          "status": "TODO",
          "commit": null
        }
      ]
    }
  ]
}
```

### 6. The Dependency Map
Which modules feed which. Identify fan-out nodes and feedback loops.

### 7. The Priority Stack
Top 5 modules to build first and why.

### 8. The Build Phases
Grouped into 3 phases with rationale.

---

## Linked Mode (called from PRD skill)

When invoked by the PRD skill after PRD creation or update:

1. Receive the PRD's P-items as input
2. For each P-item, run Steps 1-5 treating the P-item goal as the process to decompose
3. Generate module inventory + task list + JSON manifest
4. Output three files:
   - `[project]-decomposition.md` — full module breakdown (human-readable)
   - `[project]-tasks.md` — master task list (human-readable)
   - `[project]-tasks.json` — task manifest (machine-readable, for controller)
5. All three are committed to the project's repo alongside the PRD

**PRD update handling (graceful):**

When the PRD changes:
- **Added P-items:** Run decomposition on new P-items only. Append to decomposition doc.
- **Changed P-items:** Re-run on changed P-items only. Update those sections. Do not touch task statuses for other P-items.
- **Removed P-items:** Mark their tasks as `"status": "cancelled"` in JSON manifest. Do not delete. Audit trail preserved.
- **Done tasks:** Never touched by a PRD update, regardless of what changes. Progress is never erased.

---

## Facilitation Guide

The decomposition works best as a conversation, not a monologue. Here's how to facilitate:

**Opening:** Ask the user to describe the process in plain language. Don't correct or formalize yet — let them talk through it naturally. Their language reveals which stages they think about and which they skip.

**Expanding:** After the initial description, walk backward from the outcome: "Before you can do [X], what has to be true?" Repeat until you hit the true starting point. Then walk forward: "After [X] is done, what happens next?" until you hit the terminal state.

**Validating:** Read back the full chain and ask "Is anything missing between [stage N] and [stage N+1]?" People often remember intermediate steps when they see the gap.

**Generating modules:** Don't just list modules — explain the friction that creates each one. Connect the module to their felt pain.

**Iterating:** After the first pass, ask: "Looking at these modules, which ones make you think 'if I had that, everything else would be easier'?" Their answer reveals the true high-leverage modules.

---

## Domain Templates

For common domains, use these starting chains as scaffolding. See `references/domain-templates.md` for expanded templates covering:
- Sales/Lead Pipeline
- Content Creation Pipeline
- Client/Project Delivery
- Infrastructure Management
- Product Development
- Community/Audience Building
- Hiring/Recruitment
- Customer Support
- Financial Operations

---

## Anti-Patterns

**Don't generate modules for their own sake.** If a stage has low friction and works well manually, say so.

**Don't confuse stages with modules.** Stages are parts of the value chain. Modules are solutions to friction within stages.

**Don't flatten compound chains.** Every module should either consume or produce something another module cares about.

**Don't skip the user's language.** Their terminology encodes their mental model.

**Don't over-specify too early.** The first pass should be module concepts. Let the user react before going deep.

**Don't generate vague tasks.** "Implement the module" is not a task. "Define the struct, wire it to the scheduler, add the MCP tool, write 3 tests for boundary conditions" is four tasks.
