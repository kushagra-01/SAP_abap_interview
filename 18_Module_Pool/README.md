# 18 Module Pool (Dynpro: Screens, PBO/PAI)

## Concept (2–3 lines)
- Module pool = classical SAP GUI application with **screens (dynpros)** and **flow logic**.
- Core idea: **PBO** prepares the screen, **PAI** processes user input (OKCODE) and updates state.

## Interview-ready paragraph (say this)
Module pool programming is the classic screen-based ABAP model where the UI is a dynpro and logic is split into PBO and PAI modules. In PBO I set status, default values, and fetch/display data; in PAI I read user input, validate, and execute actions based on OKCODE. The main pitfall is messy global state and tightly coupled screen logic, so I keep business logic in forms/classes and keep PBO/PAI thin, with clear validations and consistent LUW handling.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is a module pool?
- Q: What is PBO vs PAI?
- Q: What is OKCODE / sy-ucomm?

### Intermediate
- Q: What is a table control and when is it used?
- Q: How do you validate user input in dynpro?
- Q: How do you navigate between screens?

### Advanced
- Q: Common performance issues in module pool apps?
- Q: How do you avoid inconsistent commits in screen flows?
- Q: How do you unit-test business logic used by dynpros?

## Best Answers (1–3 lines)
- A: In a dynpro, I use **PBO** to prepare the screen (status + field values) and **PAI** to validate input and execute actions.
- A: I read the **OKCODE** via `sy-ucomm` (like `SAVE`/`BACK`) to decide what the user wants to do.
- A: I keep PBO/PAI thin and push real business logic into reusable classes or function modules so the module pool stays maintainable.

## Code examples (minimal patterns)
### PBO/PAI skeleton (pattern)
```abap
MODULE status_0100 OUTPUT.
  SET PF-STATUS 'S100'.
  SET TITLEBAR 'T100'.
ENDMODULE.

MODULE user_command_0100 INPUT.
  CASE sy-ucomm.
    WHEN 'SAVE'.
      "validate + save (keep LUW consistent)
    WHEN 'BACK' OR 'CANC'.
      LEAVE TO SCREEN 0.
  ENDCASE.
ENDMODULE.
```

## Common mistakes / traps
- Putting heavy DB logic in PBO (screen becomes slow / flickery).
- Using global variables without clear lifecycle (hard-to-debug state bugs).
- Committing in the wrong place (partial updates across screens).

## Optimization tips (DO / DON’T)
- **DO**: fetch data once; avoid repeated selects in PBO loops.
- **DO**: keep PBO/PAI orchestration only; move logic to reusable classes.
- **DON’T**: rely on implicit state; define clear screen-to-screen contracts.

## MEMORY HACK
- **1-line**: “Dynpro = PBO shows, PAI processes; OKCODE drives actions; keep logic out of the screen.”
- **Keywords**: Dynpro, Flow logic, PBO, PAI, OKCODE, Table control

