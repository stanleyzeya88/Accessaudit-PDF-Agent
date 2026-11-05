# Accessibility Audit Report — F3_missing_alt_text.pdf

**Document hash (sha256, first 16 chars):** `714c3bd5f6bebd64`  
**Agent iterations:** 7  
**Run finalized:** True  
**Audit log entries:** 8

> **Verdict.** Advisory findings only. This is not a legal ADA or PDF/UA compliance certification. A human reviewer must decide what to remediate.

## Findings summary

| Result | Count |
|---|---|
| pass | 4 |
| fail | 1 |
| cannot verify | 1 |

| Severity | Count |
|---|---|
| Major | 1 |
| Info | 5 |

## Findings detail

### `inventory_alt_text` — FAIL (Major)

**Rationale.** The PDF contains one image that lacks alternative text, which is essential for accessibility.

**Suggested remediation.** Add appropriate alternative text to the image to ensure it is accessible to users with visual impairments.

### `check_tagged_pdf` — PASS (Info)

**Rationale.** tagged PDF detected

### `get_metadata` — PASS (Info)

**Rationale.** metadata OK (title present, lang en)

### `check_text_extractability` — PASS (Info)

**Rationale.** text extractable (1/1 pages above 100 chars)

### `check_heading_order` — PASS (Info)

**Rationale.** heading sizes decrease monotonically (heuristic pass)

### `inspect_links` — CANNOT VERIFY (Info)

**Rationale.** cannot verify — annotations carry no /Contents; visible text not inspectable

## Cannot verify

The agent could not confirm the following checks and does **not** report them as passes:

- `inspect_links`

## Recommended remediation (priority order)

1. **[Major]** inventory_alt_text — Add appropriate alternative text to the image to ensure it is accessible to users with visual impairments.

---

*Advisory findings only. This is not a legal ADA or PDF/UA compliance certification. A human reviewer must decide what to remediate.*