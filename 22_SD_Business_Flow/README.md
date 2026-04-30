# 22 SD Business Flow (Lead-to-Cash)

## Concept (2–3 lines)
- SD flow is the end-to-end process from **sales order → delivery → billing → FI posting**.
- Interview focus: knowing the flow, key documents, and where ABAP typically fits (prints, pricing exits, enhancements, interfaces).

## Interview-ready paragraph (say this)
In SD I describe the standard lead-to-cash flow: create a sales order, create delivery and pick/pack, post goods issue, and then create billing which posts to FI. From an ABAP perspective, I usually plug in at outputs (forms), enhancements/user exits/BAdIs for validations, pricing routines, or integration triggers, and I’m careful about performance and correctness because these documents are high volume. I also mention that debugging SD issues often means following document flow and understanding where data is derived (pricing, copy control, status management).

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is the SD end-to-end flow?
- Q: Sales order vs delivery vs billing?
- Q: What is PGI?

### Intermediate
- Q: Where does billing post to FI?
- Q: Common ABAP touchpoints in SD?
- Q: What is “document flow” used for?

### Advanced
- Q: Typical causes of pricing/output issues and how to debug?
- Q: How do you design enhancements to be upgrade-safe?
- Q: How do you handle high-volume SD processing safely?

## Best Answers (1–3 lines)
- A: When I explain SD end-to-end, I say **Order → Delivery → PGI → Billing → FI**.
- A: In SD, I usually touch **outputs/forms**, **enhancements/BAdIs**, **interfaces**, and SD reports/validations.
- A: To debug SD issues, I follow **document flow** and then check determination logic like pricing, outputs, and copy control.

## Common mistakes / traps
- Treating SD logic as “just ABAP” without understanding document flow/status.
- Adding heavy selects in exits that run for every item (performance killer).
- Not handling reprocessing/retries (duplicates).

## Optimization tips (DO / DON’T)
- **DO**: know the key business keys (sales doc, delivery, billing) and where they are created.
- **DO**: keep exits fast; prefetch required data; avoid select-in-loop.
- **DON’T**: change standard flow without understanding downstream impacts (billing/FI).

## MEMORY HACK
- **1-line**: “SD = Order → Delivery → PGI → Billing → FI; ABAP hooks = forms + exits + interfaces.”
- **Keywords**: VA01, VL01N, PGI, VF01, Document flow

