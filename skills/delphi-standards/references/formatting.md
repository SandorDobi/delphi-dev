# Formatting and Style — Delphi Style Guide

## 1. Indentation and Margins

- **2 spaces** per indentation level — NEVER TAB
- **Right margin: 120 characters** — maximum line length allowed
- Commands beyond the margin: break with 2 additional spaces of indentation
- Fluent syntax: place the `.` on the new line

```pascal
// ✅ Correct fluent syntax
TMultiDialog4FMX
  .Dialog
  .SetTitle('Confirmation')
  .SetMessage('Do you want to continue?')
  .Buttons
    .AddButton('Yes', procedure begin Confirm; end)
    .AddButton('No', nil)
  .&End
  .Show;
```

## 2. Begin/End

- Every `if`, `while`, `for`, `repeat` block must have `begin..end`
- Exception: single-line may omit `begin..end`
- `begin` on **its own line** — never on the same line as the command
- `end` aligned with `begin`

```pascal
// ✅ CORRECT
if LActive then
begin
  ProcessCustomer;
  UpdateScreen;
end;

// ✅ Allowed exception (single line)
if not LActive then Exit;

// ❌ WRONG
if LActive then begin ProcessCustomer; end;
```

## 3. Else

- `else` always on **its own line**
- Never together with the preceding `end`

```pascal
// ✅ CORRECT
if LCondition then
begin
  Action1;
end
else
begin
  Action2;
end;

// ❌ WRONG
if LCondition then begin Action1; end else begin Action2; end;
```

## 4. Reserved Words

Always in lowercase:
`begin`, `end`, `if`, `then`, `else`, `while`, `for`, `do`, `repeat`, `until`,
`case`, `of`, `try`, `except`, `finally`, `raise`, `function`, `procedure`,
`class`, `type`, `var`, `const`, `uses`, `interface`, `implementation`,
`string`, `array`, `record`, `object`

Primitive types respect original casing:
`Integer`, `Double`, `Boolean`, `Char`, `Currency`, `Extended`, `Byte`

## 5. Parentheses

- No space between `(` and the next character
- No space between the previous character and `)`
- No space between method name and `(`

```pascal
// ✅ CORRECT
LValue := CalculateICMS(ABase, ARate);

// ❌ WRONG
LValue := CalculateICMS( ABase, ARate );
LValue := CalculateICMS (ABase, ARate);
```

## 6. Variable Declarations

One variable per line, grouped by type:

```pascal
// ✅ CORRECT
var
  LName: string;
  LLastName: string;
  LAge: Integer;
  LTotalValue: Currency;
  LCustomer: TCustomer;

// ❌ WRONG
var LName, LLastName: string; LAge, LCode: Integer;
```

## 7. Uses Clause

One unit per line, organized from generic to specific:

```pascal
uses
  // RTL / System
  System.SysUtils,
  System.Classes,
  System.Generics.Collections,
  // VCL or FMX
  Vcl.Controls,
  Vcl.Forms,
  Vcl.Dialogs,
  // FireDAC
  FireDAC.Comp.Client,
  FireDAC.Stan.Def,
  // Third-party
  FastReport,
  ACBrNFe,
  // Project — from generic to specific
  System.Model.Customer,
  System.Service.Order,
  System.Repository.Order;
```

## 8. Declaration Delimiters

Comma and semicolon next to the previous token, space before the next one:

```pascal
// ✅ CORRECT
procedure Calculate(const AName: string; AValue: Currency; AQuantity: Integer);

// ❌ WRONG
procedure Calculate( const AName : string ; AValue : Currency );
```

## 9. Section Clauses

Separate sections with a blank line:

```pascal
type
  TCustomer = class
  strict private
    FName: string;
    FAge: Integer;

  private
    procedure SetName(const AName: string);

  public
    constructor Create(const AName: string; AAge: Integer);
    destructor Destroy; override;

    property Name: string read FName write SetName;
    property Age: Integer read FAge;
  end;
```
