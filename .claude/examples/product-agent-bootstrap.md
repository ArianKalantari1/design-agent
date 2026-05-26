# product-agent — Bootstrap Brief

Hand this file to a new Claude Code session and say:
**"Read this file and create the product-agent repository exactly as described."**

---

## Context: why this agent exists

This agent sits upstream of the design-agent. Without it, design decisions get made without a product foundation — the designer invents the product's behaviour (screens, flows, interactions) because nobody defined it first. That produces beautiful designs that solve the wrong problem.

The product-agent's job: define *what to build* before anyone touches *how it looks*.

It is the PM layer in a multi-agent pipeline:

```
product-agent   → product-spec.md   (what to build: screens, flows, interactions)
      ↓
design-agent    → design-spec.md    (how it looks: colours, type, components)
      ↓
build-agent     → working code      (future)
```

Agents communicate via files in the shared project directory. No API calls between agents — each one reads what the previous one wrote.

---

## Bigger vision (do not build yet — context only)

The founder will eventually give one idea to an orchestrator agent. That orchestrator validates the idea, challenges it, and — once the idea is solid — delegates to product-agent, design-agent, and build-agent in sequence. Each agent's output becomes the next one's input. The founder only re-enters when a decision needs a human call.

This is not in scope for V1. But the product-agent must be designed so it fits cleanly into that pipeline when the time comes.

---

## Repository to create

```
product-agent/
  AGENT.md
  .claude/
    CLAUDE.md
    skills/
      product-brief/
        SKILL.md
      product-review/
        SKILL.md
      product-flow/
        SKILL.md
    examples/
      .gitkeep
  .gitignore
```

---

## File contents

---

### AGENT.md

```markdown
# Product Agent

## What this does
Defines what a product is before anyone designs or builds it. Interviews the
founder, validates the idea, maps screens and flows, and writes a product spec
that downstream agents (design-agent, build-agent) read as their source of truth.

## Position in the pipeline
product-agent → design-agent → build-agent

Reads: nothing (it is the first agent in the chain)
Writes: product-spec.md (consumed by design-agent's design-brief skill)

## Inputs
- Founder's idea (described in natural language)
- Existing product-spec.md (for product-review and product-flow skills)

## Outputs
- product-spec.md: problem, users, screen inventory, primary interactions,
  user flows, non-goals, success metric

## Skills
| Skill | When to use |
|---|---|
| product-brief | Once per project, before design-brief |
| product-review | To audit an existing spec for gaps and scope creep |
| product-flow | To map complete user journeys after the spec is written |

## Combines well with
- design-agent (reads product-spec.md to make grounded visual decisions)
- build-agent (future — reads both product-spec.md and design-spec.md)

## Builds on (install these before using)
These existing Claude Code skill collections were researched and selected as the
methodological foundation. The product-agent's custom skills orchestrate them —
they do not reinvent what these libraries already do well.

| Repo | Stars | What it provides |
|---|---|---|
| deanpeters/Product-Manager-Skills | 4.5k | PRD, personas, journey mapping, competitive analysis, validation — all with named frameworks (Nielsen Norman, Jeff Patton, Mom Test, Geoffrey Moore) |
| phuryn/pm-skills | 11.6k | pre-mortem, job-stories, test-scenarios, competitor-analysis |
| ferdinandobons/startup-skill | 317 | Competitive battle cards, pricing landscape, feature matrices |
| anthropics/knowledge-work-plugins | Official | /competitive-brief — Anthropic-maintained competitive research |
| takechanman1228/claude-persona | 23 | Simulated concept interviews across a persona panel |
| johnnychauvet/prd-skill | 2 | JTBD output format ("When [situation], I want to [motivation]") — designed for build-agent handoff |

## Iteration log
- V1 — Three core skills: product-brief, product-review, product-flow.
```

---

### .claude/CLAUDE.md

```markdown
# Product Agent — Project Rules

## What this is
A product definition agent. It runs before the designer and the builder.
Its job is to define what the product does, not how it looks.

## Rules
- The founder's idea is always the input. Never invent the product vision.
- Challenge assumptions before accepting them. A bad product spec is worse
  than no spec.
- One question at a time during the brief. Never combine questions.
- product-spec.md is the source of truth for all downstream agents. Every
  decision in it must be traceable to something the founder said.
- Scope discipline is not optional. If something belongs in V2, name it V2
  and move on. Do not let V1 grow.
- Every screen in the spec must have: a name, a user goal, a primary
  interaction type, and an exit point. Incomplete screen definitions are
  not allowed.
- Non-goals are as important as goals. The spec must name what this product
  explicitly does not do.

## Active skills
| Skill | Status |
|---|---|
| product-brief | Built (V1) |
| product-review | Built (V1) |
| product-flow | Built (V1) |

## External skills installed (foundations)
These are installed alongside the custom skills and called from within them.
Do not duplicate their functionality — orchestrate them.

### deanpeters/Product-Manager-Skills
Core PM methodology. Provides: proto-persona, discovery-interview-prep,
write-prd, press-release, customer-journey-map, user-story-mapping,
lean-ux-canvas, pol-probe, company-research, tam-sam-som-calculator,
positioning-statement. Install instructions in that repo's README.

### phuryn/pm-skills
Supplementary. Key skills used: pre-mortem (for product-review),
job-stories (JTBD format for product-spec output), test-scenarios.

### ferdinandobons/startup-skill
Used only when competitive analysis is needed in product-brief.
Key command: /startup:startup-competitors

### anthropics/knowledge-work-plugins (marketing plugin)
Used for /competitive-brief in the research phase of product-brief.
Official Anthropic repo — reliable and maintained.

### takechanman1228/claude-persona
Used when concept validation is needed. Runs simulated interviews
across a persona panel — unique capability not in other toolkits.

### johnnychauvet/prd-skill
JTBD output framing for product-spec.md sections. Use its format
for user story sections to ensure clean build-agent handoff.

## Skills to add later

### Agentic validation (V2)
Connect product-review to real user interviews via a scheduling tool.
Instead of simulated personas, route validation questions to actual users
and ingest their responses back into the spec.

### PRD drift detection (V2)
DarrenBenson/sdlc-studio has a /reconcile command that detects when
built code has drifted from the original PRD. Valuable once build-agent
is in the pipeline — ensures what gets built matches what was specced.

### Orchestrator layer (V3)
An orchestrator agent that takes a raw idea, runs product-brief,
then hands product-spec.md to design-agent, then hands both specs to
build-agent. The founder only re-enters at decision points. This is the
"idea to shipped product" pipeline.

## Iteration log
- V1 — Frame created. Three core skills built. External skill foundations researched and documented.
```

---

### .claude/skills/product-brief/SKILL.md

```markdown
# Skill: product-brief

## Purpose
Interview the founder about their product idea. Validate and challenge it.
Produce a complete product-spec.md that the design-agent and build-agent read.

## Trigger
User runs /product-brief or asks to define, spec, or validate a product idea.

## Rules
- One question per turn. Never combine questions.
- Do not accept vague answers. If an answer is generic ("anyone could use it",
  "lots of screens", "it depends"), ask it again with more specificity.
- Challenge before accepting. If a scope decision seems too large for V1,
  name the concern before writing it into the spec.
- After all questions are answered, write the spec without further questions.

---

## Leverages
- **deanpeters/Product-Manager-Skills:** use `proto-persona` for persona
  definition, `discovery-interview-prep` (Mom Test style) to frame Q1,
  `write-prd` for the final spec structure, `company-research` and
  `tam-sam-som-calculator` if market sizing is needed
- **anthropics/knowledge-work-plugins:** use `/competitive-brief` when
  the founder names competitors in Q1 or Q2
- **takechanman1228/claude-persona:** use after Q1 if the target user
  is ambiguous — run a simulated concept interview across 3 personas
  before proceeding to Q2
- **johnnychauvet/prd-skill:** use its JTBD format ("When [situation],
  I want to [motivation], so I can [outcome]") for all user story sections
  in the output spec

---

## Interview — Six Questions (one per turn)

**Q1:** What problem does this product solve, and who specifically has this problem?
(Push for a specific person, not a demographic. "A 35-year-old ML engineer who
ships models to production but struggles to explain her work to non-technical
stakeholders" is a good answer. "Professionals" is not.)

**Q2:** What does that person do in the first five minutes of using this product?
(This defines the core action. If they can't answer this, the product is not
defined enough to spec.)

**Q3:** Walk me through every screen in the product. Name each one and say what
the user does on it.
(Accept a rough list. You will structure it — but force them to name every
screen they have in their head before you add any.)

**Q4:** What are three things this product does NOT do in V1?
(Non-goals prevent scope creep. If they struggle to name them, suggest candidate
features and ask whether each is V1 or V2.)

**Q5:** For the most important screen: what is the primary way the user interacts
with it? Typing, speaking, tapping, reading, selecting?
(This is the interaction model. It directly affects how design-agent and
build-agent approach that screen.)

**Q6:** How will you know in 30 days that this product is working?
(Forces a success metric. If the answer is vague, ask: "What is the one number
or behaviour that would tell you this was worth building?")

---

## Output: product-spec.md

Generate product-spec.md with the following sections.

### Problem
One paragraph. The specific problem, the specific person who has it, and why
existing solutions fail them. No marketing language.

### Target User
A named persona. Not a demographic — a described individual with a context,
a goal, and a frustration. One paragraph.

### Core Action
The single most important thing the user does. One sentence.
Format: "[User] [does X] in order to [achieve Y]."

### Screen Inventory
Every screen in the product. For each screen:
- **Name:** what it is called
- **User goal:** what the user is trying to accomplish
- **Primary interaction:** how the user primarily interacts (voice / text / tap
  / read / select / drag)
- **Key elements:** what must be on the screen (not visual — functional)
- **Entry points:** how the user arrives here
- **Exit points:** where the user goes next

### User Flows
The main paths through the product. At minimum:
- Happy path: new user completing the core action for the first time
- Return path: existing user completing the core action again
- Each flow as a numbered step sequence, not prose

### Non-Goals (V1)
A numbered list of things this product explicitly does not do in V1.
Each item must be specific — not "no social features" but "no ability to
see other users' content or interact with their posts."

### Success Metric
One metric. The single number or behaviour that indicates this product is
working 30 days after launch.

### Open Questions
Things that need a founder decision before build starts. If none, write "None."

---

## Save and confirm

Save as product-spec.md in the directory where the skill is run.

Output to terminal:
✅ Product spec saved: product-spec.md
```

---

### .claude/skills/product-review/SKILL.md

```markdown
# Skill: product-review

## Purpose
Audits an existing product-spec.md for gaps, ambiguities, scope creep, and
missing definitions. Run this after product-brief, or when reviewing someone
else's spec before handing it to the design-agent.

## Trigger
User runs /product-review or asks to review, audit, or check a product spec.

## Pre-flight check
Check whether product-spec.md exists in the project root.
- If it does not exist: stop and say "Run product-brief first."
- If it exists: read it fully before proceeding.

---

## Leverages
- **deanpeters/Product-Manager-Skills:** use `lean-ux-canvas` to validate
  the problem-solution fit before checking screen definitions; use `pol-probe`
  to stress-test assumptions in the spec
- **phuryn/pm-skills:** use `pre-mortem` — ask "what would make this product
  fail in 6 months?" and surface those risks as Blocking findings; use
  `test-scenarios` to check whether acceptance criteria are testable

---

## Input
Ask: "Paste the spec you want reviewed, or should I read product-spec.md?"
Accept either.

---

## Output: Four sections

Vague feedback is not allowed. Every finding must name the section, the gap,
and the recommended fix.

### Section 1 — Incomplete Screen Definitions
Any screen missing a user goal, primary interaction type, or exit point.
For each:
- Screen name
- What is missing
- Why it matters (one sentence — what goes wrong in design/build without it)

### Section 2 — Scope Concerns
Anything in V1 that likely belongs in V2. For each:
- Feature or screen
- Why it is V2 not V1 (complexity, dependency, or frequency of use)
- Recommended disposition: move to V2, simplify, or justify staying in V1

### Section 3 — Flow Gaps
User flows that are incomplete. Common gaps to look for:
- No empty state defined (what does the user see the very first time, before
  any data exists?)
- No error state defined (what happens when something fails?)
- No exit from a flow (user is stuck with no back or cancel)
- Flow assumes data that doesn't exist yet (chicken-and-egg problems)
- Return user flow missing — only the new user flow is defined

### Section 4 — Prioritised Fix List
All findings ordered by how badly they would block design or build.
Each item:
- Priority: Blocking / Important / Minor
- Section and finding
- Exact recommended change
```

---

### .claude/skills/product-flow/SKILL.md

```markdown
# Skill: product-flow

## Purpose
Takes product-spec.md and produces a complete user journey map — every
screen, every decision point, every empty and error state. This is what
the design-agent reads to know exactly what to build on each screen.

## Trigger
User runs /product-flow or asks to map, diagram, or detail user flows.

## Pre-flight check
Check whether product-spec.md exists.
- If it does not exist: stop and say "Run product-brief first."
- If it exists: read it fully before proceeding.

---

## Leverages
- **deanpeters/Product-Manager-Skills:** use `customer-journey-map` (Nielsen
  Norman Group framework) for the emotional arc of each flow — stages,
  touchpoints, pain points; use `user-story-mapping` (Jeff Patton) to
  organise flows by user activity across the backbone
- **phuryn/pm-skills:** use `job-stories` format ("When [situation], I want
  to [motivation], so I can [outcome]") for every step in the journey map
  rather than plain action descriptions — this keeps the focus on user intent,
  not system behaviour

---

## Output: Journey Map

Produce a journey map for each flow defined in product-spec.md.

For each step in the flow, define:
- **Screen:** which screen the user is on
- **User state:** what the user knows and has done so far
- **User goal:** what they are trying to do on this step
- **Primary action:** the main interaction (one action — the most important one)
- **Secondary actions:** other available actions (list, not prose)
- **Success path:** where the user goes if the action succeeds
- **Failure path:** where the user goes if the action fails
- **Empty state:** what the user sees if there is no data yet (required for
  every screen that can be empty)

### Interaction Model Declaration
For every screen in the product, explicitly state the primary interaction type:
- **Voice:** user speaks, content is transcribed or processed
- **Text:** user types into a field
- **Read:** user reads content with no required input
- **Select:** user chooses from a set of options
- **Review + edit:** user reviews AI-generated content and modifies it
- **Navigate:** user moves between sections with no data input

This section is what prevents design-agent from inventing the wrong interaction.
If a screen is voice-first, this file says so. The design-agent reads this
and builds accordingly.

### Decision Points
Any moment where the user's path branches. For each:
- What triggers the decision
- The options available
- Where each option leads

### Edge Cases
Scenarios outside the happy path that must be handled:
- First-time user with no existing data
- User returning after a long absence
- User who abandons a flow halfway through
- User on a slow or offline connection (if relevant)

---

## Save and confirm

Append the journey map to product-spec.md under a new section heading
"## User Journey Map", or save as product-flow.md if product-spec.md
should stay clean.

Output to terminal:
✅ Flow map complete.
```

---

### .gitignore

```
# macOS
.DS_Store
.AppleDouble

# Editor
.idea/
.vscode/
*.swp

# Node
node_modules/
.next/

# Environment
.env
.env.local

# Product agent output — belongs in the target project, not here
product-spec.md
product-flow.md
```

---

## After creating all files

1. Install the external skill foundations in the new repo. For each of the
   following, clone or copy the relevant skill files into .claude/skills/ or
   follow the install instructions in each repo's README:

   - github.com/deanpeters/Product-Manager-Skills (primary foundation — install first)
   - github.com/phuryn/pm-skills (install: pre-mortem, job-stories, test-scenarios)
   - github.com/ferdinandobons/startup-skill (install: startup-competitors command only)
   - github.com/anthropics/knowledge-work-plugins/marketing (install: competitive-brief)
   - github.com/takechanman1228/claude-persona (install all — small footprint)
   - github.com/johnnychauvet/prd-skill (reference only — use its JTBD output format)

   Do NOT install skills that duplicate what the custom skills already do.
   These are foundations to call from within product-brief, product-review,
   and product-flow — not replacements for them.

2. Initialise git, make an initial commit with message:
   "feat: bootstrap product-agent with three core skills"

3. Push to the remote.

---

## How this connects to design-agent

When both agents are used on the same project, the shared project directory holds both output files:

```
my-project/
  product-spec.md    ← written by product-agent
  design-spec.md     ← written by design-agent (reads product-spec.md)
```

The design-agent's design-brief skill should be updated to check for product-spec.md
and read it before asking its five questions. If it exists, several design decisions
(screen layouts, primary interactions, component choices) are already defined — the
brief interview fills in the visual layer on top.

The design-agent repository is at: github.com/ArianKalantari1/design-agent
Branch: claude/design-agent-bootstrap-ynIX7

When the product-agent is live, open a session in the design-agent repo and update
.claude/skills/design-brief/SKILL.md to add a product-spec.md check at the top of
the interview flow.
