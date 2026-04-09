---
description: Generates a complete professional technical audit report of a Delphi project
---

Run a complete technical audit of the current Delphi project or the files
provided by the user.

Follow this protocol:

1. **Gathering** — collect: Delphi version, database, access component,
   system type, audit objective (modernization / audit / sale).

2. **Analysis** — evaluate the 8 dimensions: Architecture, Clean Code, Code Smells,
   SOLID, Memory, Data Access, Security, Maintainability.

3. **Scoring** — score 1–5 per dimension, final weighted average.
   - 4.0–5.0 = 🟢 GOOD
   - 3.0–3.9 = 🟡 FAIR
   - 2.0–2.9 = 🟠 CRITICAL
   - 1.0–1.9 = 🔴 UNVIABLE

4. **Report** — generate the complete audit report with: executive summary, analysis per
   dimension, critical points, recommendations (immediate / short-term / medium-term / strategic)
   and effort estimate for modernization.

Use the `delphi-auditor` agent for in-depth analysis when available.
Load the `delphi-audit` skill for detailed audit structure.
