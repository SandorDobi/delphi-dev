# Code Smells Catalog in Delphi
## Based on Martin Fowler + Robert C. Martin + real technical audit reports

---

## 1. Long Method

**Definition:** Method with more than 50 lines of code.

**How to detect:**
```bash
find /project -name "*.pas" -exec wc -l {} + | sort -rn | head -20
grep -n "procedure\|function" file.pas | awk -F: 'prev{print $2-prev, prev_name} {prev=$2; prev_name=$0}'
```

**Real examples detected in audits:**
- `procedure TFOrders.FormCreate` — 300 lines
- `procedure TFOrders_DST.SaveOrder` — 1030 lines
- `procedure TfServerMT2.QueryOrder` — 706 lines

**Solution:** Extract Method — extract portions into smaller methods with meaningful names.

**Severity levels:**
- 50–100 lines: ⚠️ ATTENTION
- 100–300 lines: ⚠️ HIGH attention
- 300–500 lines: 🚨 CRITICAL
- 500+ lines: 🚨 God Method

---

## 2. God Class / God Method

**Definition:** A class or method that does everything — multiple completely distinct responsibilities.

**Symptoms:**
- Form with fiscal logic, order logic, email logic, and report logic at the same time
- DataModule with SQL from completely different modules
- `SaveOrder` that also calculates taxes, sends NF-e, and triggers email

**Real examples:**
- `UOrders_DST.pas` — 13,000 lines
- `UCalculationBilling.pas` — multiple methods of 1500+ lines
- `procedure SaveOrder` — 1030 lines with fiscal, inventory, and email logic

**Solution:** Extract Class — separate responsibilities into specialized classes.

---

## 3. Long Parameter List / Polyadic

**Definition:** Method with 4 or more parameters.

**How to detect:**
```bash
grep -n "procedure\|function" file.pas | grep -E "(.+;){3,}"
```

**Real examples:**
```pascal
// Polyadic — 7 parameters
procedure CreateForeignKey(pTableFK, pFieldsFK, pTableRef,
  pFieldsRef: String; pDeleteCascade: Boolean = False;
  pAddText: String = ''; pSetNullCascade: Boolean = False);

// Polyadic — 10 parameters
function LocalCustomer(pCPF, pName, pAddress, pNumber,
  pNeighborhood, pZipCode, pCity, pState, pIfoodId, pComplement: string): integer;
```

**Solution:**
- Create `Record` or class to group related parameters
- Use fluent interface (Fluent API)
- Separate responsibilities (the method is probably doing too much)

---

## 4. Duplicate Code

**Definition:** The same or very similar code appears in two or more places.

**How to detect:**
```bash
# Find methods with same name in different units
grep -rn "procedure ConfirmTrans\|function ConfirmTrans" /project --include="*.pas"
# Find identical SQL blocks
grep -rn "SQL.Add\|SQL.Text\s*:=" /project --include="*.pas" | sort | uniq -d
```

**Real examples:**
- `ConfirmTrans` repeated in two different units
- `CalculateTotals` possibly duplicated in `UCalculationBilling` and `UCalculationTax`
- REST API iFood call block repeated 26 times with ~10 lines each (260 redundant lines)

**Risks:**
- Bug fix must be done in multiple places
- Behavior divergence over time
- Unnecessary code size increase

**Solution:** Extract Method — centralize in a single location.

---

## 5. Dead Code

**Definition:** Commented code, never executed, or that doesn't affect behavior.

**How to detect:**
```bash
# Commented blocks { ... }
grep -c "^{" file.pas
grep -c "^//" file.pas
# Methods/procedures never called (verify manually)
```

**Real examples:**
```pascal
// Forgotten code commented out for years
{ TbOrd_Prods.CloseOpen(True);
  TbOrd_Servs.CloseOpen(True); }

// Entire SQL commented out with note
{if Sender = TableCODNATOPER then
begin
  Data.QryAux.SQL.Text := 'select M.DESCRIPTION...'
  ...
end; }
```

**Solution:** Remove it. Git keeps the history.

---

## 6. Primitive Obsession

**Definition:** Excessive use of primitive types (String, Integer, Double) where objects
or Records would be more appropriate.

**Symptoms:**
```pascal
// BAD — address data scattered as primitive strings
procedure SaveCustomer(AName, ACPF, AAddress, ANumber,
  ANeighborhood, AZipCode, ACity, AState: string);

// GOOD — encapsulated in Record
type
  TAddress = record
    Street: string;
    Number: string;
    Neighborhood: string;
    ZipCode: string;
    City: string;
    State: string;
  end;
procedure SaveCustomer(const AName, ACPF: string;
  const AAddress: TAddress);
```

**Other examples:**
- Using `Integer` to represent status when a `TOrderStatus = (osOpen, osClosed)` would be clearer
- Using `Double` for monetary value when `Currency` is more correct
- Using `String` for CPF/CNPJ without encapsulated validation

---

## 7. Feature Envy

**Definition:** A method that seems more interested in another class's data than its own.

```pascal
// BAD — TFOrders directly accessing another module's DataModule data
procedure TFOrders.CalculateFreight;
begin
  LRate := DMTax.QryRates.FieldByName('RATE').AsFloat;
  LState := DMCustomer.QryCustomer.FieldByName('STATE').AsString;
  LZipDest := DMCustomer.QryCustomer.FieldByName('ZIP').AsString;
  // ...
end;

// GOOD — freight calculation belongs to a freight service
LFreight := TFreightService.Calculate(ACustomer, AItems);
```

---

## 8. Data Clumps

**Definition:** Groups of variables that always appear together — candidates for Record or Class.

```pascal
// BAD — LName, LCPF, LCNPJ always appear together
procedure Proc1(ALName, ALCPF, ALCNPJ: string);
procedure Proc2(ALName, ALCPF, ALCNPJ: string);

// GOOD
type
  TCustomerData = record
    Name: string;
    CPF: string;
    CNPJ: string;
  end;
```

---

## 9. Cyclomatic Complexity

**Definition:** High number of execution paths in a method — excessive
nested if/case/while/and/or.

**How to measure:** Number of decision points + 1. Ideal: up to 5. Above 10: high risk.

```pascal
// BAD — extremely high cyclomatic complexity
procedure ProcessOrder;
begin
  if ACustomer.Active then
  begin
    if ACustomer.CreditLimit > 0 then
    begin
      if AOrder.Total <= ACustomer.CreditLimit then
      begin
        if AOrder.Items.Count > 0 then
        begin
          if not AInventory.OutOfStock then
          begin
            // 5 levels of nesting
          end;
        end;
      end;
    end;
  end;
end;
```

**Solution:** Guard clauses, Extract Method, Strategy Pattern.

---

## 10. Shotgun Surgery

**Definition:** A single change requires modifications in many different places.

**Symptoms:**
- Changing a business rule requires modifying 10+ units
- Renaming a database field requires searching hundreds of files
- Hardcoded SQL scattered across files using the same field

**Solution:** Centralize rules — repositories, constants, configurations.

---

## 11. Inappropriate Intimacy

**Definition:** Forms/units directly accessing internal fields of other forms/units.

```pascal
// BAD — accessing private field of another Form
FBillings.wwDBGrid4.Visible := False;
FOrders.Edit1.Text := LValue;

// GOOD — through public method/property
FBillings.HideGrid;
FOrders.InformValue(LValue);
```

---

## 12. Magic Numbers

**Definition:** Numeric literals without explicit meaning in the code.

```pascal
// BAD
if LStatus = 2 then ShowMessage('Approved');
if LDays > 30 then ChargeInterest;
LValue := LBase * 0.175;

// GOOD
const
  C_STATUS_APPROVED = 2;
  C_MAX_DEADLINE_DAYS = 30;
  C_ICMS_RATE_SP = 0.175;

if LStatus = C_STATUS_APPROVED then ShowMessage('Approved');
if LDays > C_MAX_DEADLINE_DAYS then ChargeInterest;
LValue := LBase * C_ICMS_RATE_SP;
```

---

## 13. RecordCount — Performance Code Smell

**Definition:** Using `RecordCount` when only `IsEmpty` is needed.

**Why it's a problem:** `RecordCount` traverses ALL DataSet records to count them.
In Client/Server systems with large tables, this can cause visible freezing.

```bash
# Count occurrences in the project
grep -rn "RecordCount" /project --include="*.pas" | wc -l
```

**Real examples:** 750+ occurrences (WMC), 720+ occurrences (ThR)

```pascal
// BAD — RecordCount to check if data exists
if qryOrders.RecordCount > 0 then ProcessOrders;

// GOOD — IsEmpty is O(1), doesn't traverse records
if not qryOrders.IsEmpty then ProcessOrders;

// If you need a real count:
// SELECT COUNT(*) FROM TABLE WHERE CONDITION
```

---

## 14. Locate — Performance Code Smell

**Definition:** Excessive use of `Locate` on large DataSets.

**Why it's a problem:** `Locate` traverses records sequentially without using indexes.

**Real examples:** 320+ occurrences (ThR)

```pascal
// BAD
qryProducts.Locate('CODPRODUCT', LCode, []);

// GOOD — FindKey uses a previously created index
qryProducts.IndexFieldNames := 'CODPRODUCT';
qryProducts.FindKey([LCode]);
// or FindNearest for partial search
```

---

## 15. WITH Statement — Structural Code Smell

**Definition:** The `with` command is forbidden by the Delphi Style Guide.

**Why it's a problem:**
- Makes debugging difficult (which object is being referenced?)
- Confuses the compiler in ambiguities
- Makes refactoring and static analysis difficult

```pascal
// BAD — with forbidden
with AuxQry do
begin
  Close;
  SQL.Clear;
  SQL.Add('SELECT...');
  Open;
end;

// GOOD — explicit reference
LAuxQry.Close;
LAuxQry.SQL.Clear;
LAuxQry.SQL.Add('SELECT...');
LAuxQry.Open;
```

---

## 16. Scattered SQL — Architectural Code Smell

**Definition:** SQL statements embedded directly in Forms and DataModules.

**How to detect:**
```bash
grep -rn "SQL.Add\|SQL.Text\s*:=" /project --include="*.pas" | wc -l
grep -l "SQL.Add" /project --include="*.pas"
```

**Solution:** Centralize in repository/SQL units:
```pascal
// Unit: System.SQL.Orders.pas
const
  C_SQL_FIND_ORDER_BY_CUSTOMER =
    'SELECT O.ORDERNUM, O.ORDERDATE, O.TOTAL ' +
    'FROM ORDERS O ' +
    'WHERE O.CUSTOMERID = :CUSTOMERID ' +
    'AND O.STATUS = :STATUS ' +
    'ORDER BY O.ORDERDATE DESC';
```
