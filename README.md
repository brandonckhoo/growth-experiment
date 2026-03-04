# growth-experiment

A Claude Code skill for Growth PMs. Give it a problem — a metric drop, a pipeline gap, a funnel leak — and it generates ranked experiment ideas using Akash Gupta's 10×10 vibe experimentation framework, then produces a complete, statistically-sound A/B experiment brief ready to launch in Amplitude Experiment or PostHog.

## What it does

**Phase 1 — Idea Generation**
- Identifies the relevant user journey stage from your problem description
- Pulls experiment ideas from Akash Gupta's 10×10 behavioral trigger matrix (10 journey stages × 10 triggers: Social Proof, Scarcity, Anchoring, Loss Aversion, Progress, Authority, Personalization, Simplification, Reciprocity, Identity)
- Surfaces the top 3 most relevant, frontend-ready ideas for your context

**Phase 2 — Prioritization**
- Scores each idea on 4 factors: Expected Impact, Statistical Power Required, Brand/UX Risk, Learning Value
- Ranks top 3 with a one-sentence rationale per factor — easy to present to a team or defend in an interview

**Phase 3 — Experiment Plan**
- Writes a full Atlassian-structured experiment brief (hypothesis, primary metric, guardrail metrics, variants)
- Calculates sample size using the Optimizely calculator method + runtime in weeks
- Outputs a copy-paste ready UI setup block matched to your tool:
  - **Amplitude Experiment**: Name, Target URL, Key, Hypothesis & Background, Pages, Metrics, Targeting/Rollout, Advanced Stats Preferences (CUPED, Bonferroni, Statistical Method, Confidence Level)
  - **PostHog**: Description (name + hypothesis + flag key), Variant Rollout, Analytics (inclusion criteria + primary/secondary metrics)
- Recommends statistical method (Sequential / T-test / Bayesian) with justification
- Generates a shareable experiment idea backlog for the team

## Install

**Via `/install-claude-skill` (recommended):**
```
/install-claude-skill github.com/brandonckhoo/growth-experiment
```

**Manual:**
```bash
mkdir -p ~/.claude/skills/growth-experiment
curl -o ~/.claude/skills/growth-experiment/SKILL.md \
  https://raw.githubusercontent.com/brandonckhoo/growth-experiment/main/SKILL.md
```

## Usage

Describe your problem in plain language:

```
"Our pricing page Contact Sales conversion is below average.
Sales needs more pipeline. We use Amplitude Experiment."
```

Or invoke directly:
```
/growth-experiment
```

The skill asks 4 upfront questions (tool, platform, weekly traffic, any context), then runs the full pipeline without stopping.

## Example output

**Input:** *"Pricing page Contact Sales CTA is below average. Sales flagged it. Web, Amplitude, ~2,000 visitors/week."*

**Output:**
- Journey stage: Landing / First Impression
- Top 3 ideas from the matrix row: One-Click Demo Access, Industry Leader Testimonials, Churn Cost Visualization
- 4-factor ranking with rationale — #1 is One-Click Demo Access
- Full experiment brief with hypothesis and guardrail metrics
- Sample size: ~3,600 per variant → 3.6 weeks at 2,000 visitors/week
- Amplitude setup block: copy-paste ready for Name, Hypothesis, Pages, Metrics, Targeting, Stats Preferences
- Statistical method: Sequential — because results will be monitored weekly

## Who this is for

Growth PMs who:
- Use Amplitude Experiment or PostHog for A/B testing
- Want rigorous experiment ideas without starting from a blank page
- Ship on web, iOS, or Android
- Need to explain experiment design clearly in team reviews or PM interviews

## Frameworks inside

- **Akash Gupta's 10×10 Vibe Experimentation Matrix** — 100 named experiment ideas across 10 journey stages and 10 behavioral triggers
- **4-factor prioritization** — Expected Impact, Statistical Power, Brand Risk, Learning Value
- **Atlassian Experiment Plan template** — structured proposal format
- **Optimizely sample size calculator method** — n ≈ 16 × p(1−p) / δ², runtime in weeks

## Requirements

- Claude Code (any version)
- Amplitude Experiment or PostHog account with experiments enabled
- A problem to solve

## What it doesn't do

- Won't query your analytics data directly (no MCP integration)
- Won't calculate exact sample sizes without a baseline conversion rate — pull that from your tool first
- Doesn't replace a statistician for multi-variant or interaction-effect experiments at scale

## License

MIT
