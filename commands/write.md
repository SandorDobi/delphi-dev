---
description: Start a Delphi code writing session with all standards applied
---

Start a standardized Delphi code writing session.

**Step 1 — Understand what will be created**

Ask the user (if not provided):
- Element type: service class / repository / model / form / utility unit / interface?
- Main responsibility (must be single — SRP)?
- Implements any interface? Which one?
- Depends on other classes? Which ones will be injected?
- Suggested name or can you propose one?

**Step 2 — Propose structure**

Before writing code, present:
```
Proposal: [TClassName]
- Inherits from: [TInterfacedObject / TObject]
- Implements: [IInterfaceName]
- Injected via constructor: [IOtherDependency]
- Responsibility: [one sentence]
- Public methods: [list]
```

Wait for approval before continuing.

**Step 3 — Write complete code**

Generate complete, compilable code with:
- Unit header with organized uses clause
- Complete interface section
- Implementation section with all methods
- All standards automatically applied

Use the `delphi-writer` agent for implementation.
