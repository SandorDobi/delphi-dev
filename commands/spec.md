---
description: Generates a complete software specification document (SPEC) by analyzing the current Delphi project source code
---

Automatically generate a software specification document (SPEC) from the
Delphi project source code in the current working directory.

**Scope:** A SPEC covers the entire project or a complete business module.
Never for a single unit or class.

**Language:** Detect the user's language and always respond in that language.
Default: English.

Follow this protocol without interrupting the user with questions:

1. **SCAN** — Use Glob to locate all `.pas`, `.dfm`, `.dpr`, `.dproj`
   files in the current working directory. Identify the main project, forms, services,
   repositories, entities, and datamodules.

2. **READ** — Read the relevant files, prioritizing domain units, services,
   repositories, main forms, and datamodules with business rules.

3. **EXTRACT** — Map directly from the code:
   - Actors (forms and permissions)
   - Functional Requirements (public methods, actions, events)
   - Non-Functional Requirements (technology, database, platform)
   - Business Rules (validations, guards, if/raise)
   - Use Cases (screen flows, main actions)
   - Data Model (entities, records, queries)
   - Integrations (HTTP, COM, DLL, WebService)
   - Technical Constraints (Delphi version, database, target OS)

4. **GENERATE** — Fill in all template sections by loading the `delphi-spec`
   skill and the `references/spec-template.md` file. Mark with `[INFERRED]` items whose
   intent is not explicit in the code. Never leave sections blank.

5. **SAVE** — Save the document as `SPEC.md` in the project root.

6. **REPORT** — Inform the user: path of the generated file, actual vs. inferred sections,
   unanalyzed files (if any), and suggestions for manual review.
