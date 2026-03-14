# File Fidelity Taxonomy

---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#good-enough`

---

Classifies the completeness and quality of files and artifacts inside `Cognitive Components`. Used to indicate to AI agents and human collaborators the maturity and reliability of the file during development.

---

## Taxonomy Tree

```
FileFidelityTaxonomy
├── #hero
├── #good-enough
├── #quick-and-dirty
├── #placeholder
└── #zero
```

## Categories

### `#hero`

**Tag:** `#hero`

HERO: Holistic. Efficient, Robust. Outstanding. 

Production-ready `Cognitive Components` files that meet high standards. Use when the file is complete, well-structured, reviewed by humans multiple times, has undergone testing including edge and corner cases, and is suitable for use in mission-critical agentic business processes.

This fidelity level is assigned manually after extensive refinement and testing.

---

### `#good-enough`

**Tag:** `#good-enough`

Sufficient for the use of majority of business processes. The file was reviewed by a subject-matter expert, the parts of the file were rewritten, grammar is fixed, and links are updated. 

The `Rulebooks` utilizing this file were tested by real-world and/or synthetic test cases. The file can be used in `Cognitive Components` but may require additional testing and performance review, especially in corner-case situations and abnormal input data.

Failure reports on these files with real-world examples are highly welcome.

This fidelity level is assigned manually after manual refinement and testing.

---

### `#quick-and-dirty`

**Tag:** `#quick-and-dirty`

Hastily created and reviewed by a human expert without putting enough effort. Content may lack quality, structure, or completeness. Use when the file was produced as a draft, for prototyping purposes, under time pressure, or as a temporary solution. Rulebooks utilizing this file may easily lead to hallucinations and unexpected behavior, especially in non-standard situations and corner-case scenarios.

These files are considered a temporary solution and should be used in mission-critical business processes with great caution.

This fidelity level is assigned manually after brief review of an LLM generated `#placeholder` file.

---

### `#placeholder`

**Tag:** `#placeholder`

A manually created stub file or a file generated from a `Template` by an LLM in a `Cognitive Component` before it is manually supervised. These files may contain serious defects or gaps and should only be used for drafting the file structure or creating raw material for further manual polishing by a subject matter expert.

Never use these files in any mission-critical business processes.

This fidelity level is assigned automatically when the file is generated from a `Template`. 

---

### `#zero`

**Tag:** `#zero`

Empty, absent, or lacking meaningful content. Use when the file is a blank template, intentionally empty, or for files populated with very raw remarks, e.g. audio file transcripts describing what can be done here. Such raw remarks may later be used as input for an LLM to generate a `#placeholder`.

Rulebooks should not rely on such files for any decisions.

