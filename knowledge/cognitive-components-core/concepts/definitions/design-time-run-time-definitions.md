# Cognitive Components Design Time and Run Time
 
---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#good-enough`

---

## Key Concepts

### Cognitive Components Design Time

Когнитивные компоненты — это структуры знаний. Они почти никогда не бывают финализированы: их развивают и уточняют эксперты по мере появления нового понимания и по мере импорта знаний из внешних источников — существующих инструкций, учебников, экспертных статей, внутренних регламентов и т.д. Расширение и корректировка знаний компонента — нормальный постоянный процесс; редко удаётся один раз собрать структуру, которая никогда больше не изменится. **Рабочее допущение по умолчанию:** знания в компоненте неполные и нефинальные, они будут постоянно дополняться по мере получения новых фактов и фидбека.

**Дизайн-тайм** — фаза, когда компонент создаётся и редактируется как набор файлов в IDE (например Cursor, Codex или аналоги). В этой фазе активно используются агентные сценарии и большие языковые модели: текст знаний дописывается, перестраивается и согласуется с задачами. Содержательно основная работа дизайн-тайма — выявлять **пробелы в знаниях (knowledge gaps)** и решать, **что именно** нужно изменить в тексте этого компонента и, при необходимости, соседних компонентов.

Типичные задачи дизайн-тайма:

- создавать новые фрагменты знаний и оформлять их в виде одного или нескольких компонентов;
- при большом домене **декомпозировать** область на несколько компонентов так, чтобы каждый нёс свою часть знаний;
- переписывать, переносить и рефакторить знания — например, выносить общие понятия в более общий компонент или, наоборот, детализировать в более частных; менять границы и именование, чтобы структура лучше соответствовала реальным сценариям использования.

**Фидбэк и итерации.** Если по обратной связи видно, что компонент недостаточно хорошо покрывает часть ситуаций, знания неполны или процедуры не закрывают все ветви — в дизайн-тайме нужно проанализировать: какие факты или данные отсутствуют, какие процедуры неполные, какие краевые случаи и ситуации не покрыты правилами, какие таксономии недостаточно детализированы, где именно возникает разрыв между ожидаемым и фактическим поведением. На основе этого анализа знания дополняют или рефакторят (в том числе структуру пакета и ссылки между компонентами).

#### Folder `_design`

Внутри компонента может существовать служебная папка `_design`: материалы, которые нужны именно в режиме разработки знаний — концепция архитектуры компонента, собранный фидбэк, заметки об архитектуре и плане развития компонента, черновики разбора пробелов и т.п. 

---

### Cognitive Components Run Time



Информация из папки `_design` **не предназначена для рантайма**: при исполнении знаний (когда агент или система следует уже согласованным rulebook и знаниям) вспомогательные артефакты дизайн-тайма **не должны подмешиваться** — конфигурация прав доступа (например разделение **Design-Time** / **Run-Time** в `IKNOW.md`) должна исключать чтение `_design` в рантайме, если для выполнения задачи это не требуется явно.

### Architecture file

The `architecture.md` file describes the structure and purpose of the `Cognitive component` or the `Cognitive package` and how it will cooperate with other components or packages.

The file is placed in the `_design` folder in the root of the component or package, and not accessible in `run-time`.

#### Create architecture.md file when you create a new package or component

When a new, empty cognitive component or package is created during design-time, describe the structure and purpose of its parts in `architecture.md`, together with a high-level view of the component or package purpose, expected elements, and logic. 

#### Use architecture.md file as a guiding document for design-time component/package development

During design-time, use `architecture.md` as the guiding document for creating elements such as concepts, taxonomies, principles, workflows, and additional components. They can be developed manually or through agentic instructions, following the vision and structured descriptions captured in `architecture.md`.

##

The architecture file captures the intended structure and purpose of the cognitive component's parts. Some parts may not be implemented yet but should still appear in the document to guide future work. While drafting, informal markers such as “FUTURE” or “TBD (later)” are fine. When the document is maintained or synchronized with the tree (see `workflows/sync-architecture-rulebook.md` in **cognitive-components-core**), use the canonical line per design unit: `**Implementation status:** FUTURE` | `IN PROGRESS` | `IMPLEMENTED`.

Later in the development lifecycle, as specific areas of the cognitive component are built out, keep `architecture.md` synchronized with those changes so it stays accurate and reflects the current architecture.

Additional ADR files (Architectural Decision Records), named `[architecture-decision]-ard.md`, may also be placed in the `architecture/` folder. They can document separate decisions and explain why a specific design was chosen.

**Where should the `architecture.md` file be stored?**
- Put `architecture.md` directly in the component root folder `{cognitive-component-root}/architecture.md` if the component is simple and does not require a complex architecture or versioning strategy.
- Create an `architecture/` folder and put `architecture.md` inside if the logic is complex. Place additional ADRs in the `architecture/` folder as well.