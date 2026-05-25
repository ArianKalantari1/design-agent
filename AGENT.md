# Design Agent

## What this does
Takes business context as input and produces UI/UX design decisions for any app. Reusable across projects. Business context is always passed in — nothing is hardcoded.

## Inputs
- Business context (product, user, core action, desired feel)
- Screen description (for design-screen skill)
- Existing screen code or description (for design-review skill)

## Outputs
- Design spec: colour system, typography, component library, layout principles, do-not-do list
- Screen spec: component breakdown, layout structure, Tailwind classes, React scaffold
- Design audit: what looks generic, what breaks consistency, prioritised fix list

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

## Iteration log
- 2026-05-25 (V1) — Frame created. Three core skills: design-brief, design-screen, design-review.
