# 02 Internal Tables

## Concept (2–3 lines)
- Internal table = **in-memory table** on app server for sets of rows (like array/list).
- Key interview focus: **table types**, **key access complexity**, **read/update patterns**.

## Interview-ready paragraph (say this)
An internal table is ABAP’s main in-memory structure for handling multiple records on the application server, like a working dataset for reports and processing. The key point is choosing the right table type based on access pattern: standard tables for sequential processing and appends, sorted tables when I need ordered output or range access, and hashed tables when I need very fast key-based lookups. For performance I avoid copying large rows, use `ASSIGNING` when needed, and I’m careful with patterns like `BINARY SEARCH` and `FOR ALL ENTRIES` because sorting and initial-table checks decide whether the code is correct and fast.

## Follow-up answers (if interviewer asks deeper)
If the interviewer asks how I choose between standard, sorted, and hashed, I explain it with access complexity and operations. Standard tables are easiest for sequential processing and appends, but key reads are linear, so they degrade with volume. Sorted tables give predictable key access and range reads because the system maintains order, while hashed tables give the fastest key lookup but don’t support index access or ordered loops the same way.

If they ask about common pitfalls, I call out two patterns. `BINARY SEARCH` only works correctly after sorting by the same key, and `FOR ALL ENTRIES` is only safe when the driving table is not initial—otherwise it can read the full database table. In production, these two mistakes are responsible for many “works in dev, fails in prod” incidents.

If they ask about performance tuning, I mention reducing copies and moving joins to the database. I use `ASSIGNING` or field-symbols for large rows, pick hashed tables for repeated lookups, and avoid nested loops that simulate joins in ABAP. For join-heavy logic, I prefer CDS/Open SQL pushdown rather than building everything in internal tables.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is an internal table?
- Q: Standard vs Sorted vs Hashed table?
- Q: What is a table key?
- Q: `APPEND` vs `INSERT`?

### Intermediate
- Q: `READ TABLE ... WITH KEY` vs `READ TABLE ... INDEX`?
- Q: When to use `BINARY SEARCH`?
- Q: What is a field-symbol and why used with tables?
- Q: `LOOP AT ... ASSIGNING` vs `INTO`?

### Advanced
- Q: When does `FOR ALL ENTRIES` help and when does it break?
- Q: What is `LOOP AT GROUP`?
- Q: How do you remove duplicates efficiently?
- Q: How do you choose hashed vs sorted for large volumes?

## Best Answers (1–3 lines)
- A: I use a **standard table** when I mainly append and loop, and I accept that key reads are linear, so it’s best for smaller lists.
- A: I use a **sorted table** when I need ordered output or range reads, and I want predictable \(O(\log n)\) key access.
- A: I use a **hashed table** for heavy key-based lookups because it gives near \(O(1)\) access, but it doesn’t support index-style processing.
- A: For performance on big rows, I loop `ASSIGNING` because it avoids copying table lines.
- A: I use `FOR ALL ENTRIES` only when the driving table is **not initial**, and I clean keys first so I don’t accidentally select the whole DB table.

## Code examples (minimal patterns)

### Declare types (3 kinds)
```abap
DATA it_std    TYPE STANDARD TABLE OF sflight WITH EMPTY KEY.
DATA it_sorted TYPE SORTED   TABLE OF sflight WITH UNIQUE KEY carrid connid fldate.
DATA it_hash   TYPE HASHED   TABLE OF sflight WITH UNIQUE KEY carrid connid fldate.
```

### Safe lookup (hashed/sorted)
```abap
READ TABLE it_hash ASSIGNING FIELD-SYMBOL(<ls>)
  WITH TABLE KEY carrid = lv_carrid connid = lv_connid fldate = lv_date.
IF sy-subrc = 0.
  "found
ENDIF.
```

### Sort + binary search (standard/sorted)
```abap
SORT it_std BY carrid connid fldate.
READ TABLE it_std INTO DATA(ls) WITH KEY carrid = lv_carrid BINARY SEARCH.
```

### Remove duplicates (standard)
```abap
SORT it_std BY carrid connid fldate.
DELETE ADJACENT DUPLICATES FROM it_std COMPARING carrid connid fldate.
```

### `FOR ALL ENTRIES` safe pattern
```abap
IF it_keys IS NOT INITIAL.
  SELECT carrid, connid, fldate
    FROM sflight
    FOR ALL ENTRIES IN @it_keys
    WHERE carrid = @it_keys-carrid
    INTO TABLE @DATA(it_out).
ENDIF.
```

## Common mistakes / traps interviewers check
- Using standard table for huge key lookups (slow linear reads).
- Using `BINARY SEARCH` without sorting by the same key.
- Forgetting `it_keys IS NOT INITIAL` in `FOR ALL ENTRIES` (reads **everything**).
- Modifying table while looping without care (skip rows / dumps).

## Optimization tips (DO / DON’T)
- **DO**: choose table type by access pattern (lookup vs append vs ranges).
- **DO**: use `ASSIGNING` / field-symbols for big rows.
- **DON’T**: `READ ... BINARY SEARCH` on unsorted data.
- **DON’T**: nested loops for joins; push to DB/CDS.

## Real-world scenario-based Qs (strong answers)
- Q: “Need fastest lookup by business key in 1M rows.”  
  A: Use **HASHED** with unique key; fill once; `READ TABLE ... WITH TABLE KEY`.
- Q: “Need range reads + always sorted output.”  
  A: Use **SORTED**; range reads are natural; no extra `SORT`.

## Drawbacks / limitations
- Hashed: no index operations; more memory; unique key required for best usage.
- Sorted: inserts cost more (maintains order); key definition matters.
- Standard: simple but can become slow for key reads at scale.

## Advanced improvements / modern alternatives
- Use **CDS views** for joins/aggregations instead of ABAP table joins in loops.
- Use **AMDP/HANA pushdown** only when justified (volume + heavy calc).

## MEMORY HACK
- **1-line**: “Standard = append list, Sorted = ordered key search, Hashed = fastest key lookup.”
- **Keywords**: Standard, Sorted, Hashed, Key, FAE
- **Quick compare**

| Type | Best for | Key read |
|---|---|---|
| Standard | append + small lists | O(n) |
| Sorted | ordered + ranges | O(log n) |
| Hashed | heavy lookups | O(1) |

