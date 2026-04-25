# 12 CDS Views

## Concept (2–3 lines)
- CDS = semantic data model on DB layer: **joins/associations/annotations** reusable across apps.
- Interview focus: **pushdown**, associations vs joins, authorization, and consumption (ALP/Fiori/OData).

## Interview-ready paragraph (say this)
CDS views are the modern way to model data in S/4 because they combine SQL logic with semantic metadata and annotations, so the same model can be reused across reports, services, and Fiori. I use CDS to push joins, filters, and aggregations down to the database, which reduces ABAP-side loops and data transfer. I’m careful with associations and cardinality so I don’t introduce duplicates or heavy joins accidentally, and I plan authorization early using DCL or access control annotations. In practice I prefer a layered approach: interface view for stable semantics, then a projection/consumption view for specific use cases like RAP or UI.

## Follow-up answers (if interviewer asks deeper)
If they ask “associations vs joins,” I explain associations are modeled relationships that enable navigation and reuse, while joins directly combine data at query time. Associations are great for semantic modeling, but you still need to be careful about cardinality and how they are consumed, because that’s where duplicates and unexpected heavy joins can appear.

If they ask about security, I explain that CDS can enforce authorization consistently through DCL and access control checks, which is critical when the same view is reused for OData and UI. I treat authorization as part of the model, not something added as an afterthought in a report.

If they ask about maintainability, I explain the layered approach: stable interface views for core semantics, then projections/consumption views for specific apps. This prevents “one mega view” from becoming unmaintainable and keeps changes safer when new requirements appear.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is CDS and why used?
- Q: CDS view vs SE11 view?
- Q: What are annotations?

### Intermediate
- Q: Association vs join?
- Q: CDS authorization: DCL/`@AccessControl`?
- Q: What is a CDS projection view?
- Q: How is CDS consumed (RAP/OData/ALV/Fiori)?

### Advanced
- Q: `@UI`, `@Consumption`, `@Analytics`: why important?
- Q: What are parameters vs filters?
- Q: Performance traps in CDS (cardinality, unnecessary associations)?

## Best Answers (1–3 lines)
- A: CDS = DB-side semantic model with **SQL + metadata annotations** for reuse.
- A: CDS enables **pushdown** and consistent semantics across reports, OData, and Fiori.
- A: Association is a modeled relationship (navigational); join is immediate combination—choose for performance/need.
- A: Authorization can be enforced via **DCL** + `@AccessControl.authorizationCheck`.

## Code examples (minimal patterns)
### Minimal CDS view skeleton (memory pattern)
```abap
@AbapCatalog.sqlViewName: 'ZVFLIGHT'
@AccessControl.authorizationCheck: #CHECK
define view ZI_Flight as select from sflight
{
  key carrid,
  key connid,
      fldate,
      price
}
```

### Consume CDS in ABAP (pattern)
```abap
SELECT FROM zi_flight
  FIELDS carrid, connid, fldate, price
  WHERE carrid = @lv_carrid
  INTO TABLE @DATA(it_f).
```

## Common mistakes / traps interviewers check
- Using CDS but still selecting huge results (no filters) → memory/runtime issues.
- Wrong association cardinality assumptions → duplicates or unexpected joins.
- Missing/incorrect authorization strategy (data leaks in services).

## Optimization tips (DO / DON’T)
- **DO**: keep CDS reusable (interface view) and expose via projection (consumption).
- **DO**: filter early; avoid selecting unnecessary fields.
- **DO**: model associations carefully; use joins only where needed.
- **DON’T**: embed complex business logic everywhere in CDS (maintainability).

## Real-world scenario-based questions with strong answers
- Q: “Need same data in report + OData.” Best approach?  
  A: Build **CDS interface view** once, consume via report and expose via **RAP/OData**.
- Q: “CDS result duplicates rows unexpectedly.” Why?  
  A: Likely join/association cardinality; fix keys, join condition, or association usage.

## Drawbacks / limitations
- Debugging can be less straightforward than ABAP; needs SQL mindset.
- Complex annotation stacks can become hard to maintain without standards.

## Advanced improvements / modern alternatives
- Use **CDS + RAP** to standardize service + behavior instead of SEGW-style manual OData.
- Use analytical CDS/query views for reporting (pushdown + semantics).

## MEMORY HACK
- **1-line**: “CDS = SQL + semantics + annotations; build once, reuse everywhere, push down heavy logic.”
- **Keywords**: Pushdown, Annotation, Association, DCL, Projection
- **Quick compare**

| Item | CDS answer |
|---|---|
| SE11 view | DB view, less semantics |
| CDS view | semantic + annotations + reuse |
| Join | immediate combine |
| Association | navigational relationship |

