# 10 Performance Optimization

## Concept (2–3 lines)
- Performance = minimize time in **DB**, **ABAP loops**, and **network/UI**.
- Interview focus: **avoid SELECT in loop**, correct table types, indexes, and pushdown (CDS/HANA).

## Interview-ready paragraph (say this)
When I optimize ABAP performance, I start by measuring and then remove the biggest bottlenecks, which are usually database roundtrips and large data volumes. The first rule is to avoid SELECT inside loops and instead fetch data in a single select using joins, `IN`, or safe `FOR ALL ENTRIES`, selecting only the fields actually needed. On the ABAP side, I choose the right internal table type—hashed for heavy key lookups and sorted for ordered or range access—and I avoid unnecessary copying of large rows. I also check indexes and WHERE conditions because a good data model and proper filtering often give the largest improvements. Finally, I keep a pushdown mindset: aggregations and joins belong in CDS/Open SQL when datasets are large.

## Follow-up answers (if interviewer asks deeper)
If they ask “how do you measure,” I answer that I start with runtime and SQL tracing rather than guessing. I look for the top expensive SQL statements, number of DB roundtrips, and whether predicates use indexes effectively, because that usually explains most of the runtime.

If they ask about `FOR ALL ENTRIES`, I explain it is useful to replace select-in-loop, but only when the driving table is not initial and keys are cleaned and deduped. Otherwise it can produce incorrect results or even select the entire table, which is a common interview trap.

If they ask about balancing pushdown and ABAP logic, I explain that joins, filters, and aggregations belong in CDS/Open SQL for scale, while orchestration and business rules remain clearer in ABAP. I optimize only after confirming the hotspot, so the code stays maintainable.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: Why is `SELECT *` bad?
- Q: What is “SELECT in LOOP” issue?
- Q: How do you reduce runtime quickly?

### Intermediate
- Q: `FOR ALL ENTRIES` pitfalls?
- Q: Standard vs sorted vs hashed for performance?
- Q: When do you use secondary index?
- Q: How to measure performance (ST05/SAT)?

### Advanced
- Q: DB pushdown: CDS vs ABAP loops?
- Q: How to avoid memory blow-ups with huge internal tables?
- Q: Locking/LUW impact on performance?

## Best Answers (1–3 lines)
- A: Biggest wins: **reduce DB calls**, **filter early**, **select only needed fields**, avoid N+1.
- A: `SELECT in LOOP` = N DB roundtrips; replace with **single select + join/FAE**.
- A: `FOR ALL ENTRIES` is safe only if driving table **not initial** + keys deduped.
- A: Use **hashed** for heavy key lookups; **sorted** for range + ordered data.
- A: Measure with **ST05 (SQL trace)** and **SAT (runtime analysis)**; optimize what’s hot.

## Code examples (minimal patterns)
### Bad: SELECT in loop (anti-pattern)
```abap
LOOP AT it_keys INTO DATA(ls_key).
  SELECT SINGLE * FROM ztab WHERE id = @ls_key-id INTO @DATA(ls_row).
ENDLOOP.
```

### Good: single select with IN / join style
```abap
SELECT id, status
  FROM ztab
  INTO TABLE @DATA(it_rows)
  FOR ALL ENTRIES IN @it_keys
  WHERE id = @it_keys-id.
```

### Hashed lookup for speed
```abap
DATA it_hash TYPE HASHED TABLE OF ztab WITH UNIQUE KEY id.
it_hash = it_rows.
READ TABLE it_hash WITH TABLE KEY id = lv_id INTO DATA(ls_hit).
```

## Common mistakes / traps interviewers check
- “Optimizing” ABAP loops while DB is the real bottleneck (wrong layer).
- Missing `it_keys IS NOT INITIAL` for `FOR ALL ENTRIES` → selects everything.
- `BINARY SEARCH` without `SORT` by same key.
- Pulling huge datasets then filtering in ABAP instead of in DB/CDS.

## Optimization tips (DO / DON’T)
- **DO**: minimize DB roundtrips (1 select > 1000 selects).
- **DO**: select only required fields; keep rows small.
- **DO**: use correct table type (hashed lookup, sorted ranges).
- **DO**: add/verify indexes for frequent WHERE predicates.
- **DON’T**: do `SELECT *` in productive code.
- **DON’T**: fetch 1M rows “just in case”; use paging/filters.

## Real-world scenario-based questions with strong answers
- Q: “Job dumps with TSV_TNEW_PAGE_ALLOC_FAILED.” Fix?  
  A: Reduce dataset, select fewer fields, stream/process in chunks, avoid huge internal tables, pushdown via CDS.
- Q: “Report slow after go-live.” First action?  
  A: Run **ST05** to see top SQL; fix **WHERE/index**, remove select-in-loop, push down heavy joins.

## Drawbacks / limitations
- Over-optimization can reduce readability; optimize only proven hotspots.
- Some optimizations depend on DB/indexes and data distribution (needs measurement).

## Advanced improvements / modern alternatives
- Prefer **CDS** for heavy joins/aggregations (pushdown).
- Use **ALV with IDA** / analytical CDS for large reporting scenarios on HANA.
- Prefer **RAP** for standardized service patterns (less custom glue code).

## MEMORY HACK
- **1-line**: “Measure first, cut DB calls, filter early, pick right table type, push down heavy logic.”
- **Keywords**: ST05, SelectInLoop, Index, Hashed, Pushdown
- **Quick compare**

| Bottleneck | Fix |
|---|---|
| Too many DB calls | single select/join/FAE |
| Slow key reads | hashed/sorted key |
| Huge data volume | filter/paging/pushdown |

