# Skill: design-review

## Purpose
Audits an existing screen against the project's design spec. Identifies generic AI patterns, spec violations, UX concerns, and produces a prioritised fix list with exact code changes.

## Trigger
User runs `/design-review` or asks to audit, review, or check an existing screen.

## Pre-flight check

Before doing anything else, check whether `design-spec.md` exists in the project root.

- **If it does not exist:** Stop immediately and say:
  > "Run design-brief first to create a design spec."
- **If it exists:** Read it fully before proceeding.

---

## Input

Ask the user: "Paste the screen code, share a screenshot, or describe the screen you want reviewed."

Accept any of:
- JSX/TSX code (pasted directly)
- A screenshot or image (read it visually — audit what you see)
- A plain description of an existing screen

When reviewing from a screenshot: describe what you observe visually before auditing. Name elements by what they appear to be (e.g. "primary CTA button, top right, blue fill"). Do not guess at class names — flag them as "visually observed, class unknown."

---

## Output: Four sections

Vague feedback is not allowed. Every finding must name the element, the current value, and the replacement value.

---

### Section 1 — Generic AI Patterns Found

List what looks AI-generated and exactly why. For each item:
- **Element:** name the specific component or element (e.g. "Primary button")
- **Property:** the specific CSS property or Tailwind class (e.g. `rounded-2xl`, `bg-indigo-600`)
- **Why it's generic:** one sentence referencing the known AI default being used

Common patterns to look for (not exhaustive):
- `rounded-2xl` or `rounded-full` applied to every element uniformly
- `bg-indigo-600` or `bg-purple-600` as primary colour
- `font-geist` or unspecified system font stack
- `shadow-lg` used uniformly on all cards regardless of elevation
- Pure `#000000` or `#ffffff` backgrounds
- Gradient overlays using purple-to-blue
- Every card having identical padding
- Glassmorphism (`backdrop-blur`, `bg-white/10`)
- `gap-4` or `gap-6` used uniformly across all layouts
- Generic placeholder copy: "Lorem ipsum", "Title here", "Description"

---

### Section 2 — Spec Violations

What breaks the colour system, typography, spacing, radius, shadow, or icon conventions defined in `design-spec.md`. For each violation:
- **Element:** name it
- **Current value:** the exact class or hex
- **Violation:** which spec rule it breaks
- **Fix:** the exact replacement e.g. "Change `bg-indigo-600` to `bg-[#c17f24]`"

Check every token: primary, background, surface, text, muted, accent, border. Check radius levels. Check shadow elevation. Check icon library. Check spacing values against the spec's scale.

---

### Section 3 — UX Concerns

Audit for usability and hierarchy problems — not just visual ones. These are the issues that hurt conversion, cause confusion, and make users leave. For each concern:
- **Issue:** name the problem clearly
- **Element(s) affected:** which part of the screen
- **Why it matters:** one sentence on the user impact
- **Recommendation:** specific change, not a general suggestion

Common UX concerns to look for:
- More than two primary CTAs competing for attention on one screen
- Primary action not visible without scrolling on mobile
- Information hierarchy is flat — everything has the same visual weight, nothing leads the eye
- Cognitive overload — too many elements, decisions, or labels without grouping or breathing room
- Interactive elements that don't look interactive (no hover state, no cursor change, no affordance)
- Non-interactive elements that look clickable (underlined text that isn't a link, card-like containers that don't click)
- Form fields with no visible label — placeholder text as label disappears on input
- Destructive actions (delete, cancel, irreversible) with no confirmation step or friction
- Trust signals absent on high-stakes screens (checkout, account creation, payment)
- Empty states missing — screen shows nothing instead of a helpful prompt when no data exists
- No loading feedback — action triggers but nothing tells the user something is happening

---

### Section 4 — Prioritised Fix List

All findings from Sections 1, 2, and 3 combined and ordered by impact. Each item must include:
- **Priority:** High / Medium / Low
- **What to change:** element and property
- **Why:** one sentence referencing the spec, a UX principle, or user impact
- **Exact change:** old value → new value, formatted as a code snippet (or plain instruction for UX changes without a code equivalent)

**High** = immediately visible or immediately harmful to usability — fix before shipping
**Medium** = noticeable on inspection, weakens the design system or causes friction
**Low** = minor polish, spacing refinement, or typographic detail
