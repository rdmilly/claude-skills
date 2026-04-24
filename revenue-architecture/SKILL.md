---
name: revenue-architecture
description: A structured methodology for mapping existing technical assets to revenue streams and creating an actionable path to income. Use this skill whenever the user asks about monetization, revenue planning, making money from what they've built, business model design, finding revenue opportunities, or transitioning from building to earning. Also trigger on "how do we make money from this", "what's the business model", "fastest path to income", or "turn this into a product". Works for any builder who has accumulated technical assets without a clear revenue model.
---

# Revenue Architecture v2

A repeatable methodology for converting accumulated technical capability into income. Designed for the builder who has spent months creating infrastructure, tools, and systems but hasn't yet generated revenue from them.

Enhanced via 3-run skill enhancer process: 33 candidates generated, 18 kept, 6 cut, 4 compressed, 3 merged.

## When to use this skill

- The user has built significant technical infrastructure but isn't generating revenue
- The user has multiple partially-built projects and needs to prioritize by revenue proximity
- The user wants to transition from building to earning
- The user needs to identify which capabilities serve multiple revenue streams
- The user asks "how do we monetize" or "what's the fastest path to income"

## The six-step process

### Step 1: Asset inventory with proximity scoring

List EVERYTHING built — not just flagship projects. Then score each by proximity to revenue.

**Scan these 8 asset categories** (builders routinely miss 3-4 of these):
1. Code and infrastructure (repos, servers, tools)
2. Data (leads, analytics, content libraries)
3. Brand and domains (websites, social accounts, logos)
4. Existing clients and relationships
5. Content (videos, articles, posts, transcripts)
6. Skills and domain expertise (what you know how to do)
7. Partially-built products (80%+ done but not launched)
8. Operational systems (CRM, billing, automation workflows)

**Score each asset into tiers:**
- **Tier 1 (Revenue-ready):** Could produce $1 in 14 days with activation, not building. If you need to BUILD something new first, it's not Tier 1.
- **Tier 2 (Near-revenue):** 80%+ built, missing last mile (packaging, pricing, payment, listing). Bounded work remaining.
- **Tier 3 (Product potential):** Strong vision, foundation exists, but months of work remain.

**For each asset, also record:**
- **Defensibility:** Can someone copy this in a weekend, or does it have a moat? Revenue from defensible assets compounds; commoditized assets get competed away.
- **Emotional attachment flag:** Are you rating this highly because the market wants it, or because you spent 6 months building it? Be honest.
- **Net value:** Revenue potential minus carrying cost (hosting + maintenance time × hourly rate). Some assets cost more to maintain than they'll ever earn.
- **Skill requirements:** What human skills does monetizing this require? (Sales calls? Content creation? Technical delivery?)

**Produce TWO outputs:**
1. The tiered asset list (what to monetize)
2. The stop-maintaining list (what to archive or sunset — assets with high maintenance cost, no revenue path, and no strategic value)

**Failure mode:** Rating everything as Tier 2 because you want it to be close to revenue. The 14-day test is the hard filter.

### Step 2: Revenue model research (external landscape)

Search for how people with similar assets actually make money. Find proven models, don't invent.

**Process:**
- Search for revenue benchmarks in the user's specific domain with real numbers
- Find 3-5 documented cases with monthly revenue, margins, team size
- **Search for failure rates and median outcomes, not just success stories.** "Most solo founders earn $3-5K/month; 2-3% cross $1M ARR" is more useful than "this founder hit $300K/month."
- Validate any conversion rate assumptions against published benchmarks before using them in projections
- Map each proven model against user's existing assets: what could they do with what they already have?

**Failure mode:** Getting excited about billion-dollar potential when the immediate need is $5K/month. Match ambition to timeline.

### Step 3: Multi-stream capability mapping

The highest-leverage step. Find capabilities that serve multiple revenue streams simultaneously.

**Process:**
- Create a matrix: capabilities × revenue streams
- **Weight each intersection** (not just checkmarks): 3 = critical (can't launch without it), 1 = enhances, 0 = irrelevant. For each non-zero intersection, write one sentence explaining HOW it serves that stream. If you can't explain in one sentence, it doesn't serve it — score it 0.
- **Add build time and reality multiplier** per capability: estimated hours × 3 for first-time builds, × 1.5 for familiar patterns. This produces value-per-hour, not just total value.
- **Extract the dependency chain:** Some capabilities must come before others (Stripe before any paid product). Produce a build sequence, not just a ranked list.
- Sum weighted scores per capability. Build highest value-per-hour first, respecting dependency order.

**Failure mode:** Marking capabilities as "serves" a stream when it only serves it theoretically. The one-sentence HOW test catches this.

### Step 4: Revenue math (conservative, not optimistic)

For each viable stream, build the math. Use the floor, not the ceiling.

**Formulas:**
- Service: (realistic client count) × (retainer) × (margin after ALL costs)
- Product: (realistic installs in 90 days) × (paid conversion %, validated against benchmarks) × (price)
- Content: (realistic audience in 90 days) × (revenue per 1K, typically very low)

**Required additions to raw math:**
- **Show profit, not just revenue.** Always subtract hosting, tools, subscriptions, tax, subcontractor costs.
- **Add best/expected/worst scenarios.** "$8K-$20K/month, expected $12K" is honest. A single number is a guess.
- **Calculate revenue per hour worked per stream.** This is the metric that matters most — $3K/month at 10 hrs = $300/hr effective. $200/month at 20 hrs = $10/hr. Prioritize high $/hr streams.
- **Calculate months-to-break-even.** At what month does total revenue cover all costs (hosting, tools, minimum living expenses)? This is the most important number in the plan.
- **Add 1 month to all timeline projections.** B2B sales cycles are longer than builders expect.

**Failure mode:** Using success-story benchmarks instead of median benchmarks. Showing revenue without profit. Assuming month-1 revenue from cold outreach.

### Step 5: Sequence the 90-day plan

Map the first 90 days in 2-week blocks. Follow the tier order: activate Tier 1 first, finish Tier 2 second, build Tier 3 in background.

**For each 2-week block:**
- What gets built or activated
- Revenue target (even if $0)
- Hours required from the human
- Dependencies (what must happen before this block starts)

**Sequencing rules:**
- Weeks 1-2: Activate ALL Tier 1 assets. Define packages. Contact leads. Set up payment.
- Weeks 3-4: Close first deals. Ship Tier 2 finishing work.
- Weeks 5-8: Deliver to first clients. Start Tier 3 build during non-client hours.
- Weeks 9-12: Scale delivery. Launch product betas. Measure and adjust.

**Failure mode:** Spending weeks 1-4 on Tier 3 product work because it's more fun than sales calls. Tier 1 activation funds everything else.

### Step 6: Decision points (max 5)

Identify the questions that only the human can answer. These are the blockers the plan can't proceed without.

**Rules:**
- **Hard cap: 5 questions maximum.** More causes decision paralysis.
- **Blocking questions only.** If the plan works regardless of the answer, it's not a blocker — don't ask it.
- **Provide 2-3 default options with tradeoffs** instead of open-ended prompts. "How many hours?" is harder than "A: 10 hrs/week (1-2 clients) B: 20 hrs/week (3-4 clients) C: 30 hrs/week (5+ clients, agency mode)."
- **Frame as commitments**, not questions: "I commit to [X] for the next 90 days." Pre-commitments have higher follow-through.
- Before listing a question, check whether existing data already answers it.

**Failure mode:** Asking 10 open-ended questions that the user stares at for a week and never answers. The defaults + commitment framing fixes this.

## Output format

After running all six steps, produce:

1. **Asset inventory** — tiered list with defensibility, net value, skill requirements, and emotional attachment flags
2. **Stop-maintaining list** — assets to archive or sunset
3. **Revenue landscape** — 3-5 proven models with median benchmarks (not success-story benchmarks)
4. **Capability matrix** — weighted, with build times, reality multipliers, and dependency chain
5. **Revenue math** — profit (not revenue) projections with best/expected/worst, $/hr per stream, and months-to-break-even
6. **90-day plan** — 2-week blocks with actions, targets, hours, and dependencies
7. **Decision points** — max 5, with default options, framed as commitments