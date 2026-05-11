# Create cognitive component elements from `architecture.md`

**Purpose:** After the user has **reviewed** `architecture.md` (from the **create-package** flow, **create-component** flow, or manual authoring), populate **`concepts/`**, **`data/`**, **`workflows/`**, and **`agent/`** for segments where there is enough specification. Update **Implementation status** lines in `architecture.md` from **FUTURE** toward **IN PROGRESS** or **IMPLEMENTED** as files appear. Surface remaining gaps.

**Prerequisite:** `{component-root}/architecture.md` or `{component-root}/architecture/architecture.md` exists.

**Related:** `workflows/sync-architecture-rulebook.md` for reconciling architecture with an already-mature tree.

---

## Scope

- **Target:** One component root (`knowledge/<component-name>/` or equivalent).
- **Sources of truth:** Reviewed `architecture.md` + user messages in this session (corrections to architecture).

If component root or architecture path is ambiguous, **stop and ask**.

---

## Procedure

### 1. Load architecture

- Resolve which file to edit and treat as canonical:
  - If **both** `{component-root}/architecture.md` and `{component-root}/architecture/architecture.md` exist, use **`architecture/architecture.md`** as canonical; **tell the user** there is also `{component-root}/architecture.md` and do not silently duplicate content—prefer consolidating or aligning with explicit user guidance.
  - Else if `{component-root}/architecture/architecture.md` exists, use it.
  - Else use `{component-root}/architecture.md`.
- Read the chosen file.
- Build a list of **segments** (headings / blocks) with their **Implementation status** and intended deliverables.

### 2. Obey user intent

- Implement only segments the user asked for (e.g. “do section 2 and 3”) unless they say **implement everything ready**.
- If the user changed `architecture.md` since last run, re-read the file before writing.

### 3. Map segments to artifacts

For each segment with sufficient detail:

| Intent in architecture | Typical outputs |
| ---------------------- | --------------- |
| Vocabulary / definitions | `concepts/definitions/*.md`, glossary entries |
| Policies / rules | `concepts/principles/*.md` |
| Classification | `concepts/taxonomies/*.md`, optional `concepts/classes/*.md` |
| Data shapes / examples | `concepts/templates/*`, `data/*` (start small) |
| Processes / automation | `workflows/**/*.md` rulebooks or blueprints |
| Agent routing | `agent/agent.md` jobs or pointers to workflows |

Create **minimal viable** files first; avoid over-building segments still marked **blocked** in your gap list.

### 4. Update `architecture.md`

- For each segment you materially implemented, set `**Implementation status:**` to **IMPLEMENTED** (fully aligned) or **IN PROGRESS** (partial).
- Leave **FUTURE** where nothing was created.
- If the architecture **explicitly limits** a segment (e.g. definitions-only, no workflows in scope), **IMPLEMENTED** once that stated scope is satisfied—do not require artifacts the section excludes.
- If implementation **diverged** from the written plan, edit the narrative to match, then **IMPLEMENTED** (same rule as sync-architecture).

### 5. Deliver

Return:

- Files created or updated (paths).
- Segment → new status table.
- **Still FUTURE** or **blocked** segments with **concrete questions** for the user.

---

## Expected result

- Component tree gains real content aligned with reviewed architecture.
- `architecture.md` reflects honest **FUTURE** / **IN PROGRESS** / **IMPLEMENTED** markers for each segment touched.
