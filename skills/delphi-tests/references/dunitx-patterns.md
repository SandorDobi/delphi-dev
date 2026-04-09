# DUnitX: Patterns and Complete Examples

## Test Project Structure

```
MyProject/
  src/
    Services/
      OrderService.pas
      CustomerService.pas
    Repositories/
      OrderRepository.pas
  tests/
    Services/
      TestOrderService.pas
      TestCustomerService.pas
    Repositories/
      TestOrderRepository.pas
    TestRunner.dpr          <- DUnitX runner project
```

---

## Complete Example: Service with Dependencies

### Target class: `OrderService.pas`

```pascal
unit OrderService;

interface

uses
  System.SysUtils,
  IOrderRepository,
  IInventoryService,
  Order;

type
  IOrderService = interface
    ['{A1B2C3D4-E5F6-7890-ABCD-EF1234567890}']
    function CreateOrder(const ACustomerId: Integer; const AProductId: Integer;
      const AQuantity: Integer): TOrder;
    procedure CancelOrder(const AOrderId: Integer);
    function FindById(const AId: Integer): TOrder;
  end;

  TOrderService = class(TInterfacedObject, IOrderService)
  strict private
    FOrderRepo: IOrderRepository;
    FInventoryService: IInventoryService;
  public
    constructor Create(const AOrderRepo: IOrderRepository;
      const AInventoryService: IInventoryService);
    function CreateOrder(const ACustomerId: Integer; const AProductId: Integer;
      const AQuantity: Integer): TOrder;
    procedure CancelOrder(const AOrderId: Integer);
    function FindById(const AId: Integer): TOrder;
  end;

implementation

...
```

### Test unit: `TestOrderService.pas`

```pascal
unit TestOrderService;

interface

uses
  DUnitX.TestFramework,
  Delphi.Mocks,
  OrderService,
  IOrderRepository,
  IInventoryService,
  Order;

type
  [TestFixture]
  TOrderServiceTests = class
  strict private
    FOrderService: IOrderService;
    FMockOrderRepo: TMock<IOrderRepository>;
    FMockInventory: TMock<IInventoryService>;
  public
    [Setup]
    procedure Setup;
    [TearDown]
    procedure TearDown;
  published
    // --- CreateOrder ---
    [Test]
    procedure Test_CreateOrder_ValidData_ReturnsCreatedOrder;
    [Test]
    procedure Test_CreateOrder_ZeroCustomerId_RaisesEArgumentException;
    [Test]
    procedure Test_CreateOrder_NegativeQuantity_RaisesEArgumentException;
    [Test]
    procedure Test_CreateOrder_NoInventory_RaisesEInsufficientInventoryException;

    // --- CancelOrder ---
    [Test]
    procedure Test_CancelOrder_ExistingOrder_CancelsSuccessfully;
    [Test]
    procedure Test_CancelOrder_OrderNotFound_RaisesENotFoundException;

    // --- FindById ---
    [Test]
    procedure Test_FindById_ValidId_ReturnsOrder;
    [Test]
    procedure Test_FindById_ZeroId_RaisesEArgumentException;
  end;

implementation

procedure TOrderServiceTests.Setup;
begin
  FMockOrderRepo := TMock<IOrderRepository>.Create;
  FMockInventory := TMock<IInventoryService>.Create;
  FOrderService := TOrderService.Create(FMockOrderRepo, FMockInventory);
end;

procedure TOrderServiceTests.TearDown;
begin
  FOrderService := nil;
end;

procedure TOrderServiceTests.Test_CreateOrder_ValidData_ReturnsCreatedOrder;
var
  LOrder: TOrder;
  LExpectedOrder: TOrder;
begin
  // Arrange
  LExpectedOrder := TOrder.Create;
  LExpectedOrder.Id := 1;
  LExpectedOrder.CustomerId := 10;
  LExpectedOrder.ProductId := 5;
  LExpectedOrder.Quantity := 3;

  FMockInventory.Setup.WillReturn(True).When.HasInventory(5, 3);
  FMockOrderRepo.Setup.WillReturn(LExpectedOrder).When.Save(It.IsAny<TOrder>);

  // Act
  LOrder := FOrderService.CreateOrder(10, 5, 3);

  // Assert
  Assert.IsNotNull(LOrder);
  Assert.AreEqual(10, LOrder.CustomerId);
  Assert.AreEqual(5, LOrder.ProductId);
  Assert.AreEqual(3, LOrder.Quantity);
end;

procedure TOrderServiceTests.Test_CreateOrder_ZeroCustomerId_RaisesEArgumentException;
begin
  Assert.WillRaise(
    procedure
    begin
      FOrderService.CreateOrder(0, 5, 3);
    end,
    EArgumentException
  );
end;

procedure TOrderServiceTests.Test_CreateOrder_NegativeQuantity_RaisesEArgumentException;
begin
  Assert.WillRaise(
    procedure
    begin
      FOrderService.CreateOrder(10, 5, -1);
    end,
    EArgumentException
  );
end;

procedure TOrderServiceTests.Test_CreateOrder_NoInventory_RaisesEInsufficientInventoryException;
begin
  FMockInventory.Setup.WillReturn(False).When.HasInventory(5, 10);

  Assert.WillRaise(
    procedure
    begin
      FOrderService.CreateOrder(10, 5, 10);
    end,
    EInsufficientInventoryException
  );
end;

procedure TOrderServiceTests.Test_CancelOrder_ExistingOrder_CancelsSuccessfully;
var
  LOrder: TOrder;
begin
  LOrder := TOrder.Create;
  LOrder.Id := 42;
  LOrder.Status := osPending;

  FMockOrderRepo.Setup.WillReturn(LOrder).When.FindById(42);

  Assert.WillNotRaise(
    procedure
    begin
      FOrderService.CancelOrder(42);
    end
  );

  FMockOrderRepo.Verify.Once.Save(It.IsAny<TOrder>);
end;

procedure TOrderServiceTests.Test_CancelOrder_OrderNotFound_RaisesENotFoundException;
begin
  FMockOrderRepo.Setup.WillReturn(nil).When.FindById(999);

  Assert.WillRaise(
    procedure
    begin
      FOrderService.CancelOrder(999);
    end,
    ENotFoundException
  );
end;

procedure TOrderServiceTests.Test_FindById_ValidId_ReturnsOrder;
var
  LOrder: TOrder;
  LMockOrder: TOrder;
begin
  LMockOrder := TOrder.Create;
  LMockOrder.Id := 7;
  FMockOrderRepo.Setup.WillReturn(LMockOrder).When.FindById(7);

  LOrder := FOrderService.FindById(7);

  Assert.IsNotNull(LOrder);
  Assert.AreEqual(7, LOrder.Id);
end;

procedure TOrderServiceTests.Test_FindById_ZeroId_RaisesEArgumentException;
begin
  Assert.WillRaise(
    procedure
    begin
      FOrderService.FindById(0);
    end,
    EArgumentException
  );
end;

initialization
  TDUnitX.RegisterTestFixture(TOrderServiceTests);
end.
```

---

## TestRunner.dpr — Execution Project

```pascal
program TestRunner;

{$APPTYPE CONSOLE}

uses
  System.SysUtils,
  DUnitX.TestFramework,
  DUnitX.Loggers.Console,
  DUnitX.Loggers.Xml.NUnit,
  TestOrderService in 'tests\Services\TestOrderService.pas',
  TestCustomerService in 'tests\Services\TestCustomerService.pas';

var
  LRunner: ITestRunner;
  LResults: IRunResults;
  LLogger: ITestLogger;
  LNUnitLogger: ITestLogger;
begin
  try
    TDUnitX.RegisterTestFixture(TOrderServiceTests);

    LRunner := TDUnitX.CreateRunner;
    LRunner.UseRTTI := True;

    LLogger := TDUnitXConsoleLogger.Create(True);
    LNUnitLogger := TDUnitXXMLNUnitFileLogger.Create(TDUnitX.Options.XMLOutputFile);
    LRunner.AddLogger(LLogger);
    LRunner.AddLogger(LNUnitLogger);

    LResults := LRunner.Execute;

    if not LResults.AllPassed then
      System.ExitCode := EXIT_ERRORS;

  except
    on E: Exception do
    begin
      Writeln(E.ClassName, ': ', E.Message);
      System.ExitCode := EXIT_ERRORS;
    end;
  end;
end.
```

---

## Advanced Patterns

### Verify Calls with Verify

```pascal
// Verify that the method was called exactly once
FMockRepo.Verify.Once.Save(It.IsAny<TCustomer>);

// Verify that it was NEVER called
FMockRepo.Verify.Never.Delete(It.IsAny<Integer>);

// Verify that it was called N times
FMockRepo.Verify.Exactly(3).Save(It.IsAny<TCustomer>);
```

### Testing Methods That Return Strings

```pascal
procedure Test_FormatName_FullNameReturnsFormatted;
var
  LResult: string;
begin
  LResult := FService.FormatName('john', 'DOE');
  Assert.AreEqual('John Doe', LResult);
end;
```

### Testing with Multiple Scenarios (DataProvider)

```pascal
[Test]
[TestCase('Valid CPF', '123.456.789-09,True')]
[TestCase('Invalid CPF', '111.111.111-11,False')]
[TestCase('Empty CPF', ',False')]
procedure Test_ValidateCPF_Scenarios(const ACPF: string; const AExpected: Boolean);
begin
  Assert.AreEqual(AExpected, FService.ValidateCPF(ACPF));
end;
```

### Setup with Complex State

```pascal
procedure Setup;
var
  LConfig: TConfiguration;
begin
  LConfig := TConfiguration.Create;
  LConfig.Timeout := 30;
  LConfig.MaxAttempts := 3;

  FMockConfig := TMock<IConfiguration>.Create;
  FMockConfig.Setup.WillReturn(LConfig).When.GetConfig;

  FService := TMyClass.Create(FMockConfig);
end;
```

---

## Test Quality Checklist

- [ ] Each test has exactly ONE main Assert
- [ ] Names follow the `Test_Method_Scenario` pattern
- [ ] Setup initializes everything needed
- [ ] TearDown releases manually created resources
- [ ] No test depends on another test
- [ ] External dependencies are mocked
- [ ] Happy path, edge cases, and errors are covered
- [ ] Test code follows the same standards as production code
- [ ] Unit compiles without errors
