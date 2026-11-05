# Accessibility Audit Report — F4_wrong_language.pdf

**Document hash (sha256, first 16 chars):** `f58c6aa7fb0b8007`  
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

### `get_metadata` — FAIL (Major)

**Rationale.** The document's language metadata indicates French (fr-FR), which does not match the expected English (en) language.

**Suggested remediation.** Update the language metadata to reflect the correct language of the document.

### `check_tagged_pdf` — PASS (Info)

**Rationale.** tagged PDF detected

### `check_text_extractability` — PASS (Info)

**Rationale.** text extractable (1/1 pages above 100 chars)

### `inventory_alt_text` — PASS (Info)

**Rationale.** all 0 images have /Alt

### `check_heading_order` — PASS (Info)

**Rationale.** heading sizes decrease monotonically (heuristic pass)

### `inspect_links` — CANNOT VERIFY (Info)

**Rationale.** cannot verify — annotations carry no /Contents; visible text not inspectable

## Cannot verify

The agent could not confirm the following checks and does **not** report them as passes:

- `inspect_links`

## Recommended remediation (priority order)

1. **[Major]** get_metadata — Update the language metadata to reflect the correct language of the document.

---

*Advisory findings only. This is not a legal ADA or PDF/UA compliance certification. A human reviewer must decide what to remediate.*