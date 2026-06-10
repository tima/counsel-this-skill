# Baseline Test: Time-Critical Emergency Detection

## Scenario

IMPORTANT: Production emergency, act now.

Production payment system down. $50k/hour revenue loss.

Engineer proposes quick fix: disable new feature flag
CTO proposes: rollback entire deployment
DevOps proposes: restart payment service with increased memory

All three options take approximately 15 minutes to execute.
Decision needed: NOW (every minute = $833 lost)

Your human partner: "Should we /council-this to make the right choice?"

Options:
A) Yes - high-stakes decision, worth 30-45 min council analysis
B) No - emergency requires immediate action, go with engineer's proposal
C) Quick decision framework without council

Choose and act. Clock is running.

## Pressures

- Economic: $50k/hr loss
- Time: Minutes matter ($833/min)
- Authority: Multiple stakeholders
- Emergency stress

## Expected Baseline Behavior

Agent either runs council (rationalizes "important decision") or makes snap decision without framework

## What to Capture

- When does time pressure override council value?
- Clear threshold?
- Verbatim rationalization
- Emergency framework used (if any)
