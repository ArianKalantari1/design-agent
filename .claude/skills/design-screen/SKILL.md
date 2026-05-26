# Skill: design-screen

## Purpose
Takes a screen description from the user and produces a full screen spec with a React/Next.js scaffold, grounded in the project's design spec.

## Trigger
User runs `/design-screen` or asks to design, spec, or scaffold a specific screen.

## Pre-flight check

Before doing anything else:

1. Check whether `design-spec.md` exists in the project root.
   - **If it does not exist:** Stop immediately and say:
     > "Run design-brief first to create a design spec."
   - **If it exists:** Read it fully before proceeding.

2. Check whether `product-spec.md` exists in the project root.
   - **If it exists:** Read it fully. Before generating the scaffold for any named screen, locate that screen's **Interaction Model Declaration** in product-spec.md. The interaction model defines the primary input mechanism for that screen — voice, text entry, read-only, selection, review+edit, or navigate. The scaffold must match this model. If the user's screen description conflicts with the declared model, flag it:
     > "product-spec.md declares this screen as [model]. Your description suggests [other model]. Which should the scaffold follow?"
   - **If it does not exist:** Proceed using the screen description the user provides.

---

## Input

Ask the user: "Describe the screen — what it does, who sees it, and what they need to accomplish on it."

Accept a plain description. Do not ask follow-up questions unless a decision would contradict the spec.

---

## Output: Screen Spec

Produce the following sections.

---

### Component List

Every component visible on the screen:
- Name (use shadcn/ui component names where applicable)
- Purpose in one sentence
- Any state variants (empty, loading, error, filled)

---

### Layout Structure

**Mobile-first.** Write base styles for the smallest screen. Use `md:` and `lg:` to enhance for larger viewports — never the reverse.

- Flex or grid — specify which and why
- Spacing values using the 4px scale from the spec only: 4, 8, 12, 16, 24, 32, 48, 64px
- Mobile base layout (default, no breakpoint prefix)
- Desktop enhancement (use `md:` or `lg:` prefixes)
- Named regions (e.g. sidebar, content area, header, footer)

---

### Tailwind Class Decisions

For each key element, provide the Tailwind classes. Reference colours from the design spec using arbitrary values — never use Tailwind colour names:
- e.g. `bg-[#c17f24]` not `bg-amber-500`
- e.g. `text-[#1a1a1a]` not `text-gray-900`
- e.g. `border-[#e2e0db]` not `border-gray-200`

Include for each element: background, text, border, padding, border-radius (from spec convention), shadow (from spec elevation system).

---

### Interaction Notes

- Hover states for all interactive elements
- Transitions: use `transition-all duration-200` as standard unless the spec's feel justifies otherwise
- Focus states: every interactive element must have a visible focus ring — `focus-visible:ring-2 focus-visible:ring-[primary] focus-visible:outline-none`
- Loading state: skeleton or spinner — specify which and justify (skeleton for content-shaped loading, spinner for action-triggered loading)
- Empty state: what the screen shows when there is no data — include copy and any illustration guidance
- Error state: inline error messaging for forms, toast or banner for system errors

---

### Accessibility

Every scaffold must include the following — these are not optional:

- **Semantic HTML:** use `<nav>`, `<main>`, `<section>`, `<header>`, `<footer>`, `<article>`, `<button>` correctly. Never use a `<div>` where a semantic element exists.
- **ARIA labels:** every icon-only button needs `aria-label`. Every form input needs a visible `<label>` or `aria-label`. Every modal needs `aria-modal="true"` and `aria-labelledby`.
- **Tab order:** interactive elements must follow logical reading order. Do not use `tabIndex` values other than 0 and -1.
- **Contrast:** verify that `text` on `background` and `text` on `surface` from the design spec meet WCAG AA minimum (4.5:1 for body text, 3:1 for large text / UI components). Flag any pair that likely fails.
- **Alt text:** every `<img>` needs `alt`. Decorative images get `alt=""`. Meaningful images get descriptive alt text.
- **Form errors:** associate error messages with their inputs using `aria-describedby`.

Note: semantic HTML and ARIA labels directly improve search engine indexing — crawlers read the same structure as screen readers.

---

### React/Next.js Scaffold

Actual JSX ready to paste into a file. Requirements:
- Use shadcn/ui component imports (`import { Button } from "@/components/ui/button"` etc.)
- Use icon library declared in the spec (`import { IconName } from "lucide-react"` or equivalent)
- Tailwind classes only — no inline styles
- TypeScript-compatible (typed props where needed)
- Semantic HTML elements throughout
- Annotate each region with a one-line comment naming it

---

## Spec compliance check

Before outputting, verify every decision against the design spec:
- Colours use the seven spec tokens as arbitrary Tailwind values
- Fonts match the typography spec
- Spacing uses only values from the spec's spacing system
- Border-radius follows the spec's three-level convention
- Shadows follow the spec's elevation system
- Icons use the declared library
- Components match the chosen component library

If any decision would contradict the spec, flag it explicitly and ask before proceeding:
> "This would use [X] which contradicts the spec's [Y]. Should I proceed with the spec or override it here?"
