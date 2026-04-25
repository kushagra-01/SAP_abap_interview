# 06 Enhancements (BAdI, User Exit)

## Concept (2–3 lines)
- Enhancements = add logic to SAP standard **without modifying** it.
- Interview focus: **User exits vs BAdI**, where found, and clean-core mindset.

## Interview-ready paragraph (say this)
Enhancements are the SAP-supported way to add custom logic to standard processes without modifying SAP objects, which keeps upgrades safer. In interviews I explain that user exits are older, procedural enhancement mechanisms, while BAdIs are the more modern, OO-based option that can support controlled implementations. I also connect it to S/4 clean core: prefer released extension points and APIs, keep enhancement logic small, and delegate heavy processing to well-structured service classes. The practical focus is maintainability—too many enhancements without discipline make debugging and upgrades painful.

## Follow-up answers (if interviewer asks deeper)
If they ask “how do you find them,” I explain that I search for enhancement spots based on the business transaction and then locate the appropriate user exit/BAdI through the standard tools and documentation. I also mention checking the call stack during debugging to identify enhancement points logically related to the process.

If they ask about implementation strategy, I say I keep the enhancement code thin and delegate to a service class, so the exit is only a hook. This reduces the risk of performance regressions and makes the logic testable and maintainable.

If they ask about clean core in S/4, I highlight using released extension points and APIs and avoiding modifications. When custom requirements are large or cross-system, I mention side-by-side extensions or event-driven integration so the ERP core stays upgrade-safe.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is an enhancement?
- Q: What is a user exit?
- Q: What is a BAdI?

### Intermediate
- Q: User exit vs BAdI difference?
- Q: How do you find enhancement spots?
- Q: What is an implicit enhancement?

### Advanced
- Q: Multiple implementations: how controlled?
- Q: How does S/4 clean core change enhancement strategy?

## Best Answers (1–3 lines)
- A: User exit = older enhancement via includes/FM; limited and often single-spot.
- A: BAdI = OO-based enhancement; can be **multiple** implementations (classic) or **single** (new).
- A: Goal is **no modification**; keep upgrade-safe.

## Code examples (minimal patterns)

### BAdI call pattern (concept)
```abap
GET BADI lo_badi.
CALL BADI lo_badi->if_ex_sample~process( EXPORTING is_data = ls ).
```

## Common mistakes / traps
- Implementing enhancements without documenting business reason (hard during upgrade).
- Heavy DB logic inside exits (performance regressions).
- Modifying standard objects (breaks upgrades; violates clean core).

## Optimization tips (DO / DON’T)
- **DO**: keep exit logic small; delegate to service class.
- **DO**: use released APIs in S/4; keep extensions upgrade-safe.
- **DON’T**: change SAP standard unless last resort (and tracked).

## Scenario-based questions (strong answers)
- Q: “Need to add validation in standard save.”  
  A: Use **BAdI/enhancement spot**; do minimal check; raise message; keep logic in class.

## Drawbacks / limitations
- Too many enhancements → hard debugging + upgrade complexity.
- Some exits are limited in context/data availability.

## Advanced improvements / modern alternatives
- In S/4: prefer **in-app extensibility**, **released enhancement points**, and **side-by-side** extensions where appropriate.

## MEMORY HACK
- **1-line**: “Enhance standard safely: BAdI (OO) > UserExit (legacy).”
- **Keywords**: BAdI, UserExit, NoModify, CleanCore, UpgradeSafe
- **Quick compare**

| Item | User Exit | BAdI |
|---|---|---|
| Style | procedural | OO |
| Multiple impl | usually no | often yes |
| Modern use | low | high |

