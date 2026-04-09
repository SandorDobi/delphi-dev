# Delphi Style Guide
## Based on ThR Softwares Coding Standards v4.0.1

---

## 1. IDE and Syntax

### 1.1 Indentation
- Standard: **2 spaces** per indentation level
- **NEVER use TAB** for indentation

### 1.2 Margins
- Right margin: **120 characters**
- Commands beyond the margin: break into multiple lines with 2 additional spaces
- Fluent syntax: place the `.` on the new line

### 1.3 Begin/End
- Every `if`, `while`, `for` block must have `begin..end`, except when it has only one line
- `begin` must appear on **its own line**
- `begin` does not receive extra indentation — aligned with the preceding command

```pascal
// CORRECT
if Condition then
begin
  Action1;
  Action2;
end
else
begin
  AlternativeAction;
end;

// Allowed exception (single line, no begin..end)
if not Active then Exit;
```

### 1.4 Parentheses
- No space between `(` and the next character
- No space between the previous character and `)`
- No space between procedure/function name and `(`

### 1.5 Char Case
- **Reserved words** in lowercase: `begin`, `end`, `if`, `then`, `else`, `while`, `for`, `function`, `procedure`, `string`, `array`, `record`, `type`, `var`, `const`
- **Primitive types** respect original casing: `Integer`, `Double`, `Boolean`, `Char`, `String` (when as type, not reserved word)
- Exception: `Register` (special procedure)

---

## 2. Commands

### 2.1 IF Command
- Compound conditions: order from **lowest to highest complexity** (left to right)
- `else` on its own line
- `if-if-if` chains → replace with `case` when possible

### 2.2 CASE Command
- Values in ascending order
- Each case indented relative to `case`
- Implementation on new indented line
- Blocks within cases: maximum 5 lines (including `begin..end`)
- `else` aligned with `case`, not indented

```pascal
case LStatus of
  0: OrderOpen;
  1: OrderConfirmed;
  2: OrderInvoiced;
  3: OrderCanceled;
else
  StatusUnknown;
end;
```

### 2.3 FORBIDDEN Commands
| Command | Status | Justification |
|---|---|---|
| `with` | 🚫 FORBIDDEN | Makes debugging difficult, confuses compiler |
| `Break` | 🚫 FORBIDDEN | Exit condition must be in the loop |
| `Continue` | 🚫 FORBIDDEN | Branch makes comprehension harder |
| `goto` | 🚫 FORBIDDEN | — |
| `Exit` | ⚠️ RESTRICTED | Only in guard clauses at the beginning of the method |

---

## 3. Exceptions

### 3.1 Try..Finally
- **Mandatory** for releasing resources whose release is the developer's responsibility
- **One resource per block** (safe practice)

```pascal
// CORRECT — one resource per try..finally
LObj1 := TClass1.Create;
try
  LObj2 := TClass2.Create;
  try
    // use of objects
  finally
    LObj2.Free;
  end;
finally
  LObj1.Free;
end;
```

### 3.2 Try..Except
- Use **only when there's a necessary reaction** to the error
- Don't use just to display a message (responsibility of the application's default handler)
- Don't catch generic `Exception` without rethrowing

---

## 4. CamelCase

### 4.1 Rules
- Compound identifiers: each word starts with an **uppercase letter**
- **No underscores** (except in constants)
- Acronyms remain in **uppercase**: `CalculateICMS`, `ValidateCNPJ`, `FindCEP`

---

## 5. Constants and Global Variables

### 5.1 Constants
- Globals: **discouraged** — declare inside the class/method when possible
- Naming: `UPPER_CASE` with `C_` as prefix
- Separate words with underscores

```pascal
const
  C_MAX_ATTEMPTS = 3;
  C_SQL_FIND_CUSTOMER = 'SELECT * FROM CUSTOMERS WHERE CUSTOMERCODE = :CODE';
  C_STATUS_ACTIVE = 1;
```

### 5.2 Global Variables
- **FORBIDDEN** — use `class var` as an alternative

---

## 6. Types

### 6.1 Naming
- Derived types: uppercase `T` prefix + CamelCase
- Pointers: uppercase `P` prefix
- Exceptions: uppercase `E` prefix

### 6.2 Enumerated Types
- Mnemonic lowercase prefix of 2+ letters matching the type name

```pascal
type
  TOrderStatus  = (osOpen, osConfirmed, osInvoiced, osCanceled);
  TCustomerType = (ctIndividual, ctLegalEntity, ctForeign);
```

### 6.3 Floating-Point Types
| Type | Usage |
|---|---|
| `Currency` | **Preferred** for monetary values (avoids rounding errors) |
| `Double` | Scientific, non-monetary calculations |
| `Extended` | Only when strictly necessary (size not optimized) |
| `Real` | **FORBIDDEN** — obsolete, exists only for Pascal compatibility |

---

## 7. Classes

### 7.1 Naming
- `T` prefix + CamelCase: `TCustomerService`, `TOrderRepository`, `TICMSCalculation`
- Exceptions inherit from `Exception` with `E` prefix: `ECustomerNotFound`

### 7.2 Visibility Scopes
Preferred order: from most restrictive to least restrictive:
```pascal
TMyClass = class
strict private
  FAttribute: string;
private
  FOtherAttribute: Integer;
protected
  procedure ProtectedMethod;
public
  property Attribute: string read FAttribute write SetAttribute;
published
  // only for components
end;
```

### 7.3 Fields (Attributes)
- Always in `private` or `strict private` (preferably `strict private`)
- `F` prefix + CamelCase: `FTotalValue`, `FCustomerCode`, `FName`

### 7.4 Methods
- Meaningful names with infinitive verb: `CalculateICMS`, `ValidateCPF`, `SaveOrder`
- CamelCase
- Return type: `:` next to the name, space before the type: `function GetName: string;`
- Getters: `Get` prefix; Setters: `Set` prefix
- Parameters: `A` prefix + CamelCase (`AValue`, `AName`, `ACode`)
- Parameters **without** type prefixes (`s`, `i`, `d` etc.)
- Always use `const` on parameters when possible
- Function result: write directly to `Result`
- Local variables: `L` prefix + CamelCase

### 7.5 Properties
- CamelCase without prefix: `TotalValue`, `CustomerCode`, `Name`

---

## 8. Anonymous Methods

- Declare on a new line with 2 spaces of indentation
- `begin` of anonymous method: new line aligned with declaration
- Other naming and parameter rules apply

---

## 9. Components

### 9.1 Standard Prefixes

| Component | Prefix |
|---|---|
| TButton | `btn` |
| TEdit | `edt` |
| TLabel | `lbl` |
| TComboBox | `cbx` |
| TListBox | `lst` |
| TPanel | `pnl` |
| TGroupBox | `grp` |
| TImage | `img` |
| TTimer | `tmr` |
| TDBGrid | `grd` |
| TDataSource | `dts` |
| TFDQuery / TQuery | `qry` |
| TFDConnection | `cnn` |
| TPageControl | `pgc` |
| TTabSheet | `tbs` |
| TMemo | `mmo` |
| TCheckBox | `chk` |
| TRadioButton | `rdb` |
| TDateTimePicker | `dtp` |
| TSpeedButton | `spb` |
| TBitBtn | `bbt` |
| TStringGrid | `sgr` |

### 9.2 Rule
- Every component **referenced via code** must be renamed with the prefix + meaningful name
- Non-referenced components: tolerable to keep default name

---

## 10. Declaration Organization

### Uses
```pascal
uses
  // RTL
  System.SysUtils,
  System.Classes,
  System.Generics.Collections,
  // VCL
  Vcl.Controls,
  Vcl.Forms,
  Vcl.Dialogs,
  // FireDAC
  FireDAC.Comp.Client,
  FireDAC.Stan.Def,
  // Third-party
  FastReport,
  ACBrNFe,
  // Project — by module
  System.Model.Customer,
  System.Service.Order,
  System.Repository.Order;
```

### Variables
```pascal
var
  // Booleans
  LActive: Boolean;
  LProcessed: Boolean;
  // Integers
  LCode: Integer;
  LQuantity: Integer;
  // Strings
  LName: string;
  LDescription: string;
  // Objects
  LCustomer: TCustomer;
  LQuery: TFDQuery;
```
