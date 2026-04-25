# 05 Modularization

## Concept (2–3 lines)
- Modularization = split logic into reusable units: **FORM**, **Function Module**, **Class/Method**.
- Interview focus: reuse, testability, coupling, and where each fits in SAP.

## Interview-ready paragraph (say this)
Modularization in ABAP means structuring logic into reusable units so the code is maintainable and consistent across programs. Historically SAP used FORMs and function modules, but for modern development I prefer classes and methods because they support clear responsibilities, interfaces, and unit testing. I still use function modules when SAP expects them—like certain framework hooks, RFC-enabled APIs, or update/task patterns—but I keep business logic out of global function group state. A key interview point is LUW safety: reusable modules should not commit unexpectedly unless that behavior is explicitly part of the contract.

## Follow-up answers (if interviewer asks deeper)
If they ask when to use function modules today, I say I use them where SAP frameworks require them or where remote calls are needed, such as RFC-enabled APIs or standard BAPI patterns. I also mention update/background tasks as classic SAP mechanisms that still rely on function modules in many systems.

If they ask about avoiding global state, I explain function groups often carry global data, which can create hidden dependencies. I keep state local where possible, pass data explicitly, and move business logic into classes so dependencies are visible and testable.

If they ask about code ownership and testability, I say methods with interfaces allow mocking and ABAP Unit tests. FORMs and includes can work for legacy reports, but for new development I prefer OO modularization so future changes remain safe and predictable.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is modularization in ABAP?
- Q: FORM vs Function Module?

### Intermediate
- Q: Why prefer methods over forms?
- Q: What is a function group?
- Q: How do exceptions work in function modules?

### Advanced
- Q: When to use update task / background task?
- Q: How to avoid global state issues in function groups?

## Best Answers (1–3 lines)
- A: FORM = local procedural; quick but less reusable/testable.
- A: Function Module = reusable with RFC/update/task options; grouped in function group.
- A: Method = OO, best for clean design, testing, dependency injection.

## Code examples (minimal patterns)

### PERFORM (legacy)
```abap
PERFORM validate_input USING p_carrid CHANGING lv_ok.
```

### Call function module (pattern)
```abap
CALL FUNCTION 'BAPI_TRANSACTION_COMMIT'
  EXPORTING wait = abap_true.
```

### Method call (modern)
```abap
lo_service->process( EXPORTING is_in = ls_in IMPORTING es_out = ls_out ).
```

## Common mistakes / traps
- Hidden dependencies via global variables (especially in function groups).
- Overusing `PERFORM` across includes (hard to trace/test).
- COMMIT inside deep utility code (breaks caller’s LUW expectations).

## Optimization tips (DO / DON’T)
- **DO**: keep side effects at the boundary (service layer), not deep inside helpers.
- **DO**: prefer classes/methods for business logic.
- **DON’T**: commit inside reusable modules unless that’s the contract.

## Scenario-based questions (strong answers)
- Q: “Where would you put validation logic?”  
  A: In a **service class method**; keep UI/report thin; reuse across channels.

## Drawbacks / limitations
- Forms and includes scale poorly; tight coupling and low testability.

## Advanced improvements / modern alternatives
- Use OO + interfaces, and align with **clean core** in S/4.

## MEMORY HACK
- **1-line**: “FORM = legacy local, FM = reusable SAP unit, Method = modern clean design.”
- **Keywords**: FORM, FM, Method, Reuse, LUW
- **Quick compare**

| Unit | When |
|---|---|
| FORM | quick legacy report |
| FM | cross-program / RFC / update task |
| Method | most new dev, testable |

