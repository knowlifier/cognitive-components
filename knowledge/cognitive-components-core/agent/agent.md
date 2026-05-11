# Cognitive Components Core Functions

---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#quick-and-dirty`

---

## When to apply the agent

Apply only in Design-Time in Cursor/ClaudeCode/Codex etc. when the user asks to create a new or refactor existing Cognitive Package or Cognitive Component.


## File Reference

- **Package Structure:** `concepts/definitions/component-package-project-definitions.md`
- **Classes Standard:** `concepts/definitions/entities-classes-taxonomies-definition.md`
- **Claude skills (official guide):** `workflows/claude-skill-generator/claude-official-guide/The-Complete-Guide-to-Building-Skills-for-Claude.md`
- **Generate Claude skill rulebook:** `workflows/claude-skill-generator/generate-claude-skill-rulebook.md`
- **Synchronize architecture rulebook:** `workflows/sync-architecture-rulebook.md`
- **Create cognitive package rulebook:** `workflows/create-cognitive-package-rulebook.md`
- **Create cognitive component rulebook:** `workflows/create-cognitive-component-rulebook.md`
- **Implement component from architecture rulebook:** `workflows/implement-component-from-architecture-rulebook.md`

**Host project:** In a consuming repository, permissions and request routing are defined at the project root (`IKNOW.md`, `AGENTS.md`). Do not duplicate those documents inside this package.

---

## Agent Jobs

### Create a new Cognitive Package

**Description:** Scaffolds a new `Cognitive Package` and an initial component under `knowledge/<component-name>/` with minimal folders (`agent/`, `concepts/`, `data/`, `workflows/`). When the user describes **purpose** or how the component should work, **do not** immediately flesh out concepts, data, and workflows: assess **simple vs complex**, place **`architecture.md`** at `{component-root}/architecture.md` (simple) or `{component-root}/architecture/architecture.md` (complex), document purpose, main ideas, and cooperation with other components, and mark each logical segment with **Implementation status:** FUTURE. Identify which segments have enough spec to implement next vs what information is missing; ask the user to review `architecture.md`. When they ask to **continue**, follow the job **Create cognitive component elements from `architecture.md`** (run `workflows/implement-component-from-architecture-rulebook.md`).

**Situation:** The user wants a new package with Cognitive Components, often including an initial component and a described goal.

**Request examples:** create a new cognitive package, create a new cognipackage, create copackage, new knoware package, scaffold cognitive package

**Actions:** run `workflows/create-cognitive-package-rulebook.md`

**Expected Result:** Package skeleton (`cognitive-package.json`, `glossary.md`, package `agent/`, `knowledge/<component>/` with minimal `agent/`, `concepts/`, `data/`, `workflows/`). Optional package-level `architecture.md` when multiple components or cross-component design apply (see `workflows/create-cognitive-package-rulebook.md`). If purpose was given: component `architecture.md` in the chosen location with **FUTURE** segments and a gap analysis; user directed to review and then invoke the follow-up job to implement ready segments.

---

### Create cognitive component elements from `architecture.md`

**Description:** Populates `concepts/`, `data/`, `workflows/`, and `agent/` from a **user-reviewed** `architecture.md`, only for segments where specification is sufficient; updates **Implementation status** to **IN PROGRESS** or **IMPLEMENTED**; leaves **FUTURE** and lists additional information needed elsewhere.

**Situation:** The user reviewed `architecture.md` (after package creation or manual authoring) and wants to **continue**, **implement from architecture**, or build out the component accordingly.

**Request examples:** continue from architecture, implement from architecture.md, create component elements from architecture, populate component from architecture, build out what we designed, fill in concepts from architecture

**Actions:** run `workflows/implement-component-from-architecture-rulebook.md`

**Expected Result:** New or updated knowledge files aligned with architecture; `architecture.md` statuses revised; explicit list of remaining gaps and questions.

---

### Create a new Cognitive Component

**Description:** Scaffolds a new component folder (typically under `knowledge/<component-name>/` next to sibling components) with minimal `agent/`, `concepts/`, `data/`, and `workflows/`. When the user describes **purpose** or how the component should work, **do not** immediately flesh out concepts, data, or workflows: assess **simple vs complex**, place **`architecture.md`** at `{component-root}/architecture.md` (simple) or `{component-root}/architecture/architecture.md` (complex), document purpose, main ideas, and cooperation with other components, and mark each logical segment with **Implementation status:** FUTURE. Identify segments with enough specification to implement next vs gaps; invite the user to review `architecture.md`. When they ask to **continue** (after review), run the job **Create cognitive component elements from `architecture.md`** via `workflows/implement-component-from-architecture-rulebook.md`.

**Situation:** The user wants to add another Cognitive Component inside an existing Cognitive Package (or an equivalent layout under allowed `knowledge/` paths).

**Request examples:** create a cognitive component, create new cocoponent, add new cogniponent to the package, scaffold another component in the package

**Actions:** run `workflows/create-cognitive-component-rulebook.md`

**Expected Result:** Minimal component tree (`agent/`, `concepts/`, `data/`, `workflows/`). If purpose was given: `architecture.md` in the chosen location with **FUTURE** segments, gap analysis, and direction to implement ready segments after user review (**continue** / implement-from-architecture job).

---

### Modify or refactor an existing Cognitive Package

**Description:** Evolves an existing Cognitive Package: `cognitive-package.json`, `glossary.md`, package-level `agent/`, optional package-level `architecture.md`, and coordination across `knowledge/*` components. Structural authority: `concepts/definitions/component-package-project-definitions.md`.

**Situation:** The user wants to reorganize, extend dependencies, or align the package as a whole—not only one component.

**Request examples:** refactor cognitive package, update package dependencies, reorganize the package, add package-level architecture, split a package

**Actions:** Apply edits per the definitions document and the user’s goals; add or refresh package-level `architecture.md` when cross-component design changes; run `workflows/sync-architecture-rulebook.md` on each **affected component** root when `architecture.md` must match the tree.

**Expected Result:** Updated package metadata, glossary, agents, and optional package architecture; component trees and architecture files consistent where touched.

---

### Modify or refactor an existing Cognitive Component

**Description:** Changes `concepts/`, `data/`, `workflows/`, `agent/`, or layout within a **single** existing component. Preserve alignment with `concepts/definitions/component-package-project-definitions.md`.

**Situation:** The user wants to evolve, split, or clean up one component under `knowledge/<component-name>/` (or an allowed equivalent path).

**Request examples:** refactor this component, reorganize workflows, merge definition files, update component after manual edits

**Actions:** Implement structural and content edits per user instructions; run `workflows/sync-architecture-rulebook.md` on that component when reconciling `architecture.md` with the tree.

**Expected Result:** Updated component files; `architecture.md` statuses and narrative accurate if sync was in scope.

---

### Create or modify a Cognitive Agent (package or component `agent.md`)

**Description:** Creates or edits `agent.md` for a **package** (`{package-root}/agent/agent.md` or package root) or **component** (component `agent/` folder or `agent.md` at component root). Jobs should list workflows and point to rulebooks in `workflows/`.

**Situation:** The user wants routing, job lists, or new rulebook links in a package or component agent file.

**Request examples:** add jobs to agent.md, update package agent, edit component agent, new agent jobs

**Actions:** Edit the target `agent.md` following `concepts/definitions/component-package-project-definitions.md` for placement and conventions; mirror the structure of this file (jobs with Description, Request examples, Actions, Expected Result) where helpful.

**Expected Result:** Updated `agent.md` with consistent job entries and references to workflow rulebooks.

---

### Generate a Claude skill for a Cognitive Component

**Description:** Creates a valid Claude skill under the component’s `workflows/skills/[skill-kebab-name]/`, following Anthropic’s skill guide and bundling only what lives inside that skill folder (self-contained).

**Situation:** The user wants a reusable Claude skill for a specific operation grounded in the component’s concepts and data, typically for use with Claude Code / Claude.ai skills.

**Request examples:** generate a Claude skill, create a skill for this component, add a skill under workflows/skills, skill for taxonomy workflow, build agent skill from our definitions

**Actions:** run `workflows/claude-skill-generator/generate-claude-skill-rulebook.md`

**Expected Result:** A skill folder at `{component-root}/workflows/skills/[skill-kebab-name]/` with `SKILL.md` and optional `references/`, `assets/`, `scripts/` per the official guide; knowledge needed at runtime copied or summarized into that skill tree.

---

### Synchronize `architecture.md` with the component

**Description:** Reviews `concepts/` (definitions, principles, taxonomies, templates, classes), `data/`, and `workflows/`, compares them to the current `architecture.md`, updates statuses, and rewrites outdated architecture text when the implementation has diverged.

**Situation:** The user wants `architecture.md` brought in line with the component—after edits elsewhere or as a periodic reconciliation.

**Request examples:** update architecture.md, update architecture file, synchronize architecture, sync architecture, bring architecture up to date, reconcile architecture with implementation, align architecture with the component, refresh architecture status, architecture drift

**Actions:** run `workflows/sync-architecture-rulebook.md`

**Expected Result:** `architecture.md` (at component root or in `architecture/`) carries correct **Implementation status** lines (**FUTURE**, **IN PROGRESS**, **IMPLEMENTED**) per major design unit, and narrative updated where the live package is the source of truth.
