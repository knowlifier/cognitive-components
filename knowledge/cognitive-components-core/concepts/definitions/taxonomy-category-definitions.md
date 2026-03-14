# Taxonomy, Category Definitions

---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#good-enough`

---

[add table of contents]

---

## Core Concepts

### Taxonomy

`Taxonomy` is a classification system that uses a hierarchy of `Categories` to organize items. 
`Taxonomy files` contain the description of the taxonomy, the hierarchical tree structure and description of `Categories`.

Example of Taxonomy:

`````markdown
# Customer Request Taxonomy

Classifies customer requests for routing to the appropriate support team. Used in CRM systems to assign tickets, prioritize handling, and track resolution metrics by request type.

## Taxonomy Tree
````
CustomerRequestTaxonomy : CustomerRequest
├── #new-order
└── #complaint
    ├── #product-quality-complaint
    └── #delivery-issue-complaint
````

## Categories

### New order

A request to place a new order, change an existing order, or inquire about ordering options. Routed to sales or order management.

### Complaint

A customer expression of dissatisfaction. Subdivided by cause to route to the responsible department for resolution.

### Product Quality Complaint

A complaint about defective, damaged, or substandard product. Routed to quality assurance or product support for investigation and remedy.

### Delivery Issue Complaint

A complaint about late, missing, or incorrect delivery. Routed to logistics or shipping for tracking and resolution.

`````

There are 3 main use cases for `Taxonomies`

#### Taxonomy as Fine-Grained Structure of Class Hierarchy

When describing a complex business logic (for example the diversity of businesses, products, locations, etc.) it may not be reasonable to create a very large tree of Classes and Subclasses. Instead, it is better to create a base class and, possibly, several subclasses for cases where individual branches of subtypes differ significantly from the base class and have special meaning for business logic.

A `Taxonomy` is introduced for a specific base `Class` and defines a tree of `Categories` for it, grouping `Entities` within `Categories` by common characteristics. Multiple `Taxonomies` can be introduced for the same `Class`, so the same `Entities` can be grouped by different characteristics; in each context, the most suitable `Taxonomy` can be chosen for the task.

**Example**: An online store may have millions of different products. We create a base class Product with properties common to all goods, and several subclasses Product Electronics, Product Clothing, Product Food, etc. with additional properties specific to electronics, clothing, and food. Then we create a large taxonomy whose root node is Product, with further branching into product categories, including branches for Electronics, Clothing, and Food. All categories outside these branches will be simply Product; all categories inside the branches will be instances of Electronics, Clothing, or Food.

`````
ProductTaxonomy : Product
  ├── #home-garden-product
  │   ├── #furniture-product
  │   └── #decor-product
  ├── #electronics-product : ElectronicsProduct
  │   ├── #computers-product
  │   ├── #phones-product
  │   └── #appliances-product
  ├── #clothing-product : ClothingProduct
  │   ├── #mens-clothing-product
  │   └── #womens-clothing-product
  └── #food-product : FoodProduct
      ├── #groceries-product
      │   ├── #dairy-product
      │   │   ├── #milk-product
      │   │   └── #cheese-product
      │   ├── #greens-product
      │   │   ├── #vegetables-product
      │   │   └── #fruits-product
      │   └── #pantry-product
      │       ├── #grains-product
      │       └── #canned-goods-product
      ├── #beverages-product
      └── #snacks-product
`````

#### Taxonomy as Enumeration (Choice of Options)

When a `Property` of a `Class` must take values from a fixed set of options, a `Taxonomy` is an effective way to define that set. Instead of a plain list of values, the taxonomy provides a structured, plain or hierarchical extensible dictionary where each option is a `Category` with its own semantics.

Each category can be documented in detail: its meaning, when to use it, and how it differs from sibling categories. This documentation enables `Rulebooks` and agents to read the taxonomy, understand the semantics of each option, and act accordingly. When a workflow refers to such a taxonomy, it gains not only the list of allowed values but also the knowledge of what each value means and how to process entities tagged with it.

**Example**: A `Document` class may have a `confidentiality` property whose values are defined by a `ConfidentialityTaxonomy` (e.g., #public, #internal, #confidential, #restricted). A `User` class may have an `accessRight` property whose values are defined by an `AccessRightTaxonomy` (e.g., #guest, #employee, #manager, #admin). Each category in both taxonomies has a description of its semantics—who can access what, retention rules, and handling requirements. A rulebook can read both taxonomies and decide whether to share the document with the user by comparing the user's `accessRight` against the document's `confidentiality`.

```
ConfidentialityTaxonomy : Document
├── #public
├── #internal
├── #confidential
└── #restricted

AccessRightTaxonomy : User
├── #guest
├── #employee
├── #manager
└── #admin
```

#### Taxonomy for Classification and Aggregation

A `Taxonomy` can be used to classify large-scale hierarchies of items — such as user requests, legal documents, production defects, job titles, geographical locations - as a tree of categories. Each category describes a type of item and how to process it.

An important advantage of using a taxonomy for classification is **graceful fallback**. When there is insufficient context to classify precisely—for example, we know the request is about a product but not which specific product—the item can still be classified into a higher-level category. Requests that cannot be classified more accurately fall into a parent node in the taxonomy. We retain at least the general topic and can process accordingly. For large volumes of requests, this allows classification by broad topic even when fine-grained classification is not possible.

Categories in a classification taxonomy can include descriptions of what to do and how to process items that match that category. For requests that could not be classified more precisely, the parent category provides guidance on how to handle them (e.g., general product inquiry, escalation path, or default response).

**Example**: For a website, we create a taxonomy of user requests: the client learns about products, the client learns about the company, the client has a complaint, the client has a delivery problem, and so on. Each category specifies how to answer such requests. The tree structure allows routing and prioritization by request type.

```
UserRequestTaxonomy : UserRequest
├── #about-products
├── #about-company
├── #complaint
│   ├── #product-quality-complaint
│   └── #delivery-issue-complaint
└── #delivery-problem
```

**Example**: When processing sales data from company departments, a `GeographyTaxonomy` can organize locations as a hierarchy: regions (NALA, EMEA, APAC), countries within each region, and major cities within each country. Sales data is recorded at the city level. The taxonomy enables aggregation: given facts about sales in cities, a rulebook can use the taxonomy to roll up totals by country or by whole region.

```
GeographyTaxonomy : Location
├── #nala
│   ├── #usa
│   │   ├── #new-york
│   │   └── #san-francisco
│   └── #brazil
│       └── #rio-de-janeiro
├── #emea
│   ├── #united-kingdom
│   │   └── #london
│   ├── #germany
│   │   ├── #berlin
│   │   └── #stuttgart
│   └── #uae
│       └── #dubai
└── #apac
    ├── #japan
    │   └── #tokyo
    └── #australia
        └── #sydney
```

#### Taxonomy Naming

A taxonomy is identified by its name. Recommended convention: `[DomainOrSubject]Taxonomy` — PascalCase, ending with the word `Taxonomy`, no spaces or special characters. The name should reflect what the taxonomy classifies (e.g., `AnimalTaxonomy`, `IncidentTaxonomy`, `ProductTaxonomy`).

The taxonomy file name should match the taxonomy name: `[TaxonomyName]Taxonomy.md` (e.g., `CustomerRequestTaxonomy.md`).

#### Taxonomy Usage Notation in Classes, Rulebooks and Templates

When referring to a taxonomy in `Classes`, `Rulebooks`, or `Templates`, use one of two notations:

- **Without hashtag** (`MyTaxonomy`): Use when referring to the taxonomy as a whole — for example, when instructing a rulebook to read specific sections of the taxonomy file before execution.
- **With hashtag** (`#MyTaxonomy`): Use when defining a property type whose values are categories from that taxonomy — for example, a property of type `#ShipmentStatusTaxonomy` accepts any category from that taxonomy.

Example in a Class file:
````markdown
# Order Class

## Properties
`Order ID` IDENTIFIER : text (UUID)
`shipment_status` : `#ShipmentStatusTaxonomy`
`department` : `#DepartmentTaxonomy`

## Guidelines

Before processing an order, read `ShipmentStatusTaxonomy` to understand available statuses and their handling rules. For department assignment, consult `DepartmentTaxonomy`.
````

#### Taxonomy Declaration

Taxonomies are declared as separate files in the `{component-root}/concepts/taxonomies/` folder of the `Cognitive Component`.

A taxonomy file typically presents the tree structure, then describes each category. For very large taxonomies, each `Category` can be placed in a separate file, with more detailed information and instructions per category.

Use the [taxonomy template](../templates/taxonomy-template.md) when creating a new taxonomy.


#### Linking Classes to Taxonomies

 A `Class` is linked to a taxonomy via a property whose values are categories from that taxonomy. 
 
 For example, a `User` class may have an `accessRight` property whose type is a category from `AccessRightTaxonomy`. When a class is linked to several taxonomies, each taxonomy has its own property: a `Document` class might have `department` (from `DepartmentTaxonomy`), `format` (from `FormatTaxonomy`—e.g., picture, video), and `status` (from `StatusTaxonomy`, e.g., draft, final, published). Each property indicates which taxonomy supplies its allowed values.


---

### Category

A `Category` is a node in a `Taxonomy` that adds specific traits for a narrower application area. Unlike a `Class`, a category does not define a data structure or properties — it contains instructions, information, and features that describe how to interpret and process entities tagged with that category.

#### Category Tag

`Category tag` is the identifier of the Category. It must be unique across the `Taxonomy` and the `Taxonomy Extensions` that extend this `Taxonomy`. 

`Category Tag` recommended notation is hashtag and kebab-case identifier: `#category-tag`

When referring `Category` inside the Component (`Rulebooks`, `Classes`, `Templates`, `Principles`, etc.) use one of these ways:
- `#category-tag` - (solo tag) when the Tag is unique across the Component/Package and clearly relates to a `SpecificTypeTaxonomy`
- `#SpecificTypeTaxonomy--category-tag` - (full taxonomy name and tag) when you want to clearly 
- `#SpecificType--category-tag` - (a shorter version omiting the word "Taxonomy" in the full taxonomy name and tag) 

Valid examples:
- `#restricted`
- `#Confidentiality--restricted`
- `#ConfidentialityTaxonomy--restricted`
- `#delivery-issue-complaint`
- `#UserRequest--delivery-issue-complaint`
- `#UserRequestTaxonomy--delivery-issue-complaint`
- `#san-francisco`
- `#Geography--san-francisco`
- `#GeographyTaxonomy--san-francisco`

Avoid tags that are ambigous, although they may become clear when combined with the name of taxonomy, in some documents, solo tags may cause wrong interpretation.

Don't do this (bad):
- `ClothingSizeTaxonomy--medium`
- `SteakDonenessTaxonomy--medium`

Do this (good):
- `ClothingSizeTaxonomy--medium-size`
- `SteakDonenessTaxonomy--medium-doneness`

#### Category Description

A `Category Description` defines the meaning, usage, and behavior of a category. It tells agents and rulebooks when to apply the category, how it differs from sibling categories, and how to process entities tagged with it.

A minimal description is a short paragraph: meaning, when to use, and how it differs from siblings. For categories that drive rulebook logic, add optional subsections:

- **Description** — detailed explanation when one paragraph is not enough
- **Instructions for Rulebooks** — how to process entities or input of this category
- **Fallback Instructions** — for parent categories with children: how to handle entities that match the parent but not any child
- **Other Sections** — add whatever sections you feel appropriate with clear section title to help interpret these instructions

See the [taxonomy template](../templates/taxonomy-template.md) for the recommended structure and examples.

---

Example of a category defined inside the Taxonomy file:

````markdown
# Confidentiality Taxonomy

Defines confidentiality levels for documents. Used by rulebooks to decide access and retention.

## Taxonomy Tree
```
ConfidentialityTaxonomy : Document
├── #public
├── #internal
├── #confidential
└── #restricted
```

## Categories

### #public
Shareable with anyone. No retention restrictions. May be published externally.

### #internal
For employees only. Do not share outside the organization. Standard retention applies.

### #confidential
Sensitive business data. Access on need-to-know basis. Extended retention; secure disposal required.

### #restricted
Highly sensitive. Requires explicit approval for access. Audit all access. Long-term secure storage.
````

Example of a category defined in a separate file:

For large taxonomies, a category can live in its own file under `concepts/taxonomies/[taxonomy-name]/categories/`:

````markdown
# Restricted Category

**Tag:** #restricted
**Taxonomy:** ConfidentialityTaxonomy  
**Parent category:** #confidential

## Description

Documents tagged `#Confidentiality--restricted` contain highly sensitive information (e.g., legal, financial, personal data). Access must be explicitly approved and audited.

## Instructions for Rulebooks

- **Access check:** Deny unless user has `#AccessRight--restricted-clearance` or higher.
- **Sharing:** Block external sharing. Log all internal access.
- **Retention:** Store in secure vault. Retain per compliance policy. Use secure deletion on expiry.
- **Fallback:** If a document is classified as #confidential but not into a subcategory, apply #confidential rules; do not assume #restricted.
````

### How Categories and Rulebooks Work Together

When a `Rulebook` receives an `Entity`, it can read the entity's category-linked properties (e.g., document status, user access level, defect type) and use the taxonomy to look up the semantics of each category. The `Rulebook` then acts according to that knowledge. 

`Rulebooks` should be able to work with the taxonomy tree: if an entity is classified into a specific leaf category, one set of actions applies; if it falls into a parent category (e.g., we know the `Entity` is a `#legal-document` but not whether it is a `#contract`, a `#claim`, or another subcategory of `#legal-document`), the rulebook should apply the processing defined for that higher-level category — for example, "legal documents that did not fall into any specific subcategory."



---

#### Taxonomy and Category Examples

**Example:** For the base class File, there are several subclasses (e.g., TextFile and ImageFile) and many categories for which there is no need to create separate classes (e.g., by formats - text doc files, md files, txt files or by themes - text files with documentation, blog materials, security policies, bird images, city images, etc.) These categories (#blog-materials, #bird-images) do not have their own class (BlogDocument, BirdImage); they use the category tag only.

````
FileTypeTaxonomy : File
├── #text-file : TextFile
│   ├── #blog-post
│   └── #corporate-document : CorporateDocument
│       ├── #business-plan
│       └── #legal-contract : Contract
│
├── #media-file
│   ├── #image-file : ImageFile
│   │   ├── #bird-image
│   │   ├── #city-landscape-image
│   │   └── #cat-image
│   └── #video-file
│
└── #source-code-file : CodeFile

FileSizeTaxonomy : File
├── #small-file
├── #large-file
└── #xlarge-file
````

---

## How Classes and Categories Work Together

### 1. Classes and categories are organized hierarchically

`Cognitive Class` hierarchy

```
ClassA
  ├── ClassB
  │   └── ClassC
  └── ClassD
```

A taxonomy starts from a base class and defines a hierarchical structure where child categories divide instances of the base class into more specialized child categories. For some nodes of the category tree, corresponding subclasses are mapped.

```
NameOfTaxonomy : ClassA
  ├── #category-1
  │   ├── #category-11
  │   └── #category-12 : ClassB
  │       ├── #category-121
  │       └── #category-122 : ClassC
  ├── #category-2
  │   └── #category-21 
  └── #category-3 : ClassD
```

### 2. Multiple different taxonomies can be defined for one base class

Multiple taxonomies can be defined for one base class, however, if subclasses of the base class appear in category nodes, they must not violate the class inheritance hierarchy.

For example, for the class hierarchy

```
ClassA
  ├── ClassB
  │   └── ClassC
  └── ClassD
```

The First Taxonomy and Second Taxonomy are correct and can be used simultaneously for different tasks, but the Third Taxonomy is incorrect because it violates the class inheritance hierarchy: ClassC should be a descendant of ClassB (as in the class hierarchy), but in Third Taxonomy ClassC and ClassB both inherit from ClassA at the same hierarchy level, which contradicts the original class hierarchy where ClassB is parent to ClassC. This taxonomy cannot be used.

```
FirstTaxonomy : ClassA
  ├── #category-1
  │   ├── #category-11
  │   └── #category-12 : ClassB
  │       ├── #category-121
  │       └── #category-122
  │           └── #category-1221 : ClassC 
  ├── #category-2
  │   └── #category-21 
  └── #category-3 : ClassD
```

```
SecondTaxonomy : ClassA
  ├── #category-alpha
  │   └── #category-alpha-1 : ClassB
  │       ├── #category-alpha-2 : ClassC
  │       └── #category-alpha-2
  ├── #category-beta
  └── #category-gamma : ClassD
```

```
ThirdTaxonomy : ClassA
  ├── #category-x
  │   ├── #category-xy : ClassB
  │   └── #category-xz : ClassC
  ├── #category-y
  │   └── #category-yz 
  └── #category-z : ClassD
```

---

## When Use Classes and When Use Taxonomy Categories

### When to use classes and subclasses

**Use classes and subclasses when:**
- You need to formally define an important concept that will be unambiguously used in this Package and possibly in other Downstream packages
- When active use of this concept in workflows and automation is planned, to exclude errors in term interpretation
- When a class needs to be linked to program code in programming languages
- When creation or integration of databases or large catalogs of objects of this type is planned

### When to use taxonomies and categories

**Use taxonomies and categories when:**
- Business logic for processing class instances requires accounting for minor differences that can be described using a category tree
- It is impractical to clutter the Package with an excessive number of classes, but a detailed system grouping objects by characteristics is required
- You need to organize several classification systems for the same objects by different characteristics simultaneously using multiple taxonomies (e.g., organize products in a catalog by manufacturers, by geography, by product types)



