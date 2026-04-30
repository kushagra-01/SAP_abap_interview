# 15 S/4HANA Concepts

## Concept (2–3 lines)
- S/4HANA = simplified data model + HANA-first + Fiori-first with **clean core** principles.
- Interview focus: **simplification**, **CDS**, **embedded analytics**, **extensibility** approach.

## Interview-ready paragraph (say this)
S/4HANA is SAP’s modern ERP optimized for HANA, with a simplified data model and a strong focus on Fiori-based user experience. In interviews I explain that the biggest shift is how we build and extend: we reuse standard content, prefer CDS as the semantic layer, and follow clean-core principles so upgrades remain safe. Extensibility is done through released APIs, enhancement points, and in-app or side-by-side extensions depending on complexity, rather than modifying standard code. From a developer perspective, I connect this to performance and consistency—pushdown, reuse, and clear contracts—so solutions scale and remain maintainable over time.

## Follow-up answers (if interviewer asks deeper)
If they ask what “simplification” means technically, I explain that S/4 removes redundant aggregates and some legacy tables, and instead encourages calculating views dynamically using CDS and HANA capabilities. That changes how custom code should read data: reuse standard views/APIs rather than hardcoding legacy table assumptions.

If they ask about clean core, I explain it’s an upgrade strategy as much as a development guideline. We avoid modifications, use released APIs and extension points, and choose between in-app and side-by-side extensibility based on how intrusive or large the custom requirement is.

If they ask about modern development direction, I connect the stack: CDS for the semantic model, RAP for transactional services, and OData/Fiori elements for consumption. This reduces custom glue code and gives a consistent, supportable architecture.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is S/4HANA?
- Q: What is “simplification” in S/4?
- Q: Why Fiori-first?

### Intermediate
- Q: Clean core: what does it mean?
- Q: Extensibility types (in-app vs side-by-side)?
- Q: CDS role in S/4?
- Q: Embedded analytics idea?

### Advanced
- Q: How do you avoid core modifications but still meet business needs?
- Q: Released APIs: why important?
- Q: Common migration impact on custom code?

## Best Answers (1–3 lines)
- A: I describe **S/4HANA** as SAP’s ERP optimized for **HANA**, with a simplified data model and a Fiori-first user experience.
- A: When I say “simplification,” I mean fewer redundant aggregates and more compute-on-the-fly via CDS views and pushdown.
- A: For **clean core**, I avoid modifications and extend using released APIs, enhancement points/BAdIs, and in-app or side-by-side extensibility depending on scope.
- A: I treat **CDS** as the semantic layer that powers apps, analytics, and service exposure in S/4.

## Code examples (minimal patterns)
### “Released API / view first” mindset (pattern)
```abap
SELECT FROM i_businesspartner
  FIELDS businesspartner, lastname, firstname
  WHERE businesspartner = @lv_bp
  INTO @DATA(ls_bp).
```

## Common mistakes / traps interviewers check
- Claiming “we modified standard” casually (clean core red flag).
- Building custom tables for data already modeled in standard CDS/APIs.
- Doing heavy ABAP calculations instead of using CDS/analytics capabilities.

## Optimization tips (DO / DON’T)
- **DO**: prefer **standard CDS / released APIs** before custom code.
- **DO**: extend via BAdIs/extension points; keep changes upgrade-safe.
- **DON’T**: create core modifications unless absolutely necessary (and documented).

## Real-world scenario-based questions with strong answers
- Q: “Need extra field in standard process.” Best approach?  
  A: Use **extensibility** (in-app/append where allowed) + enhancement points; persist only if required; avoid mod.
- Q: “Custom report slow post-migration.” Why?  
  A: Data model changes + missing filters; refactor to **CDS** + pushdown; re-check indexes.

## Drawbacks / limitations
- Clean-core constraints can limit “quick hacks”; requires discipline and API-first thinking.
- Modern stack (CDS/RAP) has learning curve for classical ABAP developers.

## Advanced improvements / modern alternatives
- Prefer **RAP + CDS + Fiori elements** for new apps.
- Prefer **side-by-side** extensions for heavy custom logic that shouldn’t live in core.

## MEMORY HACK
- **1-line**: “S/4 = HANA-first + Fiori-first + clean core; build on CDS and released APIs.”
- **Keywords**: Simplification, CleanCore, ReleasedAPI, CDS, Extensibility
- **Quick compare**

| Extensibility | Use when |
|---|---|
| In-app | small fields/rules close to ERP |
| Side-by-side | big custom apps/integrations |

