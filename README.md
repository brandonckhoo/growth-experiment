# growth-experiment

A Claude Code skill for Growth PMs. Give it a problem — a metric drop, a funnel leak, a feature that's not converting — and it generates ranked experiment ideas using [Akash Gupta's 10×10 vibe experimentation framework](https://www.news.aakashg.com/p/vibe-experimentation?utm_source=publication-search), then produces a complete, statistically sound A/B experiment brief ready to launch in Amplitude Experiment.

## What it does

**Phase 1: Idea Generation**
- Reads your problem and identifies which of the 10 user journey stages is affected — Awareness/Discovery, Landing/First Impression, Onboarding/Setup, Feature Discovery, Activation/Engagement, Habit Formation, Expansion/Upsell, Retention/Loyalty, Referral/Advocacy, or Reactivation
- Looks up that row in [Akash Gupta's 100-idea experiment bank](https://www.news.aakashg.com/p/vibe-experimentation?utm_source=publication-search) — 10 named experiment ideas per stage, each mapped to a behavioral trigger: Social Proof, Scarcity/Urgency, Anchoring, Loss Aversion, Progress/Achievement, Authority/Trust, Personalization, Simplification, Reciprocity, or Identity
- Example: Awareness/Discovery + Social Proof = Customer Logo Testimonial Wall
- Surfaces the top 3 ideas most relevant to your context, frontend-ready and feasible to ship fast

**Phase 2: Prioritization**
- Scores each of the 3 ideas across Expected Impact, Statistical Power Required, Brand/UX Risk, and Learning Value
- Every factor gets a High/Med/Low rating with a one-sentence rationale
- Outputs a ranked shortlist with tradeoffs already spelled out, ready to walk through in a team review

**Phase 3: Behavioral Hypothesis**
- Writes a single, precise behavioral hypothesis for the #1 idea before any code is generated
- Uses the formula: *who* will do *what differently* if we *change this specific thing*, because *of this psychological reason* — for example, "New B2B users will complete trial setup 40% faster if they see ROI calculations before the payment step, because they need to justify the purchase to their manager"
- Turns a vague intuition into a testable claim that anchors the variant, the proposal, and the success metrics

**Phase 4: Vibe Code the #1 Idea — 3 Design Concepts**
- Reads your codebase and design system (Figma tokens, component library, existing patterns) to understand how your product is built
- Proposes 3 distinct design directions for the #1 idea — each tests the same behavioral hypothesis through a different implementation approach
- You pick one, then the skill generates the full variant code using your actual components, colors, and typography — not a generic prototype
- Outputs the implementation behind a feature flag, ready to drop into the existing page

**Phase 5: Experiment Proposal**
- Writes a full [Atlassian-structured experiment brief](https://www.atlassian.com/team-playbook/plays/experiment): hypothesis, success metric, guardrail metrics, and variant descriptions that reference the code you just built
- Calculates sample size using the [Optimizely calculator method](https://www.optimizely.com/sample-size-calculator/) and converts it into weeks of runtime based on your traffic
- Recommends a statistical method (Sequential, T-test, or Bayesian) with a specific justification for your experiment, not a generic definition
- Outputs a complete Amplitude Experiment setup block: Name, Target URL, Key, Hypothesis, Pages, Metrics, Targeting, CUPED, Bonferroni, Statistical Method, Confidence Level

**Phase 6: Approval and Launch**
- Pauses and asks you to review the proposal and variant code before touching Amplitude
- Prompts you to run the 5-party launch-readiness review: Designers (design system compliance), Engineers (performance/scalability), Compliance (legal/privacy), Data Scientists (stats validity), Leadership (business risk sign-off)
- On your approval: calls `create_experiment` via the Amplitude MCP, no manual UI work needed
- Surfaces the direct experiment URL so you can open it and click Launch in one step
- If MCP is not connected: outputs the setup block for you to copy in manually instead

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

## Amplitude MCP integration

If you have the [Amplitude MCP server](https://amplitude.com/mcp-server) connected to Claude Code, the skill upgrades automatically:

**Without MCP (default):** Outputs a copy-paste setup block for the Amplitude Experiment UI. You manually fill in the fields and click Create.

**With MCP connected:** The skill uses `query_dataset` to pull your baseline conversion rate from Amplitude directly, then calls `create_experiment` to create the experiment in Amplitude without you touching the UI. You still need to click Launch in Amplitude — the MCP creates but does not launch.

To connect the Amplitude MCP, add it to your Claude Code settings:

```json
{
  "mcpServers": {
    "amplitude": {
      "url": "https://mcp-server.prod.us-west-2.amplitude.com/v1/mcp"
    }
  }
}
```

Or install via the Amplitude MCP Marketplace:
```
/plugin install amplitude@amplitude
```

## What it doesn't do

- Won't calculate exact sample sizes without a baseline rate. If MCP is not connected and you don't have one, it will prompt you to pull it from Amplitude first.
- Doesn't replace a statistician for multi-variant or interaction-effect experiments at scale. For anything beyond a standard 2-variant A/B test, treat the output as a starting point.
- MCP creates the experiment in draft state — launching it still requires a human click in the Amplitude UI.

## Attribution

Experiment idea generation is powered by **[Akash Gupta's Vibe Experimentation framework](https://www.news.aakashg.com/p/vibe-experimentation?utm_source=publication-search)** — a 10×10 behavioral trigger matrix mapping 10 user journey stages to 10 psychological triggers, producing 100 named experiment ideas. The framework is used here as a structured brainstorm tool. Credit belongs to Akash Gupta.

## License

MIT
