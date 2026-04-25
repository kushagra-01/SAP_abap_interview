# 00 System Design for ABAP (Interview-safe)

## Interview-ready paragraph (say this)
When I discuss system design in SAP, I focus on clean-core and enterprise reliability. I try to keep the standard system stable by using released APIs and official extension points, and I push data-intensive work down to CDS or efficient SQL instead of doing heavy loops on the application server. For integrations I define clear contracts using OData, BAPI/RFC, or IDocs depending on sync/async needs, and I always plan for retries by making processing idempotent. Finally, I define LUW boundaries, locking strategy, and observability—logging and monitoring—so the solution remains stable under real production load.

## Follow-up answers (if interviewer asks deeper)
If they ask how I choose integration style, I explain it based on coupling and latency. Interactive UIs and synchronous calls typically fit OData or RFC, while asynchronous and high-volume scenarios fit IDocs or queued processing, because they tolerate retries and temporary outages better.

If they ask about reliability, I highlight three concrete mechanisms: LUW boundaries, locks for concurrency, and idempotency for retries. Without these, systems appear fine in testing but fail under real concurrent usage and network failures.

If they ask about performance, I explain the “pushdown then orchestrate” approach: use CDS/HANA for joins and aggregations, keep ABAP for process orchestration and business rules, and always measure before optimizing. This balance keeps solutions fast without becoming unmaintainable.

## Mindset (what interviewers want)
- **Clean core**: avoid modifying standard; use extensions/released APIs
- **Pushdown**: CDS/AMDP/HANA over ABAP loops for heavy joins/aggregations
- **Integration**: stable contracts (BAPI/RFC/OData), idempotency, retries
- **Reliability**: update tasks, locks, commits, error logging, monitoring

## Common architecture prompts (short answers)
Q: How do you integrate SAP with non-SAP?  
A: Use **OData/RFC/BAPI/IDoc**, define **contract**, add **retries + logging**, handle **auth + errors**.

Q: How do you design a high-volume report?  
A: **Filter early**, select **only needed fields**, use **indexes**, avoid nested loops, prefer **CDS**.

Q: How do you ensure data consistency?  
A: Use **LOCK objects**, **LUW**, controlled `COMMIT WORK`, and avoid partial commits.

## MEMORY HACK
- **1-line**: “Keep core clean, push logic down, design stable contracts.”
- **Keywords**: CleanCore, Pushdown, Contract, LUW, Observability

