# Cognitive Component, Package and Project Definitions
 
---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#good-enough`

---

- [Cognitive Component, Package and Project Definitions](#cognitive-component-package-and-project-definitions)
  - [Key Concepts](#key-concepts)
    - [Cognitive Component](#cognitive-component)
    - [Cognitive Component Structure](#cognitive-component-structure)
      - [`agent.md` file (in component root or inside `agent/` folder)](#agentmd-file-in-component-root-or-inside-agent-folder)
      - [`concepts/` folder](#concepts-folder)
        - [`classes/` folder](#classes-folder)
        - [`definitions/` folder](#definitions-folder)
        - [`principles/` folder](#principles-folder)
        - [`taxonomies/` folder](#taxonomies-folder)
        - [`templates/` folder](#templates-folder)
      - [`data/` folder](#data-folder)
      - [`workflows/` folder](#workflows-folder)
    - [Cognitive Component Package](#cognitive-component-package)
      - [`cognitive-package.json` file](#cognitive-packagejson-file)
      - [`knowledge/` folder (contains components)](#knowledge-folder-contains-components)
      - [`agent.md` file (in component root or inside `agent/` folder)](#agentmd-file-in-component-root-or-inside-agent-folder-1)
      - [`glossary.md` file](#glossarymd-file)
      - [`bin/` folder (optional)](#bin-folder-optional)
      - [`script/` folder (optional)](#script-folder-optional)
      - [Package cohesion](#package-cohesion)
    - [Cognitive Project](#cognitive-project)
      - [`cognitive-package.json` file (project root)](#cognitive-packagejson-file-project-root)
  - [How It Works Together](#how-it-works-together)
  - [Examples](#examples)
    - [Support Ticket Routing Example](#support-ticket-routing-example)
    - [E-Commerce Website Project Example](#e-commerce-website-project-example)
  - [Related concepts](#related-concepts)

---

**Cognitive Components** are structured modules of knowledge that combine formal definitions, data catalogs, and process descriptions to enable intelligent automation and decision-making.

Users can add `Cognitive Components` into their `Projects` and enjoy the advantages of modular development of human- and AI-readable and executable components.

Advanced users may develop, reuse and distribute their own `Cognitive Components Packages` for different subject domains.


---

## Key Concepts

### Cognitive Component

A `Cognitive Component` is a self-contained module that organizes knowledge through three interconnected layers:

1. **Concepts** — Formal definitions and taxonomies that establish the vocabulary and structure.
2. **Data** — Actual catalogs and instances of entities defined in the concepts layer.
3. **Workflows** — Workflows and automation rules that operate on the concepts and data.

Together, these layers create a complete cognitive system where formal definitions enable structured data management, and processes provide the operational framework for intelligent automation.

---

### Cognitive Component Structure

A `Cognitive Component` follows this structure:

```
{cognitive-component-root}/
├── agent/                      # or agent.md at root for simple components
│   ├── agent.md
│   ├── [scenario-rulebook].md
│   └── [scenario-rulebook].md
├── concepts/
│   ├── classes/
│   │   ├── [ClassName].md
│   │   ├── [ClassName].md
│   │   └── [ClassName].md
│   ├── definitions/
│   │   ├── [terms-and-concepts]-definitions.md
│   │   ├── [terms-and-concepts]-definitions.md
│   │   └── [terms-and-concepts]-definitions.md
│   ├── principles/
│   │   ├── [operation]-principles.md
│   │   └── [operation]-principles.md
│   ├── taxonomies/
│   │   ├── [taxonomy-name]-taxonomy.md
│   │   └── [taxonomy-name]-taxonomy.md
│   └── templates/
│       ├── [category-name]-template.md
│       └── [category-name]-template.md
├── data/
│   ├── [entity-catalog]/
│   │   ├── [entity]-taxonomy.md
│   │   └── [entity-folder]/
│   ├── [entity-catalog]/
│   │   ├── [entity]-taxonomy.md
│   │   └── [entity-folder]/
│   └── [entity-catalog]/
└── workflows/
    ├── [group-of-processes-1]/
    │   ├── [process]-blueprint.md
    │   ├── [rulebook].md
    │   └── [rulebook].md
    ├── [group-of-processes-2]/
    │   ├── [rulebook].md
    │   └── [rulebook].md
    └── [group-of-processes-3]/
        ├── [process]-blueprint.md
        ├── [rulebook].md
        └── [rulebook].md
```

#### `agent.md` file (in component root or inside `agent/` folder)

`agent.md` is a `Rulebook` that describes the intelligent `Component Agent` with the list of jobs to be done on top of this `Cognitive Component`, given the knowledge described in the package. 

The main agent file `agent.md` serves as the primary `Rulebook`. Additional markdown `Rulebooks` may specify specific scenarios (jobs), which are called from the main `agent.md` file. 

**Where should the `agent.md` file be stored?**
- Put `agent.md` directly in the component root folder `{cognitive-component-root}/agent.md` if the component is simple and does not require multiple complex jobs.
- Create an `agent/` folder and put `agent.md` inside; if the logic is complex and has additional `Rulebooks` for separate jobs, put these rulebooks in the `agent/` folder.

#### `concepts/` folder

Contains optional subfolders (some folders may not be present if there is no class, definition, principle, or taxonomy introduced in this component):

##### `classes/` folder

`Class` files formally describe `Cognitive Classes` that set the data structure of `Entities` used in this component and the package. Each file is named after the class it defines (e.g., `Product.md`, `Customer.md`). Classes define the structure, properties, features, and traits of entities.

##### `definitions/` folder

`Definition` files explain terms and concepts and how they work together. The most important concepts are defined in separate files. These files describe the terms, their relationships, and classifications; they can provide short examples or refer to separate files such as principles or taxonomies. Definitions may also refer to the concepts defined in external `Cognitive Packages` by using, extending, or even redefining some concepts. The important terms shall be added to the `glossary.md` file in the root folder of the `Cognitive Package` or the `Project`.

##### `principles/` folder

`Principle` files define important guidelines, ways, or policies of operating and reasoning that should be taken into account when running the workflows of the `Cognitive Package` or the project. Use these files to calibrate the priorities and resolve ambiguous or risky situations. It is advised to reference files in the `principles/` folder from within the `Rulebooks` to guide proper behavior.

##### `taxonomies/` folder

`Taxonomy` files define common hierarchies of `Categories`. `Taxonomies` organize `Entities` into classification systems that enable categorization and intelligent routing.

##### `templates/` folder

`Template` files provide templates and examples of data structures that `Agents` and `Rulebooks` may use when processing files and data.

#### `data/` folder

Contains actual catalogs of `Entities` of the specified `Cognitive Classes`. For example:
- Product catalog (instances of the `Product` class)
- Customer catalog (instances of the `Customer` class)
- Any other entities declared in the `classes/` folder

Subfolders may also contain any structured or unstructured data that may be used by `Rulebooks` in their processing.

#### `workflows/` folder

Contains folders grouping workflows around specific knowledge domains or categories. 
These folders may contain:
- **Process Blueprints** — Documents describing supervised processes with steps, participants, and criteria.
- **Rulebooks** — Automated procedures that can be invoked within process steps. Rulebook files are typically placed directly in the workflow folder (e.g., `[rulebook].md`). When a workflow group contains many rulebooks, they may optionally be nested in a `rulebooks/` subfolder.

---

### Cognitive Component Package

A `Cognitive Component Package` (or, for short, a `Cognitive Package`) is a collection of related `Cognitive Components` that share a common vocabulary and can interoperate. It provides a unified entry point and a shared glossary for all components within the package.

A `Cognitive Component Package` follows this structure:

```
{cognitive-package-root}/
├── cognitive-package.json
├── glossary.md
├── agent/                      # or agent.md directly at package root
│   └── agent.md
└── knowledge/
    ├── [component-name-1]/
    │   ├── agent/
    │   ├── concepts/
    │   ├── data/
    │   └── workflows/
    ├── [component-name-2]/
    │   ├── agent/
    │   ├── concepts/
    │   ├── data/
    │   └── workflows/
    └── [component-name-N]/
```

#### `cognitive-package.json` file

Located at `{package-root}/cognitive-package.json`. Contains bindings and metadata for the package.

**Identity and version**

- **`name`** — Package identifier (kebab-case recommended). Used when the package is referenced as a dependency.
- **`version`** — Semantic version (e.g., `1.0.0`). Enables version-aware resolution when the package is used as a dependency.
- **`description`** — (optional) Short description of the package purpose and scope.
- **`author`** — (optional) Author or maintainer.

**Dependencies**

- **`dependencies`** — (optional) List of Cognitive Packages this package depends on. Each entry may be a package name (when resolved from `.knowlib/`) or a path. Dependencies are loaded before this package; their concepts, taxonomies, and rulebooks become available for reference and extension.

**Taxonomy resolution**

- **`taxonomy_resolution`** — (optional) Maps base taxonomy names to their effective implementations (extended or aggregated taxonomies). When a base taxonomy (e.g., `ProductTaxonomy`) is referenced in classes, rulebooks, or templates, the system resolves it to the path specified here. Paths are relative to the package root. See the [Taxonomy Extension Principle](../principles/taxonomy-extension-princilpe.md) for details.

**Example**

```json
{
  "name": "ecommerce-catalog-package",
  "version": "2.1.0",
  "description": "Product catalog with search, comparison, and recommendation workflows",
  "author": "ShopFlow Inc",
  "dependencies": [
    "ecommerce-product-core",
    "food-catalog-component",
    "electronics-catalog-component"
  ],
  "taxonomy_resolution": {
    "ProductTaxonomy": "knowledge/full-website-catalog/knowledge/full-website-catalog/concepts/taxonomies/full-website-product-taxonomy.md"
  }
}
```

#### `knowledge/` folder (contains components)

Contains the `Cognitive Components` that belong to the package. Each subfolder is a self-contained component with its own `agent/`, `concepts/`, `data/`, and `workflows/` structure.

#### `agent.md` file (in component root or inside `agent/` folder)

A centralized `agent.md` file for the entire package. This package-level agent can route incoming requests to individual components within the package, acting as a single entry point for external callers. 

Can be placed in the root folder of the package `{package-root}` or inside `{package-root}/agent/` folder.

#### `glossary.md` file

Located in the package root, this file lists all public classes, concepts, and principles exposed by the package. When a user adds the package to their project library, the knowledge described in `glossary.md` becomes available to agents. Important terms from each component's definitions should be added to this file.

#### `bin/` folder (optional)

Contains binary files required for execution of `Rulebooks` of this package.

#### `script/` folder (optional)

Contains bash, Python, etc. scripts required for execution of `Rulebooks` of this package.


#### Package cohesion

All components within a package have access to each other. They share and consistently use the same terms, concepts, and principles described in `glossary.md`, and can reference each other's data and rulebooks.

---

### Cognitive Project 

A `Cognitive Project` is a project that uses Cognitive Components. 
Your project becomes a `Cognitive Project` when it has both: 
- an `IKNOW.md` file at the project root that defines access permissions to Cognitive Components
- at least one of the following: a `Cognitive Package` in the `.knowlib/` folder (as a dependency) or one or more `Cognitive Components` in the `knowledge/` folder (developed or customized within the project).

The Project has approximately the same structure as a Cognitive Package:

```
{project-root}/
├── cognitive-package.json
├── .knowlib/
│   ├── [package-name-1]/
│   ├── [package-name-2]/
│   └── [package-name-N]/
├── glossary.md
├── IKNOW.md
├── AGENTS.md
├── agent/
│   └── agent.md
├── knowledge/
│   ├── [component-name-1]/
│   │   ├── agent/
│   │   ├── concepts/
│   │   ├── data/
│   │   └── workflows/
│   ├── [component-name-2]/
│   │   ├── agent/
│   │   ├── concepts/
│   │   ├── data/
│   │   └── workflows/
│   └── [component-name-N]/
└── [project-specific folders]
```

The `Project` contains a `knowledge/` folder where components developed and modified within the project reside (e.g., concepts, terms, and cognitive business logic for the website or application being built).

The `Project` may maintain its own `glossary.md` at the project root to aggregate or extend terms from multiple packages.

It may also have an `agent/` folder with `agent.md` if a single entry point for agentic requests is needed in design-time or run-time.

Additional project folders and files:

#### `cognitive-package.json` file (project root)

Located at `{project-root}/cognitive-package.json`. Same format as in a Cognitive Package: contains bindings and metadata, including `taxonomy_resolution` for mapping base taxonomies to their effective (extended or aggregated) implementations. When the project uses taxonomy extension, references to base taxonomies resolve to the paths specified here.

- **`.knowlib/` folder** — stores library cognitive packages (packages used as dependencies that are not intended to be modified).

- **`IKNOW.md` file** — defines links and access permissions to project packages in design-time (when the developer designs and edits component files, e.g., product data structures or recommendation logic) and in run-time (when component files are read-only but used as instructions and prompts for cognitive workflows and user data processing, e.g., orders on a website). The developer should edit this file to configure which components are accessible in design-time and run-time.

- **`AGENTS.md` file** — describes the logic for handling user requests in design-time, enabling Cognitive Components to act as Tools, Skills, and Subagents that extend IDE capabilities, and in run-time when custom agent invocation logic is required. Since different agentic platforms also use `AGENTS.md` for their settings, the developer should merge this file with existing platform configurations so that instructions do not conflict.

- **`.cursor/rules/knowlifier-agents.mdc`** — (optional) an optional file for Cursor IDE users. It contains instructions to use `Cognitive Components` in the `Project` as a primary source of knowledge, redirects to `IKNOW.md` (which accesses the `Cognitive Components` in the `Project` and requests their help in processing user commands). This file should contain:

  ```
  ---
  alwaysApply: true
  ---

  **Trigger**: Every user request. Execute immediately, before any response.

  **Actions**: run `IKNOW.md`
  ```

- **Scripts and binary files** — folders and files typical for the programming language and frameworks used by the developer (e.g., project and library folders for Python, Node.js, etc.).

---

## How It Works Together

1. `Cognitive Component` is the container for structured knowledge for a specific domain.
2. `Definitions` introduce common language and terms, explain how concepts relate and operate.
3. `Classes` define the data structure of `Entities` that will be used in `Workflows`.
4. `Taxonomies` organize `Entities` into `Categories`, allowing `Rulebooks` to recognize entities and apply rules appropriate to their type.
5. `Principles` provide guidance to `Rulebooks`, introducing centralized prompts and instructions.
6. `Templates` help `Rulebooks` to format data in the right structures while processing.
7. The `data/` folder stores `Entities` and other supplementary data required for `Rulebooks` to process.
8. `Workflows` describe the logic of the `Cognitive Component` and define actions in the form of `Rulebooks`.
9. `Cognitive Package` organizes several `Cognitive Components` into one space with shared knowledge.
10. `Glossary` collects all important links to the terms, `Classes`, `Taxonomies`, and `Principles` across the `Cognitive Package`, providing this knowledge for external processes and users who use the package.
11. `Agent` is a centralized agentic AI entry point and routing layer allowing external processes to call and run `Rulebooks` of the entire `Cognitive Package` or individual `Cognitive Components` inside the package through requests.
12. A user's `Project` can be written in their preferred language and frameworks. To use Cognitive Components, the user may add the `.knowlib/` folder (containing Cognitive Packages) and `IKNOW.md` to the project root, and merge `AGENTS.md` from the Cognitive Components repository.
13. Users can create their own `Cognitive Components` in the `knowledge/` folder of their `Project`.
14. Any such `Project` can be saved as a `Cognitive Package` and reused or distributed as a cognitive library without changing the folder structure.

---

## Examples

### Support Ticket Routing Example

This is a Cognitive Project for a support team that handles customer tickets. All components are developed within the project; no external library packages are used.

**Project Structure:**
```
SupportTeamProject/
├── IKNOW.md
├── agent.md
├── glossary.md
└── knowledge/
    ├── company-information/                      # component containing information about the company
    │   ├── concepts/
    │   │   └── taxonomies/
    │   │       └── product-taxonomy.md           # categories of products
    │   ├── data/
    │   │   ├── about-the-company.md              # company mission, values, history
    │   │   └── product-overview.md               # product features
    │   └── workflows/
    │       └── product-lookup-rulebook.md        # find relevant product information for a ticket
    ├── customer-support/                         # component containing process knowledge for support agents  
    │   ├── concepts/
    │   │   ├── definitions/
    │   │   │   └── customer-support-definitions.md
    │   │   ├── principles/
    │   │   │   ├── escalation-principles.md      # when and how to escalate
    │   │   │   ├── refund-policy-principles.md   # refund policy
    │   │   │   └── sla-principles.md             # SLA rules and policies for this team
    │   │   └── taxonomies/
    │   │       ├── faq-taxonomy.md               # catalog of frequently asked questions
    │   │       ├── common-issues-taxonomy.md     # catalog of known issues & workarounds
    │   │       └── support-tier-benefits.md      # what each tier gets (response time, channels)
    │   ├── data/
    │   └── workflows/
    │       └── non-standard-issues-rulebook.md   # rules for non-standard issues not found in common-issues-taxonomy.md
    └── ticket-routing/                           # component for ticket handling
        ├── agent.md
        ├── concepts/
        │   ├── classes/
        │   │   └── Ticket.md
        │   ├── definitions/
        │   │   └── ticket-routing-definitions.md
        │   └── taxonomies/
        │       └── request-taxonomy.md           # new order, complaint, inquiry
        ├── data/
        └── workflows/
            ├── routing-rulebook.md
            └── escalation-rulebook.md
```

**Project components** (`knowledge/`). Components developed within this project:

- **company-information** — Component containing company and product knowledge for support agents. Contains product taxonomy, data files (company overview, product features), and `product-lookup-rulebook` to find relevant product information when handling tickets.
- **customer-support** — Component containing process knowledge for support agents. Defines customer-support concepts, principles (escalation, refund policy, SLA rules), and catalogs structured in the form of taxonomies (frequently asked questions, common issues and workarounds, support-tier benefits). The `non-standard-issues-rulebook` handles cases not covered by the common-issues taxonomy.
- **ticket-routing** — Component that defines the `Ticket` class and request taxonomy (New Order, Complaint, Inquiry). Contains rulebooks for routing incoming tickets and escalating when needed. References principles and taxonomies from the `customer-support` component.

The project's `IKNOW.md` configures access to these components so agents can route tickets, apply escalation logic, and enforce SLAs.

---

### E-Commerce Website Project Example

This is a Cognitive Project for an e-commerce website that sells food and consumer electronics. The project uses a reusable Cognitive Package from the library and extends it with domain-specific components developed within the project.

**Project Structure:**
```
MyECommerceWebsite/
├── IKNOW.md
├── agent.md
├── glossary.md
├── .knowlib/
│   └── ecommerce-package/
│       ├── glossary.md
│       ├── agent.md
│       └── knowledge/
│           ├── ecommerce-product-catalog/  # base component defining ecommerce products
│           │   ├── concepts/
│           │   │   ├── definitions/
│           │   │   │   └── ecommerce-product-definitions.md
│           │   │   ├── classes/
│           │   │   │   └── Product.md
│           │   │   └── taxonomies/
│           │   │       └── product-taxonomy.md
│           │   ├── data/
│           │   └── workflows/
│           │       ├── product-search-rulebook.md
│           │       ├── product-comparison-rulebook.md
│           │       └── recommendation-rulebook.md
│           └── ecommerce-crm/             # base component defining ecommerce crm logic
│               ├── concepts/
│               │   ├── definitions/
│               │   │   └── ecommerce-customer-definitions.md
│               │   └── classes/
│               │       └── Customer.md
│               ├── data/
│               └── workflows/
│                   ├── new-customer-rulebook.md
│                   └── customer-personalization-rulebook.md
└── knowledge/
    ├── food-catalog/                  # component describing your food products
    │   ├── concepts/
    │   │   └── taxonomies/
    │   │       └── food-product-taxonomy.md
    │   ├── data/
    │   │   └── food-products-database.csv
    │   └── workflows/
    ├── electronics-catalog/           # component describing your consumer electronics products
    │   ├── concepts/
    │   │   └── taxonomies/
    │   │       └── electronics-product-taxonomy.md
    │   ├── data/
    │   │   └── electronics-products-database.csv
    │   └── workflows/
    │       └── electronics-comparison-rulebook.md
    ├── full-website-catalog/          # component that combines all subcatalogs
    │   ├── agent.md
    │   ├── concepts/
    │   │   └── taxonomies/
    │   │       └── full-website-product-taxonomy.md
    │   ├── data/
    │   └── workflows/
    │       └── website-product-search-rulebook.md
    ├── website-crm/                   # component defining the website chatbot scenarios
    │   ├── concepts/
    │   ├── data/
    │   │   └── customers-data-provider.md       
    │   └── workflows/
    └── website-chatbot/               # component defining the website chatbot scenarios
        ├── agent.md
        ├── concepts/
        │   ├── principles/
        │   │   └── how-to-talk-with-customer-principles.md
        │   └── taxonomies/
        │       └── customer-request-taxonomy.md
        ├── data/
        │   └── about-the-company.md
        └── workflows/
            ├── customer-request-routing-rulebook.md
            ├── new-order-rulebook.md
            └── complaint-rulebook.md
```

**Cognitive library package** (`.knowlib/ecommerce-package/`). A reusable Cognitive Package that provides generic e-commerce knowledge. It contains two base components:

- **ecommerce-product-catalog** — Defines the `Product` class, product taxonomy, and generic rulebooks for product search, comparison, and recommendation. This component establishes the shared vocabulary and workflows for product data across any e-commerce site.
- **ecommerce-crm** — Defines the `Customer` class and customer-related workflows (new customer onboarding, personalization). It provides the base CRM logic that projects can extend.

The package exposes a shared `glossary.md` and `agent.md` so external callers can route requests to the appropriate component. Projects add this package as a dependency and do not modify it.

**Project components** (`knowledge/`). Components developed or customized for this specific website:

- **food-catalog** — This component extends the core `ecommerce-product-catalog` component from the library package and contains product taxonomy and data for food items. It extends the base product model with food-specific categories and product data. It has a full list of food products in the `csv` format in the `data/` folder. 
- **electronics-catalog** — This component extends the core `ecommerce-product-catalog` component from the library package and contains product taxonomy and data for consumer electronics, plus an electronics-specific comparison rulebook. It has a full list of consumer electronics products in the `csv` format in the `data/` folder.
- **full-website-catalog** — This component extends the core `ecommerce-product-catalog` component from the library package and combines food and electronics into a unified catalog with a single product search rulebook and entry point via `agent.md`.
- **website-crm** — This component extends the core `ecommerce-crm` component from the library package, contains connectors to custom CRM data and customizes the workflow logic for this website’s customers.
- **website-chatbot** — This component defines the website chatbot behavior: principles for customer communication, request taxonomy (e.g., new order, complaint), and rulebooks for routing, new orders, and complaints. Uses `about-the-company.md` as context data for communications.

The project’s `IKNOW.md` configures access to both the library package and these project components so agents can use the full cognitive stack.

---

## Related concepts

- [Entity](entity-class-definitions.md#entity)
- [Class](entity-class-definitions.md#class)
- [Taxonomy](taxonomy-category-definitions.md#taxonomy)
- [Category](taxonomy-category-definitions.md#category)
