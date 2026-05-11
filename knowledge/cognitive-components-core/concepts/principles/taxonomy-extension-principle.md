# Taxonomy Inheritance and Extension Principle

---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#good-enough`

---

**Key Principles:**

- [Taxonomy Inheritance and Extension Principle](#taxonomy-inheritance-and-extension-principle)
  - [Principle: Large Taxonomies Rarely Fit Inside a Single Component](#principle-large-taxonomies-rarely-fit-inside-a-single-component)
  - [Principle: Base Taxonomy and Extensions via Downstream Components](#principle-base-taxonomy-and-extensions-via-downstream-components)
    - [How It Works](#how-it-works)
    - [Ways to Extend a Taxonomy](#ways-to-extend-a-taxonomy)
  - [Principle: Taxonomy Polymorphism](#principle-taxonomy-polymorphism)
  - [Examples](#examples)
    - [Base Taxonomy](#base-taxonomy)
    - [Extension (add details in Legal branch only)](#extension-add-details-in-legal-branch-only)
    - [Extension (adding details Marketing branch only)](#extension-adding-details-marketing-branch-only)
    - [Extension (extending the siblings)](#extension-extending-the-siblings)
    - [Aggregator: MyCompanyDocumentTaxonomy](#aggregator-mycompanydocumenttaxonomy)
    - [Project Structure](#project-structure)
    - [cognitive-package.json — Taxonomy Polymorphism Mapping](#cognitive-packagejson--taxonomy-polymorphism-mapping)
    - [Entity Class](#entity-class)
    - [Rulebook Using Extended Taxonomy](#rulebook-using-extended-taxonomy)

---

## Principle: Large Taxonomies Rarely Fit Inside a Single Component

Large taxonomies are rarely contained within a single `Cognitive Component`. Instead, define a base `Class` and base `Taxonomy` inside a foundational `Cognitive Package`, then add downstream components that introduce subclasses of `Class` for specific subtypes and extend the taxonomy with more detailed `Categories`.

---

## Principle: Base Taxonomy and Extensions via Downstream Components

### How It Works

1. **Define a base taxonomy** (e.g., `DocumentTaxonomy`) in a base library with a core category tree.

2. **Add `Cognitive Components` that extend the taxonomy.** Introduce extensions as new taxonomy files (e.g., `LegalDocumentTaxonomy(DocumentTaxonomy)`, `ProductDocumentTaxonomy(DocumentTaxonomy`). In extension files you do not need to reproduce the entire tree of the extended taxonomy—only the branches you are extending. You may reference existing categories by their `Category Tags` and add new categories (tree nodes), or add more detail (instructions, descriptions) to existing categories. When overriding an existing category tag, mark it explicitly as an extension, not a new node.

3. **If multiple components extend the same taxonomy**, create a separate component that merges all extensions into a single taxonomy used by the project. In that component, declare references to all taxonomies you aggregate (base and extensions). 
   
4. **Declare plymorphism** At the package or project level, specify that this aggregator file is the effective extension of the base taxonomy.

5. **Throughout the package** (and projects that use it as a library), when referencing the base taxonomy (e.g., `DocumentTaxonomy`), the system treats it as extended — with more categories and richer descriptions. The actual implementation in the project is the extended taxonomy. This is **taxonomy polymorphism**.

### Ways to Extend a Taxonomy

- **Add new sibling nodes** to the existing tree (e.g., more options in an enumeration taxonomy).
- **Detail existing categories** by adding a subtree of subcategories.
- **Enrich existing categories** with more instructions, descriptions, or clarifications.

---

## Principle: Taxonomy Polymorphism

Files such as `Classes`, `Rulebooks`, and `Templates` may continue to reference the base taxonomy (e.g., `DocumentTaxonomy`). The real implementation in the project is the extended taxonomy. References to the base taxonomy resolve to the extended version configured at the package or project level.

---

## Examples

### Base Taxonomy

```
DocumentTaxonomy : Document
├── #general-document
├── #legal-document
├── #product-document
└── #marketing-document
```

### Extension (add details in Legal branch only)

Extension file adds subcategories without reproducing the full tree:

```
LegalDocumentTaxonomy (DocumentTaxonomy) : Document
└── #legal-document
    ├── #contract
    ├── #claim
    ├── #court-filing
    └── #legal-opinion
```
### Extension (adding details Marketing branch only)

Extension file adds subcategories without reproducing the full tree:

```
MarketingDocumentTaxonomy (DocumentTaxonomy) : Document
└── #marketing-document
    ├── #brochure
    ├── #campaign-brief
    ├── #case-study
    ├── #press-release
    └── #social-media-content
        ├── #post
        ├── #story
        ├── #reel
        └── #ad
```

### Extension (extending the siblings)

```
CoprorateDocumentTaxonomy (DocumentTaxonomy) : Document
├── #technical-document
├── #policy-document
├── #report-document
└── #meeting-minutes
```

### Aggregator: MyCompanyDocumentTaxonomy

Merges base and extensions:

```
MyCompanyDocumentTaxonomy (DocumentTaxonomy) : Document
├── #general-document
├── #legal-document <LegalDocumentTaxonomy>             # extendind legal subcategories
├── #marketing-document <MarketingDocumentTaxonomy>     # extending marketing documents
└── <CorportateDocumentTaxonomy>                        # adding more types of documents
``` 

---

### Project Structure

```
project-root/
├── cognitive-package.json
├── .knowlib/
│   ├── document-core/
│   │   └── knowledge/document-core/
│   │       ├── concepts/
│   │       │   ├── classes/Document.md
│   │       │   └── taxonomies/DocumentTaxonomy.md
│   │       └── agent/
│   ├── legal-component/
│   │   └── knowledge/document-legal/
│   │       └── concepts/taxonomies/LegalDocumentTaxonomy.md
│   ├── corporate-component/
│   │   └── knowledge/document-legal/
│   │       └── concepts/taxonomies/CorporateDocumentTaxonomy.md
│   └── marketing-component/
│       └── knowledge/document-product/
│           └── concepts/taxonomies/ProductDocumentTaxonomy.md
└── knowledge/
    └── document-project/
        └── knowledge/document-project/
            ├── concepts/taxonomies/MyCompanyDocumentTaxonomy.md
            └── rulebooks/document-processing.md
```

### cognitive-package.json — Taxonomy Polymorphism Mapping

The `cognitive-package.json` file at project root declares key mappings. The `taxonomy_resolution` section specifies that whenever `DocumentTaxonomy` is referenced in the package, the system resolves to `MyCompanyDocumentTaxonomy`:

```json
{
  "taxonomy_resolution": {
    "DocumentTaxonomy": "knowledge/document-project/knowledge/document-project/concepts/taxonomies/MyCompanyDocumentTaxonomy.md"
  }
}
```

Classes, rules, and templates reference `DocumentTaxonomy`; the system uses `MyCompanyDocumentTaxonomy` as the effective implementation.

---

### Entity Class

```markdown
## Properties
`document_id` IDENTIFIER : text (UUID)
`document_type` : `#DocumentTaxonomy`
`title` MANDATORY : text
```

### Rulebook Using Extended Taxonomy

```markdown
1. Read the `received_document` - the document received from the `User`.
2. Read `DocumentTaxonomy`. and identify the `document_category` of the `received_document`.
3. Look up category in taxonomy **Instructions for Rulebooks**.
4. Plan and execute your next actions based on the instructions.
```