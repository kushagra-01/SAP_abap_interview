# 21 Testing + UAT + Support

## Concept (2–3 lines)
- Testing proves correctness; UAT proves business acceptance; support keeps production stable.
- Interview focus: ABAP Unit/ATC basics, defect triage, and “how you work with functional + basis”.

## Interview-ready paragraph (say this)
I explain testing in SAP as layered: developer testing with ABAP Unit and ATC checks, functional testing in QA, and UAT for real business scenarios and sign-off. For support, the key is fast triage and reproducibility: confirm impact, find the failing document/user, check logs (SLG1), dumps (ST22), and traces, then implement the smallest safe fix and transport it with a clear test plan. I also highlight that stable logging and idempotent design make production issues much easier to resolve.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is UAT?
- Q: What is production support in SAP?
- Q: What is ABAP Unit?

### Intermediate
- Q: What is ATC and why is it used?
- Q: What is a good defect triage flow?
- Q: What evidence do you ask for from users?

### Advanced
- Q: How do you debug production issues safely?
- Q: How do you reduce recurring incidents?
- Q: How do you design logs for supportability?

## Best Answers (1–3 lines)
- A: I treat **UAT** as the business sign-off phase where real scenarios are validated in a QA-like system.
- A: I use **ABAP Unit** to test business logic in isolation, and I rely on **ATC** to enforce code quality, performance, and security checks.
- A: In support, I follow a simple flow: reproduce the issue, isolate the root cause, implement the smallest safe fix, test it, transport it, and then monitor production.

## Common mistakes / traps
- Fixing symptoms without root cause (incident repeats).
- No logs/keys in error messages (support can’t trace documents).
- Making risky changes directly in production.

## Optimization tips (DO / DON’T)
- **DO**: log with business keys (doc no, partner, msgid), not just “failed”.
- **DO**: create a tight test plan (happy path + edge + regression).
- **DON’T**: ignore ATC/performance checks; they prevent expensive prod issues.

## MEMORY HACK
- **1-line**: “Test (ABAP Unit/ATC) → QA → UAT sign-off; Support = logs + dumps + minimal safe fix.”
- **Keywords**: UAT, ABAP Unit, ATC, SLG1, ST22, Triage

