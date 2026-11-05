# AccessAudit: Semi-Autonomous PDF Accessibility Compliance Auditing Agent

## 📌 Overview
PDFs published by public agencies, universities, and businesses are frequently required to be accessible under the Americans with Disabilities Act (ADA) and related standards such as PDF/UA. However, manual accessibility auditing is slow, inconsistent, and often deferred. 

**AccessAudit** is a semi-autonomous agentic AI system designed to bridge the gap between raw accessibility checkers and human decision-making. It ingests a user-supplied PDF, runs a conditionally ordered sequence of accessibility checks, interprets the results, and produces a structured, advisory remediation report. 

The system is built on a bounded **Plan-and-Execute** workflow. It utilizes deterministic tools (backed by `pypdf`, `pymupdf`, and `reportlab`) to inspect the document's internal structure, and uses an LLM (`gpt-4o-mini`) strictly as a bounded interpreter to reword technical findings into plain language without altering the underlying verdicts.

**Core Design Principle:** AccessAudit autonomously inspects and advises, but a human reviewer retains the authority to decide what to fix and how.

---

## 🚀 Quick Start

### 1. Prerequisites
- Python 3.9 or higher.
- An OpenAI API key (for the LLM interpreter).

### 2. Environment Setup
It is highly recommended to use a virtual environment (Conda or venv).

```bash
# Create and activate a virtual environment (example using conda)
conda create -n access-audit python=3.10
conda activate access-audit

# Install dependencies
pip install -r requirements.txt
```

### 3. API Key Configuration
This project requires an OpenAI API key to power the LLM interpreter. **Never hardcode your API key.**
Create a `.env` file in the root directory of the project and add your key:

```env
OPENAI_API_KEY="sk-your-api-key-here"
```
*(Note: If no API key is found, the system will automatically fall back to a deterministic, rule-based rationale generator. The audit will still run successfully, but the findings will not be reworded by the LLM).*

### 4. Running the Notebook
Launch Jupyter Notebook or JupyterLab:

```bash
jupyter notebook agentic_system.ipynb
```

Run all cells from top to bottom. The notebook will automatically generate five test PDF fixtures in a `FIXTURES/` directory, execute the agent on all five, and save the audit reports to a `report_samples/` directory.

---

## 🏗️ Architecture & Features

![Architecture Diagram](Architecture%20Diagram.png)

### The Agentic Loop
1. **Input Gate:** Validates the file extension (`.pdf`), size (max 25MB), page count (max 200), and magic bytes (`%PDF-`) before any parsing occurs.
2. **Planner:** Selects the next tool based on prior observations. If the PDF is untagged, it skips dependent checks (like alt-text).
3. **Tool Execution:** Runs one of six registered deterministic tools.
4. **Interpreter:** Converts raw tool output into a structured `Finding` with a hardcoded severity (Critical, Major, Minor, Info).
5. **Re-check Gate:** If a tool fails, it triggers exactly one re-execution to confirm stability before finalizing the failure.
6. **Report Builder:** Generates advisory-only reports in JSON, Markdown, and DataFrame formats.

### The Six Tools
- `check_tagged_pdf`: Detects if the PDF has a structural tag tree.
- `get_metadata`: Extracts the title and `/Lang` attribute.
- `check_text_extractability`: Heuristic to detect image-only/scanned PDFs.
- `inventory_alt_text`: Counts images and checks for missing `/Alt` text.
- `check_heading_order`: Heuristic check of heading sizes.
- `inspect_links`: Identifies link annotations.

---

## 🛡️ Safety & Safeguards
- **Read-Only:** The agent never modifies the input PDF.
- **Hard Caps:** The loop is strictly bounded by `MAX_CHECKS` (6) and `MAX_ITERATIONS` (12) to prevent runaway execution.
- **Explicit Uncertainty:** If a tool cannot confirm a check (e.g., due to library limitations), it returns "cannot verify" rather than assuming a "pass."
- **Advisory Verdict:** Every report includes a mandatory disclaimer: *"Advisory findings only. This is not a legal ADA or PDF/UA compliance certification."*
- **Bounded LLM:** The LLM cannot change severities or pass/fail states; it only rewords rationales.

---

## 📂 Repository Structure
```text
├── agentic_system.ipynb      # Main agent code, execution, and evaluation notebook
├── requirements.txt          # Python dependencies
├── README.md                 # This file
├── module_summary.pdf        # The written project report (design, ethics, evaluation)
├── .env.example              # Template for environment variables (do not commit .env)
├── FIXTURES/                 # Auto-generated test PDFs (F1-F5)
└── report_samples/           # Auto-generated audit reports for each fixture
```

---

## 📝 Limitations & Future Work
- **Link Inspection:** The `inspect_links` tool currently returns "cannot verify" because `pypdf` cannot correlate annotation coordinates to visible text streams. Future work will integrate `pymupdf` layout analysis.
- **Tag-Tree Traversal:** The heading check is currently a font-size heuristic. Future work will use `pikepdf` to traverse the actual `/StructTreeRoot`.
- **Active Content:** The input gate does not scan for embedded JavaScript, though all tools parse documents in read-only mode. Future work will add a `pikepdf` pre-scan for `/JavaScript` and `/OpenAction` keys.
```
