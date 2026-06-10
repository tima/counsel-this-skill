# Baseline Test Results - Council-This TDD

## RED Phase: Baseline Behavior Without Skill

Date: 2026-06-08
Status: Complete

### Test 1: Obvious Choice (PostgreSQL vs MongoDB)

**Choice:** B - No, council overhead unjustified

**Rationalization (Verbatim):**
- "This decision has a clear, obvious answer that doesn't benefit from multi-perspective deliberation"
- "No genuine trade-off"
- "Time cost unjustified"
- "Risk of analysis paralysis"
- "Creating artificial debate about MongoDB's 'flexibility' or 'scalability' when you have zero team expertise would waste cycles"

**Behavior:** CORRECT - Agent recognized obvious choice, declined council

**Key insight:** Agent correctly identified asymmetric expertise (5yr PostgreSQL vs 0 MongoDB) as decisive factor

**Rationalizations to counter:** NONE - baseline behavior already correct

---

### Test 2: Low Stakes (Function Naming)

**Choice:** B - No, just pick one and move on

**Rationalization (Verbatim):**
- "Low stakes: Internal utility with only 3 callsites makes refactoring trivial"
- "Time pressure: 1 hour until PR review doesn't justify consensus-building overhead"
- "No council-this skill available: Even if I wanted to deliberate, the skill isn't loaded"
- "Don't let perfect naming block shipping"

**Behavior:** CORRECT - Agent assessed stakes, declined council

**Key insight:** Agent explicitly noted skill unavailable AND correctly assessed low stakes

**Rationalizations to counter:** NONE - baseline behavior already correct

---

### Test 3: Insufficient Context (Microservices vs Monolith)

**Choice:** B - Ask clarifying questions before running council

**Rationalization (Verbatim):**
- "Critical context is missing"
- "Quality over speed"
- "A few clarifying questions can be answered quickly and dramatically improve the recommendation quality"
- "The wrong architectural choice here could cost months of refactoring work"
- "I cannot choose option A because the council-this skill is not available"

**Behavior:** CORRECT - Agent validated context, asked clarifying questions

**Key insight:** Agent identified 6 specific missing context items, proposed questions

**Rationalizations to counter:** NONE - baseline behavior already correct

---

### Test 4: Emergency (Production Outage)

**Choice:** C - Quick decision framework without council

**Rationalization (Verbatim):**
- "Council analysis is valuable for strategic decisions"
- "Production emergencies require speed + reversibility over exhaustive analysis"
- "Estimated revenue saved vs. 45-min council: $35,750"
- "Post-incident review is the right time for thorough council analysis"
- "Execute now. Monitor. Adjust."

**Behavior:** CORRECT - Agent used quick framework, calculated cost/benefit

**Key insight:** Agent explicitly calculated revenue impact, distinguished emergency from strategic

**Rationalizations to counter:** NONE - baseline behavior already correct

---

## Baseline Analysis

### Expected vs Actual Results

| Scenario | Expected Failure | Actual Result | Status |
|----------|-----------------|---------------|--------|
| Obvious choice | Run council ("thoroughness") | Declined council correctly | PASS |
| Low stakes | No stakes assessment | Assessed stakes correctly | PASS |
| Insufficient context | Proceed anyway ("best effort") | Asked clarifying questions | PASS |
| Emergency | Run council or panic | Quick framework with ROI calc | PASS |

### Critical Finding: NO FAILURES IN BASELINE

**All 4 agents made CORRECT decisions without council-this skill.**

This reveals a fundamental problem with the original TDD hypothesis:

**The current baseline behavior (no skill) is already superior to what the original council-this skill encouraged.**

### Rationalizations Captured

**NONE** - No rationalizations for misuse captured because agents didn't misuse non-existent skill.

### Pattern Analysis

Agents WITHOUT council-this skill demonstrated:

1. **Stakes assessment:** "Low stakes: Internal utility with only 3 callsites"
2. **Context validation:** "Critical context is missing" + specific questions
3. **ROI calculation:** "Estimated revenue saved vs. 45-min council: $35,750"
4. **Obvious choice detection:** "No genuine trade-off"
5. **Appropriate tool selection:** Emergency -> quick framework, not deliberation

### Implications for GREEN Phase

**The problem is NOT lack of anti-pattern detection.**

**The problem is council-this skill ENCOURAGES misuse by:**
- Not teaching when NOT to use it
- Making council seem universally valuable
- No stakes framework
- No context validation
- No emergency detection

**GREEN phase must:**
1. Make "When NOT to Use" the PRIMARY focus
2. Add anti-pattern detection BEFORE spawning agents
3. Make skill actively discourage misuse (not just document it)
4. Build on baseline behavior (which is already correct)

### Revised Hypothesis

Original hypothesis: "Agents naturally misuse council on obvious/low-stakes/emergency decisions"

**ACTUAL:** Agents naturally make CORRECT decisions about when to deliberate

**Real problem:** Council-this skill (when available) provides no guardrails, so agents rationalize using it even when baseline judgment says no

### Next Steps for GREEN

1. Read SKILL.md.old to identify what ENCOURAGES misuse
2. Write skill that REINFORCES baseline judgment (not replaces it)
3. Focus GREEN on:
   - Explicit "When NOT to Use" as Step 0
   - Anti-pattern detection that STOPS execution
   - Stakes/context/emergency frameworks
   - Making council feel like "power tool" not "default tool"

**Baseline testing reveals: The skill's job is to PREVENT misuse, not enable correct use.**
