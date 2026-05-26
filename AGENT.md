# Design Agent

## What this does
Takes business context as input and produces UI/UX design decisions for any app. Reusable across projects. Business context is always passed in — nothing is hardcoded.

## Inputs
- Business context (product, user, core action, desired feel)
- Screen description (for design-screen skill)
- Existing screen code or description (for design-review skill)

## Outputs
- Design spec: 7-token colour system, mode, typography, spacing, radius convention, shadow system, icon library, component library, layout principles, do-not-do list
- Screen spec: component breakdown, mobile-first layout, Tailwind classes, accessibility requirements, React scaffold
- Design audit: generic AI patterns, spec violations, UX concerns, prioritised fix list

## Dependencies
None. Standalone agent.

## Combines well with
- build-agent (consumes the design spec to scaffold UI code)

## Skills
| Skill | When to use |
|---|---|
| design-brief | Once per project, at the start |
| design-screen | For each new screen to build |
| design-review | To audit an existing screen |

## Roadmap
- **V2** — Memory layer: `memory/patterns.md` + `memory/industries/` + `design-retro` skill. Agent learns from real projects over time.
- **V3** — Multi-model design review swarm: multiple Claude personas + one external model (GPT-4o) as challenger. Model diversity prevents correlated blind spots.
- **V3.5** — "Steal like an artist" pipeline: computer use visits Q4 reference products, extracts real design tokens, grounds the spec in actual data.

## Iteration log
- 2026-05-26 (V1.1) — 12 fixes: colour system expanded to 7 tokens, radius/shadow conventions added (adjective-derived), mode decision, spacing system, icon library, 4th component library option, mobile-first, accessibility, screenshot input, UX concerns section, minimal .gitignore.
- 2026-05-25 (V1) — Frame created. Three core skills: design-brief, design-screen, design-review.
