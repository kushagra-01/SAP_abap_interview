# 23 Debugging (IDoc + Batch)

## Concept (2–3 lines)
- Debugging in SAP is about tracing execution + data flow across dialog, batch, and integration layers.
- Interview focus: **breakpoints**, **external debugging**, and the standard monitoring tools for **IDocs** and **batch**.

## Interview-ready paragraph (say this)
When debugging SAP issues, I start from the symptom and choose the right trace level: runtime dumps (ST22), application logs (SLG1), and then step debugging with breakpoints. For integration, I follow the message lifecycle: for IDocs I check status and segments in WE02/WE05 and reprocess in BD87, then debug the inbound function module or user-exit when needed. For batch issues I inspect the job log/spool in SM37 or BDC sessions in SM35, and I use external/batch debugging where appropriate, always being careful about production safety.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: How do you start the ABAP debugger?
- Q: What is ST22 used for?
- Q: Where do you check application logs?

### Intermediate
- Q: How do you debug a background job?
- Q: How do you analyze an IDoc failure?
- Q: WE02 vs WE05 vs BD87?

### Advanced
- Q: How do you debug an inbound IDoc processing function?
- Q: How do you debug BDC failures (batch input)?
- Q: What information do you capture for supportability?

## Best Answers (1–3 lines)
- A: I triage fast with standard tools: **ST22** for dumps, **SLG1** for application logs, **SM37** for job log/spool, and **SM35** for BDC sessions.
- A: For IDocs, I check status and segments in **WE02/WE05**, and I reprocess failures from **BD87**.
- A: For an IDoc error, I read the status message first, identify the processing step, and then debug the inbound processing function module or exit.

## Common mistakes / traps
- Debugging without business key context (can’t reproduce).
- Ignoring logs/dumps and jumping straight to step debugging.
- In IDoc: fixing data mapping without understanding status records and partner profile config.

## Optimization tips (DO / DON’T)
- **DO**: capture keys early: document number, partner, msgid, user, timestamp.
- **DO**: use monitoring first, debugger second (faster root cause).
- **DON’T**: debug in production unless policy allows; prefer QA reproduction.

## MEMORY HACK
- **1-line**: “Debug by layer: ST22 dumps, SLG1 logs, SM37 batch, WE02/05 + BD87 IDocs, SM35 BDC.”
- **Keywords**: /h, Breakpoint, ST22, SLG1, WE02, BD87, SM37, SM35

