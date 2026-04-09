# delphi-dev Plugin — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create the `delphi-dev` plugin for Claude Code — an automatic Delphi expert with coding standards, technical audit, and standardized code generation. Extended with Factory.ai droid harness.

**Architecture:** Modular plugin with 6 skills (delphi-standards, delphi-write, delphi-audit, delphi-spec, delphi-tests, delphi-ignore), 4 specialized agents, 7 commands, and 5 Factory.ai droids. The `delphi-standards` skill auto-activates on Delphi code detection; other features are invoked via commands or intent triggers.

**Tech Stack:** Claude Code Plugin System — markdown, YAML frontmatter, plugin.json. No executable code, no MCP servers.

**Repo:** `https://github.com/adrianosantostreina/delphi-dev`

---

## Task 1: Plugin Metadata

**Files:**
- Create: `.claude-plugin/plugin.json`

**Step 1: Create the metadata file**

```json
{
  "name": "delphi-dev",
  "description": "Delphi development expert for Claude Code. Automatically applies Delphi Style Guide, Clean Code principles, and SOLID patterns. Includes technical audit, code review, standardized code writing, project scaffolding, spec generation, and DUnitX test generation.",
  "version": "1.3.0",
  "author": {
    "name": "Adriano Santos",
    "email": "adrianosantostreina@gmail.com"
  },
  "homepage": "https://github.com/adrianosantostreina/delphi-dev",
  "repository": "https://github.com/adrianosantostreina/delphi-dev",
  "license": "MIT",
  "keywords": [
    "delphi",
    "object-pascal",
    "clean-code",
    "style-guide",
    "code-review",
    "audit",
    "firemonkey",
    "vcl",
    "firedac",
    "rad-studio"
  ]
}
```

**Step 2: Commit**

```bash
git add .claude-plugin/plugin.json
git commit -m "feat: add plugin.json metadata"
```

---

## Task 2: MIT License

**Files:**
- Create: `LICENSE`

**Step 1: Create LICENSE file**

(Standard MIT License — Copyright 2026 Adriano Santos)

**Step 2: Commit**

```bash
git add LICENSE
git commit -m "chore: add MIT license"
```

---

## Task 3: Skill `delphi-standards` — Main SKILL.md

**Files:**
- Create: `skills/delphi-standards/SKILL.md`

**Step 1: Create SKILL.md**

The main skill with frontmatter triggering on Delphi file detection, containing:
- Complete summary of mandatory prefixes (F, A, L, C_, T, I, E, P)
- Prohibited commands (with, Break, Continue, Exit, Real, global vars)
- Floating point type rules (Currency preferred, Real prohibited)
- Parameter passing rules (const for string/record, never for interfaces)
- References to 5 modular reference files

**Step 2: Commit**

```bash
git add skills/delphi-standards/SKILL.md
git commit -m "feat: add delphi-standards skill"
```

---

## Task 4: `delphi-standards` References — naming-conventions.md

**Files:**
- Create: `skills/delphi-standards/references/naming-conventions.md`

**Step 1: Create file**

Covers CamelCase, scope prefixes (F, A, L, C_, T, I, E, P), method naming, enumerated type prefixes, constant naming, visual component prefixes, and prohibition of global variables.

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/naming-conventions.md
git commit -m "feat: add naming-conventions reference"
```

---

## Task 5: Reference `formatting.md`

**Files:**
- Create: `skills/delphi-standards/references/formatting.md`

**Step 1: Create file**

Covers indentation (2 spaces, no TAB), 120-char margin, begin/end rules, else rules, reserved words in lowercase, parentheses style, one variable per line, uses clause organization, and section spacing.

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/formatting.md
git commit -m "feat: add formatting reference"
```

---

## Task 6: Reference `forbidden-commands.md`

**Files:**
- Create: `skills/delphi-standards/references/forbidden-commands.md`

**Step 1: Create file**

Covers prohibited commands (with, Break, Continue, goto, Abort, Real), restricted commands (Exit only in guard clauses, Extended discouraged), loop rules (for/while/repeat), if condition ordering, case rules, and exception handling (try..finally per resource, never silent except).

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/forbidden-commands.md
git commit -m "feat: add forbidden-commands reference"
```

---

## Task 7: Reference `classes-structure.md`

**Files:**
- Create: `skills/delphi-standards/references/classes-structure.md`

**Step 1: Create file**

Covers visibility order (strict private → private → protected → public → published), fields naming, method naming (verb infinitive), parameter rules (A prefix, const for strings, DTOs for 4+ params), function Result usage, properties, interfaces (I prefix, GUID), DTOs, and SOLID principles table.

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/classes-structure.md
git commit -m "feat: add classes-structure reference"
```

---

## Task 8: Reference `component-prefixes.md`

**Files:**
- Create: `skills/delphi-standards/references/component-prefixes.md`

**Step 1: Create file**

Complete prefix table for VCL and FMX components: data entry (edt, mmo, cbx, lst, chk, rdb, dtp, spe, trk, nbx), display (lbl, img, prg), buttons (btn, bbt, spb, tbn), layout (pnl, grp, pgc, tbs, scb, spl, frm, lyt, rct), grids (grd, sgr, lsv, trv), data (dts, qry, tbl, cnn, trn, cds, mdt), menus (mnu, pmu, mni, tlb), dialogs (odlg, sdlg, cdlg, fdlg), timing/comms (tmr, rtc, rtr, rsp), and FMX-specific (tbc, tbi, swt, cal).

**Step 2: Commit**

```bash
git add skills/delphi-standards/references/component-prefixes.md
git commit -m "feat: add component-prefixes reference"
```

---

## Task 9: Skill `delphi-write`

**Files:**
- Create: `skills/delphi-write/SKILL.md`

**Step 1: Create SKILL.md**

Contains frontmatter with auto-trigger on new code intent, complete writing checklist (naming, formatting, structure, security, prohibitions), and writing flow (identify element type → sketch structure → write complete code).

**Step 2: Commit**

```bash
git add skills/delphi-write/SKILL.md
git commit -m "feat: add delphi-write skill"
```

---

## Task 10: Skill `delphi-audit` — Port from Production

**Files:**
- Create: `skills/delphi-audit/SKILL.md`
- Create: `skills/delphi-audit/references/audit-report-structure.md` (was estrutura-laudo)
- Create: `skills/delphi-audit/references/clean-code-delphi.md`
- Create: `skills/delphi-audit/references/code-smells-delphi.md`
- Create: `skills/delphi-audit/references/delphi-technologies.md` (was tecnologias-delphi)
- Create: `skills/delphi-audit/references/modernization-estimates.md` (was estimativas-modernizacao)
- Create: `skills/delphi-audit/references/style-guide.md`

**Step 1:** Port all files from the original production skill, translating from Portuguese to English where applicable.

**Step 2: Commit**

```bash
git add skills/delphi-audit/
git commit -m "feat: add delphi-audit skill (ported and translated from production)"
```

---

## Task 11: Skill `delphi-spec`

**Files:**
- Create: `skills/delphi-spec/SKILL.md`
- Create: `skills/delphi-spec/references/spec-template.md`

**Step 1:** Create the specification generation skill with auto-trigger on `/spec` command.

**Step 2: Commit**

```bash
git add skills/delphi-spec/
git commit -m "feat: add delphi-spec skill"
```

---

## Task 12: Skill `delphi-tests`

**Files:**
- Create: `skills/delphi-tests/SKILL.md`

**Step 1:** Create the DUnitX test generation skill, ported and translated from the Portuguese `delphi-testes` version.

**Step 2: Commit**

```bash
git add skills/delphi-tests/
git commit -m "feat: add delphi-tests skill (DUnitX test generation)"
```

---

## Task 13: Skill `delphi-ignore`

**Files:**
- Create: `skills/delphi-ignore/SKILL.md`

**Step 1:** Create the `.claudeignore` management skill for token optimization in Delphi projects.

**Step 2: Commit**

```bash
git add skills/delphi-ignore/
git commit -m "feat: add delphi-ignore skill (claudeignore management)"
```

---

## Task 14: Agent `delphi-auditor`

**Files:**
- Create: `agents/delphi-auditor.md`

**Step 1: Create agent file**

Contains frontmatter with description and model:inherit, the 4-step audit protocol (Initial Assessment → Systematic Analysis → Scoring → Report), and professional tone guidelines.

**Step 2: Commit**

```bash
git add agents/delphi-auditor.md
git commit -m "feat: add delphi-auditor agent"
```

---

## Task 15: Agent `delphi-writer`

**Files:**
- Create: `agents/delphi-writer.md`

**Step 1: Create agent file**

Contains inviolable rules (prohibited patterns), the 3-step writing process (Understand → Sketch → Write complete), and automatic standards application.

**Step 2: Commit**

```bash
git add agents/delphi-writer.md
git commit -m "feat: add delphi-writer agent"
```

---

## Task 16: Agent `delphi-spec-writer`

**Files:**
- Create: `agents/delphi-spec-writer.md`

**Step 1: Create agent file**

Specialized subagent for generating SPEC documents from source code analysis.

**Step 2: Commit**

```bash
git add agents/delphi-spec-writer.md
git commit -m "feat: add delphi-spec-writer agent"
```

---

## Task 17: Agent `delphi-tester`

**Files:**
- Create: `agents/delphi-tester.md`

**Step 1: Create agent file**

Specialized subagent for creating DUnitX unit test suites for Delphi classes.

**Step 2: Commit**

```bash
git add agents/delphi-tester.md
git commit -m "feat: add delphi-tester agent"
```

---

## Task 18: Commands

**Files:**
- Create: `commands/audit.md`
- Create: `commands/review.md`
- Create: `commands/write.md`
- Create: `commands/new-project.md`
- Create: `commands/spec.md`
- Create: `commands/tdd.md`
- Create: `commands/about.md`

**Step 1–7:** Create all 7 command files, each with frontmatter description and clear instructions.

**Step 8: Commit**

```bash
git add commands/
git commit -m "feat: add all commands (audit, review, write, new-project, spec, tdd, about)"
```

---

## Task 19: Factory.ai Droid Harness

**Files:**
- Create: `.factory/droids/delphi-expert.md`
- Create: `.factory/droids/delphi-auditor.md`
- Create: `.factory/droids/delphi-writer.md`
- Create: `.factory/droids/delphi-spec-writer.md`
- Create: `.factory/droids/delphi-tester.md`

**Step 1:** Create 5 droid configuration files with embedded knowledge for optimal AI usage.

**Step 2: Commit**

```bash
git add .factory/droids/
git commit -m "feat: add Factory.ai droid harness (5 droids)"
```

---

## Task 20: README

**Files:**
- Create: `README.md`

**Step 1:** Create English-only README with features table, installation instructions, standards summary, skills/agents/droids tables, and license.

**Step 2: Commit**

```bash
git add README.md
git commit -m "docs: add README"
```

---

## Task 21: Verification

**Step 1: Verify complete structure**

```bash
find . -not -path "*/.git/*" -not -path "*/.git" | sort
```

**Step 2: Verify git log**

```bash
git log --oneline
```

**Step 3: Push to repository**

```bash
git remote add origin https://github.com/adrianosantostreina/delphi-dev.git
git push -u origin master
```

---

## Post-Implementation Steps

1. **Install locally for testing:**
   ```bash
   /plugin install delphi-dev@github:adrianosantostreina/delphi-dev
   ```

2. **For Factory.ai droids:** Already available in `.factory/droids/` — no separate installation needed.

3. **Submit to marketplace:**
   - `https://clau.de/plugin-directory-submission`
