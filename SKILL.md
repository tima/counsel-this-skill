---
name: counsel-this
description: "Use when faced with a high-stakes decision that has genuine trade-offs and no obvious right answer. Triggers on: 'help me decide between X and Y', 'I need a second opinion on...', 'what should I do about...', 'talk me through this decision', 'weigh the options for...'. Skip for obvious choices, low-stakes calls, emergencies, or when context is insufficient — your baseline judgment handles those correctly."
---

# Counsel-This — Multi-Persona Decision Analysis

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

Select agents based on mode: all 5 (Researcher, Sceptic, Strategist, Operator, Creative) in full mode, or 3 in quick mode based on question type. Launch all selected agents in a SINGLE message with multiple Agent tool calls. Each agent returns a VERDICT (1-2 sentences) and ANALYSIS (3 numbered points), staying strictly within their assigned role — evidence, risk, strategy, execution, or creative alternatives.

For persona definitions and instructions, see references/council-personas.md

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

Weight agent verdicts by question type (Technical: Operator 40%, Researcher 30%, Sceptic 20%; Strategic: Strategist 40%, Researcher 30%, Operator 20%; Process/Product: Operator 35%, Strategist 25%, Researcher 20%). Apply conflict resolution hierarchy and deal-breaker criteria to produce a structured synthesis: Classification, Key Tensions, Weighting Applied, Options Analysis, and Recommended Next Step with BLUF, Action, Reasoning, Success criteria, and Reversal conditions. Validate synthesis has all required elements before proceeding.

For the full rubric, see references/synthesis-rubric.md

---

### Step 6: Get Timestamp

```bash
date -u +"%b-%d-%Y %H:%M GMT"
```

---

### Step 7: Format Report

Assemble the final markdown report: header with question/date/type/context score, BLUF section, full Chairman's Synthesis, and individual Council Member Perspectives for each agent (role, weight, verdict, analysis points). Close with model and timestamp attribution.

For the full report template, see references/report-format.md

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
