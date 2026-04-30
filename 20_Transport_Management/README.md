# 20 Transport Management (CTS)

## Concept (2–3 lines)
- Transports move repository/config changes across systems (DEV → QA → PRD) in a controlled way.
- Interview focus: tasks/requests, sequencing, and avoiding production breaks.

## Interview-ready paragraph (say this)
Transport management is about safely moving changes through the landscape with traceability and correct sequencing. I create a transport request and task (SE09/SE10), release tasks then the request, and coordinate imports via STMS. The key pitfall is dependency and order: customizing and repository objects must be consistent, and missing/dependent transports cause runtime dumps in QA/PRD, so I keep changes small, track dependencies, and verify in QA with a clear test plan.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is a transport request?
- Q: SE09 vs SE10?
- Q: What is STMS used for?

### Intermediate
- Q: Task vs request?
- Q: Why transport sequencing matters?
- Q: How do you handle urgent fixes (hotfix) safely?

### Advanced
- Q: How do you avoid import conflicts and missing dependencies?
- Q: What is a transport of copies (ToC) used for?
- Q: How do you handle cross-client customizing carefully?

## Best Answers (1–3 lines)
- A: I manage transport requests and tasks in **SE09/SE10**, and I control/import them through the landscape using **STMS**.
- A: I release **tasks** first and then the **request**, and I watch import sequencing because dependencies can break runtime in QA/PRD.
- A: I keep transports small, traceable, and testable, and I always validate in QA before moving to production.

## Common mistakes / traps
- Large “mega” transports (hard to test/rollback).
- Missing dependent objects (runtime errors after import).
- Mixing unrelated fixes/features in one request.

## Optimization tips (DO / DON’T)
- **DO**: use one request per logical change; document “what + why”.
- **DO**: coordinate with functional/config changes; verify client-dependent objects.
- **DON’T**: import without checking sequences/queues in QA/PRD.

## MEMORY HACK
- **1-line**: “CTS = request/task (SE09/10) + import (STMS); small transports + correct order.”
- **Keywords**: CTS, SE09, SE10, STMS, Sequence, ToC

