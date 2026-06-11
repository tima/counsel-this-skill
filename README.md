# council-this

A Claude Code skill for high-stakes decision analysis using multi-persona deliberation. Spawns 5 expert agents (Researcher, Sceptic, Strategist, Operator, Creative) to stress-test decisions from multiple perspectives, then synthesizes their insights into actionable recommendations.

Built with TDD methodology following the writing-skills discipline.

---

## When to Use

Council-this is a HIGH-STAKES POWER TOOL for decisions with genuine trade-offs. Use it when:

- [ ] High stakes (>1 week reversal time, >$10k cost, or critical path)
- [ ] Genuine trade-offs (multiple valid approaches, no obvious winner)
- [ ] Sufficient context (can answer who/what/when/why/how)
- [ ] 30-45 minutes available for analysis
- [ ] Open decision (not validation-seeking)

**Examples:**
- Architecture decisions (microservices adoption, database migration)
- Strategic choices (market positioning, product direction)
- High-stakes hiring (executive roles, critical positions)
- Major process changes (development workflow, team structure)

---

## When NOT to Use

**SKIP council-this for:**

1. **Obvious choices** - Expertise/experience heavily asymmetric (5yr in A, 0yr in B)
2. **Low stakes** - Easy to reverse (<1 day, <$1000 cost)
3. **Insufficient context** - Missing critical background (team size, scale, timeline, budget)
4. **Emergencies** - Decision needed <30min (production outage, security incident)
5. **Validation-seeking** - Decision already made, looking for confirmation

**Why:** Your baseline judgment is already correct for these. Council adds delay without insight.

---

## Usage

```
/council-this <question or decision>
```

### Examples

```
/council-this Should we adopt microservices for our monolithic app with 12 engineers, 50k users growing 20% MoM, 6-month migration window, and $200k budget?

/council-this What's our hiring strategy for scaling engineering from 8 to 20 people over the next year?

/council-this How should we prioritize technical debt vs new features given our upcoming Series A and product-market fit validation phase?
```

---

## How It Works

### 1. Anti-Pattern Detection (Step 0)

Before spawning agents, validates:
- Not an obvious choice (expertise asymmetry check)
- Not low-stakes (reversal cost check)
- Not an emergency (time pressure check)
- Sufficient context available
- Not validation-seeking

**User can override with `--force` flag**

### 2. Question Classification (Step 1)

Classifies question type to weight perspectives appropriately:

**Technical** (architecture, tools, infrastructure)
- Emphasizes: Operator (40%) + Researcher (30%)

**Strategic** (business, market, long-term)
- Emphasizes: Strategist (40%) + Researcher (30%)

**Process/Product** (workflow, features, coordination)
- Emphasizes: Operator (35%) + Strategist (25%)

### 3. Context Validation (Step 2)

Checks for required context by question type (who/what/when/why/how):
- 0-2 items: REFUSE, require clarification
- 3-4 items: WARN, proceed with caveats
- 5 items: FULL CONFIDENCE

### 4. Spawn 5 Council Members (Step 3)

Launches 5 agents in parallel with question-type-specific prompts:

1. **The Researcher** - Evidence-first analysis, data, benchmarks
2. **The Sceptic** - Stress-testing, risks, case against leading option
3. **The Strategist** - Long-term view, compounding effects, leverage
4. **The Operator** - Execution feasibility, blockers, cost/timeline
5. **The Creative** - Unconventional approaches, reframes, third paths

Each provides:
- VERDICT (1-2 sentences)
- ANALYSIS (3 specific points)

### 5. Validate Output (Step 4)

**Fail-fast validation:**
- All 5 agents must provide valid VERDICT + 3-point ANALYSIS
- If ANY agent fails format check → council ABORTS with specific error
- No partial output, no best-effort

### 6. Cross-Examination (Step 5)

Replaces peer review theater with adversarial challenges:
- Sceptic challenges Researcher
- Operator challenges Strategist
- Researcher challenges Creative
- Strategist challenges Operator
- Creative challenges Sceptic

Each responds to direct challenge (2-3 sentences).

### 7. Chairman Synthesis (Step 6)

Applies decision rubric:
- Weights perspectives by question type
- Conflict resolution hierarchy (evidence > opinion, execution > theory)
- Deal-breaker criteria (Sceptic + Operator veto)
- Decision tree: alignment → recommendation, deal-breaker → reject, options → ranked

**Output includes:**
- BLUF (2-4 sentence bottom line)
- Key tensions between perspectives
- Options analysis with confidence levels
- Recommended next step
- Success criteria
- Reversal conditions

### 8. Generate Report (Steps 7-10)

Produces markdown report with:
- Question, date, type, context score
- BLUF
- Chairman's synthesis
- 5 council member perspectives (with cross-examination)
- Cross-examination summary
- Metadata footer (provider, model, timestamp)

**Filename:** `council-<descriptive-slug>-<YYYY-MM-DD>.md`

---

## Output Structure

```markdown
# Council Decision Report: [QUESTION]

**Question:** [Full question]
**Date:** [Month DD, YYYY]
**Type:** [Technical/Strategic/Process-Product]
**Context Score:** [X/5 required items]

------------------------------

## BLUF

[2-4 sentence bottom line recommendation]

## Chairman's Synthesis

### Classification
[Question type, stakes level, context completeness]

### Key Tensions
[2-3 primary tensions from cross-examination]

### Options Analysis
[2-3 options with supporting agents, strengths, risks, confidence]

### Recommended Next Step
**BLUF:** [Bottom line]
**Action:** [Specific next step with timeline]
**Reasoning:** [Why this option]
**Success criteria:** [How to know if correct]
**Reversal conditions:** [When to reconsider]

## Council Member Perspectives

[5 sections: Researcher, Sceptic, Strategist, Operator, Creative]
[Each with: Role, Weight, Verdict, 3-point Analysis, Challenge response]

## Cross-Examination Summary

[Key tensions, agreements, unresolved contradictions]

---

Claude Code | Claude [Model] | [Timestamp UTC]
```

---

## Installation

**From this repository:**

```bash
# Clone the repo
git clone https://github.com/tima/counsel-this-skill.git ~/projects/counsel-this-skill

# Create symlink
ln -sf ~/projects/counsel-this-skill ~/.claude/skills/council-this

# Reload Claude Code or restart your session
```

**Verify installation:**

```
/council-this --help
```

---

## Development

This skill was built following TDD methodology from the writing-skills discipline.

### TDD Cycle

**RED Phase:** Baseline testing without skill
- Created 4 pressure scenarios (obvious choice, low stakes, insufficient context, emergency)
- Ran scenarios with test agents (no skill present)
- Result: 100% correct baseline behavior (agents naturally avoid misuse)
- **Key insight:** Skill's job is to PREVENT misuse, not enable correct use

**GREEN Phase:** Skill implementation
- Wrote skill focusing on anti-pattern detection
- "When NOT to Use" as primary focus (5 anti-patterns)
- Step 0 stops execution if anti-pattern detected
- Context validation with fail-fast
- Cross-examination replaces peer review
- Chairman synthesis with decision rubric
- Result: 100% compliance (4/4 scenarios)

**REFACTOR Phase:** Verification
- Re-ran 4 scenarios WITH skill present
- All agents made correct choices + cited skill sections
- No new rationalizations emerged
- No REFACTOR iterations needed

### Testing Artifacts

- `baseline-scenarios/` - 4 pressure test scenarios
- `baseline-results.md` - RED phase documentation (100% correct baseline)
- `green-verification.md` - GREEN phase verification (100% compliance)

---

## Design Philosophy

### Power Tool, Not Default Tool

Council-this is designed to feel like a power tool for exceptional cases, not a default option for all decisions. The skill actively discourages misuse through:

1. **Description emphasis:** "HIGH-STAKES... SKIP for obvious choices, low-stakes, emergencies"
2. **Step 0 anti-pattern detection:** Stops execution before spawning agents
3. **Explicit time costs:** 30-45 minute analysis clearly stated
4. **When NOT to Use prominence:** Anti-patterns listed first
5. **Baseline judgment reinforcement:** "Your baseline judgment is already correct"

### Preventing Theater

**Peer review replaced with cross-examination:**
- Original design: Present anonymized summaries to same 5 agents (theater)
- New design: Adversarial challenge pairs with specific questions
- Result: Real tensions surface, not artificial consensus

**Fail-fast validation:**
- Original design: Best-effort synthesis with partial agent output
- New design: All 5 agents must provide valid output or council aborts
- Result: No garbage-in-garbage-out, clear error messages

### Question-Type Adaptation

Not all decisions benefit from same perspective weighting:
- **Technical:** Operator + Researcher matter most (execution feasibility + evidence)
- **Strategic:** Strategist + Researcher matter most (long-term + market data)
- **Process/Product:** Operator + Strategist matter most (execution + scalability)

Prompts adapt by question type to emphasize relevant expertise per persona.

---

## Common Mistakes

| Excuse | Reality |
|--------|---------|
| "Council adds thoroughness" | On obvious choices, adds delay not insight |
| "Naming/low-stakes need rigor" | Council overhead unjustified for easily reversible |
| "Work with available context" | Garbage in, garbage out - get context first |
| "Important decisions need time" | Emergencies need frameworks, not 45min analysis |
| "Due diligence requires multi-perspective" | Due diligence on obvious choice is theater |

---

## Contributing

Built following TDD methodology. To contribute:

1. Create baseline test scenario (pressure test without skill)
2. Document failure (what agents naturally do wrong)
3. Implement fix in SKILL.md
4. Verify compliance (re-run scenario with skill)
5. Submit PR with test scenario + verification results

---

## License

MIT

---

## Credits

Original concept by [Eliot Prince](https://github.com/eliotcowley).

TDD implementation by Claude Sonnet 4.5 with [@tima](https://github.com/tima).
