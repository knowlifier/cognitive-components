# Cognitive Components Core Library

A structured knowledge package that defines the format and conventions for **Cognitive Components** — modular, AI-ready knowledge modules that combine formal definitions, data catalogs, and process descriptions for intelligent automation and decision-making.

| Package | Version | Author |
|---------|---------|--------|
| `cognitive-components` | 0.1.0 | Artemy Malkov |

---

## Overview

Cognitive Components are the building blocks of **Knoware** — the structured intelligence layer on top of software. They treat knowledge like a git library: modular, version-controlled, and structured in Markdown. Instead of training an AI on raw data, you "knowlify" domain knowledge into Cognitive Components that both humans and machines can read, edit, and execute.

This package provides the core definitions, taxonomies, principles, and templates needed to create, structure, and operate Cognitive Components and Cognitive Packages.

---

## Core Definitions

### Cognitive Component

A **Cognitive Component** is a self-contained module that organizes knowledge through three interconnected layers:

1. **Concepts** — Formal definitions, classes, taxonomies, and principles that establish vocabulary and structure.
2. **Data** — Actual catalogs and instances of entities defined in the concepts layer.
3. **Workflows** — Process descriptions and rulebooks that operate on concepts and data.

### Cognitive Component Package

A **Cognitive Package** is a collection of related Cognitive Components that share a common vocabulary and can interoperate. It provides a unified entry point (`agent.md`), shared glossary (`glossary.md`), and optional scripts/binaries for execution.

### Cognitive Project

A **Cognitive Project** is any project that uses Cognitive Components. It becomes a Cognitive Project when it has:
- An `IKNOW.md` file defining access permissions to components
- At least one Cognitive Package in `.knowlib/` or one or more Cognitive Components in `knowledge/`
### Entity & Class

- **Entity** — A structured piece of data representing an object (product, person, file, organization, etc.). Entities may contain semi-structured data and support **Typecasting** to map into formats required by business processes.
- **Cognitive Class** — Defines a type of entities united by common characteristics. Classes define data structure (properties) and do not support methods. Located in `concepts/classes/`.
- **Subclass** — A specialized class that inherits from a parent and adds properties for a narrower application area. Supports multiple inheritance.
- **Property** — Individual elements of the class data structure. Properties can be IDENTIFIER, MANDATORY, OPTIONAL, or COMMON. Support types such as text, number, date, url, `SomeClass`, or `#SomeTaxonomy`.

### Taxonomy & Category

- **Taxonomy** — A classification system using a hierarchy of categories to organize items. Taxonomies serve three main use cases:
  1. **Fine-grained class hierarchy** — Group entities by characteristics without creating many subclasses
  2. **Enumeration** — Define fixed sets of options for class properties
  3. **Classification and aggregation** — Organize items for routing, aggregation, and graceful fallback when precise classification is not possible

- **Category** — A node in a taxonomy that adds specific traits. Categories use **category tags** (e.g., `#category-tag`, `#TaxonomyName--category-tag`) and contain descriptions, instructions for rulebooks, and fallback behavior.

### Entity Template & Typecasting

- **Entity Template** — Defines the format for storing entity data and instructions for **Typecasting** — converting unstructured or semi-structured data into the format required by a Cognitive Class. Templates use `[instruction]` or `{instruction}` syntax.
- **Typecasting** — Maps entities (from any source) into the data format defined by a target Cognitive Class, enabling participation in business processes that expect that class.

---

## Package Structure

```
cognitive-components/
├── cognitive-package.json
├── IKNOW.md
├── AGENTS.md
├── README.md
└── knowledge/
    └── cognitive-components-core/
        ├── agent/
        │   └── agent.md
        └── concepts/
            ├── definitions/
            │   ├── component-package-project-definitions.md
            │   ├── entity-class-definitions.md
            │   ├── taxonomy-category-definitions.md
            │   └── data-collection-definitions.md
            ├── principles/
            │   └── incomplete-unstructured-data-handling-principle.md
            ├── taxonomies/
            │   └── FileFidelityTaxonomy.md
            └── templates/
                └── taxonomy-template.md
```

---

## Contents

| Path | Description |
|------|-------------|
| `cognitive-package.json` | Package manifest: name, version, author, dependencies, taxonomy_resolution. |
| `concepts/definitions/component-package-project-definitions.md` | Defines Cognitive Component, Package, and Project structure, folder layout, and how they work together. Includes examples (support ticket routing, e-commerce). |
| `concepts/definitions/entity-class-definitions.md` | Defines Entity, Class, Subclass, Property, Guidelines, Entity Template, Typecasting, and inheritance. |
| `concepts/definitions/taxonomy-category-definitions.md` | Defines Taxonomy, Category, category tags, taxonomy use cases, and how classes and categories work together. |
| `concepts/definitions/data-collection-definitions.md` | Placeholder for Data Collection definitions. |
| `concepts/principles/incomplete-unstructured-data-handling-principle.md` | Principles for handling incomplete or unstructured data. |
| `concepts/taxonomies/FileFidelityTaxonomy.md` | Taxonomy for file fidelity levels (e.g., `#good-enough`, `#zero`). |
| `concepts/templates/taxonomy-template.md` | Template for creating new taxonomies. |
| `agent/agent.md` | Component agent with jobs for creating packages, components, and refactoring. |

---

## Usage

### In Your Project

1. Copy `IKNOW.md` and `AGENTS.md` into your project root.
2. Add this package to `.knowlib/cognitive-components/` (or install via package manager).
3. Configure `IKNOW.md` permissions to grant READ/WRITE/RUN access as needed.
4. Create your own Cognitive Components in `knowledge/` following the structure defined in this package.

### Agent Jobs (Design-Time)

The component agent supports:

- **Create a new Cognitive Package** — Scaffolds folder and file structure for a new package.
- **Create a new Cognitive Component** — Scaffolds structure for a new component inside a package.
- **Modify or refactor** — Existing package or component.
- **Create or modify agent.md** — Package-level agent configuration.

---

## Related

- [IKNOW.md](IKNOW.md) — Permissions and configuration for design-time vs. run-time access.
- [AGENTS.md](AGENTS.md) — Agent operating rules and request routing.
- [Knowlifier](https://github.com/knowlifier) — Open-source initiative for converting human knowledge into Cognitive Components.
