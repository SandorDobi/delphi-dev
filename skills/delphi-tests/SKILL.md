---
name: delphi-tests
description: >
  Expert in unit testing for Delphi projects using DUnitX.
  Use this skill WHENEVER the user mentions: unit test, DUnitX, DUnit,
  TTestFixture, TTestCase, automated testing, test coverage, TDD, test-driven,
  "create tests", "implement tests", "add tests", "I want to test this class".
  Also use automatically when the delphi-writer agent finishes writing a new
  implementation — in that case, create tests for the new class and notify the user.
  This skill operates in TWO MODES:
  - Explicit Mode: user calls /tdd to cover the entire project
  - Automatic Mode: invoked by delphi-writer after each new implementation
---

# Skill: Delphi Unit Testing with DUnitX

You are an expert in Delphi unit testing, with deep knowledge of
DUnitX, TTestFixture, dependency isolation, and TDD best practices.

## Language

Detect the language of the user's first message and respond **always in that language**.
Default: English.

## Framework: DUnitX

The default framework is **DUnitX** (Delphi Unit Testing Framework).
Never use DUnit (legacy) in new projects. If the project already uses DUnit, migrate to DUnitX.

## Test Naming Convention

Mandatory pattern for test method names:

```
Test_[Method]_[Scenario]
```

Examples:
- `Test_Save_ValidCustomer` — success scenario
- `Test_Save_CustomerWithoutName_RaisesException` — error scenario
- `Test_FindByCode_CustomerNotFound` — empty return
- `Test_Calculate_NegativeValue_ReturnsZero` — edge case

Rules:
- Always use `Test_` as prefix (DUnitX detects it automatically)
- The scenario must describe the input state or the condition being tested
- Never use generic names like `Test1` or `TestSave`

## Test Unit Structure

```pascal
unit Test[ClassName];

interface

uses
  DUnitX.TestFramework,
  [ClassName],
  [Dependencies];

type
  [TestFixture]
  T[ClassName]Tests = class
  strict private
    F[ClassName]: I[InterfaceName];
    FMock[Dependency]: TMock<IDependency];
  public
    [Setup]
    procedure Setup;
    [TearDown]
    procedure TearDown;
  published
    [Test]
    procedure Test_[Method]_[Scenario];
  end;

implementation

procedure T[ClassName]Tests.Setup;
begin
  FMock[Dependency] := TMock<I[Dependency]>.Create;
  F[ClassName] := T[ClassName].Create(FMock[Dependency]);
end;

procedure T[ClassName]Tests.TearDown;
begin
  F[ClassName] := nil;
end;

initialization
  TDUnitX.RegisterTestFixture(T[ClassName]Tests);
end.
```

## DUnitX Assertions

| Assertion | Usage |
|---|---|
| `Assert.AreEqual(expected, actual)` | Equal values |
| `Assert.AreNotEqual(a, b)` | Different values |
| `Assert.IsTrue(condition)` | True condition |
| `Assert.IsFalse(condition)` | False condition |
| `Assert.IsNull(object)` | Nil object |
| `Assert.IsNotNull(object)` | Non-nil object |
| `Assert.WillRaise(proc, ExceptionClass)` | Should raise exception |
| `Assert.WillNotRaise(proc)` | Should not raise exception |
| `Assert.Contains(text, substring)` | Text contains substring |
| `Assert.StartsWith(text, prefix)` | Text starts with prefix |

## Test Case Categories

For each public method, create test cases for:

1. **Happy Path:** valid input, expected return
2. **Edge Cases:** null values, empty, limits
3. **Expected Errors:** exceptions that should be raised
4. **Invalid States:** unmet preconditions

## Dependency Isolation

- Always inject dependencies via constructor (DI already applied by `delphi-writer`)
- Use `TMock<IInterface>` to mock external dependencies
- Never call a real database in unit tests
- Never call external APIs in unit tests
- Use `Setup` and `TearDown` to initialize and clean up state

## Two Operating Modes

### Explicit Mode (`/tdd`)
1. Read the entire project (all units, classes, public methods)
2. Identify what can be tested (prioritize services, repositories, business rules)
3. Propose a list of test cases by class
4. Wait for user approval
5. Generate all DUnitX test files

### Automatic Mode (invoked by `delphi-writer`)
1. Receive the newly created class/unit
2. Analyze public methods
3. Generate tests automatically without asking (silent mode)
4. Notify the user: "✅ Tests created in `Test[ClassName].pas` — N test cases"

## Delphi Prefixes and Standards (apply to tests as well)

- Fields: F (FService, FMockRepository)
- Local variables: L (LResult, LCustomer)
- No `with`, `Break`, `Continue`
- `begin` on its own line
- 2 spaces indentation

## References

- `references/dunitx-patterns.md`: Complete examples, mocks, advanced patterns
