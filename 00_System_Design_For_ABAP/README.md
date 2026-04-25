# 00 System Design for ABAP (Interview-safe)

## Interview-ready paragraph (say this)
When I discuss system design in SAP, I focus on clean-core and enterprise reliability. I try to keep the standard system stable by using released APIs and official extension points, and I push data-intensive work down to CDS or efficient SQL instead of doing heavy loops on the application server. For integrations I define clear contracts using OData, BAPI/RFC, or IDocs depending on sync/async needs, and I always plan for retries by making processing idempotent. Finally, I define LUW boundaries, locking strategy, and observability—logging and monitoring—so the solution remains stable under real production load.

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

