# 17 Conversions (LSMW, BDC)

## Concept (2–3 lines)
- Conversions = moving legacy/external data into SAP in a controlled way (one-time or repeated loads).
- Interview focus: **LSMW** for quick legacy loads, **BDC** for screen-based automation, and safe alternatives (BAPIs/IDocs).

## Interview-ready paragraph (say this)
For data conversions, I first try to use stable business APIs like BAPIs or IDocs because they validate business rules and are upgrade-safe. If that’s not possible, I use LSMW for faster legacy loading and BDC when we must simulate a transaction screen flow. The production pitfalls are data quality, error handling, and repeatability: I always design with validation, clear logs, and restartability so a failed run can be corrected and reprocessed without duplicates.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is LSMW used for?
- Q: What is BDC?
- Q: BDC session vs call transaction?

### Intermediate
- Q: Why prefer BAPI/IDoc over BDC?
- Q: What is SHDB used for?
- Q: Where do you monitor BDC sessions?

### Advanced
- Q: How do you make a conversion restartable/idempotent?
- Q: Typical causes of BDC failures and how to debug?
- Q: How do you validate and reconcile loaded data?

## Best Answers (1–3 lines)
- A: I use **LSMW** when I need a quick legacy load with mapping + conversion for one-time migrations.
- A: I use **BDC** when I must automate a transaction by feeding dynpro fields like a user would.
- A: I choose **Session BDC** (SM35) for high-volume/background robustness, and **CALL TRANSACTION** when I need immediate execution with tight message handling.
- A: When possible, I prefer **BAPIs/IDocs** because they apply business validations and are less brittle than screen automation.

## Code examples (minimal patterns)
### BDC data + call transaction (pattern)
```abap
DATA: lt_bdc TYPE TABLE OF bdcdata,
      lt_msg TYPE TABLE OF bdcmsgcoll.

APPEND VALUE #( program  = 'SAPMV45A' dynpro = '0101' dynbegin = 'X' ) TO lt_bdc.
APPEND VALUE #( fnam = 'VBAK-AUART' fval = 'OR' ) TO lt_bdc.
APPEND VALUE #( fnam = 'BDC_OKCODE' fval = '/00' ) TO lt_bdc.

CALL TRANSACTION 'VA01'
  USING lt_bdc
  MODE  'N'
  UPDATE 'S'
  MESSAGES INTO lt_msg.
```

## Common mistakes / traps
- Treating BDC as stable long-term integration (it breaks on screen changes/customizing).
- No reconciliation (loaded counts don’t match source; missing “success criteria”).
- No restart logic (a rerun creates duplicates).

## Optimization tips (DO / DON’T)
- **DO**: prefer BAPI/IDoc (or RAP/OData for modern APIs) over BDC where possible.
- **DO**: design restartability: store run ID, source key, status, and error text.
- **DON’T**: ignore commit/LUW; control commits in consistent batches.

## MEMORY HACK
- **1-line**: “Conversion: prefer BAPI/IDoc; LSMW quick; BDC = screen automation (brittle) + logs + restartability.”
- **Keywords**: LSMW, BDC, SHDB, SM35, Reconcile, Idempotent

