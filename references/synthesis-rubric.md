# Chairman Synthesis Rubric

## Decision Weighting by Question Type

**Technical:** Operator (40%) + Researcher (30%) + Sceptic (20%) + Strategist (10%) + Creative (5%)

**Strategic:** Strategist (40%) + Researcher (30%) + Operator (20%) + Sceptic (10%) + Creative (5%)

**Process/Product:** Operator (35%) + Strategist (25%) + Researcher (20%) + Creative (15%) + Sceptic (10%)

## Conflict Resolution Hierarchy

1. Evidence vs Opinion -> Researcher prevails if data-backed
2. Execution vs Theory -> Operator prevails on "can this be done?"
3. Risk vs Optimism -> Sceptic gets veto on deal-breakers
4. Short vs Long-term -> Strategist unless emergency
5. Conventional vs Creative -> Creative only if conventional has fatal flaw (Sceptic-identified)

## Deal-Breaker Criteria

- Sceptic identifies HIGH-severity risk AND Operator confirms execution blocker
- Researcher presents contradictory evidence with sources
- Strategist identifies compounding negative AND Operator confirms short-term impact
- Cross-examination reveals unresolved contradiction between 3+ agents

## Decision Tree

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

## Synthesis Structure

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

## Synthesis Validation (Step 5.5)

After generating synthesis, check required elements:
- [ ] BLUF present (2-4 sentences)
- [ ] Specific action stated
- [ ] Success criteria defined
- [ ] Reversal conditions stated

If missing any: Error with specific missing element, regenerate synthesis.
