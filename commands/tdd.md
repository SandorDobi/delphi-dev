---
description: Generates DUnitX unit tests for the entire Delphi project or a specific class
---

Start creating DUnitX unit tests for the current Delphi project.

**Language:** Detect the user's language and always respond in that language.
Default: English.

This command operates in **Phase 1 (initial setup)**:

1. **Project Analysis** — Read all units in the project and identify what can
   be tested. Priority: business services, repositories, utilities with logic.
   Ignore: Forms, DataModules, classes without their own logic.

2. **Proposal** — Present the proposed test suite with the list of cases per class:
   ```
   [TClienteService] — N cases
     ✓ Test_Salvar_ClienteValido_Salva
     ✓ Test_Salvar_ClienteSemNome_LancaExcecao
     ...
   ```
   Wait for user approval before generating.

3. **Generation** — Produce all `Teste[NomeDaClasse].pas` files and the
   `TestRunner.dpr` project with the DUnitX runner.

**Phase 2 (continues automatically):** After each new implementation via `/write`,
the `delphi-tester` agent creates tests for the new class automatically and notifies
the user with the list of created cases.

Use the `delphi-tester` agent to perform analysis and generation.
Load the `delphi-tests` skill and the patterns in `references/dunitx-patterns.md`.
