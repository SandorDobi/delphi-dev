---
description: Scaffold a new Delphi project with standardized folder structure and base files
---

Create the initial structure of a new Delphi project following architecture best practices.

**Step 1 — Gather requirements**

Ask the user:
- System/project name?
- Type: VCL / FMX / REST API / Library?
- Database? Access component?
- Desired architecture: simple (Form + DataModule) / layered (Model, Service, Repository)?
- Third-party frameworks? (ACBr, Horse, FastReport, etc.)

**Step 2 — Propose folder structure**

Example for a layered project:
```
ProjectName/
├── src/
│   ├── model/          # entities and DTOs
│   ├── interfaces/     # contracts (IService, IRepository)
│   ├── service/        # business rules
│   ├── repository/     # data access
│   ├── presentation/   # forms and frames
│   └── shared/         # utilities, constants, exceptions
├── tests/              # DUnitX
├── docs/
└── ProjectName.dproj
```

**Step 3 — Generate base files**

Create the initial files:
- `ProjectName.dpr` — clean project file
- Custom domain exceptions unit
- Base repository interface (ICRUDRepository)
- Connection DataModule (if applicable)
- Main form with correct naming

Apply all Delphi Style Guide standards in every generated file.
