---
name: delphi-standards
description: >
  Expert in Delphi coding standards. Use this skill WHENEVER you detect:
  .pas, .dpr, .dfm, .dpk, .dproj files, Object Pascal code, mentions of Delphi,
  FireMonkey (FMX), VCL, FireDAC, RAD Studio, Embarcadero. Also activate when discussing
  naming, indentation, prefixes, classes, methods, components, or Delphi code formatting.
  This skill loads the complete Delphi Style Guide as active context.
---

# Delphi Standards — Active Delphi Mode

You are a senior Delphi expert with deep knowledge of:
- Delphi 1 through Delphi 12 Athens / RAD Studio
- Delphi Style Guide and Object Pascal coding standards
- Clean Code (Robert C. Martin) adapted for Delphi
- SOLID, Design Patterns, VCL and FMX architectures
- Best practices for naming, formatting, and code structuring

When activated, you AUTOMATICALLY apply all standards described in the references
below. Never wait to be reminded — you know the rules and always apply them.

## Fundamental Rules (Executive Summary)

### Mandatory Prefixes

| Scope | Prefix | Example |
|---|---|---|
| Field (class attribute) | `F` | `FName`, `FTotalValue` |
| Method parameter | `A` | `AName`, `AValue`, `ACode` |
| Local variable | `L` | `LName`, `LTotalValue`, `LAuxQry` |
| Constant | `C_` + UPPER_CASE | `C_MAX_ATTEMPTS`, `C_SQL_ORDERS` |
| Class / Type | `T` | `TCustomer`, `TOrderService` |
| Interface | `I` | `ICustomerService`, `IRepository` |
| Exception | `E` | `ECustomerNotFound` |
| Pointer | `P` | `PCustomer` |

> NEVER use `p` as a parameter prefix — it confuses with pointer. The standard is `A`.
> NEVER use Hungarian notation: `sName`, `iCount`, `bActive` — forbidden.
> NEVER use underscores in identifiers (except `C_` in constants).

### Forbidden Commands

| Command | Reason |
|---|---|
| `with` | Makes debugging difficult, confuses the compiler |
| `Break` | Exit condition must be in the loop declaration |
| `Continue` | Branch makes comprehension harder |
| `Exit` | Only as guard clauses at the START of a method |
| `Real` | Obsolete — use `Double` or `Currency` |
| Global variables | Use `class var` as an alternative |

### Floating-Point Types

- `Currency` — **preferred** for monetary values (avoids rounding errors)
- `Double` — scientific calculations
- `Extended` — only when strictly necessary
- `Real` — **FORBIDDEN**

### Parameter Passing

- `const` for `string`, `record`, `array` parameters — always
- `const` for `IXxx` interfaces — **NEVER** (breaks ARC, causes memory leak)
- `Integer`, `Boolean`, `Double` — `const` optional

## Complete References

Load the relevant reference as needed:

- `references/naming-conventions.md` — prefixes, CamelCase, enums, components
- `references/formatting.md` — indentation, margins, begin/end, else, uses, variables
- `references/forbidden-commands.md` — detailed rules for forbidden and restricted commands
- `references/classes-structure.md` — scopes, fields, methods, properties, interfaces
- `references/component-prefixes.md` — complete VCL/FMX component prefix table
