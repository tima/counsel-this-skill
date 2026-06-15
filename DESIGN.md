# Design Philosophy

Council-this is designed as a high-stakes power tool, not a default decision-making aid. Every design choice reinforces appropriate use while actively discouraging misuse.

## Power Tool, Not Default Tool

The skill is intentionally designed to feel exceptional, not routine:

**1. Description emphasis**
Skill description leads with "HIGH-STAKES... SKIP for obvious choices, low-stakes, emergencies"

**2. Step 0 anti-pattern detection**
Stops execution before spawning agents if anti-patterns detected

**3. No arbitrary figures**
Removed specific time/dollar thresholds - use qualitative descriptions instead ("easy to reverse" not "<1 day, <$1000")

**4. "When NOT to Use" prominence**
Anti-patterns listed first, more detailed than positive criteria

**5. Baseline judgment reinforcement**
Repeated message: "Your baseline judgment is already correct" for obvious/low-stakes/emergency cases

## Preventing Theater

### Agent Role Boundaries

Each agent has strict lane enforcement to prevent redundant output:

- **Researcher:** ONLY evidence, data, facts (no opinions or recommendations)
- **Sceptic:** ONLY risks, downsides, failures (no solutions)
- **Strategist:** ONLY long-term consequences (no immediate execution)
- **Operator:** ONLY practical execution (no theoretical benefits)
- **Creative:** ONLY unconventional alternatives (no conventional analysis)

Explicit "DO NOT" statements enforce uniqueness and prevent five agents saying the same thing in different words.

### Fail-Fast Validation

**Original design:** Best-effort synthesis with partial agent output
**Current design:** All 5 agents must provide valid output or council aborts

Result: No garbage-in-garbage-out, clear error messages, no partial recommendations that give false confidence.

Validation gates at two points:
1. Agent output structure (Step 4)
2. Chairman synthesis completeness (Step 5.5)

If either fails, execution stops with specific error - no proceeding with incomplete analysis.

## Question-Type Adaptation

Not all decisions benefit from the same perspective weighting.

**Technical decisions** (architecture, tools, infrastructure):
- Operator + Researcher matter most (execution feasibility + evidence)
- Strategist less critical (unless long-term migration)
- Weight: Operator 40%, Researcher 30%, Sceptic 20%, Strategist 10%, Creative 5%

**Strategic decisions** (business, market, long-term):
- Strategist + Researcher matter most (long-term view + market data)
- Operator important for feasibility check
- Weight: Strategist 40%, Researcher 30%, Operator 20%, Sceptic 10%, Creative 5%

**Process/Product decisions** (workflow, features, coordination):
- Operator + Strategist matter most (execution + scalability)
- Creative more valuable (process innovation opportunities)
- Weight: Operator 35%, Strategist 25%, Researcher 20%, Creative 15%, Sceptic 10%

Agent prompts adapt by question type to emphasize relevant expertise per persona.

## Context Requirements

**Flexible, not rigid:** Different question types need different context

**Technical:** problem, scale, team expertise, integration points
**Strategic:** business state, resources, success criteria, constraints
**Process:** current state, pain points, team structure, metrics

**Timeline is optional** - helpful but not required. User may not know timeline, and guessing isn't helpful. Better to note timeline uncertainty in synthesis than refuse analysis.

**Threshold:** 3 of 4 core items required. Below that, garbage-in-garbage-out risk too high.

## Decision Rubric

Chairman synthesis applies structured decision logic:

**Conflict resolution hierarchy:**
1. Evidence vs Opinion -> Researcher prevails if data-backed
2. Execution vs Theory -> Operator prevails on "can this be done?"
3. Risk vs Optimism -> Sceptic gets veto on deal-breakers
4. Short vs Long-term -> Strategist unless emergency
5. Conventional vs Creative -> Creative only if conventional has fatal flaw

**Deal-breaker criteria:**
- Sceptic identifies high-severity risk AND Operator confirms execution blocker
- Researcher presents contradictory evidence with sources
- Strategist identifies compounding negative AND Operator confirms short-term impact

**Decision tree:**
- All agents align? -> Strong recommendation
- Deal-breaker identified? -> Recommend against + alternatives
- 2-3 clear options emerge? -> Ranked options + criteria
- Insufficient context revealed? -> Recommend gather info + what to gather
- Otherwise -> Present conflicting evidence, no recommendation

## Three-Tier Framework

Council provides middle option between snap judgment and paralysis:

**Tier 1: Baseline judgment** (obvious/low-stakes/emergency)
- Fast, reversible, low-cost decisions
- Your natural judgment already correct
- Council overhead unjustified

**Tier 2: Quick mode** (`--quick` flag, medium-stakes)
- 3 agents instead of 5
- Question-type adaptive selection
- Faster analysis for important-but-not-critical decisions

**Tier 3: Full council** (high-stakes, genuine trade-offs)
- All 5 agents
- Complete synthesis with weighted perspectives
- When thorough deliberation justified

## Anti-Pattern Detection Philosophy

**Core insight from TDD:** Baseline agent behavior is already correct. Agents naturally avoid obvious choices, assess stakes, validate context, distinguish emergencies.

**Problem:** Skills that make council seem universally valuable encourage misuse by providing no guardrails.

**Solution:** Make Step 0 anti-pattern detection the PRIMARY focus. Council's job is to prevent misuse more than enable correct use.

**Result:** Skill reinforces baseline judgment rather than replacing it. Agent uses council when appropriate, declines when not, and cites skill sections for reasoning.

## Design Principles

1. **Appropriate use > ease of use** - Better to make misuse uncomfortable than make council frictionless
2. **Qualitative > quantitative** - Avoid hardcoded thresholds, focus on characteristics
3. **Fail-fast > best-effort** - Better to abort with clear error than proceed with incomplete data
4. **Reinforcement > replacement** - Build on baseline judgment, don't override it
5. **Flexibility > rigidity** - Adapt requirements to question type, don't one-size-fits-all
6. **Uniqueness > completeness** - Each agent stays in lane, no redundant coverage
