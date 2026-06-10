---
name: council-this
description: Use for HIGH-STAKES decisions with genuine trade-offs, sufficient context, and time for 30-45min analysis. SKIP for obvious choices, low-stakes, emergencies, or insufficient context - your baseline judgment is already correct for those.
---

# Council-This — Multi-Persona Decision Analysis

HIGH-IMPACT POWER TOOL for complex decisions requiring multi-perspective stress-testing.

30-45 minute analysis time. Use sparingly. Your default judgment handles most decisions correctly.

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
- Internal-only with <10 usage points
- 1-day reversal time, <$1000 cost
- Tool choice with trivial migration

**Detection:** "How long to undo?" If <1 day + <$1000 -> SKIP COUNCIL

**Why:** Council overhead (30-45min) exceeds reversal cost.

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

### 4. Emergency (Decision Needed <30min)
- Production outage
- Revenue-critical deadline
- Security incident
- Council takes 30-45min minimum

**Detection:** "When decision needed?" If "now" or <30min -> SKIP COUNCIL

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

- [ ] High stakes (>1 week reversal, >$10k cost, or critical path)
- [ ] Genuine trade-offs (multiple valid approaches, no obvious winner)
- [ ] Sufficient context (can answer who/what/when/why/how)
- [ ] 30-45min available for analysis
- [ ] Open decision (not validation-seeking)

**Examples:**
- Architecture decisions (microservices adoption, database migration)
- Strategic choices (market positioning, product direction)
- High-stakes hiring (exec, critical role)
- Major process changes (development workflow, team structure)

---

## Process

### Step 0: Anti-Pattern Check

**BEFORE proceeding, evaluate:**

```
Obvious choice? (expertise asymmetry, clear winner)
  -> YES: "Obvious choice detected. [Explain]. Skip council? (y/n)"

Low stakes? (easy reversal, <1 day + <$1000)
  -> YES: "Low-stakes (<1 day reversal). Council overhead unjustified. Skip? (y/n)"

Emergency? (<30min decision time)
  -> YES: "Emergency detected. Council takes 30-45min. Use quick framework? (y/n)"

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

**Classify by type** (affects persona emphasis):

**Technical:** architecture, database, framework, infrastructure, implementation
- Keywords: architecture, database, framework, API, service, scalable, performant
- Emphasize: Operator (40%) + Researcher (30%) + Sceptic (20%)

**Strategic:** business direction, market, long-term, organizational
- Keywords: market, customer, revenue, vision, roadmap, long-term, 12 months, 3 years
- Emphasize: Strategist (40%) + Researcher (30%) + Operator (20%)

**Process/Product:** workflow, features, team coordination, prioritization
- Keywords: workflow, pipeline, feature, sprint, process, organize, prioritize
- Emphasize: Operator (35%) + Strategist (25%) + Researcher (20%)

---

### Step 2: Validate Sufficient Context

**Required context by question type:**

**Technical:**
- [ ] Problem being solved
- [ ] Scale requirements (users, data, traffic)
- [ ] Team expertise
- [ ] Timeline
- [ ] Integration requirements

**Strategic:**
- [ ] Current business state
- [ ] Resources available (budget, people, time)
- [ ] Success criteria
- [ ] Constraints (market, regulatory, competitive)
- [ ] Timeline

**Process/Product:**
- [ ] Current state
- [ ] Pain points (specific problems)
- [ ] Team size/structure
- [ ] Success metrics
- [ ] Timeline for change

**Context scoring:**
- 0-2 items: REFUSE, require clarification
- 3-4 items: WARN, proceed with caveats noted in report
- 5 items: FULL CONFIDENCE

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

### Step 3: Spawn 5 Council Members (Parallel)

**CRITICAL:** Launch all 5 in SINGLE message with multiple Agent tool calls.

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

Your job: What do we actually know? What assumptions are made? What's missing?

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

Regex pattern:
```
VERDICT:\s*(.+?)
ANALYSIS:
1\.\s*(.+?)
2\.\s*(.+?)
3\.\s*(.+?)
```

**Validation rules:**
- VERDICT: Required, non-empty, 10+ chars
- Each ANALYSIS point: Required, non-empty, 20+ chars

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

### Step 5: Cross-Examination (Adversarial Challenges)

**Replace peer review theater with direct challenges:**

Spawn 5 challenge agents (parallel):

**Challenge 1: Sceptic challenges Researcher**
```
Researcher verdict: [verdict]
Researcher analysis:
1. [point 1]
2. [point 2]
3. [point 3]

Your job (Sceptic): Your analysis assumes [key assumption from Researcher]. What's the strongest evidence this assumption is WRONG?

Respond (2-3 sentences): Challenge + reasoning.
```

**Challenge 2: Operator challenges Strategist**
```
Strategist verdict: [verdict]
[analysis]

Your job (Operator): You focus on [long-term outcome]. What specific execution blocker makes this impossible in practice?

Respond (2-3 sentences).
```

**Challenge 3: Researcher challenges Creative**
```
Creative verdict: [verdict]
[analysis]

Your job (Researcher): Your unconventional approach of [suggestion] - what evidence exists this works at scale?

Respond (2-3 sentences).
```

**Challenge 4: Strategist challenges Operator**
```
Operator verdict: [verdict]
[analysis]

Your job (Strategist): You emphasize [quick path]. What long-term cost are we ignoring for this short-term win?

Respond (2-3 sentences).
```

**Challenge 5: Creative challenges Sceptic**
```
Sceptic verdict: [verdict]
[analysis]

Your job (Creative): You identify [risk]. What alternative reframing eliminates this risk entirely?

Respond (2-3 sentences).
```

**Collect all 5 challenge responses.**

---

### Step 6: Chairman Synthesis with Rubric

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

---

### Step 7: Get Timestamp

```bash
date -u +"%b-%d-%Y %H:%M GMT"
```

---

### Step 8: Format Report

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

**Challenged by Sceptic:** [Challenge response]

### The Sceptic
**Role:** Constructive critic
**Weight:** [X%]
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]

**Challenged by Creative:** [Challenge response]

### The Strategist
**Role:** Long-term thinker
**Weight:** [X%]
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]

**Challenged by Operator:** [Challenge response]

### The Operator
**Role:** Execution expert
**Weight:** [X%]
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]

**Challenged by Strategist:** [Challenge response]

### The Creative
**Role:** Lateral thinker
**Weight:** [X%]
**Verdict:** [Verdict]

**Analysis:**
1. [Point 1]
2. [Point 2]
3. [Point 3]

**Challenged by Researcher:** [Challenge response]

## Cross-Examination Summary

**Key tensions identified:**
- [Tension 1 between perspectives]
- [Tension 2]

**Points of agreement despite challenge:**
- [Agreement 1]
- [Agreement 2]

**Unresolved contradictions:**
- [Contradiction requiring decision]

---

Claude Code | Claude [Model] | [Timestamp UTC]
```

---

### Step 9: Generate Filename

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

### Step 10: Write Report

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
