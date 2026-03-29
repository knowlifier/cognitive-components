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


## Agent Jobs

### Create a new Cognitive Package

**Description**: Creates new folder and file structure for a new Cognitive Package.

**Situation**: The user wants to initiate a new project with `Cognitive Components` or create a new package.

**Request examples**: create a new cognitive package, create a new cognipackage, create copackage

**Actions**: run `workflows/create-cognitive-package-rulebook.md`

**Expected Result**: created folders and files of the new `Cogntitive Package`

---

### Create a new Cognitive Component

**Description**: Creates new folder and file structures for a new Cognitive Component. 

**Situation**: The user wants to create a new component inside a `Cogntitive Package`.

**Request examples**: create a cognitive component, create new cocoponent, add new cogniponent to the package

**Actions**: выполнить `workflows/create-cognitive-component-rulebook.md`

**Expected Result**: created folders and files of the new `Cognitive Component`.

---

### Modify or refactor an existing Cognitive Package

---

### Modify or refactor an existing Cognitive Component

---

### Create or modify a Cognitive Agent (an agent.md file inside the package)

---

### Generate a Claude skill for a Cognitive Component

**Description:** Creates a valid Claude skill under the component’s `workflows/skills/[skill-kebab-name]/`, following Anthropic’s skill guide and bundling only what lives inside that skill folder (self-contained).

**Situation:** The user wants a reusable Claude skill for a specific operation grounded in the component’s concepts and data, typically for use with Claude Code / Claude.ai skills.

**Request examples:** generate a Claude skill, create a skill for this component, add a skill under workflows/skills, skill for taxonomy workflow, build agent skill from our definitions

**Actions:** run `workflows/claude-skill-generator/generate-claude-skill-rulebook.md`

**Expected Result:** A skill folder at `{component-root}/workflows/skills/[<]skill-kebab-name]/` with `SKILL.md` and optional `references/`, `assets/`, `scripts/` per the official guide; knowledge needed at runtime copied or summarized into that skill tree.
