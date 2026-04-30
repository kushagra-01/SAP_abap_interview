# 26 Authorization Check (Security)

## Concept (2–3 lines)
- Authorizations control what a user can do via **roles** (authorization objects/fields).
- Interview focus: **PFCG roles**, checking failures (SU53), and explicit checks in ABAP.

## Interview-ready paragraph (say this)
In SAP security, users get roles that contain authorization objects with field values like activity and organizational level. When troubleshooting an authorization issue, I reproduce with the user/context, check SU53, and use traces if needed to find the missing object/field. In ABAP, I add explicit checks where business logic needs protection using `AUTHORITY-CHECK`, and I keep it aligned with role design (SU24 proposals) so security is consistent and maintainable.

## Interview Questions (Beginner → Advanced)
### Beginner
- Q: What is a role (PFCG)?
- Q: What is an authorization object?
- Q: What is SU53 used for?

### Intermediate
- Q: When do you use AUTHORITY-CHECK in ABAP?
- Q: Why can authorization fail even if “role exists”?
- Q: How do you troubleshoot missing authorizations?

### Advanced
- Q: How do you design authorization checks to avoid security gaps?
- Q: What is the risk of checking only `sy-subrc` without context?
- Q: How do you handle org-level restrictions (company code/plant)?

## Best Answers (1–3 lines)
- A: I manage access through **PFCG roles**, which contain authorization objects and field values like activity and org level.
- A: When a user gets “no authorization,” I check **SU53** first to see the last failed authorization check.
- A: For sensitive operations, I add an explicit `AUTHORITY-CHECK` and fail fast with a clear error and enough context for support.

## Code examples (minimal patterns)
### AUTHORITY-CHECK (pattern)
```abap
AUTHORITY-CHECK OBJECT 'V_VBAK_VKO'
  ID 'ACTVT' FIELD '03'
  ID 'VKORG' FIELD lv_vkorg.

IF sy-subrc <> 0.
  MESSAGE e001(zmsg) WITH 'Not authorized'.
ENDIF.
```

## Common mistakes / traps
- No check on sensitive operations (security gap).
- Checking wrong org field (over-permissive or blocks valid users).
- Debugging without SU53/trace evidence.

## Optimization tips (DO / DON’T)
- **DO**: validate both authorization and business ownership rules (they’re different).
- **DO**: log denied actions with business key for audit/support.
- **DON’T**: hard-code user IDs; use roles/objects.

## MEMORY HACK
- **1-line**: “Security = roles (PFCG) → objects/fields; troubleshoot via SU53; enforce via AUTHORITY-CHECK.”
- **Keywords**: PFCG, Authorization object, SU53, AUTHORITY-CHECK, Org level

