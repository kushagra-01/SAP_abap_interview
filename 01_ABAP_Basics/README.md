# 01 ABAP Basics

## Concept (2–3 lines)
- ABAP = SAP’s business language running on **application server** + talking to DB via **Open SQL**.
- Interview focus: **types**, **Open SQL**, **control flow**, **LUW/COMMIT basics**, **error handling**.

## Interview-ready paragraph (say this)
ABAP is SAP’s application-server language used to build reports, enhancements, interfaces, and business services. In day-to-day development I focus on clean Open SQL: filtering early with `WHERE`, selecting only required fields, and avoiding SELECT inside loops to keep performance stable. I rely on DDIC types for consistency, use internal tables for processing sets of records, and I’m disciplined about `sy-subrc` checks and LUW handling so updates are correct. If something is slow or inconsistent, I first look at database roundtrips, missing filters or indexes, and improper commits or locking.

## Follow-up answers (if interviewer asks deeper)
If they ask about modern ABAP style, I mention I use ABAP 7.40+ syntax like inline declarations and host variables, because it makes code shorter and less error-prone. I still understand classical syntax, but I keep a consistent style so teams can maintain code easily.

If they ask about updates and consistency, I explain LUW boundaries and why “commit location” matters. I avoid committing deep inside helper code unless it is explicitly required, and I use lock objects when concurrent updates can cause lost updates or inconsistent states. The aim is predictable transactional behavior, not just “code that runs.”

If they ask about performance mindset, I explain that database design and access patterns dominate runtime. I prefer filtering in SQL/CDS, avoid N+1 selects, and measure with traces rather than guessing. Once DB access is clean, I optimize internal table access and copying.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is ABAP used for?
- Q: What is Open SQL?
- Q: What is a work area?
- Q: What is a structure vs internal table?
- Q: What is a DDIC type?
- Q: What is `sy-subrc`?

### Intermediate
- Q: `SELECT SINGLE` vs `SELECT ... UP TO 1 ROWS`?
- Q: When do you use `INTO TABLE` vs `APPENDING TABLE`?
- Q: What is LUW? What is `COMMIT WORK`?
- Q: What is an enqueue/dequeue lock?
- Q: What are ABAP exceptions vs `sy-subrc` checks?

### Advanced
- Q: Client handling: what is `MANDT` and why it matters?
- Q: What is implicit database commit risk?
- Q: When is `SELECT *` harmful?
- Q: Open SQL host variables `@` and inline declarations?

## Best Answers (1–3 lines)
- A: I use ABAP to build SAP **reports, interfaces, enhancements, and services**, and it runs mainly on the **application server**.
- A: I use **Open SQL** because it’s SAP-managed and portable across databases, and it integrates with SAP semantics like buffering.
- A: I treat a work area as a **single-row buffer** for a structure or an internal table line.
- A: I always check `sy-subrc` because it’s the **return code** (0 means success) after reads and DB operations.
- A: I describe **LUW** as the unit of consistent work, and I end it with `COMMIT WORK` to persist changes (or rollback on error).

## Code examples (minimal patterns)

### Inline declarations + Open SQL
```abap
SELECT carrid, connid
  FROM sflight
  WHERE carrid = @p_carrid
  INTO TABLE @DATA(it_flight).
```

### `sy-subrc` check
```abap
READ TABLE it_flight INTO DATA(ls_flight) INDEX 1.
IF sy-subrc <> 0.
  "not found
ENDIF.
```

### Update + COMMIT (concept only)
```abap
UPDATE ztable SET status = 'X' WHERE id = @lv_id.
IF sy-subrc = 0.
  COMMIT WORK.
ENDIF.
```

## Common mistakes / traps
- Using `SELECT *` (slow + fragile) instead of selecting needed fields.
- Forgetting `@` host variables (7.40+), mixing old/new syntax incorrectly.
- Missing `sy-subrc` checks after `READ TABLE`, `SELECT SINGLE`, `UPDATE`.
- Doing DB work inside loops (N+1 selects).

## Optimization tips (DO / DON’T)
- **DO**: filter in DB (`WHERE`), select only required fields.
- **DO**: use modern syntax (`DATA(...)`, `@lv`).
- **DON’T**: `SELECT *` in productive code.
- **DON’T**: nested loops + DB selects inside.

## Scenario-based questions (with strong answers)
- Q: “User complains report is slow.” What do you check first?  
  A: Check **DB selects count**, **missing WHERE**, **SELECT in loop**, and **indexes**; push down via **CDS** if heavy.
- Q: “Update sometimes inconsistent.” Why?  
  A: Likely LUW issues: missing/extra **COMMIT**, no **locks**, or update task timing.

## Drawbacks / limitations
- `sy-subrc` style can hide errors if not checked consistently.
- Classical procedural coding becomes hard to test/maintain vs OOP.

## Advanced improvements / modern alternatives
- Prefer **CDS** for reusable data models over raw selects in reports.
- Prefer **RAP** for transactional services over hand-written service logic.

## MEMORY HACK
- **1-line**: “ABAP = app-server code + Open SQL; always check `sy-subrc` and LUW.”
- **Keywords**: OpenSQL, WorkArea, DDIC, sy_subrc, LUW
- **Quick compare**

| Topic | ABAP-safe memory |
|---|---|
| Open SQL | portable, SAP-managed |
| Native SQL | DB-specific, risky |

