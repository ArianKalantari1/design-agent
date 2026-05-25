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

## Interview — Five Questions (one per turn)

**Q1:** What does this product do and who uses it?

**Q2:** What is the one thing the user needs to accomplish in under 2 minutes?

**Q3:** Describe how this product should feel. Give three adjectives.

**Q4:** Name two or three products your target user already uses and respects.

**Q5:** What should this product absolutely not look like or feel like?

---

## Output: design-spec.md

After all five answers, generate `design-spec.md` with the following sections. Every decision must be justified against the user's answers.

### Colour System

Provide hex values for:
- `primary` — the dominant action colour
- `background` — page/app background
- `accent` — secondary highlight
- `surface` — card/panel background
- `muted` — subdued text colour

**Hard constraints:**
- Must differ deliberately from shadcn defaults (zinc/slate), Vercel palette (black/white/blue), and Linear palette (indigo/purple)
- No purple or indigo as primary
- No pure black (`#000000`) as background
- Each colour must be justified against the adjectives the user gave in Q3

### Typography

- One heading font from Google Fonts
- One body font from Google Fonts
- Justify the pairing against the brief
- Include sizes: h1, h2, h3, body, small (use rem)

### Component Library

- Base: shadcn/ui (always)
- Specialist library — choose one and justify against the product description:
  - **Tremor** — dashboards and data views
  - **Magic UI** — motion-forward products
  - **Aceternity** — marketing-heavy products

### Layout Principles

Five principles specific to this product and its user. Not generic rules — derived directly from the brief answers.

### Do-Not-Do List

Five things to actively avoid for this product. Specific, not generic — derived from Q5 and the overall brief.

---

## Save and confirm

Save the completed spec as `design-spec.md` in the directory where the skill is run.

Output to terminal:

```
✅ Design spec saved: design-spec.md
```
