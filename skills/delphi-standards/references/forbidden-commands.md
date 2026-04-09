# Forbidden and Restricted Commands — Delphi Style Guide

## Summary Table

| Command | Status | Reason |
|---|---|---|
| `with` | 🚫 FORBIDDEN | Makes debugging difficult, confuses the compiler |
| `Break` | 🚫 FORBIDDEN | Exit condition must be in the loop declaration |
| `Continue` | 🚫 FORBIDDEN | Branch makes comprehension harder |
| `goto` | 🚫 FORBIDDEN | — |
| `Exit` | ⚠️ RESTRICTED | Only in guard clauses at the beginning |
| `Abort` | 🚫 FORBIDDEN | Hides the call stack |
| `Real` | 🚫 FORBIDDEN | Obsolete, replaced by `Double` |
| `Extended` | ⚠️ DISCOURAGED | Size not optimized for modern processors |

---

## WITH — Forbidden

**Reason:** Makes debugging difficult (which object is being referenced?),
confuses the compiler in ambiguities, prevents static analysis.

```pascal
// ❌ WRONG
with AuxQry do
begin
  Close;
  SQL.Clear;
  SQL.Add('SELECT * FROM CUSTOMERS');
  Open;
end;

// ✅ CORRECT
LAuxQry.Close;
LAuxQry.SQL.Clear;
LAuxQry.SQL.Add('SELECT * FROM CUSTOMERS');
LAuxQry.Open;
```

---

## BREAK and CONTINUE — Forbidden

**Reason:** They create flow deviations that make comprehension harder. The exit
condition of the loop must be in the loop declaration.

```pascal
// ❌ WRONG — Break in loop
for LI := 0 to LList.Count - 1 do
begin
  if LList[LI].Active then
    Break;
  ProcessItem(LList[LI]);
end;

// ✅ CORRECT — condition in loop declaration
LI := 0;
while (LI < LList.Count) and not LList[LI].Active do
begin
  ProcessItem(LList[LI]);
  Inc(LI);
end;
```

---

## EXIT — Restricted to Guard Clauses

`Exit` is permitted **exclusively** at the beginning of the method as a guard clause,
to reject invalid conditions before the main logic.

```pascal
// ✅ CORRECT — guard clauses at the beginning
procedure TService.ProcessOrder(const AOrder: TOrder);
begin
  if not Assigned(AOrder) then Exit;
  if AOrder.Value <= 0 then Exit;
  if not FCustomerValid then Exit;

  // main logic here — no nested ifs
  FRepository.Save(AOrder);
  FLogger.Log('Order processed: ' + AOrder.Number);
end;

// ❌ WRONG — Exit in the middle of logic
procedure TService.ProcessOrder(const AOrder: TOrder);
begin
  FRepository.Save(AOrder);
  if not FSendEmail then Exit; // Exit in the middle — forbidden
  FEmail.Send(AOrder);
end;
```

---

## Loops — Rules

### FOR: use when the number of iterations is defined
### WHILE: use when the number of iterations is undefined (minimum 0 iterations)
### REPEAT: use when the loop must execute at least once

```pascal
// ✅ FOR with defined iterations
for LI := 0 to LList.Count - 1 do
  ProcessItem(LList[LI]);

// ✅ WHILE with compound condition (lowest complexity first)
while LActive and (GetCurrentValue < C_MAX_LIMIT) do
begin
  ProcessIteration;
end;

// ✅ REPEAT with condition in until
repeat
  LAttempt := LAttempt + 1;
  LResult := TryConnect;
until LResult or (LAttempt >= C_MAX_ATTEMPTS);
```

---

## IF — Condition Order

Compound conditions must be ordered from **lowest to highest complexity**
(left to right). The compiler uses short-circuit evaluation — it stops when one
condition determines the result.

```pascal
// ✅ CORRECT — simple boolean first, complex calculation only if needed
if FActive and (GetComplexCalculation < C_LIMIT) then
  Process;

// ❌ WRONG — heavy calculation evaluated unnecessarily
if (GetComplexCalculation < C_LIMIT) and FActive then
  Process;
```

---

## CASE — Rules

- Values in ascending order
- Each case indented relative to `case`
- Blocks with maximum 5 lines (including begin..end)
- `else` aligned with `case`, no extra indentation

```pascal
// ✅ CORRECT
case LStatus of
  0: OrderOpen;
  1: OrderConfirmed;
  2: OrderInvoiced;
  3:
  begin
    CancelOrder;
    NotifyCustomer;
  end;
else
  raise EInvalidStatus.Create('Unknown status: ' + IntToStr(LStatus));
end;
```

---

## Exceptions

### try..finally — One resource per block
```pascal
// ✅ CORRECT — one resource per block
LObj1 := TClass1.Create;
try
  LObj2 := TClass2.Create;
  try
    LObj1.UseWith(LObj2);
  finally
    LObj2.Free;
  end;
finally
  LObj1.Free;
end;

// ❌ WRONG — two resources in the same block
LObj1 := TClass1.Create;
LObj2 := TClass2.Create;
try
  LObj1.UseWith(LObj2);
finally
  LObj1.Free;
  LObj2.Free; // if LObj1.Free raises an exception, LObj2 leaks
end;
```

### try..except — Never silent
```pascal
// ❌ FORBIDDEN — empty block
try
  qry.ExecSQL;
except
  // empty — hides the error
end;

// ✅ CORRECT — specific catch, log, and rethrow friendly message
try
  qry.ExecSQL;
except
  on E: EDatabaseError do
  begin
    Logger.LogError('Failed to save order: ' + E.Message);
    raise Exception.Create('Could not save the order. Please try again.');
  end;
end;
```
