# Clean Code in Delphi
## Based on "Clean Code" — Robert C. Martin

---

## 1. Meaningful Names

### 1.1 General Rule
Names must reveal intent. If a name needs a comment to be understood, it's wrong.

```pascal
// BAD
var d: Integer; // days since creation

// GOOD
var LDaysSinceOpening: Integer;
```

### 1.2 Scope Prefixes

| Scope | Prefix | Example |
|---|---|---|
| Local variable | `L` | `LValueICMS`, `LAuxQry` |
| Field/attribute | `F` | `FTotalValue`, `FCustomerCode` |
| Parameter | `A` | `ARate`, `AValue`, `AName` |
| Constant | `C_` | `C_SQL_ORDERS`, `C_MAX_ATTEMPTS` |
| Button component | `btn` | `btnSave`, `btnCancel` |
| Edit component | `edt` | `edtName`, `edtCPF` |
| Label component | `lbl` | `lblTotal`, `lblStatus` |
| Combo component | `cbx` | `cbxState`, `cbxStatus` |
| Grid component | `grd` | `grdOrders`, `grdItems` |
| Query | `qry` | `qryCustomers`, `qryOrders` |

> NEVER use `p` as a parameter prefix (confuses with pointer). The Delphi standard uses `A`.

### 1.3 Renaming Examples

```pascal
// BAD — abbreviations, cryptic acronyms, no prefix
procedure CalcICMS(pRate: Double; pVal: Double);
var i, v, s: Integer;
procedure ConfirmTrans;
procedure LaunchServMaster;

// GOOD — clear intent, standard applied
procedure CalculateICMS(const ARate: Double; const AValue: Double);
var LIndex, LValue, LStatus: Integer;
procedure ConfirmTransaction;
procedure LaunchServerMaster;
```

### 1.4 Components
Every component referenced via code must be renamed:

```pascal
// BAD — Delphi default name
Edit1.Text := 'Adriano';
Label1.Caption := 'Name:';
Button1.Enabled := False;

// GOOD — meaningful name with prefix
edtName.Text := 'Adriano';
lblName.Caption := 'Name:';
btnSave.Enabled := False;
```

---

## 2. Functions and Methods

### 2.1 Size
- **Ideal:** up to 20 lines (Clean Code original)
- **Recommended for Delphi:** up to 50 lines
- **Maximum tolerance:** +5 lines (55 total)
- **Above 100 lines:** Long Method — document as Code Smell
- **Above 300 lines:** Critical
- **Above 500 lines:** God Method — 🚨 CRITICAL

### 2.2 Parameters (Polyadic)
- **0–2 parameters:** ideal
- **3 parameters:** tolerable
- **4+ parameters:** Polyadic — critical Code Smell

```pascal
// BAD — Polyadic (7 parameters)
procedure OnSendInvoicesACBR(ADateSent: TDate; ASentTime: TTime;
  APreview: Boolean; ASelectedList: TwwBookmarkList;
  ADateIssued: TDateTime; AContingSVC: Boolean;
  ABatchIssuance: Boolean);

// GOOD — use Record to group related parameters
type
  TInvoiceIssuanceParams = record
    DateSent: TDate;
    SentTime: TTime;
    Preview: Boolean;
    DateIssued: TDateTime;
    ContingSVC: Boolean;
    BatchIssuance: Boolean;
  end;

procedure SendInvoicesACBR(const AParams: TInvoiceIssuanceParams;
  ASelectedList: TwwBookmarkList);
```

### 2.3 Single Responsibility
Each method does ONE thing. If you need to write "and" in the method description, it's wrong.

```pascal
// BAD — does everything at once (God Method)
procedure TFOrders.SaveOrder; // 1030 lines doing validation, calculation, saving, NF-e, email...

// GOOD — separated responsibilities
procedure TfrmOrders.BtnSaveClick(Sender: TObject);
begin
  if not ValidateRequiredFields then Exit;
  if not ConfirmSave then Exit;
  SaveOrder;
end;

procedure TOrderService.SaveOrder(const AOrder: TOrder);
begin
  ValidateOrder(AOrder);
  CalculateTotals(AOrder);
  SaveToDatabase(AOrder);
end;
```

### 2.4 Guard Clauses (correct use of Exit)
`Exit` is permitted only at the beginning of the method as a guard clause:

```pascal
// GOOD — guard clauses
procedure TfrmOrders.SaveClick(Sender: TObject);
begin
  if not ValidateCustomer then Exit;
  if not ValidateItems then Exit;
  if not ConfirmOperation then Exit;

  // main code here, no nested ifs
  SaveOrder;
end;
```

### 2.5 Result in Functions

```pascal
// BAD — unnecessary auxiliary variable
function CalculateDiscount(const AValue: Double; const APercentage: Double): Double;
var LDiscount: Double;
begin
  LDiscount := AValue * APercentage / 100;
  Result := LDiscount;
end;

// GOOD — use Result directly
function CalculateDiscount(const AValue: Double; const APercentage: Double): Double;
begin
  Result := AValue * APercentage / 100;
end;
```

---

## 3. Comments

### 3.1 Comments that explain "why" (good)
```pascal
// Tax Rule: ICMS must be calculated before IPI per IN 971/2009
LBaseICMS := CalculateICMSBase(AItem);
```

### 3.2 Comments that explain "how" (bad — code should be self-explanatory)
```pascal
// BAD — the name already says what it does
// Increments the counter
LCounter := LCounter + 1;

// Checks if the customer is active
if LCustomer.Active then
```

### 3.3 Dead Code in comments (Code Smell)
```pascal
// BAD — remove commented code; Git keeps the history
{ procedure TFOrders.TableAfterRefresh(DataSet: TDataSet);
begin
  TbOrd_Prods.CloseOpen(True);
  TbOrd_Servs.CloseOpen(True);
end; }
```

---

## 4. Formatting

### 4.1 Indentation and Margins
- 2 spaces per level — NEVER TABs
- Right margin: 120 characters
- Commands exceeding the margin: break line with 2 additional spaces

### 4.2 Begin/End and Else
```pascal
// BAD
procedure Example; begin
  if Condition then begin x := 1; end
  else begin x := 2; end;
end;

// GOOD
procedure Example;
begin
  if Condition then
  begin
    x := 1;
  end
  else
  begin
    x := 2;
  end;
end;
```

### 4.3 Uses Declaration
```pascal
// BAD — everything on one line
uses Windows, Messages, SysUtils, Variants, Classes, Graphics, Controls, Forms;

// GOOD — one per line, grouped by origin
uses
  // RTL/VCL
  Windows,
  Messages,
  SysUtils,
  Classes,
  Graphics,
  Controls,
  Forms,
  // Third-party
  FireDAC.Comp.Client,
  FastReport,
  // Project
  System.Model.Customer,
  System.Service.Order;
```

### 4.4 Variable Declarations
```pascal
// BAD — multiple variables per line
var LName, LLastName: string; LAge, LCode: Integer;

// GOOD — one per line, grouped by type
var
  LName: string;
  LLastName: string;
  LAge: Integer;
  LCode: Integer;
```

---

## 5. Exception Handling

```pascal
// BAD — multiple resources in a single try..finally
procedure Example;
var LQuery1, LQuery2: TFDQuery;
begin
  LQuery1 := TFDQuery.Create(nil);
  LQuery2 := TFDQuery.Create(nil);
  try
    // ...
  finally
    LQuery1.Free;
    LQuery2.Free; // if LQuery1.Free raises an exception, LQuery2 leaks!
  end;
end;

// GOOD — one resource per try..finally
procedure Example;
var LQuery1, LQuery2: TFDQuery;
begin
  LQuery1 := TFDQuery.Create(nil);
  try
    LQuery2 := TFDQuery.Create(nil);
    try
      // ...
    finally
      LQuery2.Free;
    end;
  finally
    LQuery1.Free;
  end;
end;
```

---

## 6. SOLID Principles in Delphi

### SRP — Single Responsibility Principle
Each class/unit has a single reason to change.
```pascal
// BAD — order unit with fiscal calculation, email sending, and logging
// GOOD — TOrderService, TTaxCalculationService, TEmailService, TLogService separated
```

### OCP — Open/Closed Principle
Open for extension, closed for modification. Use inheritance and interfaces.

### DIP — Dependency Inversion Principle
Depend on abstractions (interfaces), not implementations:
```pascal
// BAD — direct dependency
FService := TCustomerServiceFireDAC.Create;

// GOOD — dependency on interface
FService: ICustomerService;
FService := TCustomerServiceFireDAC.Create;
```

---

## 7. Fluent Interface

An elegant alternative to Polyadic — reducing parameters using chaining:

```pascal
// BAD — Polyadic
procedure ConfigureReport(ATitle: string; ADate: TDate;
  AFilterActive: Boolean; ASortOrder: string; AShowTotals: Boolean);

// GOOD — Fluent Interface
TReport.New
  .WithTitle('Customer Report')
  .ForDate(Date)
  .ActiveOnly
  .SortedBy('Name')
  .WithTotals
  .Generate;
```
