# Skill: design-screen

## Purpose
Takes a screen description from the user and produces a full screen spec with a React/Next.js scaffold, grounded in the project's design spec.

## Trigger
User runs `/design-screen` or asks to design, spec, or scaffold a specific screen.

## Pre-flight check

Before doing anything else, check whether `design-spec.md` exists in the project root.

- **If it does not exist:** Stop immediately and say:
  > "Run design-brief first to create a design spec."
- **If it exists:** Read it fully before proceeding.

---

## Input

Ask the user: "Describe the screen — what it does, who sees it, and what they need to accomplish on it."

Accept a plain description. Do not ask follow-up questions unless a decision would contradict the spec.

---

## Output: Screen Spec

Produce the following sections.

### Component List

Every component visible on the screen:
- Name (use shadcn/ui component names where applicable)
- Purpose in one sentence
- Any state variants (empty, loading, error, filled)

### Layout Structure

- Flex or grid — specify which and why
- Spacing values using the 4px scale only: 4, 8, 12, 16, 24, 32, 48, 64px
- Mobile layout (single column unless justified otherwise)
- Desktop layout (breakpoint: `md:` or `lg:`)
- Named regions (e.g. sidebar, content area, header)

### Tailwind Class Decisions

For each key element, provide the Tailwind classes. Reference colours from the design spec using arbitrary values:
- e.g. `bg-[#c17f24]` not `bg-amber-500`
- e.g. `text-[#1a1a1a]` not `text-gray-900`

Include: background, text, border, padding, border-radius, shadow (if any).

### Interaction Notes

- Hover states for interactive elements
- Transitions: use `transition-all duration-200` as standard unless justified otherwise
- Loading state: skeleton or spinner — specify which and why
- Empty state: what the screen shows when there is no data

### React/Next.js Scaffold

Actual JSX ready to paste into a file. Requirements:
- Use shadcn/ui component imports (`import { Button } from "@/components/ui/button"` etc.)
- Tailwind classes only — no inline styles
- TypeScript-compatible (typed props where needed)
- Annotate each section with a one-line comment naming the region

---

## Spec compliance check

Before outputting, verify every decision against the design spec:
- Colours match the colour system
- Fonts match the typography spec
- Spacing uses the 4px scale
- Components match the chosen component library

If any decision would contradict the spec, flag it explicitly and ask the user before proceeding:
> "This would use [X] which contradicts the spec's [Y]. Should I proceed with the spec or override it here?"
