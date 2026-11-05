
## Ethical and Responsible Use Considerations

### 1. The difference between "cannot verify" and "pass" (cases F-3)
The agent's most consequential ethical decision is that `inspect_links`
returns *cannot verify* rather than *pass* on documents where the tool cannot
confirm that a link's visible text is descriptive. On F1, F3, and F4, this
appears in the report as a first-class "cannot verify" entry, distinct from
any pass. A weaker design — treating "no obvious problem detected" as a
pass — would have produced a *false sense of compliance* on exactly the
documents the agent was designed to audit. The choice to surface uncertainty
explicitly is grounded in the observed behaviour: `inspect_links` returned
cannot verify on 3 of 4 audited fixtures, and the reports never conflated
this with a pass.

### 2. The bounded LLM does not equal a safe LLM (case F-4)
The §4C design bounds the LLM's authority: it may reword the tool's
rationale, but not change the severity or verdict. On F3, the LLM reworded
"1 of 1 images lack /Alt" into a sentence that added "which is essential for
accessibility" — an evaluative claim not present in the tool output. The
severity remained Major and the verdict remained fail, so the *guardrails
held*, but the LLM still added an unverified interpretation. The lesson,
grounded in the observed F3 output, aligns with broader findings on the limits 
of constrained prompting in agentic workflows: bounding what an LLM may *decide* does
not bound what it may *say* (Yao et al., 2023). Any production deployment should also bound
what the LLM may *write* — through paraphrase constraints, token-level
checks, or by dropping the LLM path entirely when the rule-based rationale
is already sufficient (as it is for the six deterministic tools in §4A).

### 3. Compliance theater and the advisory-only verdict (cases F-3, F-5)
The agent could be misused as a compliance certificate, a form of "compliance theater" 
where automated tools substitute for genuine accessibility and legal compliance (Liu & Wang, 2023). 
An organisation might scan its PDFs, see zero Major findings on most of them, and claim ADA
compliance without addressing the "cannot verify" checks or the fields the
gate does not inspect (active content, logical reading order, contrast in
scanned images). The report's mandatory `verdict` block — "Advisory findings
only. This is not a legal ADA or PDF/UA compliance certification" — is
designed to make this misuse *hard*, though not impossible. The ethical
responsibility for what happens after the report lands sits with the human
reviewer; the agent's job is to make the advisory nature of its findings
impossible to overlook, and the §5 report builder enforces this by placing
the verdict in every output format.

### 4. Accountability in the re-check gate (case F-2)
On F2, the agent ran *more* iterations than the design predicted because
two mandatory checks failed and each was re-verified once. This is not a
fault — it is the accountability mechanism working: the agent did not accept
a single observed failure as definitive, it re-verified, and only after the
re-check agreed did it emit a Critical finding. A design that trusted a
single tool result would run faster but audit less rigorously. The extra
iterations are the cost of verifiability.
