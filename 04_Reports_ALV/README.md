# 04 Reports + ALV

## Concept (2–3 lines)
- Reports = ABAP programs for **data extraction + display** (classic: selection screen + output).
- ALV = standard SAP list UI for **sorting/filtering/layout/export** with minimal code.

## Interview-ready paragraph (say this)
In ABAP, reports are typically built around a selection screen, a single efficient data read, and a clean output. For output, ALV is the standard choice because it provides sorting, filtering, totals, layout variants, and export with minimal custom UI work. When performance matters, I focus on pushing filtering and aggregation to the database or CDS, selecting only required fields, and avoiding building massive internal tables unnecessarily. For large datasets on HANA, I mention options like ALV with IDA or analytics-style CDS approaches where appropriate.

## Follow-up answers (if interviewer asks deeper)
If they ask “SALV vs classical ALV,” I explain SALV is OO-based and quickest for standard grids, while classical REUSE_ALV_* function modules offer deeper event customization but are more procedural and verbose. For most report outputs, SALV is the fastest clean solution unless advanced event handling is required.

If they ask about performance for big lists, I explain the real bottleneck is usually data volume and DB access, not the grid itself. I enforce filters, select only needed fields, push joins/aggregations to CDS, and avoid loading hundreds of thousands of rows into memory if the user only needs a summary or paging.

If they ask about real-world usability, I mention layout variants, totals, sorting/filtering, and user commands like hotspots. I keep the UI standard so users get consistent SAP behavior, and I focus my custom code on business validations and correct data.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is a report?
- Q: What is ALV and why used?
- Q: Classical list vs ALV?

### Intermediate
- Q: REUSE_ALV_* vs SALV?
- Q: What is field catalog?
- Q: How do you add hotspot / user command?

### Advanced
- Q: ALV performance for large data?
- Q: When to use ALV with IDA?
- Q: How to handle authorization + large volumes cleanly?

## Best Answers (1–3 lines)
- A: I use **ALV** because it gives standard UI features (sort, filter, totals, export) without building custom screens.
- A: I use **SALV** when I want the fastest OO setup for a standard grid, and I use classic ALV function modules only when I need deep legacy event customization.
- A: For huge datasets, I push logic down (CDS/SQL) and consider **ALV with IDA** on HANA or a paging/summary strategy instead of loading everything.

## Code examples (minimal patterns)

### SALV quick display (fast interview pattern)
```abap
cl_salv_table=>factory(
  IMPORTING r_salv_table = DATA(lo_alv)
  CHANGING  t_table      = it_out ).
lo_alv->display( ).
```

## Common mistakes / traps
- Selecting too much data then ALV (slow) instead of filtering in DB/CDS.
- Over-customizing ALV when standard features already exist.

## Optimization tips (DO / DON’T)
- **DO**: minimize fields; aggregate/filter in DB/CDS.
- **DO**: consider **IDA** for very large datasets on HANA.
- **DON’T**: build huge internal tables unnecessarily.

## Scenario-based questions (strong answers)
- Q: “ALV takes 2 minutes for 500k rows. Fix?”  
  A: Reduce rows/fields, push logic to **CDS**, add indexes, consider **IDA/paging**.

## Drawbacks / limitations
- Classic ALV function modules can get messy for complex events/customization.
- IDA requires HANA capabilities and suitable data model.

## Advanced improvements / modern alternatives
- Prefer **Fiori elements (CDS + OData + RAP)** for modern UI-driven apps.
- Use **CDS Analytical queries** for reporting style scenarios.

## MEMORY HACK
- **1-line**: “Reports = selection + data; ALV = standard grid features with minimal code.”
- **Keywords**: Report, SelectionScreen, SALV, FieldCatalog, IDA
- **Quick compare**

| Option | Best for |
|---|---|
| Classical list | tiny output, quick debug |
| SALV | fastest ALV setup |
| IDA | huge data on HANA |

