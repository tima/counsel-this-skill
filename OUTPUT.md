# Output Structure

Council reports follow a standardized markdown format designed for decision documentation and archival.

## Report Template

```markdown
# Council Decision Report: [QUESTION]

**Question:** [Full question]
**Date:** [Month DD, YYYY]
**Type:** [Technical/Strategic/Process-Product]
**Context Score:** [X/5 required items]

## BLUF

[2-4 sentence bottom line recommendation]

## Chairman's Synthesis

### Classification
[Question type, stakes level, context completeness]

### Key Tensions
[2-3 primary tensions between perspectives]

### Options Analysis
[2-3 options with supporting agents, strengths, risks, confidence]

### Recommended Next Step
**BLUF:** [Bottom line]
**Action:** [Specific next step with timeline]
**Reasoning:** [Why this option]
**Success criteria:** [How to know if correct]
**Reversal conditions:** [When to reconsider]

## Council Member Perspectives

[5 or 3 sections depending on mode: Researcher, Sceptic, Strategist, Operator, Creative]
[Each with: Role, Weight, Verdict, 3-point Analysis]

---

Claude Code | Claude [Model] | [Timestamp UTC]
```

## Section Breakdown

### Header

**Question:** The decision or question being analyzed
**Date:** Report generation date (Month DD, YYYY format)
**Type:** Classification (Technical/Strategic/Process-Product)
**Context Score:** How many required context items were available (X/5)

### BLUF (Bottom Line Up Front)

2-4 sentence executive summary of the recommendation. Written for readers who only have 30 seconds.

### Chairman's Synthesis

The core analysis combining all perspectives:

**Classification:** Question type, stakes assessment, context completeness evaluation

**Key Tensions:** 2-3 primary points where council members disagreed or identified competing priorities

**Options Analysis:** 2-3 viable paths forward, each with:
- Which agents support it
- Primary strength
- Primary risk
- Confidence level (High/Medium/Low)

**Recommended Next Step:** Specific actionable recommendation with:
- BLUF (one sentence)
- Concrete action with timeline
- Reasoning referencing weighted perspectives
- Success criteria (how to measure if decision was correct)
- Reversal conditions (when to reconsider)

### Council Member Perspectives

Full perspective from each council member (5 in full mode, 3 in quick mode):

**Role:** What perspective this member brings
**Weight:** Percentage weighting for this question type
**Verdict:** 1-2 sentence position
**Analysis:** 3 specific points supporting the verdict

Members:
- **The Researcher** - Evidence, data, benchmarks, prior art
- **The Sceptic** - Risks, downsides, failure modes, case against
- **The Strategist** - Long-term consequences, compounding effects
- **The Operator** - Execution feasibility, blockers, practical constraints
- **The Creative** - Unconventional approaches, reframes, third paths

### Metadata Footer

Provider (Claude Code), model used, UTC timestamp

## Filename Convention

Pattern: `council-<descriptive-slug>-<YYYY-MM-DD>.md`

**Examples:**
- `council-postgresql-mongodb-choice-2026-06-15.md`
- `council-scale-engineering-team-2026-06-15.md`
- `council-microservices-adoption-2026-06-15.md`

Slug generated from question core (remove "should I", "what's best", etc.), lowercase, hyphens, max 40 chars. Collision detection appends -2, -3, etc.

## Quick Mode Differences

When using `--quick` flag:

- **3 agents instead of 5** (selected by question type)
- **Simpler synthesis** (equal weighting)
- **Faster analysis** (medium-stakes decisions)

Agent selection:
- Technical: Operator + Researcher + Sceptic
- Strategic: Strategist + Researcher + Operator
- Process/Product: Operator + Strategist + Creative

Report structure identical, just fewer perspectives documented.
