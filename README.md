# council-this

Multi-persona decision analysis for high-stakes choices with genuine trade-offs. Spawns expert agents to stress-test decisions from multiple perspectives, then synthesizes insights into actionable recommendations with markdown reports.

## When to Use

HIGH-STAKES POWER TOOL for decisions with genuine trade-offs. Use when:

- High stakes (difficult or costly to reverse, critical path)
- Genuine trade-offs (multiple valid approaches, no obvious winner)
- Sufficient context (can answer who/what/when/why/how)
- Time available for thorough deliberation
- Open decision (not validation-seeking)

**Examples:** Architecture decisions (microservices adoption, database migration), strategic choices (market positioning, product direction), high-stakes hiring (executive roles), major process changes (development workflow, team structure)

## When NOT to Use

SKIP for:

1. **Obvious choices** - Expertise/experience heavily asymmetric, one option clearly superior
2. **Low stakes** - Easy to reverse quickly, minimal cost
3. **Insufficient context** - Missing critical background (team size, scale, constraints)
4. **Emergencies** - Decision needed immediately (production outage, security incident)
5. **Validation-seeking** - Decision already made, looking for confirmation

Your baseline judgment is already correct for these. Council adds delay without insight.

## Common Mistakes

| Excuse | Reality |
|--------|---------|
| "Council adds thoroughness" | On obvious choices, adds delay not insight |
| "Naming/low-stakes need rigor" | Council overhead unjustified for easily reversible |
| "Work with available context" | Garbage in, garbage out - get context first |
| "Important decisions need time" | Emergencies need frameworks, not deliberation |
| "Due diligence requires multi-perspective" | Due diligence on obvious choice is theater |

## Usage

```bash
/council-this <question or decision>
/council-this --quick <question>        # 3 agents for medium-stakes
/council-this --type=technical <question>  # Manual classification override
```

**Examples:**
```bash
/council-this Should we adopt microservices for our monolithic app with 12 engineers, 50k users growing 20% MoM?

/council-this --quick Should we refactor the auth module now or defer to next quarter?

/council-this --type=strategic How should we prioritize technical debt vs new features for Series A?
```

## How It Works

Council-this prevents expensive mistakes by surfacing blind spots before you commit.

**The value:** High-stakes decisions fail when we only see from one perspective. Your technical lead optimizes for clean architecture while your operator worries about the 6-month migration timeline. Your strategist sees compounding long-term value while your sceptic identifies the team expertise gap that makes execution risky. Council forces these perspectives to surface and challenge each other before you choose.

**The process:**

First, council validates this isn't theater. Obvious choices don't need deliberation. Low-stakes decisions cost more to analyze than to reverse. Emergencies need speed, not synthesis. Context-free questions produce generic advice. If any anti-pattern detected, execution stops - your baseline judgment is already correct.

Second, council classifies the question type (Technical/Strategic/Process) to weight perspectives appropriately. Execution feasibility matters more for technical decisions. Long-term consequences matter more for strategic choices. Different decisions need different lenses.

Third, council spawns expert agents in parallel - five for high-stakes (Researcher, Sceptic, Strategist, Operator, Creative) or three for medium-stakes with `--quick` flag. Each agent stays strictly in their lane: Researcher brings only evidence, Sceptic only risks, Strategist only long-term view. No redundant output, no generic advice.

Fourth, Chairman synthesis weighs the perspectives, resolves conflicts through evidence hierarchy (data beats opinion, execution beats theory, deal-breakers veto), and produces a recommendation with success criteria and reversal conditions. Not "here are pros and cons" - actual guidance on what to do next.

Finally, council generates a markdown report with BLUF (bottom line up front), full synthesis, and all perspectives documented. Saved as `council-<slug>-<date>.md` for decision archival and future reference.

**What you get:** A decision recommendation backed by multi-perspective analysis, with clear success criteria and reversal conditions. Documentation you can point to later when someone asks "why did we choose this?"

See [OUTPUT.md](OUTPUT.md) for report structure details.


## Installation

```bash
# User scope — available in all sessions (recommended)
npx skills add tima/counsel-this -g

# Project scope — available in this project only
npx skills add tima/counsel-this
```

Target a specific agent:
```bash
npx skills add tima/counsel-this -g -a claude-code
```

Local development install:
```bash
git clone https://github.com/tima/counsel-this.git ~/projects/counsel-this
ln -sf ~/projects/counsel-this ~/.claude/skills/council-this
```

### Uninstall

```bash
npx skills remove council-this           # project scope
npx skills remove council-this --global  # user scope
```

## Documentation

- [OUTPUT.md](OUTPUT.md) - Report format specification
- [DESIGN.md](DESIGN.md) - Design philosophy and principles
- [DEVELOPMENT.md](DEVELOPMENT.md) - TDD methodology and testing

## Credits

Original concept by [Eliot Prince](https://github.com/eliotcowley). TDD implementation by Claude Sonnet 4.5 with [@tima](https://github.com/tima).

## License

MIT — see [LICENSE](LICENSE).
