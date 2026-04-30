# 27 LUW + Locking (Enqueue/Dequeue)

## Concept (2–3 lines)
- LUW (Logical Unit of Work) = the boundary of a consistent business change (all-or-nothing).
- Locking prevents **lost updates** under concurrency using **enqueue objects** (SM12 monitoring).

## Interview-ready paragraph (say this)
I treat LUW and locking as the backbone of data correctness in SAP. A business operation should be atomic: validate, lock the key, update all dependent data, and then commit once so the system never persists a partial state. Locks are handled via enqueue objects, and I always define a clear lock key, handle lock failures gracefully, and keep the lock duration minimal. The common production issues are missing locks (overwrites), wrong LUW boundaries (partial commits), and retries without idempotency.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is LUW?
- Q: What is an enqueue lock object?
- Q: What is SM12 used for?

### Intermediate
- Q: When do you use COMMIT WORK vs BAPI_TRANSACTION_COMMIT?
- Q: What happens if you don’t lock and two users update the same record?
- Q: What is update task (V1/V2) at a high level?

### Advanced
- Q: How do you choose the lock key granularity?
- Q: How do you avoid deadlocks / long locks?
- Q: How do you design idempotent posting with retries?

## Best Answers (1–3 lines)
- A: I explain **LUW** as the unit of consistent business work, and I end it with commit/rollback so changes are atomic.
- A: I use **enqueue locks** to prevent concurrent overwrites, and I can inspect active locks in **SM12**.
- A: My safe pattern is: lock → validate/check → update → commit → unlock, and I keep the lock duration as short as possible.

## Code examples (minimal patterns)
### Lock → update → commit → unlock (pattern)
```abap
CALL FUNCTION 'ENQUEUE_EZMYOBJ'
  EXPORTING id = lv_id
  EXCEPTIONS foreign_lock = 1 system_failure = 2 OTHERS = 3.

IF sy-subrc <> 0.
  "handle lock failure (message/retry)
  RETURN.
ENDIF.

UPDATE ztab SET status = @lv_status WHERE id = @lv_id.
COMMIT WORK AND WAIT.

CALL FUNCTION 'DEQUEUE_EZMYOBJ' EXPORTING id = lv_id.
```

## Common mistakes / traps
- No lock → overwrites under concurrency.
- Locking too broadly (hurts throughput) or too narrowly (still allows conflicts).
- Multiple commits inside one business transaction → inconsistent partial state.

## Optimization tips (DO / DON’T)
- **DO**: lock on the business key that defines the “thing being updated”.
- **DO**: validate before locking when possible; lock only for the critical section.
- **DON’T**: hold locks across long operations (UI waits, RFC calls) unless required.

## MEMORY HACK
- **1-line**: “Correctness = LUW boundary + lock key; lock-update-commit-unlock; SM12 to inspect.”
- **Keywords**: LUW, Enqueue, Dequeue, SM12, Commit, Concurrency

