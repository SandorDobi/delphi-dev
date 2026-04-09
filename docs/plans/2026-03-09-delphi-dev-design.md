# Design: `delphi-dev` Plugin for Claude Code

**Date:** 2026-03-09
**Author:** Adriano Santos
**Status:** Approved

---

## Overview

A Claude Code plugin that transforms the assistant into a senior Delphi expert.
When Delphi code is detected, Claude automatically applies coding standards —
without being asked. Explicit commands enable technical audits, reviews, and
standardized code generation.

Designed to be published as open source on the Claude Code marketplace and as
Factory.ai droids.

---

## Problem It Solves

Delphi developers — individually or in teams — frequently violate naming,
formatting, and architecture standards due to the lack of automatic guardrails.
Without specialized context, Claude doesn't know the Delphi Style Guide,
mandatory prefixes (F, A, L, C_, T, I), prohibited commands (with, Break, Continue),
or how to structure professional technical audit reports.

---

## Knowledge Sources

The plugin standards are based on:

- **Adriano Santos — Delphi Coding Standards v4.0.1** (primary style document)
- **Clean Code and Best Practices in Delphi** (textbook based on Clean Code + SOLID + Delphi Style Guide)
- **Good Practices Guide for Writing Code v1.0**
- **Delphi Excellence Standards — Clean Code**
- **Clean Code — Robert C. Martin** (universal reference adapted to Delphi)
- **delphi-audit skill** (production-validated technical audit skill)

---

## File Structure

```
delphi-dev/
├── .claude-plugin/
│   └── plugin.json
├── .factory/
│   └── droids/
│       ├── delphi-expert.md
│       ├── delphi-auditor.md
│       ├── delphi-writer.md
│       ├── delphi-spec-writer.md
│       └── delphi-tester.md
├── skills/
│   ├── delphi-standards/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── naming-conventions.md
│   │       ├── formatting.md
│   │       ├── forbidden-commands.md
│   │       ├── classes-structure.md
│   │       └── component-prefixes.md
│   ├── delphi-write/
│   │   └── SKILL.md
│   ├── delphi-audit/
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── audit-report-structure.md
│   │       ├── clean-code-delphi.md
│   │       ├── code-smells-delphi.md
│   │       ├── delphi-technologies.md
│   │       ├── modernization-estimates.md
│   │       └── style-guide.md
│   ├── delphi-spec/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── spec-template.md
│   ├── delphi-tests/
│   │   └── SKILL.md
│   └── delphi-ignore/
│       └── SKILL.md
├── agents/
│   ├── delphi-auditor.md
│   ├── delphi-writer.md
│   ├── delphi-spec-writer.md
│   └── delphi-tester.md
├── commands/
│   ├── audit.md
│   ├── review.md
│   ├── write.md
│   ├── new-project.md
│   ├── spec.md
│   ├── tdd.md
│   └── about.md
├── docs/
│   ├── PROJECT-STATE.md
│   ├── Handoffs/
│   │   └── handoff-001.md
│   └── plans/
│       ├── 2026-03-09-delphi-dev-design.md
│       └── 2026-03-09-delphi-dev-implementation.md
├── LICENSE
├── privacy-policy.md
└── README.md
```

---

## Skills

### `delphi-standards` — Automatic Delphi Mode

**Purpose:** Always-present skill when working with Delphi. Loads the full
coding standards as active context.

**Auto-trigger:** Detects `.pas`, `.dpr`, `.dfm`, `.dpk`, `.dproj` files,
mentions of Delphi, FireMonkey, VCL, FireDAC, RAD Studio.

**Responsibilities:**
- Mandatory prefixes: `F` (fields), `A` (params), `L` (locals), `C_` (constants)
- Type prefixes: `T` (classes/types), `I` (interfaces), `E` (exceptions), `P` (pointers)
- Enumerations: 2+ letter mnemonic prefix in lowercase
- Indentation: 2 spaces, no TAB, 120-char margin
- `begin` on its own line, `else` on its own line
- Reserved words in lowercase (`begin`, `end`, `if`, `string`)
- No underscores in identifiers (except `C_` in constants)
- No global variables (use `class var`)
- No Hungarian notation in parameters (`sNome`, `iCount` → prohibited)
- `const` for string/record params; NEVER `const` on interfaces (ARC)
- Monetary types: `Currency` preferred; `Real` prohibited

**References:** 5 thematic reference files to avoid overloading the main context.

---

### `delphi-write` — New Code Writing

**Purpose:** Guides writing new Delphi code from scratch, ensuring bad habits
never enter the code before they happen.

**Auto-trigger:** Detects intent to create new class, unit, method, form,
service, repository, or interface in Delphi.

**Responsibilities:**
- Structure classes in the correct order (strict private → private → protected → public)
- Small, focused functions (20–30 lines max)
- `Result` directly in functions (no auxiliary variable)
- DTOs for methods with 4+ parameters
- Guard clauses with `Exit` at the top (only permitted use of Exit)
- try..finally per resource (never release two resources in the same block)
- SQL always parameterized, never concatenated
- Uses organized: RTL → VCL/FMX → FireDAC → Third-party → Project

---

### `delphi-audit` — Professional Technical Audit

**Purpose:** Generate professional technical audit reports for Delphi projects.
Ported integrally from the production-validated `delphi-audit` skill.

**Auto-trigger:** Detects requests for analysis, audit, report, diagnosis,
code smells, `.pas`/`.dfm`/`.dpr` files sent for analysis.

**Responsibilities:**
- Initial assessment (Delphi version, database, access component, goal)
- 8 analysis dimensions: Architecture, Clean Code, Code Smells, SOLID,
  Memory, Data Access, Security, Maintainability
- 1–5 scoring system with classification: GOOD / REGULAR / CRITICAL / UNVIABLE
- Auto-flags: CRITICAL, ATTENTION, RECOMMENDATION
- Outputs: full report, executive summary, checklist, modernization plan

**References:** 6 reference files (ported from the original ZIP).

---

### `delphi-spec` — Specification Generation

**Purpose:** Analyze the current project source code and auto-generate a
complete SPEC.md specification document.

**Auto-trigger:** Activated by the `/spec` command.

**Responsibilities:**
- Scan project source code for modules, classes, and relationships
- Generate a comprehensive SPEC following the spec template
- Cover business rules, data models, and interface contracts

**References:** `spec-template.md` for the output structure.

---

### `delphi-tests` — DUnitX Test Generation

**Purpose:** Generate complete DUnitX unit test suites for Delphi projects.

**Auto-trigger:** Activated by the `/tdd` command or automatically after
`delphi-write` completes code generation.

**Responsibilities:**
- Generate DUnitX test classes and methods for Delphi units
- Cover happy paths, edge cases, and expected exceptions
- Follow naming conventions (TTestXxx, Test_MethodName_Scenario_ExpectedResult)

---

### `delphi-ignore` — Token Optimization

**Purpose:** Auto-activated on Delphi project detection to optimize token
usage by configuring `.claudeignore` appropriately.

**Auto-trigger:** Detects Delphi project structure.

---

## Agents

### `delphi-auditor`

Specialized subagent for deep technical auditing. Invoked by the `/audit`
command or automatically by the `delphi-audit` skill when analysis requires
systematic scanning of multiple files.

Profile: Senior Delphi expert with experience in Delphi 1–12, knowledge of
all frameworks and components in the ecosystem, familiarity with legacy and
modern systems.

---

### `delphi-writer`

Specialized subagent for writing standardized code. Invoked by the `/write`
command or by the `delphi-write` skill.

Profile: Writes Delphi code as a senior developer who has internalized all
standards — never violates rules, never needs reminders, produces code ready
for production and peer review.

---

### `delphi-spec-writer`

Specialized subagent for generating SPEC documents. Invoked by the `/spec`
command or by the `delphi-spec` skill.

Profile: Analyzes source code exhaustively and produces complete, well-structured
specification documents covering business rules, data models, and interfaces.

---

### `delphi-tester`

Specialized subagent for creating DUnitX test suites. Invoked by the `/tdd`
command or automatically after code generation.

Profile: Creates comprehensive DUnitX unit tests covering happy paths, edge
cases, and expected exceptions with proper naming conventions.

---

## Commands

| Command | Description |
|---|---|
| `/audit` | Full technical audit — 8 dimensions, scoring, recommendations |
| `/review` | Quick code review — violations and corrections |
| `/write` | Start a standardized code writing session |
| `/new-project` | Scaffold a new Delphi project with correct structure |
| `/spec` | Auto-generate a SPEC document from source code analysis |
| `/tdd` | Generate DUnitX unit test suites |
| `/about` | Display plugin info, version, and available commands |

---

## Factory.ai Droid Harness

Five pre-configured droids for Factory.ai are included in `.factory/droids/`:

| Droid | Purpose |
|---|---|
| `delphi-expert` | Auto-activates on Delphi files — applies all coding standards |
| `delphi-auditor` | Deep technical audit — 8 dimensions, scoring, 17-section report |
| `delphi-writer` | Writes production-ready Delphi code with all standards |
| `delphi-spec-writer` | Generates SPEC documents from source code analysis |
| `delphi-tester` | Creates DUnitX unit test suites for Delphi classes |

---

## GitHub Repository

- **URL:** `https://github.com/adrianosantostreina/delphi-dev`
- **License:** MIT
- **README:** English (international reach)
- **Install:** `droid plugin marketplace add adrianosantostreina/delphi-dev`

---

## Quality Criteria for Marketplace Approval

- [x] Structure compatible with `claude-plugins-official`
- [x] `plugin.json` with all required fields
- [x] README with real usage examples
- [x] Explicit MIT license
- [x] No MCP servers (safer profile for approval)
- [x] No external code execution
- [x] Well-defined, unambiguous skill triggers
- [x] Skills with modular references (don't overload context)
- [x] Agents with complete, focused system prompts
- [x] Commands with clear execution instructions
- [x] Factory.ai droid harness for cross-platform compatibility

---

## Out of Scope (v1.0)

- Delphi LSP integration
- Static analysis via external tools
- Support for languages other than Delphi/Object Pascal
