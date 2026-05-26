# Skill: design-brief

## Purpose
Run once per project at the start. Asks five questions one at a time, then produces a complete design spec saved as `design-spec.md` in the project root.

## Trigger
User runs `/design-brief` or asks to create a design spec / design brief.

## Rules
- Ask exactly one question per turn. Never combine questions.
- Wait for the user's answer before asking the next question.
- Do not offer examples or suggestions during the interview — record what the user says, not what you think they mean.
- After all five answers are collected, produce the full spec without asking further questions.

---

## Pre-flight check

Before starting the interview, check whether `product-spec.md` exists in the project root.

- **If it does not exist:** Proceed with all five questions below.
- **If it exists:** Read it fully. Note which of the following are already defined:
  - Product name and description
  - Target user
  - Core action / primary task
  - Screen inventory and Interaction Model Declaration per screen
  - Mode preference (if stated)
  - User flows and non-goals

Then adapt the interview:
- **Skip Q1** if product-spec.md fully defines the product and user. Open with one sentence summarising what you read: "I've read product-spec.md — [one sentence]. Does that match how you'd describe it?" Then move directly to the first unanswered question.
- **Skip Q2** if the core action is defined in product-spec.md — carry it forward silently into the spec output.
- **Always ask Q3, Q4, Q5** — these are visual and tonal questions that product-spec.md does not answer.

Do not recite product-spec.md back to the user. Summarise in one sentence what you're skipping, then continue.

---

## Interview — Five Questions (one per turn)

**Q1:** What does this product do and who uses it?

**Q2:** What is the one thing the user needs to accomplish in under 2 minutes?

**Q3:** Describe how this product should feel. Give three adjectives.

**Q4:** Name two or three products your target user already uses and respects.

**Q5:** What should this product absolutely not look like or feel like?

---

## Output: design-spec.md

After all five answers, generate `design-spec.md` with the following sections. Every decision must be justified against the user's answers.

---

### Colour System

Provide hex values for all seven tokens:

- `primary` — dominant action colour (buttons, links, key UI)
- `background` — page/app background
- `surface` — card, panel, modal background
- `text` — primary foreground colour for body copy and headings
- `muted` — subdued text, placeholders, captions
- `accent` — secondary highlight, badges, tags
- `border` — card borders, dividers, input outlines

**Hard constraints:**
- Must differ deliberately from shadcn defaults (zinc/slate), Vercel palette (black/white/blue), and Linear palette (indigo/purple)
- No purple or indigo as primary
- No pure black (`#000000`) as background
- No pure white (`#ffffff`) as background unless the product explicitly demands clinical sterility
- Each colour must be justified against the adjectives from Q3

---

### Mode

Declare one of the following and justify against the brief:

- **Light default** — most consumer and business products
- **Dark default** — justified only for: developer tools, media consumption, creative tools, anything primarily used at night
- **System default** — when both modes are equally important and you are building both from day one

Do not choose dark default because it looks impressive. Choose it because the use context demands it.

---

### Typography

- One heading font from Google Fonts
- One body font from Google Fonts
- Justify the pairing against the brief — reference specific adjectives from Q3 and the reference products from Q4
- Include sizes in rem: h1, h2, h3, body, small
- Include font weights used: headings, body, labels

---

### Spacing System

This product uses the 4px base scale. All spacing values must come from this list:

`4px · 8px · 12px · 16px · 24px · 32px · 48px · 64px`

Declare the density preference for this product:
- **Compact** — data-dense products (dashboards, tables, admin tools): prefer values up to 32px, use 48–64 sparingly
- **Standard** — most products: full range available
- **Generous** — content-heavy or consumer products: prefer 16px and above, avoid 4–8px except for tight inline spacing

---

### Border-Radius Convention

Radius communicates personality. It must be derived from Q3 adjectives, not chosen arbitrarily.

Define three levels — do not use a single value for all elements:

- **Interactive** (buttons, inputs, selects): derived from adjectives
- **Container** (cards, panels, modals): one step larger than interactive
- **Pill** (tags, badges, chips): only use `rounded-full` if the brand explicitly calls for it

**Derivation guide — use adjectives to justify, not this as a rule:**
- Precise / technical / clinical → interactive: `rounded` (4px), container: `rounded-md` (6px)
- Warm / approachable / friendly → interactive: `rounded-lg` (8px), container: `rounded-xl` (12px)
- Elegant / refined / luxury → interactive: `rounded-sm` (2px), container: `rounded` (4px)
- Bold / direct / confident → interactive: `rounded-sm` (2px), container: `rounded-md` (6px)
- Playful / expressive / creative → interactive: `rounded-xl` (12px), container: `rounded-2xl` (16px)

Every radius value must reference a specific adjective the user gave. If `rounded-2xl` appears, it must be earned by the brief — not defaulted to.

---

### Shadow & Elevation System

Shadows define depth hierarchy. Derive from Q3 adjectives — some brands use no shadows at all (borders only).

Define three elevation levels and which elements use them:

- **Level 0** (flat): inputs, inline elements
- **Level 1** (subtle): cards, panels
- **Level 2** (lifted): modals, dropdowns, tooltips

**Derivation guide — justify against adjectives:**
- Minimal / flat / technical → no shadows at any level; use `border` token for separation
- Warm / soft / approachable → `shadow-sm` at Level 1, `shadow-md` at Level 2
- Premium / bold / confident → `shadow-sm` at Level 1, `shadow-lg` at Level 2, no shadow on inputs
- Playful / expressive → `shadow-md` at Level 1, `shadow-xl` at Level 2

Shadows must escalate with elevation. Do not use the same shadow value for cards and modals.

---

### Icon Library

Choose one and justify against the product's feel:

- **Lucide** — clean, consistent stroke weight, default for shadcn/ui projects, suits most products. Import: `import { IconName } from "lucide-react"`
- **Heroicons** — Tailwind-native, two styles (outline / solid), minimal and structured. Import: `import { IconName } from "@heroicons/react/24/outline"`
- **Phosphor** — most expressive, six weights (thin through bold), suited to personality-forward or consumer-facing products. Import: `import { IconName } from "phosphor-react"`

State the chosen library, the style variant (if applicable), and the default size (`w-4 h-4` for inline, `w-5 h-5` for standalone).

---

### Component Library

- Base: **shadcn/ui** (always — no exceptions)
- Specialist library — choose one and justify against the product description:
  - **Tremor** — dashboards, data tables, analytics views
  - **Magic UI** — motion-forward products where animation is part of the experience
  - **Aceternity** — marketing-heavy products, landing pages, hero sections
  - **None (shadcn/ui only)** — when the product is a simple CRUD app, task manager, or any product that does not fit the above three categories. Valid and often the right choice.

---

### Layout Principles

Five principles specific to this product and its user. Not generic rules — derived directly from the five brief answers. Each principle must name the product context it comes from.

---

### Do-Not-Do List

Five things to actively avoid for this product. Specific, not generic — derived from Q5 and the overall brief. Each item must name the element, the pattern, and why it is wrong for this product specifically.

---

## Save and confirm

Save the completed spec as `design-spec.md` in the directory where the skill is run.

Output to terminal:

```
✅ Design spec saved: design-spec.md
```
