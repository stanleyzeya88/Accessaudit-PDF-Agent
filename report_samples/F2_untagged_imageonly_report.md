# Accessibility Audit Report — F2_untagged_imageonly.pdf

**Document hash (sha256, first 16 chars):** `c26d4c398be8dc0d`  
**Agent iterations:** 4  
**Run finalized:** True  
**Audit log entries:** 5

> **Verdict.** Advisory findings only. This is not a legal ADA or PDF/UA compliance certification. A human reviewer must decide what to remediate.

## Findings summary

| Result | Count |
|---|---|
| fail | 2 |

| Severity | Count |
|---|---|
| Critical | 1 |
| Major | 1 |

## Findings detail

### `check_tagged_pdf` — FAIL (Critical)

**Rationale.** The PDF is untagged as it lacks a /StructTreeRoot and /MarkInfo /Marked.

**Suggested remediation.** To improve accessibility, ensure the PDF is tagged properly with a /StructTreeRoot and /MarkInfo.

### `get_metadata` — FAIL (Major)

**Rationale.** The PDF metadata is missing the language attribute (/Lang), which is essential for accessibility.

**Suggested remediation.** Add the /Lang attribute to the PDF metadata to specify the document's language.

## Recommended remediation (priority order)

1. **[Critical]** check_tagged_pdf — To improve accessibility, ensure the PDF is tagged properly with a /StructTreeRoot and /MarkInfo.
2. **[Major]** get_metadata — Add the /Lang attribute to the PDF metadata to specify the document's language.

---

*Advisory findings only. This is not a legal ADA or PDF/UA compliance certification. A human reviewer must decide what to remediate.*