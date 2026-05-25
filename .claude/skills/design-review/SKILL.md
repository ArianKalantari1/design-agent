# Skill: design-review

## Purpose
Audits an existing screen against the project's design spec. Identifies generic AI patterns, spec violations, and produces a prioritised fix list with exact code changes.

## Trigger
User runs `/design-review` or asks to audit, review, or check an existing screen.

## Pre-flight check

Before doing anything else, check whether `design-spec.md` exists in the project root.

- **If it does not exist:** Stop immediately and say:
  > "Run design-brief first to create a design spec."
- **If it exists:** Read it fully before proceeding.

---

## Input

Ask the user: "Paste the screen code or describe the screen you want reviewed."

Accept either:
- JSX/TSX code (pasted directly)
- A plain description of an existing screen

---

## Output: Three sections

Vague feedback is not allowed. Every finding must name the element, the current value, and the replacement value.

---

### Section 1 — Generic AI Patterns Found

List what looks AI-generated and exactly why. For each item:
- **Element:** name the specific component or element (e.g. "Primary button")
- **Property:** the specific CSS property or Tailwind class (e.g. `rounded-2xl`, `bg-indigo-600`)
- **Why it's generic:** one sentence referencing the known AI default being used

Common patterns to look for (not exhaustive):
- `rounded-2xl` or `rounded-full` applied everywhere
- `bg-indigo-600` or `bg-purple-600` as primary colour
- `font-geist` or default system font stack
- `shadow-lg` used uniformly on all cards
- Pure `#000000` or `#ffffff` backgrounds
- Gradient overlays using purple-to-blue
- Every card having the same padding
- Glassmorphism (`backdrop-blur`, `bg-white/10`)

---

### Section 2 — Spec Violations

What breaks the colour system, typography, or layout principles defined in `design-spec.md`. For each violation:
- **Element:** name it
- **Current value:** the exact class or hex
- **Violation:** which spec rule it breaks
- **Fix:** the exact replacement e.g. "Change `bg-indigo-600` to `bg-[#c17f24]`"

---

### Section 3 — Prioritised Fix List

Ordered by visual impact. Each item must include:
- **Priority:** High / Medium / Low
- **What to change:** element and property
- **Why:** one sentence referencing the spec or UX principle
- **Exact change:** old value → new value, formatted as a code snippet

**High** = immediately visible, breaks brand consistency
**Medium** = noticeable on close inspection, weakens the design system
**Low** = minor polish, spacing, or typographic detail
