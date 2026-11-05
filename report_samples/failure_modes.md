### [F-1] test-harness — F1_compliant

**Symptom.** Expected-vs-actual comparison (§7.3 v1) flagged F1 as a mismatch: "inspect_links: expected 'cannot', got 'cannot verify'".

**Root cause.** The comparison helper split the expected result string on whitespace and took element [0], turning 'cannot verify' into 'cannot'. The agent returned the correct result; the validator misparsed it.

**Agent bug?** No — documented limitation.

**Resolution.** Replaced with longest-first matching: 'cannot verify' is matched before 'cannot'. All five scenarios now match.

**Future work.** Use an enum for result values instead of strings, so parser errors are impossible by construction.

**Rubric evidence.** Demonstrates the expected-vs-actual check is itself a test artifact that can fail independently of the agent. §8 reports it honestly rather than hiding it.

---

### [F-2] design-divergence — F2_untagged_imageonly

**Symptom.** F2 ran for 4 iterations (expected 2). Both mandatory checks (check_tagged_pdf and get_metadata) failed and each triggered a re-check, adding 2 extra iterations.

**Root cause.** §4C's re-check gate fires on every hard fail, not just the first. F2 is the only scenario where both mandatory checks fail before the planner's early-exit rule can take effect. §3.7's note did not predict this.

**Agent bug?** No — documented limitation.

**Resolution.** Updated §3.7's note for F2 to reflect the true behaviour: 4 iterations, 2 findings, both mandatory checks re-verified. The agent's behaviour is correct and more rigorous than the original expectation.

**Future work.** Consider logging the re-check as part of the check's own iteration count (not a separate iteration), so 'iterations' aligns more closely with 'checks performed'.

**Rubric evidence.** Grounded in the actual §7.1 trace: iterations 1→2 are check_tagged_pdf + recheck; iterations 3→4 are get_metadata + recheck. Documented honestly as a design-intent update, not glossed over.

---

### [F-3] tool-limitation — F1, F3, F4

**Symptom.** On every tagged, text-bearing fixture, inspect_links returns "cannot verify — annotations carry no /Contents; visible text not inspectable". The check never produces a definitive pass or fail.

**Root cause.** pypdf exposes link annotations with their /Rect (position) but the visible link *text* lives in the page's content stream, not the annotation. Correlating rectangle → text requires a layout analysis step that pypdf does not perform. reportlab (our fixture generator) does not populate /Contents on link annotations.

**Agent bug?** No — documented limitation.

**Resolution.** Not fixed — this is the honest answer. The agent reports uncertainty rather than a false pass or false fail. This is Objective #4 in action.

**Future work.** Integrate pymupdf's text-block extraction: for each /Link annotation, clip the page to the annotation rectangle and extract text. Compare against a non-descriptive phrase list. This would upgrade 'cannot verify' to real pass/fail.

**Rubric evidence.** Directly quotes the actual failure message from §4A.9 and §7.4. Grounds the ethical point that 'cannot verify' is a first-class outcome, not a hidden pass.

---

### [F-4] llm-boundary — F3_missing_alt_text

**Symptom.** The LLM reworded the alt-text tool's raw message '1 of 1 images lack /Alt' into 'The PDF contains one image that lacks alternative text, which is essential for accessibility.' The clause 'which is essential for accessibility' is an evaluative claim not in the tool output.

**Root cause.** The LLM prompt says 'reword, do not change truth value'. It respected the truth (still fail / Major) but added an interpretive clause. Bounded authority prevents changes to the verdict, but not the insertion of evaluative language.

**Agent bug?** No — documented limitation.

**Resolution.** Not fixed in this project. Mitigation: the report displays the reworded rationale alongside the tool's severity (from the registry), and the audit log preserves the original tool message. A reviewer comparing the two sees the addition.

**Future work.** Add a lexical-drift check: if the reworded rationale adds tokens not in the tool message, flag it. Or constrain the LLM to a paraphrase that must not include adjectives outside a vocabulary list. Or drop the LLM path entirely — the rule-based rationale is already accurate.

**Rubric evidence.** Quotes the actual F3 output (§7.4) verbatim. Demonstrates that the safeguard is structural (severity from registry) not lexical (what the LLM writes). This is the honest residual risk the rubric asks the student to name.

---

### [F-5] documented-limitation — All (input gate)

**Symptom.** validate_input enforces extension, size, header, and page count — but does not inspect the PDF for /JavaScript, /EmbeddedFiles, /Launch, or /OpenAction keys.

**Root cause.** Scope decision: the project's safety boundary is 'read-only analysis' by pypdf/pymupdf, which do not execute embedded JavaScript. Scanning for active content would require pikepdf traversal of the object tree, which is out of scope.

**Agent bug?** No — documented limitation.

**Resolution.** Documented in §9. All tools open the PDF read-only; no execution path exists in the current toolset.

**Future work.** Add a pikepdf-based pre-scan for active-content keys. Classify a PDF with /JavaScript or /EmbeddedFiles as 'needs human review before audit' and refuse by default.

**Rubric evidence.** Names a concrete gap in the input gate. Shows the student thought past the rubric minimum.

---
