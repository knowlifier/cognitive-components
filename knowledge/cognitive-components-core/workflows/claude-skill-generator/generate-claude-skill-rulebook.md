# Generate Claude Skill (Cognitive Component)

**Purpose:** Create a valid [Claude skill](https://claude.com/blog/skills) inside a Cognitive Component so the skill is self-contained and aligned with Anthropic’s official skill requirements.

**Authoritative spec:** Read and follow `workflows/claude-skill-generator/claude-official-guide/The-Complete-Guide-to-Building-Skills-for-Claude.md` (paths relative to the **cognitive-components-core** package root, or copy the same guide into your package if you maintain a fork).

---

## Scope and placement

- **Component root:** The folder of the target Cognitive Component—the directory that contains that component’s `concepts/`, `data/`, and typically `agent/` (and may live under `knowledge/<component-name>/` in a project).
- **Skill output root:** `{component-root}/workflows/skills/[skill-kebab-name]/`

Every generated skill must be a **valid Claude skill folder**:

- `SKILL.md`: Markdown with YAML frontmatter
- `scripts/`: Executable helpers (optional)
- `references/`: Docs loaded on demand (optional) — a good place for summaries of concepts, definitions, and principles, and for taxonomies required for correct skill operation.
- `assets/`: Templates and static files for outputs — a good place for templates and data-structure declarations such as `Cognitive Classes` and `SubjectDetails` files.

**Naming:** Skill folder name and YAML `name` field: **kebab-case**, lowercase, no spaces (see *Reference B: YAML Frontmatter* and *Reference A: Quick Checklist* in the official guide).

---

## Operating assumption (critical)

Treat the skill as **the only context** at execution time: the runtime must not rely on reading `concepts/` or `data/` **outside** `{component-root}/workflows/skills/[skill-kebab-name]/`.

Therefore:

1. **Inventory** the component’s `concepts/` and `data/` (recursively) for anything relevant to the user-described operation.
2. **Decide** for each relevant file or excerpt:
   - **Inline in `SKILL.md`:** Short, always-needed rules, triggers, and step lists (keep the body lean; prefer progressive disclosure).
   - **`references/`:** Longer definitions, taxonomies, glossaries, principle text, or multi-page guides. Use clear filenames and link them from `SKILL.md` so the model loads them when needed.
   - **`assets/`:** Templates, examples, checklists, data structures (`Cognitive Classes`, `SubjectDetails`) as files the skill applies or fills (e.g. `.md`, `.json`, `.yaml` templates), or non-text resources the skill instructions say to use.
3. **Copy** (do not merely link to paths outside the skill folder) any material the skill needs. If you summarize instead of copying, the summary in `SKILL.md` or `references/` must be **sufficient on its own** to perform the job.
4. **Do not** assume access to the rest of the repo, IKNOW, or other components unless the user explicitly asks to document that dependency in the skill.
5. The only allowed reference to the original Cognitive Component is a short note in the metadata section of `SKILL.md`, in this format: "This Skill was generated based on the knowledge of [cognitive-component-name] (see https://github.com/knowlifier/cognitive-components)." Do not assume this name or link is read or used by skill execution; it only documents that the skill was generated, not handwritten.

---

## Inputs

- **User request:** The specific operation or workflow the skill should implement (e.g. “apply our taxonomy extension rules when classifying files”).
- **Target component root:** Confirmed path to the Cognitive Component being equipped with the skill.
- **Specific or general skill decision:** From the input, determine whether the skill will cover the entire Cognitive Component or only part of it (e.g. several specific jobs).

If the component root is ambiguous (multiple `knowledge/*` candidates), **stop and ask** which component to use before writing files.

When updating one of several existing skills in a single Cognitive Component, be sure you know which skill to update (or several at once). If a critical change could follow from an ambiguous request, **stop and clarify** before writing files.

---

## Procedure

### 1. Clarify the job (if needed)

From the user description, extract:

- Primary **trigger** phrases and scenarios (for YAML `description`).
- **Success criteria** and outputs.
- Whether **scripts** are appropriate (deterministic checks, format validation) per the guide’s “Instructions not followed” and scripting patterns.

### 2. Study the official guide

Apply:

- **Progressive disclosure:** frontmatter = when to use; body = full procedure; `references/` / `assets/` = deep detail.
- **Composability:** Skill instructions must not assume this skill is the only one in use, unless the user asks otherwise.
- **Portability:** No instructions that only work in a single IDE unless the user asked for that scope.
- **Checklists:** *Reference A* before finalizing.

Forbidden in YAML per guide: XML angle brackets in frontmatter; reserved `name` prefixes `claude` / `anthropic`.

### 3. Analyze `concepts/` and `data/`

- Map which definitions, principles, taxonomies, or data files **directly** apply to the operation.
- Exclude irrelevant material to avoid bloating the skill.
- For large sources: prefer **one topic per reference file** and link from `SKILL.md` with short “when to read” cues.

### 4. Author `SKILL.md`

- **YAML frontmatter** (required fields from *Reference B*):

  ```yaml
  ---
  name: <kebab-case-skill-name>
  description: <what it does and when to use it; include concrete trigger phrases>
  ---
  ```

- **Body (Markdown):**
  - `##` sections for workflow steps, validation gates, and examples.
  - Explicit pointers: “Read `references/…` before step X” when appropriate.
  - If MCP or tools are involved, name tools and failure modes per the guide’s MCP section.
  - Keep the main body **concise**; move long material to `references/`.

### 5. Populate `references/`, `assets/`, and `scripts/`

Apply the same three-way split as in **Operating assumption**: only short, always-needed material stays in `SKILL.md`; everything else is copied or adapted here.

- **`references/`:** Bundle **longer definitions, taxonomies, glossaries, principle text, and multi-page guides**—including summaries of concepts, definitions, and principles required for correct operation (see **Scope and placement**). Paste or adapt excerpts from `concepts/` / `data/` so each file is readable without the original tree; **copy**, do not point outside the skill folder. Preserve headings where useful; add `<!-- Source: concepts/... -->` at the top when helpful. If you summarize instead of copying, the text must still be **sufficient on its own** (per **Operating assumption** above).
- **`assets/`:** Put **templates, examples, checklists**, and **data-structure declarations** the skill applies or fills (e.g. Cognitive Classes, `SubjectDetails`), plus static files or non-text resources the instructions name for output—aligned with **Scope and placement** and the `assets/` bullet under **Operating assumption**. These are files to copy, fill, or hand to users—not narrative documentation (that belongs in `references/`).
- **`scripts/`:** Add only when **deterministic checks** or **format validation** (see step 1 *Clarify the job* and the official guide’s scripting / “instructions not followed” guidance) improve reliability beyond prose alone.

### 6. Validate

Run through **Reference A: Quick Checklist** in the official guide (folder name, `SKILL.md` spelling, frontmatter delimiters, `name`/`description` rules, no `<` `>` in YAML, links to references, examples, error handling).

### 7. Deliver

- Create or update files only under `{component-root}/workflows/skills/[skill-kebab-name]/`.
- Summarize for the user: skill name, purpose, trigger phrases, and what was copied into `references/` vs `assets/` vs inlined.

---

## Expected result

- A **valid**, **self-contained** Claude skill at `{component-root}/workflows/skills/[skill-kebab-name]/` that implements the user-described operation using the component’s knowledge, with **no dependency** on reading `concepts/` or `data/` outside that skill folder.

---

## Related paths (cognitive-components-core)

- Official guide (bundled): `workflows/claude-skill-generator/claude-official-guide/The-Complete-Guide-to-Building-Skills-for-Claude.md`
- This rulebook: `workflows/claude-skill-generator/generate-claude-skill-rulebook.md`
