# Naming Conventions — Delphi Style Guide

## 1. CamelCase

All identifiers use CamelCase (each word starts with uppercase).
Acronyms remain in UPPER_CASE: `CalculateICMS`, `ValidateCNPJ`, `FindCEP`.

## 2. Scope Prefixes

| Scope | Prefix | Examples |
|---|---|---|
| Field (private attribute) | `F` | `FName`, `FTotalValue`, `FCustomerCode` |
| Method parameter | `A` | `AName`, `AValue`, `ACode`, `ARate` |
| Local variable | `L` | `LName`, `LAuxQry`, `LValueICMS` |
| Constant | `C_` | `C_MAX_ATTEMPTS`, `C_SQL_FIND_CUSTOMER` |
| Class | `T` | `TCustomer`, `TOrderService`, `TCalculationICMS` |
| Interface | `I` | `ICustomerService`, `IRepository`, `IOrder` |
| Exception (inherits Exception) | `E` | `ECustomerNotFound`, `EOrderInvalid` |
| Pointer | `P` | `PCustomer`, `PNode` |

### Prefix Prohibitions

- ❌ `p` as parameter — confuses with Pascal pointer
- ❌ Hungarian notation: `sName`, `iCount`, `bActive`, `dValue`
- ❌ Underscores in identifiers (except `C_` in constants)
- ❌ Cryptic abbreviations: `Vlr`, `Qty`, `Func` → use `Value`, `Quantity`, `Employee`

## 3. Methods and Functions

- Meaningful names with **infinitive verb**: `CalculateICMS`, `ValidateCPF`, `SaveOrder`
- CamelCase without prefix
- Getter: `Get` prefix — `GetName`, `GetTotalValue`
- Setter: `Set` prefix — `SetName`, `SetTotalValue`

## 4. Enumerated Types

Mnemonic lowercase prefix of 2+ letters matching the type name:

```pascal
type
  TOrderStatus  = (osOpen, osConfirmed, osInvoiced, osCanceled);
  TCustomerType = (ctIndividual, ctLegalEntity, ctForeign);
  TPaymentMethod = (pmCash, pmCredit, pmDebit, pmPIX);
```

Delimiters: comma next to the previous item, space before the next one.

## 5. Constants

```pascal
const
  C_MAX_ATTEMPTS      = 3;
  C_MAX_DEADLINE_DAYS = 30;
  C_ICMS_RATE_SP      = 0.175;
  C_SQL_FIND_CUSTOMER =
    'SELECT CODCLIENTE, NOME FROM CLIENTES WHERE CODCLIENTE = :COD';
```

- Globals: **discouraged** — declare inside the class when possible
- `C_` prefix mandatory
- Body in UPPER_CASE with underscores separating words

## 6. Visual Components

Every component referenced via code must be renamed.
See `component-prefixes.md` for the complete table.

```pascal
// ❌ WRONG — default name
Button1.Enabled := False;
Edit1.Text := '';

// ✅ CORRECT — renamed with prefix
btnSave.Enabled := False;
edtCustomerName.Text := '';
```

## 7. Global Variables

**Forbidden.** Use `class var` as an alternative:

```pascal
type
  TConfiguration = class
  strict private
    class var FInstance: TConfiguration;
  public
    class function GetInstance: TConfiguration;
  end;
```
