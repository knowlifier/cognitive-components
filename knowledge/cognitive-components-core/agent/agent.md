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
