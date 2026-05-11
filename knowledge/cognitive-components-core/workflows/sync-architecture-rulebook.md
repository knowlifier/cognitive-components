# Synchronize `architecture.md` with implementation (Cognitive Component)

**Purpose:** Keep `architecture.md` truthful relative to the component’s knowledge tree. After reviewing `concepts/`, `data/`, and `workflows/` (including templates and package-level structure where relevant), classify each major design idea in `architecture.md` and update the file in the **same change set** as structural edits elsewhere when the user runs this job after changing the component.

**Authoritative layout:** `concepts/definitions/component-package-project-definitions.md` (paths relative to the **cognitive-components-core** package root, or the equivalent section in your fork).

---

## Scope

- **Target:** One `Cognitive Component` root—the directory containing that component’s `concepts/`, `data/`, and typically `workflows/` and `agent/`.
- **Architecture file:** Either `{component-root}/architecture.md` or `{component-root}/architecture/architecture.md`. If both exist, prefer `architecture/architecture.md` and note the duplicate to the user.
- **Review surface (recursive where present):**
  - `concepts/` — `definitions/`, `principles/`, `taxonomies/`, `templates/`, `classes/`
  - `data/`
  - `workflows/` (rulebooks, process blueprints, nested workflow folders)

Do **not** silently change other components; if the user’s scope is unclear, **stop and ask** which component root to use.

---

## Implementation status values

Use exactly these labels when describing the state of a design idea **in `architecture.md`** (per section, subsection, or bullet block—whichever best matches how the document is written):

| Label | When to use |
| ----- | ----------- |
| **FUTURE** | The architecture describes it, but there is no meaningful counterpart in the tree yet (no definitions, data, workflows, or templates that realize it). |
| **IN PROGRESS** | Part of the idea is present (e.g. concepts defined but no workflows, or workflows stubbed but no data); the rest is missing or skeletal. |
| **IMPLEMENTED** | The tree reflects the design: concepts **and** the expected workflows/data/templates/classes coverage exist and align with what the section claims. |

**Explicit scope:** If a section states that it is intentionally limited (e.g. concepts and definitions only, no workflows in scope), treat that section as **IMPLEMENTED** when the tree matches that limited scope—do not require workflows or data that the architecture excludes.

**Drift (architecture wrong, implementation right):** If the **implementation is mature** but **materially differs** from what `architecture.md` says, **rewrite that part of `architecture.md`** so it describes the **current** structure and behavior, then label it **IMPLEMENTED**. Prefer updating the document over leaving stale text with a misleading status.

---

## Status markup convention

Keep the doc readable for humans and agents:

- For each major claim or planned element, add a single line (same block as the description):

  `**Implementation status:** FUTURE` | `IN PROGRESS` | `IMPLEMENTED`

- If the file already uses a similar pattern (e.g. `Status:`), **normalize to** `**Implementation status:** …` within the sections you touch.
- Optional short rationale (one line) is allowed after the status when non-obvious, e.g. why something is **IN PROGRESS**.

---

## Procedure

### 1. Confirm target

- Resolve **component root** path from the user request or working context.
- If multiple candidates exist (e.g. several under `knowledge/*/`), **ask** before editing.

### 2. Load `architecture.md`

- If missing, **stop** and tell the user to create a starter `architecture.md` (see package definitions). Do not invent a full architecture without their intent.
- Parse it into **design units** (sections / subsections / numbered lists)—each unit should receive at most one **Implementation status** line after review.

### 3. Build an evidence inventory (read-only pass)

Summarize what actually exists:

- **Concepts:** List notable files under `concepts/` (definitions, principles, taxonomies, templates, classes).
- **Data:** List folders/files under `data/` and whether they look populated vs placeholder.
- **Workflows:** List workflow folders and rulebooks; note obvious stubs (TODO-only, empty sections).
- **Cross-links:** Note references from `agent/` rulebooks to workflows if they clarify what is “live.”

### 4. Map architecture claims to evidence

For each **design unit** in `architecture.md`:

1. Decide whether the tree contains a **clear counterpart** (same names, same flows, or clearly described entities/workflows).
2. Apply **FUTURE** / **IN PROGRESS** / **IMPLEMENTED** using the table above.
3. If the text **contradicts** the tree and the tree looks intentional and complete, **edit the narrative** to match the tree, then set **IMPLEMENTED** for that unit.

**IN PROGRESS** examples: taxonomy file exists but no data using it; classes defined but empty `data/`; architecture promises a workflow folder that exists with only placeholders.

### 5. Edit `architecture.md`

- Insert or refresh `**Implementation status:** …` for each reviewed unit.
- Remove obsolete status lines if you split or merge sections (avoid duplicate statuses).
- Preserve ADR references in `architecture/`; if a drift implies a **decision change**, mention that a new or updated `-ard.md` may be warranted (do not create ADRs unless the user asks).

### 6. Same change set (maintenance rule)

- If this job is run **because** other files were just changed, ensure `architecture.md` updates are **committed together** with those changes when the user uses version control (communicate this in the summary).

### 7. Deliver

Return a short report:

- Path to `architecture.md` updated
- Table or list: design unit → status (FUTURE / IN PROGRESS / IMPLEMENTED)
- Any sections **rewritten** for drift (one line each)
- Open gaps (what would move **FUTURE** → **IMPLEMENTED** next)

---

## Expected result

- `architecture.md` at the component root (or under `architecture/`) reflects the **current** package: accurate narrative where the implementation matured, and honest **FUTURE** / **IN PROGRESS** / **IMPLEMENTED** markers elsewhere.
