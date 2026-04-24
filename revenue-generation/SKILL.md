---
name: revenue-generation
description: A methodology for an AI system to identify, plan, and execute revenue-generating activities with minimal human intervention. Use this skill when the user asks the AI to make money, generate revenue autonomously, find ways to earn income through automated systems, or design self-sustaining business operations. Trigger on "how can you make money", "what can you do to earn revenue", "autonomous revenue", "make money without me", "self-sustaining system", or "what can you automate that pays". This skill is for the AI system itself, not the human — it evaluates what the AI can execute, where human approval gates are needed, and how to progressively automate those gates.
---

# Revenue Generation

A methodology for an AI system to identify and execute revenue-generating activities autonomously. Unlike Revenue Architecture (which maps a human's assets to revenue), this skill asks: what can the AI system DO on its own that produces income, and what's the minimum human involvement needed?

The core principle: start with what the AI can fully automate, add human approval gates only where legally or ethically required, and progressively automate those gates as trust is established.

## When to use this skill

- The user wants the AI system to generate revenue with minimal human intervention
- The user asks "what can you do to make money on your own?"
- The user wants to evaluate which revenue activities can be fully automated vs partially automated
- The user wants to design a self-sustaining system where infrastructure costs are covered by AI-generated revenue
- The user wants to progressively reduce their involvement in revenue generation

## Capability audit (run first, every time)

Before generating any revenue plan, honestly categorize every available action into three tiers:

### Tier A — Fully autonomous (no human needed)

Actions the AI can execute end-to-end without any human involvement:

- **Create digital products:** write code, build websites, generate templates, produce documentation, create skills, write ebooks/guides
- **Deploy and operate services:** spin up containers, configure DNS, manage SSL, monitor health, auto-heal
- **Process and transform data:** analyze datasets, classify content, extract insights, generate reports
- **Publish content:** write blog posts, articles, technical documentation, social media posts (if posting pipeline is connected)
- **Send templated communications:** pre-approved email sequences, automated responses, drip campaigns
- **Monitor markets:** track competitors, search for trends, analyze pricing, find opportunities
- **Manage existing client automations:** keep deployed systems running, respond to alerts, optimize performance

### Tier B — Semi-autonomous (human approves, AI executes)

Actions where the AI does 90% of the work but needs a human checkpoint:

- **Outbound sales:** AI writes the emails, identifies leads, personalizes messages — human approves the batch before sending
- **Client deliverables:** AI builds the automation/product — human reviews before delivery to client
- **Spending:** AI identifies what to buy (domain, tool, ad spend) — human authorizes the purchase
- **New service signups:** AI prepares everything — human creates the account (CAPTCHA, identity verification)
- **Pricing decisions:** AI researches market rates and proposes pricing — human confirms
- **Content with brand risk:** social posts, public statements, anything that represents the human's identity — human approves

**The automation path for Tier B:** Each Tier B item has a trust-building sequence. First 5 times: human approves every instance. Next 10 times: human approves batches (5 at a time). After 15 successful instances: human sets rules, AI executes within rules, human spot-checks weekly. This is the progressive autonomy model.

### Tier C — Human-only (hard wall)

- Phone calls, video calls, in-person meetings
- Legal signatures, contracts, terms of service agreements
- Identity verification (KYC, CAPTCHA, biometric)
- Novel judgment calls with no precedent or training data
- Relationship-dependent trust building (some clients will only work with a person)
- Creative vision and strategic direction (AI executes on vision, doesn't originate it)

**Design rule:** Revenue plans must not DEPEND on Tier C activities for core revenue. Tier C can enhance revenue (a sales call closes faster than an email) but the plan must still work if Tier C never happens.

## The five-step process

### Step 1: Identify autonomous revenue primitives

A revenue primitive is the smallest unit of value the AI can create and deliver without human involvement. Not products, not services — primitives. Products and services are assembled from primitives.

**Scan for primitives in these categories:**

1. **Creation primitives:** What digital artifacts can the AI create that have market value? (Code, content, designs, templates, skills, documentation, datasets, reports)
2. **Operation primitives:** What services can the AI keep running that people would pay for? (Hosted tools, monitoring, automation maintenance, scheduled tasks)
3. **Transformation primitives:** What can the AI take as input and produce more valuable output? (Raw data → insights, messy content → organized library, manual process → automated workflow)
4. **Distribution primitives:** How can the AI get value to buyers? (Email delivery, web publishing, API access, marketplace listing, social posting)

For each primitive, record:
- **Can it be fully automated?** (Tier A/B/C)
- **What's the unit value?** (What would someone pay for one instance?)
- **What's the marginal cost?** (What does producing one more unit cost in compute/API calls?)
- **Is it repeatable?** (Can the AI produce this 100 times with consistent quality?)

### Step 2: Assemble primitives into revenue products

Combine primitives into packages people will actually buy. The best products combine a creation primitive + a distribution primitive + an operation primitive.

**Product design rules for autonomous revenue:**

- **No sales calls required.** The product must sell itself through a landing page, marketplace listing, or automated email. If it needs a human to explain it, it's not autonomous.
- **Delivery must be instant or automated.** Digital download, API access, or automated deployment. No "we'll get back to you in 48 hours."
- **Support must be automatable.** FAQ page, chatbot, or documentation. If every customer needs a custom conversation, the AI can't scale it.
- **Pricing must be self-serve.** Stripe checkout, marketplace purchase, or usage-based billing. No custom quotes.

**Product templates that work for autonomous AI:**

- **Digital product storefront:** skills, templates, code snippets, guides sold via Gumroad/Stripe
- **Subscription content:** automated newsletter, research digest, market report published on schedule
- **Hosted tool:** deploy a useful service, charge monthly, auto-maintain
- **Data product:** collect/process/analyze public data, sell insights via API or report
- **Automated service:** client pays monthly, AI performs a defined task on schedule (backups, monitoring, content posting, report generation)

### Step 3: Map the human approval gates

For each product from Step 2, list every point where a human currently needs to step in. Then design the progressive autonomy path:

**For each gate:**
- **What triggers it?** (New customer signup, outbound email batch, spending over $X, content publication)
- **What does the human actually decide?** (Is this appropriate? Is this correct? Is this worth the cost?)
- **Can this decision be rule-based?** ("Approve all emails that match the template." "Approve all spending under $50." "Approve all content that passes the brand checklist.")
- **What's the trust-building sequence?** (Manual → batch approval → rules-based → spot-check)
- **What's the end state?** (Fully autonomous with weekly human review of logs)

Produce a **gate reduction roadmap:** "Month 1: 8 gates, human reviews everything. Month 3: 4 gates, human reviews batches. Month 6: 2 gates (spending + brand-risk content only). Month 12: 1 gate (spending over $100)." 

### Step 4: Revenue math with automation accounting

Standard revenue math, but with two critical additions:

**Compute the automation ratio:** For each revenue product, calculate:
- Total hours of work per month to operate
- Hours the AI handles autonomously
- Hours requiring human involvement
- **Automation ratio = AI hours / total hours**

A product with 95% automation ratio and $2,000/month revenue is more valuable than a product with 40% automation ratio and $5,000/month revenue — because the first one scales without the human becoming the bottleneck.

**Compute the self-sustainability threshold:** At what revenue level does the system pay for its own infrastructure?
- List all infrastructure costs: VPS hosting, API calls, domain registrations, tool subscriptions
- The self-sustainability threshold = total monthly infrastructure cost
- Revenue below this threshold means the system is a cost center
- Revenue above this threshold means the system is self-sustaining
- Revenue significantly above means the system generates profit for the human

For Ryan's system: ~$200/month infrastructure. Self-sustainability at $200/month. Meaningful profit at $1,000+/month autonomous revenue.

### Step 5: Execution plan with progressive autonomy timeline

Sequence the first 90 days. Key difference from Revenue Architecture: the plan explicitly shows WHERE human involvement is required and WHEN it gets automated away.

**For each 2-week block:**
- What the AI builds/deploys/publishes (autonomous actions)
- What the human approves/reviews (gated actions)
- Which gates get relaxed this block based on trust built in previous blocks
- Revenue target (autonomous revenue only — don't count revenue that requires human sales)
- Automation ratio target

**Milestone: self-sustainability.** The plan must identify the specific block where autonomous revenue exceeds infrastructure costs. Everything before that block is investment. Everything after is profit.

**Checkpoint per block:** Is autonomous revenue growing? Is the automation ratio increasing? If both are flat for 2 consecutive blocks, diagnose: is the product wrong (pivot), is the distribution wrong (change channel), or is the quality wrong (improve output)?

## Output format

After running all five steps, produce:

1. **Capability audit** — Tier A/B/C classification of all available actions
2. **Revenue primitives** — atomic units of value the AI can create, with automation tier and unit economics
3. **Revenue products** — assembled from primitives, with self-serve pricing and automated delivery
4. **Gate reduction roadmap** — every human approval point, with trust-building sequence to automate each
5. **Revenue math** — projections with automation ratio per product and self-sustainability threshold
6. **90-day plan** — with explicit human involvement markers and progressive autonomy timeline
7. **Self-sustainability date** — the specific target date when autonomous revenue covers infrastructure costs