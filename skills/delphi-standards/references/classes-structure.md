# Class Structure — Delphi Style Guide

## 1. Visibility Scope Order

From most restrictive to least restrictive:

```pascal
type
  TCustomer = class(TInterfacedObject, ICustomer)
  strict private
    // Fields — always here
    FName: string;
    FAge: Integer;
    FTotalValue: Currency;

  private
    // Getters and Setters
    procedure SetName(const AName: string);
    function GetName: string;

  protected
    // Methods accessible by subclasses
    function GetAge: Integer; virtual;

  public
    // Public interface
    constructor Create(const AName: string; AAge: Integer);
    destructor Destroy; override;

    procedure ProcessOrder(const AOrder: TOrder);
    function CalculateDiscount(const AValue: Currency): Currency;

    property Name: string read GetName write SetName;
    property Age: Integer read GetAge;
    property TotalValue: Currency read FTotalValue;

  published
    // Only for registered components
  end;
```

## 2. Fields (Attributes)

- Always in `strict private` (preferred) or `private`
- `F` prefix mandatory
- CamelCase after prefix
- NEVER exposed directly — always via properties

```pascal
strict private
  FName: string;           // ✅
  FTotalValue: Currency;   // ✅
  FActive: Boolean;        // ✅
```

## 3. Methods

- Infinitive verb: `Calculate`, `Validate`, `Save`, `Find`
- CamelCase
- Ideal size: 20–30 lines. Above 50: refactor with Extract Method
- One responsibility per method (SRP)

```pascal
// ✅ Focused method
function TCustomerService.CalculateDiscount(const AValue: Currency): Currency;
begin
  if AValue <= 0 then
    raise EArgumentInvalidError.Create('Value must be greater than zero');

  Result := AValue * FDiscountPercentage / 100;
end;
```

## 4. Parameters

- `A` prefix mandatory
- CamelCase after prefix
- No type prefix (`sName`, `iCount` — forbidden)
- `const` for `string`, `record`, `array` — always
- `const` for interfaces — NEVER (breaks ARC)
- 3 parameters: tolerable. 4+: create DTO

```pascal
// ✅ CORRECT
procedure ProcessSale(const AName: string; AValue: Currency;
  AQuantity: Integer);

// ❌ WRONG — Hungarian notation, no const
procedure ProcessSale(sName: string; dValue: Double; iQty: Integer);
```

## 5. Function Results

Write directly to the `Result` variable:

```pascal
// ✅ CORRECT
function CalculateICMS(const ABase: Currency; const ARate: Double): Currency;
begin
  Result := ABase * ARate / 100;
end;

// ❌ WRONG — unnecessary auxiliary variable
function CalculateICMS(const ABase: Currency; const ARate: Double): Currency;
var LValue: Currency;
begin
  LValue := ABase * ARate / 100;
  Result := LValue;
end;
```

## 6. Properties

- CamelCase without prefix
- Getter/Setter with `Get`/`Set` prefix

```pascal
property Name: string read GetName write SetName;
property TotalValue: Currency read FTotalValue;  // read-only
property Active: Boolean read FActive write FActive; // no dedicated getter/setter
```

## 7. Interfaces

- `I` prefix mandatory
- Inherit from `IInterface`
- GUID generated with Ctrl+Shift+G in Delphi IDE (NEVER typed manually)
- Implementing classes inherit from `TInterfacedObject`

```pascal
type
  ICustomerService = interface
    ['{GUID-GENERATED-BY-IDE}']
    function FindByCode(ACode: Integer): TCustomer;
    procedure Save(const ACustomer: TCustomer);
  end;

  TCustomerServiceFireDAC = class(TInterfacedObject, ICustomerService)
  strict private
    FConnection: IDbConnection;
  public
    constructor Create(AConnection: IDbConnection);
    function FindByCode(ACode: Integer): TCustomer;
    procedure Save(const ACustomer: TCustomer);
  end;
```

## 8. DTOs (Data Transfer Objects)

For methods with 4+ parameters, group into Record or class:

```pascal
type
  TCustomerData = record
    Name: string;
    CPF: string;
    Address: string;
    City: string;
    State: string;
  end;

// ✅ With DTO — clean
function RegisterCustomer(const AData: TCustomerData): Boolean;

// ❌ Polyadic — 5 parameters
function RegisterCustomer(AName, ACPF, AAddress, ACity, AState: string): Boolean;
```

## 9. SOLID Principles

| Letter | Principle | Delphi Application |
|---|---|---|
| S | Single Responsibility | One class = one reason to change |
| O | Open/Closed | Inheritance and interfaces for extension |
| L | Liskov Substitution | Subclasses replace base without breaking |
| I | Interface Segregation | Multiple specific interfaces > one general |
| D | Dependency Inversion | Depend on `IInterface`, not `TClass` |
