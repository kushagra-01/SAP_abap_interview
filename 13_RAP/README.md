# 13 RAP (RESTful ABAP Programming)

## Concept (2–3 lines)
- RAP builds **transactional services** with a standardized model: data model (CDS) + **behavior** + service exposure.
- Interview focus: **managed vs unmanaged**, **behavior definition**, **EML**, draft, validations/actions.

## Interview-ready paragraph (say this)
RAP is SAP’s modern programming model for building transactional business services in a consistent, upgrade-friendly way. I start with a CDS-based data model, then define behavior for create/update/delete along with validations, determinations, and actions, and finally expose the service for consumption—typically as OData for Fiori or integrations. For straightforward CRUD I choose managed RAP so the framework handles persistence, and for complex legacy save logic I choose unmanaged RAP while still keeping a clean service contract. I also pay attention to concurrency, meaningful error reporting, and performance by pushing heavy joins and filtering down to CDS.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is RAP in SAP?
- Q: Why RAP over classical CRUD coding?
- Q: What is a Business Object in RAP?

### Intermediate
- Q: Managed vs Unmanaged scenario?
- Q: What is Behavior Definition (BDEF)?
- Q: What is EML?
- Q: What is draft?

### Advanced
- Q: Validations vs Determinations vs Actions?
- Q: How do you handle locks/concurrency in RAP?
- Q: How to expose RAP as OData?

## Best Answers (1–3 lines)
- A: RAP = framework to build **transactional REST services** with consistent patterns (model + behavior + service).
- A: Managed = RAP handles persistence; Unmanaged = you implement save logic (legacy/complex cases).
- A: BDEF defines allowed operations + validations/actions; behavior impl contains logic.
- A: EML is ABAP language to **read/modify entities** in RAP uniformly.
- A: Draft supports UI “edit/save later” with temporary data + validations.

## Code examples (minimal patterns)
### EML modify (pattern)
```abap
MODIFY ENTITIES OF zi_booking
  ENTITY Booking
  CREATE FROM VALUE #( ( %cid = '1' CustomerId = lv_cust Status = 'N' ) )
  REPORTED DATA(lt_reported)
  FAILED   DATA(lt_failed).
```

### Action call (pattern)
```abap
MODIFY ENTITIES OF zi_booking
  ENTITY Booking
  EXECUTE confirm FROM VALUE #( ( BookingId = lv_id ) )
  FAILED DATA(lt_failed).
```

## Common mistakes / traps interviewers check
- Treating RAP like “just another OData tool” (it’s **transaction + behavior** model).
- Choosing managed when you need complex legacy persistence (or unmanaged when not needed).
- Ignoring validation/error reporting (returned failures not handled).

## Optimization tips (DO / DON’T)
- **DO**: model data in CDS; keep behavior logic focused (validate/determine/action).
- **DO**: use EML patterns; return meaningful messages and failed keys.
- **DON’T**: write heavy DB logic inside behavior without pushdown/CDS thinking.

## Real-world scenario-based questions with strong answers
- Q: “Need standard CRUD + validations quickly.” What choose?  
  A: **Managed RAP** with CDS + behavior validations; expose service binding.
- Q: “Existing legacy tables with complex save.” What choose?  
  A: **Unmanaged RAP**; implement save/determinations while still exposing consistent API.

## Drawbacks / limitations
- Learning curve: artifacts (CDS, behavior, service binding) + framework rules.
- Unmanaged scenarios can become complex if legacy logic is messy.

## Advanced improvements / modern alternatives
- RAP is the modern alternative to **SEGW** and many classical custom CRUD approaches.
- Combine with **Fiori elements** for rapid UI generation.

## MEMORY HACK
- **1-line**: “RAP = CDS model + behavior rules + service binding → standard transactional API.”
- **Keywords**: Managed, Unmanaged, BDEF, EML, Draft
- **Quick compare**

| Type | Use when | Who saves |
|---|---|---|
| Managed | simple CRUD on tables | RAP |
| Unmanaged | complex legacy save | You |

