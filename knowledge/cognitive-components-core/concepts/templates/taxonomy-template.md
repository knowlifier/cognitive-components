# [Taxonomy Name] Taxonomy

---

[Metadata-Details]

---

[Brief description of what this taxonomy classifies and how it is used (e.g., by rulebooks, for routing, for aggregation).]

---

## Taxonomy Tree

```
[TaxonomyName]Taxonomy ( : [BaseClass] )
├── #[category-1]
│   ├── #[category-1-1] ( : [SubClass] )
│   └── #[category-1-2]
├── #[category-2]
└── #[category-3]
```

## Categories

### [Category Tag or `#category-tag`]

[**Tag:** `#category-tag` don't repeat if the section title is already `#category-tag`]

[Short description: meaning, when to use, how it differs from siblings. Instructions for rulebooks if applicable.]

---

### [Category Tag or `#category-tag`]

[**Tag:** `#category-tag` don't repeat if the section title is already `#category-tag`]

[Short Description...]

---

### [Category Tag or `#category-tag`]

( [Detailed description and intstuctions below if one paragraph is not enough and the subsections are applicable] )

[**Tag:** `#category-tag` don't repeat if the section title is already `#category-tag`]

#### Description

[Detailed description.]

#### Instructions for Rulebooks

[Instructions how to process the entities/input of this category.]

#### Fallback Instructions

[For parent categories with children: how to process entities or input that match the parent but not any child category.]

---

## Related Files

( **[Taxonomy]Taxonomy:** [Markdown link to the related taxonomy file] if the taxonomy is extended or is an extension of another taxonomy)

( **[ClassName] Class:** [Markdown link to base Class definition if applicable] )

( **[SubclassName] Class:** [Markdown link to base Subclass definition if applicable] )

---

# END OF TEMPLATE, EXAMPLES BELOW THIS LINE

---

# Customer Request Taxonomy

---

**Version:** 0.1.0
**Author:** John Smith
**Fidelity:** `#placeholder`

---

Classifies customer requests for routing to the appropriate support team. Used in CRM systems to assign tickets, prioritize handling, and track resolution metrics by request type.

---

## Taxonomy Tree

```
CustomerRequestTaxonomy : CustomerRequest
├── #new-order
└── #complaint
    ├── #product-quality-complaint
    └── #delivery-issue-complaint
```

## Categories

### New order

**Tag:** `#new-order`

A request to place a new order, change an existing order, or inquire about ordering options. Routed to sales or order management.

---

### `#complaint`

A customer expression of dissatisfaction. Subdivided by cause to route to the responsible department for resolution.

#### Instructions for Rulebooks

Route to complaint-handling queue; escalate if sentiment is severe.

#### Fallback Instructions

If the complaint does not clearly fit `#product-quality-complaint` or `#delivery-issue-complaint`, assign to general complaint triage for manual categorization before routing.

---

### `#product-quality-complaint`

A complaint about defective, damaged, or substandard product. Routed to quality assurance or product support for investigation and remedy.

---

### `#delivery-issue-complaint`

A complaint about late, missing, or incorrect delivery. Routed to logistics or shipping for tracking and resolution.

---

## Related Files

**CustomerRequest Class:** [CustomerRequest](entity-class-definitions.md#customerrequest)
