# Create a new Cognitive Package

**Purpose:** Scaffold a new `Cognitive Package`. When the user describes purpose and intent, author an `architecture.md` file with **FUTURE** segments, gap-analyze the request, and hand off to the user for review.

**Authoritative layout:** `concepts/definitions/component-package-project-definitions.md` (paths relative to the **cognitive-components-core** package root).

---

## Package and component layout to create

**Package root** `{cognitive-package-root}/`:

- `cognitive-package.json` — minimal identity (`name`, `version`, optional `description`)
- `glossary.md` — stub (heading + short note to extend as terms are added)
- `agent/agent.md` — stub for package-level routing (optional minimal body)

### Optional package-level `architecture.md`

The package as a whole may also include `{cognitive-package-root}/architecture.md` or `{cognitive-package-root}/architecture/architecture.md` (same simple-vs-complex placement rules as for components) to describe planned sibling cognitive components, cross-component contracts, dependency integration, and how the package evolves. Add or stub package-level architecture when:

- the user names **multiple components** or future modules in the package, or
- **cross-component** integration (data flows, shared taxonomies, routing) **matters** from day one.

For components that do not exist yet, mark the corresponding segments **FUTURE** (using `**Implementation status:** FUTURE` per section), so readers can tell what is planned versus implemented. Keep this file updated as new components land (see **Package Architecture** in `component-package-project-definitions.md`). If the package has only one component for now, you may omit package-level architecture until you add a second component or a major split.

---

## Creating components inside the package (when the request describes components in the package and their purpose)

**Trigger:** The user explains which components are needed, what they are for, how they should behave, or how they relate to other knowledge—even if they did not say “architecture.”

**Do not** immediately create rich `concepts/`, workflow rulebooks, or data files in that same session unless the user explicitly asks to “implement everything now” or “skip architecture.”

Follow `workflows/create-cognitive-component-rulebook.md` to add components. Do not flesh out every part of a component before the user has reviewed the relevant `architecture.md`, unless the component design and content are already specified in detail.

---

## Notifying the user when creation is complete

Once the package and components (if any) are created, notify the user, explaining what you just created and what files are expected to be reviewed.

---

## Expected result

- New package skeleton with `cognitive-package.json`, `glossary.md`, package `agent/`, and `knowledge/<component-name>/` with **`agent/`**, **`concepts/`**, **`data/`**, **`workflows/`** (minimal).
- When purpose was provided: component **`architecture.md`** (or `architecture/architecture.md`) in the chosen location, segments marked **FUTURE**, plus a clear **gap analysis** and invitation to run the **implement-from-architecture** job after review.
- When applicable: package-level **`architecture.md`** (or `architecture/architecture.md`) stubbed or drafted per **Optional package-level `architecture.md`**, with **FUTURE** segments if the user envisions multiple components or cross-component design.
