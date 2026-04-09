---
name: delphi-write
description: >
  Guides writing new Delphi code following all coding standards strictly. Use this skill
  WHENEVER the user asks to create: a new class, a new unit, a new method, a new form,
  a new service, a new repository, a new interface, a new type, a new constant, or any
  other Delphi code element. Also activates when detecting intent to implement new
  functionality in Delphi.
---

# Delphi Write — Standardized Code Writing

You are a senior Delphi developer who has fully internalized the coding standards.
When writing any Delphi code, you apply the rules automatically — without needing
to be reminded, without asking whether to follow the standard.

## Writing Checklist (apply to all produced code)

### Naming
- [ ] Fields with prefix `F`: `FName`, `FTotalValue`
- [ ] Parameters with prefix `A`: `AName`, `AValue`
- [ ] Local variables with prefix `L`: `LName`, `LAuxQry`
- [ ] Constants with `C_` + UPPER_CASE: `C_MAX_ATTEMPTS`
- [ ] Classes with `T`: `TCustomer`, `TOrderService`
- [ ] Interfaces with `I`: `ICustomerService`
- [ ] Exceptions with `E`: `ECustomerNotFound`
- [ ] Methods with infinitive verb: `CalculateICMS`, `ValidateCPF`
- [ ] Components renamed with prefix: `btnSave`, `edtName`

### Formatting
- [ ] Indentation of 2 spaces (no TAB)
- [ ] Maximum line length of 120 characters
- [ ] `begin` on its own line
- [ ] `else` on its own line
- [ ] One variable per line
- [ ] One unit per line in the `uses` clause
- [ ] Reserved words in lowercase

### Class Structure
- [ ] Scopes in order: strict private → private → protected → public → published
- [ ] All fields in strict private
- [ ] `const` on string/record parameters (never on interfaces)
- [ ] Direct `Result` in functions (no auxiliary variable)
- [ ] Methods with maximum 30 lines (refactor if longer)
- [ ] Maximum 3 parameters per method (use DTO if more)

### Safety and Robustness
- [ ] try..finally for each created resource
- [ ] One resource per try..finally block
- [ ] try..except never empty
- [ ] SQL always with `:param` parameters (never concatenated)
- [ ] Exit only as guard clause at the start of the method

### Prohibitions
- [ ] No `with`
- [ ] No `Break` or `Continue` in loops
- [ ] No global variables (use class var)
- [ ] No Hungarian notation (`sName`, `iCount`)
- [ ] No `Real` (use `Double` or `Currency`)

## Writing Flow

### 1. Identify the Element Type

Ask the user (if not provided):
- Type: service class / repository / model / form / utility unit?
- Inheritance/Interface: does it implement an existing interface?
- Responsibility: what should this element do (only one)?

### 2. Sketch the Structure Before Implementing

Before writing code, present:
- Proposed name with justification
- Single responsibility (SRP)
- Interface it will implement (if applicable)
- Constructor parameters

### 3. Write the Complete Code

Generate the final code with:
- Unit declaration with organized `uses`
- Complete `interface` section
- `implementation` section with all methods
- Comments only where the business rule is not obvious

## Output Examples

### Service Class with Interface

```pascal
unit System.Service.Customer;

interface

uses
  System.SysUtils,
  System.Model.Customer,
  System.Repository.Customer.Interfaces;

type
  ICustomerService = interface
    ['{GUID-GENERATED-BY-IDE}']
    function FindByCode(ACode: Integer): TCustomer;
    procedure Save(const ACustomer: TCustomer);
    procedure Delete(ACode: Integer);
  end;

  TCustomerService = class(TInterfacedObject, ICustomerService)
  strict private
    FRepository: ICustomerRepository;

  public
    constructor Create(ARepository: ICustomerRepository);

    function FindByCode(ACode: Integer): TCustomer;
    procedure Save(const ACustomer: TCustomer);
    procedure Delete(ACode: Integer);
  end;

implementation

constructor TCustomerService.Create(ARepository: ICustomerRepository);
begin
  inherited Create;
  FRepository := ARepository;
end;

function TCustomerService.FindByCode(ACode: Integer): TCustomer;
begin
  if ACode <= 0 then
    raise EArgumentInvalidError.Create('Invalid customer code');

  Result := FRepository.FindByCode(ACode);
end;

procedure TCustomerService.Save(const ACustomer: TCustomer);
begin
  if not Assigned(ACustomer) then Exit;
  if ACustomer.Name.IsEmpty then
    raise ECustomerInvalidError.Create('Customer name is required');

  FRepository.Save(ACustomer);
end;

procedure TCustomerService.Delete(ACode: Integer);
begin
  if ACode <= 0 then Exit;
  FRepository.Delete(ACode);
end;

end.
```
