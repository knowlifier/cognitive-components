# Create a new Cognitive Component (architecture-first within an existing package)

**Purpose:** Add a **new** `Cognitive Component` folder under an existing Cognitive Package layout (typically `knowledge/<component-name>/` next to sibling components). Create the minimal tree (`agent/`, `concepts/`, `data/`, `workflows/`). When the user describes **purpose and intent**, do **not** fully populate concepts, data, and workflows in the same turn—analyze complexity, author `architecture.md` with **FUTURE** segments, gap-analyze, and hand off for review. After review, element creation runs `workflows/implement-component-from-architecture-rulebook.md`.

**Authoritative layout:** `concepts/definitions/component-package-project-definitions.md` (paths relative to the **cognitive-components-core** package root).

---

## Layout to create

**Component root** `{component-root}/` (often `…/knowledge/<component-name>/`):

Minimal directories:

- `agent/` — optional placeholder or minimal `agent.md` stub
- `concepts/` — empty subfolders optional; **do not** add many definition files on the purpose-driven first pass unless the user explicitly asks to implement everything immediately
- `data/` — empty or `.gitkeep` only
- `workflows/` — empty or `.gitkeep` only

Adjust if the user’s repo uses an allowed variant location; stay consistent with `component-package-project-definitions.md`.

**Do not** create a new package-level `cognitive-package.json` or package `agent/` unless the user asked to scaffold an entire package (use `workflows/create-cognitive-package-rulebook.md` for that).

---

## Architecture-first path (when the request describes component purpose)

**Trigger:** The user explains what the component is for, how it should behave, or how it relates to other components—even without saying “architecture.”

**Do not** immediately create rich `concepts/`, workflow rulebooks, or substantial `data/` in that same session unless the user explicitly says to **implement everything now** or **skip architecture.**

### 1. Simple vs complex → where to put `architecture.md`

| Placement | Use when |
| --------- | -------- |
| `{component-root}/architecture.md` | **Simple:** narrow scope, one main workflow theme, few external dependencies, no expectation of many ADRs or subsystem layers. |
| `{component-root}/architecture/architecture.md` | **Complex:** multiple subsystems, heavy interaction with other components or packages, branching taxonomies/workflows, or the user signals long-lived design decisions (ADRs later under `architecture/`). |

If both interpretations fit, prefer **`architecture/`** when the user names more than two distinct concerns (e.g. ingestion + classification + publishing).

### 2. Author `architecture.md`

Include:

- **Purpose** of the component (what problem it solves).
- **Main ideas** to implement (logical segments—headings or bullet groups).
- **Cooperation** with other components or packages (interfaces, shared glossaries, dependencies)—mark uncertainty when speculative.
- For **each logical segment**, add: `**Implementation status:** FUTURE` (not yet realized in the tree; eligible to implement soon after review).

Keep segment boundaries clear so **Create cognitive component elements from `architecture.md`** can map them to files.

### 3. Gap analysis (required in the assistant reply)

Deliver:

- **Ready:** Segments where the user gave enough detail to draft definitions, templates, workflows, or sample data.
- **Blocked / thin:** Segments needing more detail—**specific questions** (entities, edge cases, taxonomy depth, integration contracts, etc.).

### 4. User handoff

- Ask the user to **review** `architecture.md`.
- Explain they can ask you to **continue** so you create elements only for **ready** segments once they have reviewed the file.
- Point to **`workflows/implement-component-from-architecture-rulebook.md`** for that follow-up.

---

## Structure-only fast path

If the user wants only folders (no described purpose):

- Create the minimal `agent/`, `concepts/`, `data/`, `workflows/` tree.
- Omit `architecture.md` or add a one-line stub; say in the summary that purpose can drive a full architecture pass next.

---

## After “continue” or “implement from architecture”

Do **not** extend this rulebook ad hoc—**run** `workflows/implement-component-from-architecture-rulebook.md` for the target `{component-root}`.

---

## Preconditions

- Resolve `{component-root}` (component name/path). If ambiguous among `knowledge/*` siblings, **stop and ask**.
- Respect project `IKNOW.md` **WRITE** paths; do not place components outside allowed trees without explicit user confirmation.

---

## Expected result

- New **`{component-root}/`** with minimal **`agent/`**, **`concepts/`**, **`data/`**, **`workflows/`**.
- When purpose was given: **`architecture.md`** at the chosen path, segments marked **FUTURE**, gap analysis (**Ready** / **Blocked**), and clear pointer to implement-from-architecture after user review.
