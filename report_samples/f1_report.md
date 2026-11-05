# Accessibility Audit Report — F1_compliant.pdf

**Document hash (sha256, first 16 chars):** `db4a273f291568b5`  
**Agent iterations:** 6  
**Run finalized:** True  
**Audit log entries:** 6

> **Verdict.** Advisory findings only. This is not a legal ADA or PDF/UA compliance certification. A human reviewer must decide what to remediate.

## Findings summary

| Result | Count |
|---|---|
| pass | 5 |
| cannot verify | 1 |

| Severity | Count |
|---|---|
| Info | 6 |

## Findings detail

### `check_tagged_pdf` — PASS (Info)

**Rationale.** tagged PDF detected

### `get_metadata` — PASS (Info)

**Rationale.** metadata OK (title present, lang en)

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

---

*Advisory findings only. This is not a legal ADA or PDF/UA compliance certification. A human reviewer must decide what to remediate.*