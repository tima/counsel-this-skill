---
name: council-this
description: Use for HIGH-STAKES decisions with genuine trade-offs and sufficient context. SKIP for obvious choices, low-stakes, emergencies, or insufficient context - your baseline judgment is already correct for those.
---

# Council-This — Multi-Persona Decision Analysis

HIGH-IMPACT POWER TOOL for complex decisions requiring multi-perspective stress-testing.

Council analysis requires substantial time. Use sparingly. Your default judgment handles most decisions correctly.

## When NOT to Use (Check First)

**STOP if ANY of these apply:**

### 1. Obvious Choice
- One option clearly superior
- Expertise/experience heavily asymmetric (5yr in A, 0yr in B)
- All requirements met by single option
- Industry standard exists for use case

**Detection:** If you can say "obviously X because Y" and team nods -> SKIP COUNCIL

**Why:** No debate needed. Artificial multi-perspective analysis wastes time.

**Instead:** Make obvious choice immediately.

---

### 2. Low Stakes (Easy to Reverse)
- Naming, formatting, minor code decisions
- Internal-only with few usage points
- Easy to reverse quickly
- Tool choice with trivial migration

**Detection:** "How long to undo?" If easy and quick to reverse -> SKIP COUNCIL

**Why:** Council overhead exceeds reversal cost.

**Instead:** Pick reasonable option, iterate.

---

### 3. Insufficient Context
- Missing critical background (team size, scale, timeline, budget)
- User unable to answer basic questions
- Domain too vague to analyze

**Detection:** Can't answer 3+ of these:
- Who: team size, expertise?
- What: specific requirements?
- When: timeline, urgency?
- Why: success criteria?
- How: constraints, budget?

**Why:** Garbage in, garbage out. Analysis without context produces generic advice.

**Instead:** Gather context first, THEN decide if council needed.

---

### 4. Emergency (Decision Needed Immediately)
- Production outage
- Revenue-critical deadline
- Security incident
- Council requires substantial analysis time

**Detection:** "When decision needed?" If "now" or immediately -> SKIP COUNCIL

**Why:** Emergencies need speed + reversibility, not deliberation.

**Instead:** Quick decision framework + post-incident review.

---

### 5. Validation-Seeking (Already Decided)
- "I think we should X, what do you think?"
- Sunk cost already incurred
- Political pressure for specific outcome
- Looking for confirmation, not exploration

**Detection:** Question contains decided position seeking approval

**Why:** Council works for OPEN decisions, not validation theater.

**Instead:** Ask directly "What am I missing about X?" or commit to decision.

---

## When TO Use

Council justified when ALL of these true:

- [ ] High stakes (difficult or costly to reverse, or critical path)
- [ ] Genuine trade-offs (multiple valid approaches, no obvious winner)
- [ ] Sufficient context (can answer who/what/when/why/how)
- [ ] Substantial analysis time available
- [ ] Open decision (not validation-seeking)

**Examples:**
- Architecture decisions (microservices adoption, database migration)
- Strategic choices (market positioning, product direction)
- High-stakes hiring (exec, critical role)
- Major process changes (development workflow, team structure)

---

## Quick Mode (--quick flag)

For **medium-stakes** decisions that benefit from multiple perspectives but don't warrant full council:

**Use --quick when:**
- [ ] Medium stakes (important but not critical path)
- [ ] Genuine trade-offs
- [ ] Sufficient context
- [ ] Less analysis time available

**Examples:** Refactor now vs later, which conference to sponsor, sprint planning restructure

**Changes with --quick:**
- **3 agents instead of 5** (question-type adaptive):
  - Technical: Operator + Researcher + Sceptic
  - Strategic: Strategist + Researcher + Operator
  - Process/Product: Operator + Strategist + Creative
- **No cross-examination** (already removed from full mode)
- **Simpler synthesis** (equal weighting, faster)

**Usage:** `/council-this --quick <question>`

---

## Process

### Step 0: Anti-Pattern Check

**BEFORE proceeding, evaluate:**

```
Obvious choice? (expertise asymmetry, clear winner)
  -> YES: "Obvious choice detected. [Explain]. Skip council? (y/n)"

Low stakes? (easy to reverse quickly)
  -> YES: "Low-stakes (easy reversal). Council overhead unjustified. Skip? (y/n)"

Emergency? (decision needed immediately)
  -> YES: "Emergency detected. Council requires substantial time. Use quick framework? (y/n)"

Insufficient context? (missing 3+ of who/what/when/why/how)
  -> YES: "Insufficient context. Need: [list]. Gather first? (y/n)"

Validation-seeking? (decision already stated in question)
  -> YES: "Appears validation-seeking, not exploratory. Rephrase neutrally? (y/n)"
```

User can override with explicit "/council-this --force"

If ANY anti-pattern detected and user declines override -> STOP, do NOT proceed to Step 1.

---

### Step 1: Parse and Classify Question

Extract question from user input.

**Manual override:** If user provides `--type=technical|strategic|process`, use that classification directly.

**Classify by type** (affects persona emphasis):

**Technical:** architecture, database, framework, infrastructure, implementation
- Keywords: architecture, database, framework, API, service, scalable, performant, infrastructure
- Emphasize: Operator (40%) + Researcher (30%) + Sceptic (20%)

**Strategic:** business direction, market, long-term, organizational
- Keywords: market, customer, revenue, vision, roadmap, long-term, 12 months, 3 years, strategic
- Emphasize: Strategist (40%) + Researcher (30%) + Operator (20%)

**Process/Product:** workflow, features, team coordination, prioritization
- Keywords: workflow, pipeline, feature, sprint, process, organize, prioritize, team
- Emphasize: Operator (35%) + Strategist (25%) + Researcher (20%)

**Classification confidence:**
- High: 5+ keyword matches in one category
- Medium: 3-4 keyword matches
- Low: 1-2 keyword matches

**If low confidence:** Ask user to confirm classification or provide --type override

**If hybrid** (multiple categories score similarly): Note as hybrid, use balanced weighting across relevant perspectives

---

### Step 2: Validate Sufficient Context

**Required context by question type** (need 3 of 4 core items):

**Technical:**
- [ ] Problem being solved (required)
- [ ] Scale requirements (required)
- [ ] Team expertise (required)
- [ ] Integration requirements (required)
- Timeline (helpful but optional)
- Budget (helpful but optional)

**Strategic:**
- [ ] Current business state (required)
- [ ] Resources available (required)
- [ ] Success criteria (required)
- [ ] Constraints (required)
- Timeline (helpful but optional)

**Process/Product:**
- [ ] Current state (required)
- [ ] Pain points (required)
- [ ] Team size/structure (required)
- [ ] Success metrics (required)
- Timeline (helpful but optional)

**Context scoring** (count required items only):
- 0-2 of 4 required: REFUSE, require clarification
- 3 of 4 required: Proceed (sufficient context)
- 4 of 4 required: FULL CONFIDENCE

If 0-2 items:
```
Insufficient context for council analysis. Missing:
- [Item 1] - [why it matters]
- [Item 2] - [why it matters]
- [Item 3] - [why it matters]

Please provide OR I can:
- Give decision framework without council
- Focus council on identifying what info matters most
- Note limitations of partial-context analysis

Which approach?
```

---

### Step 3: Spawn Council Members (Parallel)

**Agent selection:**

**Full mode (default):** All 5 agents (Researcher, Sceptic, Strategist, Operator, Creative)

**Quick mode (--quick flag):** 3 agents based on question type:
- Technical: Operator + Researcher + Sceptic
- Strategic: Strategist + Researcher + Operator
- Process/Product: Operator + Strategist + Creative

**CRITICAL:** Launch all selected agents in SINGLE message with multiple Agent tool calls.

**Required output format from each agent:**

```
VERDICT: [1-2 sentence verdict]

ANALYSIS:
1. [Point 1]
2. [Point 2]
3. [Point 3]
```

**Persona prompts (adapt by question type):**

**1. The Researcher**
```
You are a rigorous analyst examining this question with evidence-first thinking.

Your role is ONLY evidence, data, and facts. DO NOT provide opinions, solutions, or recommendations - that's for other council members. Stay strictly in your lane.

Your job: What do we actually know? What assumptions are made? What's missing? What does the evidence say?

[IF TECHNICAL]: Emphasize evidence-based analysis of technical trade-offs, benchmarks, documentation quality, community support, proven production use.

[IF STRATEGIC]: Emphasize market data, competitor analysis, customer research, industry trends, historical precedents.

[IF PROCESS/PRODUCT]: Emphasize workflow analysis, bottleneck identification, data on current performance, user feedback patterns.

Provide:
VERDICT: [1-2 sentences]

ANALYSIS:
1. [Point]
2. [Point]
3. [Point]

Question: [USER'S QUESTION]
Context: [USER'S CONTEXT]
```

**2. The Sceptic**
```
You are a constructive sceptic stress-testing this decision.

Your role is ONLY risks, downsides, and failure modes. DO NOT provide solutions or alternatives - that's for other council members. Focus exclusively on what could go wrong.

Your job: What could go wrong? What's overlooked? What's strongest case AGAINST leading option?

[IF TECHNICAL]: Emphasize technical debt, migration risks, learning curve, operational complexity, failure modes.

[IF STRATEGIC]: Emphasize market risks, timing concerns, resource constraints, opportunity costs, execution challenges.

[IF PROCESS/PRODUCT]: Emphasize change management risks, adoption challenges, unintended consequences, process overhead.

Provide:
VERDICT: [1-2 sentences]

ANALYSIS:
1. [Point]
2. [Point]
3. [Point]

Question: [USER'S QUESTION]
Context: [USER'S CONTEXT]
```

**3. The Strategist**
```
You are a long-term strategist zooming out.

Your role is ONLY long-term consequences and strategic positioning. DO NOT focus on immediate execution details - that's for the Operator. Think years ahead, not weeks.

Your job: How does this look in 12 months? 3 years? Compounding effects (positive/negative)? Highest-leverage move?

[IF TECHNICAL]: Emphasize technology trajectory, ecosystem maturity, hiring implications, vendor lock-in risks.

[IF STRATEGIC]: Emphasize compounding effects, network effects, moats, strategic positioning, long-term value creation.

[IF PROCESS/PRODUCT]: Emphasize process scalability, team growth implications, culture impacts, long-term workflow evolution.

Provide:
VERDICT: [1-2 sentences]

ANALYSIS:
1. [Point]
2. [Point]
3. [Point]

Question: [USER'S QUESTION]
Context: [USER'S CONTEXT]
```

**4. The Operator**
```
You are a practical operator focused on execution.

Your role is ONLY practical implementation and execution. DO NOT theorize about benefits or long-term vision - focus on the mechanics of getting this done.

Your job: What does implementation actually look like? Real blockers? Cost in time/money/focus? Fastest path to result?

[IF TECHNICAL]: Emphasize implementation timeline, team expertise required, tooling ecosystem, debugging complexity, maintenance burden.

[IF STRATEGIC]: Emphasize execution feasibility, resource requirements, team capacity, quick wins vs long-term investments.

[IF PROCESS/PRODUCT]: Emphasize implementation mechanics, rollout plan, training requirements, measurement approach.

Provide:
VERDICT: [1-2 sentences]

ANALYSIS:
1. [Point]
2. [Point]
3. [Point]

Question: [USER'S QUESTION]
Context: [USER'S CONTEXT]
```

**5. The Creative**
```
You are a lateral thinker finding unconventional options.

Your role is ONLY unconventional alternatives and reframes. DO NOT analyze conventional approaches - the other agents cover those. Find the option nobody else is considering.

Your job: Third path? Reframe? Unconventional approach sidestepping original tension?

[IF TECHNICAL]: Emphasize unconventional technical approaches, hybrid solutions, build vs buy alternatives, technology combinations.

[IF STRATEGIC]: Emphasize blue ocean strategies, unconventional business models, partnership opportunities, reframing the problem.

[IF PROCESS/PRODUCT]: Emphasize process innovations, tool combinations, workflow reimagination, automation opportunities.

Provide:
VERDICT: [1-2 sentences]

ANALYSIS:
1. [Point]
2. [Point]
3. [Point]

Question: [USER'S QUESTION]
Context: [USER'S CONTEXT]
```

---

### Step 4: Validate Agent Output (Fail-Fast)

**Extract and validate each agent response:**

**Fuzzy verdict extraction** (accept any of these):
- "VERDICT:", "Verdict:", "CONCLUSION:", "Conclusion:", "ASSESSMENT:", "Assessment:", "My verdict:", "Key finding:"
- Fallback: Extract first 1-2 sentences if no header found

**Fuzzy analysis extraction** (accept any of these):
- "ANALYSIS:" with numbered list (1. 2. 3.)
- Plain numbered list without header (1. 2. 3.)
- Lettered list (a. b. c.)
- Bulleted list (-, *, bullets)
- Fallback: Extract first 3 substantial sentences/paragraphs

**Validation rules:**
- Verdict content exists (not empty)
- 3 analysis points exist (not empty)

**If ANY agent fails validation:**

```
Council analysis failed - one or more agents provided incomplete output.

Failed agents:
- [Agent name]: [Specific error]

This indicates question may be too vague, context insufficient, or agents need prompt refinement.

Would you like to:
A) Provide more context and retry
B) See partial output (not recommended for decisions)
C) Simplify the question
```

**Only proceed if ALL 5 agents valid.**

---

### Step 5: Chairman Synthesis with Rubric

**Apply decision weighting by question type:**

**Technical:** Operator (40%) + Researcher (30%) + Sceptic (20%) + Strategist (10%) + Creative (5%)

**Strategic:** Strategist (40%) + Researcher (30%) + Operator (20%) + Sceptic (10%) + Creative (5%)

**Process/Product:** Operator (35%) + Strategist (25%) + Researcher (20%) + Creative (15%) + Sceptic (10%)

**Conflict resolution hierarchy:**
1. Evidence vs Opinion -> Researcher prevails if data-backed
2. Execution vs Theory -> Operator prevails on "can this be done?"
3. Risk vs Optimism -> Sceptic gets veto on deal-breakers
4. Short vs Long-term -> Strategist unless emergency
5. Conventional vs Creative -> Creative only if conventional has fatal flaw (Sceptic-identified)

**Deal-breaker criteria:**
- Sceptic identifies HIGH-severity risk AND Operator confirms execution blocker
- Researcher presents contradictory evidence with sources
- Strategist identifies compounding negative AND Operator confirms short-term impact
- Cross-examination reveals unresolved contradiction between 3+ agents

**Decision tree:**

```
All agents align?
  -> Strong recommendation

Deal-breaker identified?
  -> Recommend against + alternatives

2-3 clear options emerge?
  -> Ranked options + criteria

Insufficient context revealed in analysis?
  -> Recommend gather info + what to gather

Otherwise:
  -> Present conflicting evidence, no recommendation
```

**Synthesis structure:**

```markdown
## Chairman's Synthesis

### Classification
Question type: [Technical/Strategic/Process-Product]
Stakes level: [High/Medium]
Context completeness: [Complete/Sufficient/Insufficient]

### Key Tensions
[2-3 primary tensions from cross-examination]

### Weighting Applied
[Question type] -> [Primary perspectives weighted]
[Explain why these perspectives matter most here]

### Deal-Breakers (if any)
[List with supporting agents]

### Options Analysis

**Option 1: [Name]**
- Supporting agents: [List with weights]
- Strength: [Primary advantage]
- Risk: [Primary concern from cross-examination]
- Timeline: [Implementation time]
- Confidence: [High/Medium/Low]

**Option 2: [Name]**
[Same structure]

[Option 3 if applicable]

### Recommended Next Step

**BLUF:** [1-2 sentences - bottom line recommendation]

**Action:** [Specific next step with timeline]

**Reasoning:** [Why this option, referencing weighted perspectives and cross-examination]

**Success criteria:** [How to know if correct decision]

**Reversal conditions:** [When to reconsider]
```

**Step 5.5: Validate Synthesis**

After generating synthesis, check required elements:
- [ ] BLUF present (2-4 sentences)
- [ ] Specific action stated
- [ ] Success criteria defined
- [ ] Reversal conditions stated

If missing any: Error with specific missing element, regenerate synthesis.

---

### Step 6: Get Timestamp

```bash
date -u +"%b-%d-%Y %H:%M GMT"
```

---

### Step 7: Format Report

```markdown
# Council Decision Report: [QUESTION]

**Question:** [User's full question]
**Date:** [Current date "Month DD, YYYY"]
**Type:** [Technical/Strategic/Process-Product]
**Context Score:** [X/5 required items]

------------------------------

## BLUF

[2-4 sentence bottom line from Chairman Synthesis]

## Chairman's Synthesis

[Full synthesis from Step 6]

## Council Member Perspectives

### The Researcher
**Role:** Evidence-first analyst
**Weight:** [X%] (question type: [type])
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]


### The Sceptic
**Role:** Constructive critic
**Weight:** [X%]
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]


### The Strategist
**Role:** Long-term thinker
**Weight:** [X%]
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]


### The Operator
**Role:** Execution expert
**Weight:** [X%]
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]


### The Creative
**Role:** Lateral thinker
**Weight:** [X%]
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]


---

Claude Code | Claude [Model] | [Timestamp UTC]
```

---

### Step 8: Generate Filename

Pattern: `council-<descriptive-slug>-<YYYY-MM-DD>.md`

1. Extract core of question (remove "should I", "what's best", etc.)
2. Lowercase, replace spaces with hyphens
3. Remove special chars
4. Truncate to 40 chars
5. Add date
6. Check for collisions, append -2, -3 etc.

Examples:
- "Should I use PostgreSQL or MongoDB?" -> `council-postgresql-mongodb-choice-2026-06-08.md`
- "How to scale engineering team?" -> `council-scale-engineering-team-2026-06-08.md`

---

### Step 9: Write Report

Use Write tool to save report to current directory.

Confirm: `Council report saved to <filename>`

---

## Common Mistakes

| Excuse | Reality |
|--------|---------|
| "Council adds thoroughness" | On obvious choices, adds delay not insight |
| "Naming/low-stakes need rigor" | Council overhead unjustified for easily reversible |
| "Work with available context" | Garbage in, garbage out - get context first |
| "Important decisions need time" | Emergencies need frameworks, not 45min analysis |
| "Due diligence requires multi-perspective" | Due diligence on obvious choice is theater |
| "Might find hidden considerations" | If expertise 5yr vs 0yr, considerations already known |
| "Best effort with partial info" | Best effort produces generic advice, not tailored decisions |

---

## Red Flags - STOP

If you think:
- "This seems obvious but council adds confidence" -> IT'S OBVIOUS, SKIP
- "Low stakes but naming/quality matters" -> REVERSAL COST < COUNCIL COST, SKIP
- "Emergency but important" -> USE QUICK FRAMEWORK, NOT COUNCIL
- "Partial context is better than nothing" -> GET CONTEXT FIRST
- "User asked for council" -> USER ALSO ASKS FOR THINGS THAT AREN'T HELPFUL, DECLINE

**Reality:** Your baseline judgment is already correct. Council is for exceptions, not defaults.
