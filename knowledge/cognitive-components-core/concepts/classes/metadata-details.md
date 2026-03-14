# Metadata Details

---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#good-enough`

---

`Metadata-Details` is a SubjectDetails block for document metadata: version, author, and fidelity. The fidelity level indicates whether the cognitive component is ready for production use or requires human revision.

---

## Properties

`version` : text (X.X.X) — version string, e.g. 0.1.0

`author` OPTIONAL : text or `Person` — author name, git name, or Person entity

`fidelity` OPTIONAL : `#FileFidelityTaxonomy` — readiness for production (use `#placeholder` if freshly generated)

`source` OPTIONAL : text or url — origin of the data (e.g. API endpoint, file path)

---

## Template

````markdown

**Version:** [version string in X.X.X format]
**Author:** [author name, git name or Person entity]
**Fidelity:** [#FileFidelityTaxonomy, make #placeholder if the file is freshly generated]

````

---

## Related Files

- **Entity, Class Definitions:** [entity-class-definitions](../definitions/entity-class-definitions.md#subjectdetails)
