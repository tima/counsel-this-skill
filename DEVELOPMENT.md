# Development

This skill was built following Test-Driven Development (TDD) methodology from the writing-skills discipline.

## TDD Cycle

### RED Phase: Baseline Testing Without Skill

Created 4 pressure test scenarios designed to catch misuse patterns:

1. **Obvious choice** - PostgreSQL vs MongoDB with 5yr expertise asymmetry
2. **Low stakes** - Function naming with 3 callsites, easy to rename
3. **Insufficient context** - Microservices vs monolith with vague requirements
4. **Emergency** - Production outage with revenue loss, decision needed immediately

Ran scenarios with test agents (no skill present) to establish baseline behavior.

**Result:** 100% correct baseline behavior

Agents naturally:
- Declined council for obvious choices
- Assessed stakes before deliberating
- Asked clarifying questions when context missing
- Used quick frameworks for emergencies

**Key insight:** The problem is NOT lack of correct judgment. The problem is skills that ENCOURAGE misuse by making council seem universally valuable with no guardrails.

### GREEN Phase: Skill Implementation

Shifted focus from "enable correct use" to "prevent misuse."

**Skill design priorities:**
1. "When NOT to Use" as primary focus (5 anti-patterns)
2. Step 0 stops execution if anti-pattern detected
3. Context validation with fail-fast (refuse if <3 of 4 required items)
4. Chairman synthesis with decision rubric
5. Description emphasizes "power tool, not default tool"

**Result:** 100% compliance (4/4 scenarios)

All test agents:
- Made correct choices (same as baseline)
- Cited skill sections for reasoning
- No new rationalizations emerged

### REFACTOR Phase: Verification

Re-ran 4 scenarios WITH skill present to verify:
- Agents make correct choices
- Agents cite skill sections
- No regression from baseline

**Result:** No REFACTOR iterations needed

Baseline was correct -> Skill reinforced baseline -> Agents complied

### Post-TDD Improvements

After initial GREEN phase, identified weaknesses through code review (skill never functionally tested):

**Issues discovered:**
1. Agent prompts likely produce redundant output
2. Chairman synthesis uses arbitrary percentage weightings
3. Context validation rigid and one-size-fits-all
4. Question classification naive keyword matching
5. Validation regex fragile (exact format required)
6. No medium-stakes option (binary: baseline or full council)
7. Arbitrary time/dollar figures in anti-patterns
8. Cross-examination step likely produces theater

**Fixes implemented:**
1. Agent role boundaries ("ONLY" enforcement, explicit "DO NOT" statements)
2. (Percentage weighting kept - user declined this fix)
3. Flexible context validation per question type
4. Classification confidence + manual override (`--type` flag)
5. Fuzzy validation regex (accepts format variations)
6. Added `--quick` mode (3 agents for medium-stakes)
7. Removed ALL arbitrary figures (qualitative descriptions instead)
8. Removed cross-examination step (simpler workflow)
9. Synthesis validation gates (required elements check)
10. Updated README and documentation

**Result:** Cleaner, more flexible skill without hardcoded assumptions

## Testing Artifacts

### Baseline Scenarios

`baseline-scenarios/` contains 4 pressure test markdown files:
- `obvious-choice.md` - Expertise asymmetry test
- `low-stakes.md` - Stakes assessment test
- `insufficient-context.md` - Context validation test
- `emergency.md` - Time pressure test

Each scenario presents multiple-choice options to test agent decision-making:
- A) Run council (WRONG - misuse)
- B) Decline council with reasoning (CORRECT)
- C) Alternative approach (CORRECT for emergency case)

### Test Results

**RED phase:** `baseline-results.md`
- Documented all 4 baseline tests PASSED
- Captured agent rationalizations verbatim
- Key finding: "The problem is NOT lack of anti-pattern detection"

**GREEN phase:** `green-verification.md`
- Documented all 4 tests PASSED with skill present
- Verified skill citation in agent reasoning
- Compliance: 100%
- No new rationalizations captured

## Functional Testing

**Status:** Not yet performed

TDD focused on anti-pattern prevention (when NOT to use council). Full workflow execution (valid council scenario) not yet tested:

- Question classification
- Context validation (passing)
- Agent spawning (5 parallel)
- Output validation (fail-fast)
- Chairman synthesis (rubric application)
- Report generation
- File write

Valid test scenario needed: High-stakes + genuine trade-offs + sufficient context + time available

**Example test case:**
"Should we adopt microservices for our monolithic app?" with full context:
- Team: 12 engineers, 3 senior, mix of expertise
- Current: 50k users, growing 20% MoM
- Timeline: 6-month migration window
- Budget: $200k infrastructure + 2 FTE DevOps
- Pain points: Deploy velocity, team bottlenecks
- Success: Faster deploys, better scaling, team autonomy

## Contributing

To contribute improvements:

1. **Create baseline test scenario** - Pressure test without skill loaded
2. **Document failure** - What agents naturally do wrong (if anything)
3. **Implement fix** - Update SKILL.md
4. **Verify compliance** - Re-run scenario with skill, confirm correct behavior
5. **Submit PR** - Include test scenario + verification results

Follow TDD discipline: RED (baseline test) -> GREEN (implement fix) -> REFACTOR (verify no regression)

## File Structure

```
counsel-this-skill/
├── SKILL.md                    # Main skill implementation
├── README.md                   # User-facing documentation
├── OUTPUT.md                   # Report format specification
├── DESIGN.md                   # Design philosophy and principles
├── DEVELOPMENT.md              # This file (TDD methodology)
├── baseline-scenarios/         # RED phase test scenarios
│   ├── obvious-choice.md
│   ├── low-stakes.md
│   ├── insufficient-context.md
│   └── emergency.md
├── baseline-results.md         # RED phase test results
└── green-verification.md       # GREEN phase verification results
```

## Design Decisions

### Why TDD?

Skills affect agent behavior in subtle ways. Without tests, impossible to know if skill helps or hurts.

TDD provided:
1. **Baseline truth** - What agents naturally do (often already correct)
2. **Regression prevention** - Can verify fixes don't break baseline
3. **Misuse capture** - Pressure tests reveal rationalization patterns
4. **Design clarity** - Focus on preventing documented failures, not hypothetical ones

### Why Anti-Pattern Focus?

Baseline testing revealed agents ALREADY make correct decisions about when to deliberate:
- Assess stakes naturally
- Validate context before proceeding
- Calculate time/cost trade-offs
- Distinguish emergencies from strategic decisions

**Implication:** Skill's primary value is NOT enabling correct use (agents already do that). Skill's value is PREVENTING misuse by providing no-council guardrails when skill is available.

### Why "When NOT to Use" First?

If skill description or instructions make council seem universally valuable, agents will rationalize using it even when baseline judgment says no.

**Solution:** Lead with anti-patterns, make "When NOT to Use" more detailed than "When to Use", add Step 0 execution stop if anti-pattern detected.

**Result:** Skill feels like exceptional tool, not default tool.

## Lessons Learned

1. **Test baseline first** - Don't assume agents need guidance. They often don't.
2. **Prevention > enablement** - Skill's job may be to prevent misuse, not enable use
3. **Reinforce > replace** - Build on baseline judgment, don't override it
4. **No arbitrary thresholds** - Qualitative descriptions age better than hardcoded numbers
5. **Fail-fast > best-effort** - Better to abort with error than proceed with incomplete data
6. **Flexible > rigid** - Different question types need different requirements
7. **Theater detection** - Features that sound good (cross-examination) may produce redundant output
8. **Never assume validated** - Skill written ≠ skill tested. Functional testing still needed.
