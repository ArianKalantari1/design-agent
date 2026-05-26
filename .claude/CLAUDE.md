# Design Agent — Project Rules

## What this is
A reusable design agent. Drop it into any project, run design-brief first, then use design-screen and design-review from there.

## Rules
- Business context is always passed in. Never invent it.
- Every design decision must be justified against the business context or the design spec.
- design-spec.md is the source of truth for any project. If it does not exist, run design-brief first.
- Never default to: purple or indigo as primary colour, glassmorphism, uniform rounded-2xl, dark mode as default, Geist font, pure black backgrounds, pure white backgrounds.
- Border-radius and shadow values must be derived from the user's adjectives — not picked from a default. Justify every value against the brief.
- One question at a time during the brief interview. Never ask multiple questions in one turn.
- All scaffolds are mobile-first. Base styles target mobile. md: and lg: enhance for larger screens.
- Accessibility is not optional: semantic HTML, ARIA labels, focus rings, and contrast checks are required in every scaffold.

## Active skills
| Skill | Status |
|---|---|
| design-brief | Built (V1) |
| design-screen | Built (V1) |
| design-review | Built (V1) |

## Skills to add later

### ui-ux-suite (Aboudjem/ui-ux-suite)
Scores 12 design dimensions quantitatively (colour systems, typography, layout, accessibility, hierarchy, interactions, responsiveness) using established UX research — WCAG 2.2, APCA contrast, Hick's Law, Fitts's Law. Runs locally with zero dependencies. Worth integrating as a post-build audit step.
Key commands: `/ui-ux-suite:audit`, `/ui-ux-suite:colors`, `/ui-ux-suite:a11y [--deep]`, `/ui-ux-suite:typography`, `/ui-ux-suite:components`.
V2 use: run after design-review to get evidence-based scores rather than qualitative findings.

### styleseed (bitjaru/styleseed)
69 brand-agnostic visual rules across six categories (colour discipline, spatial rhythm, information hierarchy, shadow/elevation, component variance, motion) plus 48 React components and 11 slash commands. Includes 5 reference skins (Toss, Stripe, Linear, Vercel, Notion) for grounding AI design decisions against known-good systems. Teaches AI coding tools professional design judgment rather than just data.
Key commands: `/ss-review`, `/ss-lint`, `/ss-a11y`, `/ss-audit`, `/ss-flow`, `/ss-page`, `/ss-component`.
V2 use: pull the 69 visual rules into design-brief output; use `/ss-lint` as a complement to design-review.

### color-contrast (rohitg00/awesome-claude-code-toolkit)
Checks colour combinations for accessibility compliance and suggests accessible alternatives that maintain visual hierarchy. Direct complement to the colour system section of design-brief.
V2 use: call after design-spec.md is written to validate every colour pair in the colour system against WCAG AA.

### rapid-prototyper (rohitg00/awesome-claude-code-toolkit)
Quick prototype scaffolding via a `/mockup` command — generates minimal viable structure from a description. Sits between design-screen and build-agent in the workflow.
V2 use: add as an optional step after design-screen to produce a low-fidelity scaffold before full implementation.

### Playwright-driven design-review (OneRedOak/claude-code-workflows)
Extends design-review by using Playwright MCP to test live components rather than reviewing static code. Catches responsive behaviour, interaction states, and WCAG AA+ violations that static analysis misses. Triggered via PR or `/design-review` slash command.
V2 use: add a `design-review --live` mode that spins up the dev server and drives Playwright for interactive auditing.

### Figma MCP integration
Connect to Figma via MCP to pull component specs, spacing tokens, and colour values directly into design-spec.md rather than generating them from scratch. Relevant when a project already has a Figma file.
V2 use: add a `design-brief --from-figma <url>` mode that reads an existing design system instead of running the interview.

## Iteration log
- 2026-05-26 (V1.1) — 12 fixes applied: colour system expanded to 7 tokens (added text, border), border-radius convention added (adjective-derived, 3-level), shadow/elevation system added (adjective-derived), mode decision added, spacing system added to spec output, icon library section added, fourth component library option added (shadcn/ui only), mobile-first declared in design-screen, accessibility section added to design-screen, screenshots accepted in design-review, UX concerns section added to design-review (Section 3), .gitignore replaced with minimal version.
- 2026-05-25 (V1) — Frame created. Three core skills built.
