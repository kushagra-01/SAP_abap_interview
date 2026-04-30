# 19 Background Jobs (SM36 / SM37)

## Concept (2–3 lines)
- Background jobs run ABAP programs **asynchronously** on schedule (batch processing, nightly loads).
- Interview focus: job steps, variants, monitoring (SM37), and safe restart/error handling.

## Interview-ready paragraph (say this)
I use background jobs for heavy or scheduled processing where a dialog user shouldn’t wait, like nightly reconciliation, IDoc reprocessing, or mass updates. I define a program variant, schedule via SM36, and monitor in SM37, and I design the job to be restartable and idempotent so reruns don’t duplicate postings. The pitfall is “it works in dev” but fails in batch due to missing authorizations, missing variants, or no spool/logging, so I always include clear logs and failure visibility.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: Why use background jobs?
- Q: What do SM36 and SM37 do?
- Q: What is a variant?

### Intermediate
- Q: What is a job step?
- Q: How do you monitor job errors and spool?
- Q: Dialog vs background differences (authority, user, timeouts)?

### Advanced
- Q: How do you make a batch job restartable?
- Q: How do you avoid duplicates when a job is re-run?
- Q: How do you handle locking/concurrency in batch?

## Best Answers (1–3 lines)
- A: I schedule background processing in **SM36** and I monitor job status, logs, and spool in **SM37**.
- A: I use **variants** so job parameters are stable, repeatable, and transportable.
- A: For production-safe batch, I design for clear logging, restartability, idempotency, and correct LUW/locking so reruns don’t create duplicates.

## Code examples (minimal patterns)
### Batch-friendly error handling (pattern)
```abap
TRY.
    "process in chunks
  CATCH cx_root INTO DATA(lx).
    "write application log (SLG1) or spool message
ENDTRY.
```

## Common mistakes / traps
- No spool/logs → support can’t diagnose failures.
- Hard-coded parameters (no variant) → fragile scheduling.
- No idempotency → rerun creates duplicates.

## Optimization tips (DO / DON’T)
- **DO**: process in chunks (packages) and commit in controlled boundaries.
- **DO**: add monitoring hooks (application log, counts, timestamps).
- **DON’T**: assume dialog behavior (authorizations/timeouts differ in background).

## MEMORY HACK
- **1-line**: “Batch = SM36 schedule, SM37 monitor; variants + logs + restartable/idempotent design.”
- **Keywords**: SM36, SM37, Variant, Spool, SLG1, Restartable

