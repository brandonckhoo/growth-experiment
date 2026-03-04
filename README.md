# growth-experiment

A Claude Code skill for Growth PMs. Give it a problem — a metric drop, a funnel leak, a feature that's not converting — and it generates ranked experiment ideas using Akash Gupta's 10×10 vibe experimentation framework, then produces a complete, statistically-sound A/B experiment brief ready to launch in Amplitude Experiment.

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
- Outputs a copy-paste ready Amplitude Experiment setup block: Name, Target URL, Key, Hypothesis & Background, Pages, Metrics, Targeting/Rollout, Advanced Stats Preferences (CUPED, Bonferroni, Statistical Method, Confidence Level)
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
"Free trial activation rate is 28%, below our 40% target.
We redesigned onboarding 6 weeks ago and it's been flat since.
~3,500 new signups/week on web."
```

Or invoke directly:
```
/growth-experiment
```

The skill asks 3 upfront questions (platform, weekly traffic, any context), then runs the full pipeline without stopping.

## Example output

**Input:** *"Free trial activation rate — users who complete their first key action within 24 hours of signup — is 28%, below our 40% target. We redesigned onboarding 6 weeks ago and it's been flat since. ~3,500 new signups/week. Web."*

**Output:**
- Journey stage: Onboarding / First Use
- Top 3 ideas from the matrix row: Interactive Product Tour, Progress Milestone Celebrations, Quick Win Activation Prompt
- 4-factor ranking with rationale — #1 is Interactive Product Tour
- Full experiment brief with hypothesis and guardrail metrics
- Sample size: ~2,800 per variant → 1.6 weeks at 3,500 signups/week
- Amplitude setup block: copy-paste ready for Name, Hypothesis, Pages, Metrics, Targeting, Stats Preferences
- Statistical method: Sequential — because results will be monitored weekly and early stopping is valuable

## Who this is for

Growth PMs who:
- Use Amplitude Experiment for A/B testing
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
- Amplitude Experiment account with experiments enabled
- A problem to solve

## What it doesn't do

- Won't query your analytics data directly (no MCP integration)
- Won't calculate exact sample sizes without a baseline conversion rate — pull that from your tool first
- Doesn't replace a statistician for multi-variant or interaction-effect experiments at scale

## Attribution

Experiment idea generation is powered by **Akash Gupta's Vibe Experimentation framework** — a 10×10 behavioral trigger matrix mapping 10 user journey stages to 10 psychological triggers, producing 100 named experiment ideas. The framework is used here as a structured brainstorm tool; credit belongs to Akash Gupta.

## License

MIT
