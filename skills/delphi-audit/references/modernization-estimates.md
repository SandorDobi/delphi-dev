# Modernization Effort Estimation Guide for Delphi

## Estimation Methodology

Use the multipliers below based on system size:

| System Size | Forms | Units | Lines of Code |
|---|---|---|---|
| Small | < 20 | < 50 | < 10,000 |
| Medium | 20–80 | 50–200 | 10,000–50,000 |
| Large | 80–200 | 200–500 | 50,000–200,000 |
| Enterprise | > 200 | > 500 | > 200,000 |

---

## Activities and Base Estimates (medium system)

### BDE to FireDAC Migration
| Subtask | Effort (days) |
|---|---|
| Map all BDE connections | 2–3 |
| Replace TTable with TFDTable | 3–5 per DataModule |
| Replace TQuery with TFDQuery | 2–3 per DataModule |
| Adjust incompatible SQL | 1–2 per unit |
| Regression testing | 5–10 |
| **Medium system total** | **20–40 days** |

> Risk factor: +50% if the system uses BLOB fields or Paradox/dBase-specific stored procedures

### ADO to FireDAC Migration
| Subtask | Effort (days) |
|---|---|
| Map ADO connections | 1–2 |
| Replace TADOQuery | 1–2 per DataModule |
| SQL compatibility testing | 3–5 |
| **Medium system total** | **10–20 days** |

### Business Rule Extraction (Forms → Classes)
| Subtask | Effort (days) |
|---|---|
| Analysis and mapping of rules per Form | 1–2 per Form |
| Domain class creation | 2–3 per module |
| Form refactoring | 1–2 per Form |
| Basic unit testing | 3–5 |
| **Medium system total (30 forms)** | **60–120 days** |

> This is typically the highest effort activity in a modernization

### Delphi Version Upgrade
| From → To | Effort (days) |
|---|---|
| Delphi 7 → Delphi 10.4+ | 15–40 |
| Delphi 2007/2009 → Delphi 11/12 | 10–25 |
| Delphi XE → Delphi 12 | 5–15 |
| Delphi 10.x → Delphi 12 | 2–8 |

> Main effort sources in version migration:
> - Third-party components without updated version
> - AnsiString → UnicodeString strings (Delphi < 2009)
> - Unsafe typecasts
> - Version-specific compilation directives

### Security Implementation
| Subtask | Effort (days) |
|---|---|
| Authentication with password hashing (bcrypt/SHA-256) | 3–5 |
| Profiles/permissions system | 5–10 |
| Centralized audit logging | 3–5 |
| Configuration encryption | 2–3 |
| **Total** | **13–23 days** |

### Technical Documentation
| Subtask | Effort (days) |
|---|---|
| Units/procedures documentation | 0.5 day per 500 lines |
| Developer manual | 3–5 |
| Architecture diagram | 2–3 |
| **Medium system total** | **10–20 days** |

---

## Consolidated Estimation Formula

```
Total Effort = Sum of activities × Size Factor × Risk Factor

Size Factor:
  Small     = 0.5x
  Medium    = 1.0x  (base)
  Large     = 2.5x
  Enterprise = 5.0x+

Risk Factor (accumulate applicable ones):
  No existing documentation       = +20%
  High team turnover               = +15%
  No test environment              = +25%
  Unsupported components            = +20%
  Legacy database (Access)          = +30%
  External system integrations     = +15% per integration
```

---

## Estimation Examples

### Example 1: Small Financial System (BDE + Delphi 7)
```
Activities:
  BDE → FireDAC migration:              15 days
  Delphi 7 → 12 upgrade:                25 days
  Rule extraction (10 forms):           25 days
  Security implementation:               15 days
  Documentation:                          8 days
                              Subtotal: 88 days

Size Factor (Small):                    × 0.5 = 44 days
Risk Factors (no docs + no tests):      +45% = ~64 days

Final Estimate: ~3 months with 1 senior dev
```

### Example 2: Medium ERP (ADO + Delphi 2009)
```
Activities:
  ADO → FireDAC migration:              15 days
  Delphi 2009 → 12 upgrade:             20 days
  Rule extraction (40 forms):            80 days
  Security implementation:               20 days
  Documentation:                         15 days
                              Subtotal: 150 days

Size Factor (Medium):                   × 1.0 = 150 days
Risk Factors (components + integrations): +35% = ~200 days

Final Estimate: ~10 months with 1 senior dev
                  or ~5 months with 2 senior devs
```

---

## Recommendations for Estimation Presentation

1. **Always present a range** (minimum–maximum), never a fixed number
2. **Detail the assumptions** (team, dedication, environment access)
3. **Indicate what is NOT included** (training, infrastructure, data)
4. **Suggest validation** of estimates after more detailed analysis (discovery phase)
5. **Phase the delivery** to reduce risk and generate incremental value
