---
description: Quick Delphi code review — detects standard violations and suggests corrections
---

Perform a quick review of the Delphi code provided by the user.

Analyze and report:

**Naming**
- Correct prefixes (F, A, L, C_, T, I, E)?
- Hungarian notation detected?
- Meaningful names or obscure abbreviations?
- Components correctly renamed?

**Formatting**
- 2-space indentation (no TAB)?
- 120-char margin respected?
- `begin` on its own line?
- `else` on its own line?

**Forbidden Statements**
- Use of `with`?
- `Break` or `Continue` in loops?
- `Exit` outside guard clauses?
- `Real` as a type?
- Concatenated SQL (SQL Injection)?

**Structure**
- Methods with more than 50 lines?
- Multiple responsibilities in the same class/method?
- try..finally with multiple resources?
- `const` on interface parameters?

**Output format:**

For each violation found:
- Approximate line and problem description
- Classification: 🚨 CRITICAL / ⚠️ ATTENTION / 💡 SUGGESTION
- Correct example of how it should be written
