# 07 OOP ABAP

## Concept (2–3 lines)
- OOP ABAP = organize code into **classes + objects**, improving reuse, testing, and maintainability.
- Interview focus: **encapsulation**, **inheritance**, **polymorphism**, **interfaces**, **events**.

## Interview-ready paragraph (say this)
In ABAP I prefer object-oriented design when code needs to be reusable, testable, and easy to extend. I model core logic in service classes, expose contracts through interfaces, and use polymorphism so the caller can depend on behavior instead of concrete implementations. I avoid deep inheritance unless it’s a true “is-a” relationship, and I usually favor composition and dependency injection to keep coupling low. This approach makes unit testing easier and keeps reports and UI layers thin and focused on orchestration.

## Follow-up answers (if interviewer asks deeper)
If they ask “interface vs abstract class,” I say interfaces are for contracts and multiple implementations, while abstract classes are useful when I need shared base behavior plus extension points. In ABAP I still prefer interfaces plus composition for flexibility, and I use inheritance only when the domain relationship is truly “is-a.”

If they ask about dependency injection, I explain it as passing dependencies into the constructor so the class doesn’t create its own collaborators. That reduces coupling and makes unit testing simple because I can inject a test double instead of calling real DB or external services.

If they ask how I apply OOP in real SAP work, I say I keep reports thin and move logic into service classes, keep DB access behind a dedicated layer, and return clear exceptions/messages. This structure prevents global state issues and improves maintainability during changes and upgrades.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: Class vs object?
- Q: What is an interface?
- Q: Public/protected/private?

### Intermediate
- Q: Inheritance vs interface?
- Q: Polymorphism example?
- Q: Abstract class vs interface?

### Advanced
- Q: Dependency injection in ABAP?
- Q: When to use events?
- Q: How to unit test ABAP code?

## Best Answers (1–3 lines)
- A: Class = blueprint; Object = runtime instance.
- A: Interface = **contract**; class can implement many interfaces.
- A: Inheritance = “is-a” reuse; Polymorphism = call same method on different implementations.
- A: Prefer interfaces + composition over deep inheritance.

## Code examples (minimal patterns)

### Interface + two implementations (polymorphism)
```abap
INTERFACE lif_printer.
  METHODS print IMPORTING iv_text TYPE string.
ENDINTERFACE.

CLASS lcl_console DEFINITION.
  PUBLIC SECTION.
    INTERFACES lif_printer.
ENDCLASS.

CLASS lcl_console IMPLEMENTATION.
  METHOD lif_printer~print.
    WRITE: / iv_text.
  ENDMETHOD.
ENDCLASS.
```

### Inject dependency (pattern)
```abap
CLASS lcl_service DEFINITION.
  PUBLIC SECTION.
    METHODS constructor IMPORTING io_printer TYPE REF TO lif_printer.
  PRIVATE SECTION.
    DATA mo_printer TYPE REF TO lif_printer.
ENDCLASS.
```

## Common mistakes / traps
- Global state in classes (hard tests, unpredictable behavior).
- Overusing inheritance (tight coupling); missing interfaces.
- Mixing DB access everywhere instead of a clear data-access layer.

## Optimization tips (DO / DON’T)
- **DO**: program to **interfaces**.
- **DO**: keep business logic in service classes; keep reports thin.
- **DON’T**: static global data unless truly constant.

## Scenario-based questions (strong answers)
- Q: “Need switchable output: spool/email/UI.”  
  A: Use an **interface** (strategy pattern); inject implementation based on context.

## Drawbacks / limitations
- Over-engineering small reports can slow delivery if not balanced.

## Advanced improvements / modern alternatives
- Use ABAP Unit + seams/test doubles; align with clean architecture where possible.

## MEMORY HACK
- **1-line**: “Interfaces for contracts, classes for implementations, polymorphism for flexibility.”
- **Keywords**: Interface, Encapsulation, Polymorphism, DI, Testable
- **Quick compare**

| Choice | Use when |
|---|---|
| Interface | multiple implementations needed |
| Inheritance | true is-a reuse |
| Composition | prefer for flexibility |

