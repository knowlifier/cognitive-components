# Incomplete and Unstructured Data Handling Principles

---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#good-enough`

---

**Key Principles:**

- [Incomplete and Unstructured Data Handling Principles](#incomplete-and-unstructured-data-handling-principles)
  - [Principle: Cognitive Components Accept Incomplete and Unstructured Data](#principle-cognitive-components-accept-incomplete-and-unstructured-data)
  - [Principle: Cognitive Components are not Bound to Any Specific Natural Language, Programming Language, or Notation](#principle-cognitive-components-are-not-bound-to-any-specific-natural-language-programming-language-or-notation)

---

## Principle: Cognitive Components Accept Incomplete and Unstructured Data

`Cognitive Components` are designed for cognitive tasks and support working with data received from external sources in different languages that may not follow any standards. When processing data, `Cognitive Components` perceive and process information using fuzzy logic. When standardized input is expected, `Cognitive Classes` can be defined within the `Cognitive Component`, and incoming data can be converted to the `Class` format using `Typecasting`. During execution of processes described in `Rulebooks`, output data is also processed using fuzzy logic, attempting to extract the required information from the provided context. If data is insufficient and the operation is critical, each `Rulebook` independently defines how to resolve uncertainty - whether to stop the process, request additional data, fill in default values, or make reasonable assumptions based on context.

---

## Principle: Cognitive Components are not Bound to Any Specific Natural Language, Programming Language, or Notation

`Cognitive Components` support the use of different formats for describing knowledge, including notations from programming languages or natural languages (English, Spanish, Chinese, etc.) provided that several conditions are met:

- key elements of the component (agent, definitions, classes, principles, taxonomies, templates, data sources, rulebooks) are easily found within the component and do not cause ambiguity
- component and package interfaces (via `agent.md` and `glossary.md`) are easily readable and interpretable for LLMs and humans
- concepts introduced in languages other than English have an English translation in `glossary.md` and `definitions` to ensure clear interaction between components across packages
- job descriptions in `agent.md` that are written in languages other than English should include one or more example requests in English to ensure matching
- the `Cognitive Component` developers can use their preferred notations (code-style or markdown-style files, kebab-case or snake_case or another format of choice for properties naming), but should maintain consistent formatting and notation within the `Cognitive Component`.
