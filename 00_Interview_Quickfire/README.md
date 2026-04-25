# 00 Interview Quickfire (Ultra-short)

## How to use
- Read as flashcards: **Q → answer in 5 seconds**
- If you can’t answer: jump to the linked topic

## Quickfire (sample set — full set is distributed across topics)

### ABAP basics
Q: What is a work area?  
A: **Single row buffer** for a structure/table line; used in `LOOP`, `READ`, `SELECT ... INTO`.

Q: Open SQL vs Native SQL?  
A: **Open SQL** is DB-agnostic + SAP-managed; **Native SQL** is DB-specific, less portable.

### Internal tables
Q: Standard vs Sorted vs Hashed?  
A: **Standard**: index access; **Sorted**: log(n) key read; **Hashed**: O(1) key read.

Q: Biggest `FOR ALL ENTRIES` trap?  
A: If driving table is **initial**, DB returns **everything** → always `IF it IS NOT INITIAL`.

### CDS / RAP
Q: Why CDS in S/4?  
A: **Pushdown** computations to DB + semantic model + reuse in UI/services.

Q: RAP vs SEGW OData?  
A: RAP = **behavior + transactional** model; SEGW = older, manual patterns.

