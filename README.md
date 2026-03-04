# growth-experiment

A Claude Code skill for Growth PMs. Give it a problem — a metric drop, a funnel leak, a feature that's not converting — and it generates ranked experiment ideas using [Akash Gupta's 10×10 vibe experimentation framework](https://www.news.aakashg.com/p/vibe-experimentation?utm_source=publication-search), then produces a complete, statistically sound A/B experiment brief ready to launch in Amplitude Experiment.

## What it does

**Phase 1: Idea Generation**

Most experiment backlogs start from gut feel or Slack threads. This skill starts from a structured behavioral framework instead. It reads your problem description, identifies which stage of the user journey is affected, and looks up that row in [Akash Gupta's 10×10 matrix](https://www.news.aakashg.com/p/vibe-experimentation?utm_source=publication-search) — 100 named experiment ideas mapped across 10 journey stages and 10 psychological triggers (Social Proof, Scarcity, Anchoring, Loss Aversion, Progress, Authority, Personalization, Simplification, Reciprocity, Identity). From that row, it surfaces the top 3 ideas that are most relevant to your context and feasible to ship as frontend changes.

**Phase 2: Prioritization**

Each of the top 3 ideas is scored across 4 factors: Expected Impact, Statistical Power Required, Brand/UX Risk, and Learning Value. Every factor gets a High/Med/Low rating and a one-sentence rationale explaining the reasoning. The result is a ranked shortlist that's easy to walk through in a team review, with the tradeoffs already spelled out rather than left as assumptions.

**Phase 3: Experiment Plan**

Once you've picked your experiment, the skill writes the full plan. It follows the [Atlassian experiment brief](https://www.atlassian.com/team-playbook/plays/experiment) structure covering hypothesis, success metric, guardrail metrics, and variant descriptions. It calculates sample size using the [Optimizely sample size calculator](https://www.optimizely.com/sample-size-calculator/) method (n ≈ 16 × p(1−p) / δ²) and translates that into weeks of runtime based on your weekly traffic. It recommends a statistical method — Sequential, T-test, or Bayesian — with a specific justification for your situation, not just a generic definition. And it outputs a copy-paste ready Amplitude Experiment setup block covering Name, Target URL, Key, Hypothesis and Background, Pages, Metrics, Targeting and Rollout, and Advanced Stats Preferences including CUPED, Bonferroni correction, Statistical Method, and Confidence Level.

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
- 4-factor ranking with rationale per factor, #1 is Interactive Product Tour
- Full experiment brief with hypothesis and guardrail metrics
- Sample size: ~2,800 per variant, 1.6 weeks at 3,500 signups/week
- Amplitude setup block: copy-paste ready for Name, Hypothesis, Pages, Metrics, Targeting, Stats Preferences
- Statistical method: Sequential, because results will be monitored weekly and early stopping is valuable

## Who this is for

Growth PMs who use Amplitude Experiment for A/B testing and want to run rigorous, well-reasoned experiments without starting from a blank page. Works for web, iOS, and Android. Useful when you're preparing an experiment proposal for team review, trying to move fast without skipping the thinking, or need to articulate the tradeoffs between competing ideas clearly.

## Frameworks inside

- **[Akash Gupta's 10×10 Vibe Experimentation Matrix](https://www.news.aakashg.com/p/vibe-experimentation?utm_source=publication-search)**: 100 named experiment ideas across 10 journey stages and 10 behavioral triggers, used here as a structured idea generation tool
- **4-factor prioritization**: Expected Impact, Statistical Power Required, Brand/UX Risk, Learning Value — each scored and explained, not just listed
- **[Atlassian Experiment Plan template](https://www.atlassian.com/team-playbook/plays/experiment)**: hypothesis, primary metric, guardrail metrics, variant descriptions, all in one structured brief
- **[Optimizely sample size calculator](https://www.optimizely.com/sample-size-calculator/) method**: n ≈ 16 × p(1−p) / δ², with runtime calculated in weeks based on your actual traffic

## Requirements

- Claude Code (any version)
- Amplitude Experiment account with experiments enabled
- A problem to solve

## What it doesn't do

- Won't query your analytics data directly (no MCP integration). You'll need to pull your baseline conversion rate from Amplitude before running the skill.
- Won't calculate exact sample sizes without a baseline rate. If you don't have one, it will prompt you to estimate.
- Doesn't replace a statistician for multi-variant or interaction-effect experiments at scale. For anything beyond a standard 2-variant A/B test, treat the output as a starting point.

## Attribution

Experiment idea generation is powered by **[Akash Gupta's Vibe Experimentation framework](https://www.news.aakashg.com/p/vibe-experimentation?utm_source=publication-search)** — a 10×10 behavioral trigger matrix mapping 10 user journey stages to 10 psychological triggers, producing 100 named experiment ideas. The framework is used here as a structured brainstorm tool. Credit belongs to Akash Gupta.

## License

MIT
