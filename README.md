# Latvia IMF Financial Stability Analysis (2026)

**Student registration number:** 24164952  
**Country and reporting period:** Latvia, 2025–2026  
**Source:** IMF, *Republic of Latvia: 2026 Article IV Consultation—Press Release; and Staff Report*, Country Report No. 26/248 (September 2026)

## Overview

This Google Colab notebook implements a PDF-to-dashboard workflow for analysing Latvia's 2026 IMF Article IV report. It combines PDF text extraction, evidence-grounded summarisation, structured financial information extraction, semantic retrieval, automated evaluation, and financial visualisation. An optional extension compares single-query and multi-query Retrieval-Augmented Generation (RAG).

## Files

| File | Purpose |
| --- | --- |
| `24164952.ipynb` | Main executable notebook (14 code cells) |
| `Latvia.pdf` | Source IMF report (68 pages) |
| `README.md` | Setup instructions and notebook overview |

Generated results are saved by the notebook in `/content/latvia_task1/`.

## How to run in Google Colab

1. Open `24164952.ipynb` in **Google Colab**.
2. Upload `Latvia.pdf` into the Colab session so that it is available at **`/content/Latvia.pdf`**. The filename and path must match exactly.
3. Under **Secrets** in Colab, create a secret named **`CreatingAPI`**, enter your Gemini API key, and enable notebook access. Alternatively, the notebook checks `GEMINI_API_KEY` or prompts for a hidden key at runtime.
4. Run the cells **in order, from Cell 1 through Cell 14**. The notebook installs its required Python packages in the relevant cells.
5. Review the printed validation results, figures, and files in **`/content/latvia_task1/`**. Download any outputs you need before the Colab session ends.

**Security:** Do not include the API key in the notebook, README, screenshots, or repository. Colab Secrets are recommended.

## Notebook structure

| Cells | Task | Main operation |
| --- | --- | --- |
| 1 | Setup | Configure Gemini client, paths, and reusable file helpers |
| 2 | Task 1 | Extract and clean PDF text; create token-aware overlapping chunks |
| 3 | Task 1 | Identify and summarise 13 major report sections |
| 4 | Task 1 | Generate an executive summary and LLM-as-a-Judge evaluation |
| 5 | Task 1 | Validate and consolidate document-processing outputs |
| 6 | Task 2 | Count mentions across seven financial themes |
| 7 | Task 2 | Extract six strengths and six vulnerabilities |
| 8 | Task 2 | Extract seven schema-defined financial indicators |
| 9 | Task 2 | Extract 2025–2031 economic series and reshape data |
| 10 | Task 2 | Validate indicators against IMF evidence using a judge prompt |
| 11 | Task 2 | Consolidate and check Task 2 outputs |
| 12 | Task 3 | Build MiniLM embeddings and a FAISS index; test grounded RAG |
| 13 | Task 4 | Generate a four-panel financial dashboard at 300 DPI |
| 14 | Extra credit | Compare baseline and Multi-Query RAG using completeness and faithfulness scores |

### Main methods

- **PDF extraction:** PyMuPDF; cleaning removes recurring page artefacts.
- **Chunking:** `tiktoken` (`cl100k_base`) with a target of 400 tokens and 50-token overlap. These are approximate tokens for the Gemini model.
- **Generative model:** `gemini-3.5-flash-lite`, as configured in the notebook.
- **RAG:** `sentence-transformers/all-MiniLM-L6-v2` embeddings with FAISS `IndexFlatL2`.
- **Evaluation:** Separate prompts using the configured Gemini model as an LLM judge. These assessments are not independent ground-truth verification.
- **Visualisation:** pandas, NumPy, and Matplotlib; `pd.melt()` is used for long-format data.
- **Multi-Query RAG:** Query expansion in Python and Reciprocal Rank Fusion, retaining five retrieved passages per answer for comparison.

## Recorded notebook results

The uploaded notebook contains the following execution results:

| Measure | Recorded result |
| --- | --- |
| PDF pages | 68 |
| Extracted chunks | 153 |
| Summarised sections | 13 (each under 100 words) |
| Financial themes | 7; 397 matched mentions |
| Strengths / vulnerabilities | 6 / 6 |
| Required JSON indicator fields | 7 |
| Economic trend years | 2025–2031 (35 year-indicator observations) |
| RAG indexed chunks | 153 |
| Dashboard | Four panels, exported at 300 DPI |

**Key extracted indicators:** 2026 GDP growth forecast **1.8%**; 2026 headline inflation forecast **4.0%**; 2026 unemployment forecast **6.7%**; 2025 non-performing loan ratio **2.5%**; 2025 Tier 1 capital ratio **19.4%**. The central bank policy rate is recorded as `null` because the necessary explicit value was not established from the selected IMF source evidence. Forecasts and historical figures are labelled separately.

### Extra-credit benchmark (Cell 14)

| Mean judge score (1–5) | Single-Query RAG | Multi-Query RAG |
| --- | ---: | ---: |
| Completeness | 4.00 | 4.67 |
| Faithfulness | 5.00 | 5.00 |

The average completeness gain was **+0.67 points**, but performance differed by question. This is a three-question, model-judged comparison, not a statistically validated accuracy improvement.

## Generated outputs

The notebook writes these files under `/content/latvia_task1/`:

**Document processing**
- `cleaned_text.txt`, `chunks.json`, `section_summaries.json`
- `executive_summary.json`, `summary_evaluation.json`, `task1_results.json`

**Financial extraction**
- `thematic_counts.json`, `risk_insights.json`, `core_indicators.json`
- `indicator_audit.json`, `temporal_trends.json`, `temporal_trends_long.csv`
- `indicator_evaluation.json`, `task2_results.json`

**Retrieval and figures**
- `rag_index.faiss`, `rag_metadata.json`, `rag_demo.json`
- `dashboard_data.csv`, `latvia_academic_dashboard_clean.png`
- `multi_query_benchmark.json`, `rag_benchmark.csv`, `rag_benchmark.png`

## Reproducibility and limitations

- The notebook uses cached JSON outputs and index metadata to avoid unnecessary repeated processing; rerun cells sequentially when inputs or parameters change.
- API availability and model access are required for generative steps; the embedding model may need an initial download.
- PDF layout and section-boundary parsing may affect extracted context.
- Keyword mention counts represent **document emphasis**, not financial risk severity.
- LLM-as-a-Judge ratings are subjective. Check key figures, their years, and definitions against the IMF tables before reporting them as verified.
- All discussion and submitted interpretations should follow the university's academic-integrity requirements.

## References

- International Monetary Fund (2026). *Republic of Latvia: 2026 Article IV Consultation—Press Release; and Staff Report*. IMF Country Report No. 26/248. [IMF publications](https://www.imf.org/en/Publications/CR).
- [Google Gemini API documentation](https://ai.google.dev/gemini-api/docs)
- [PyMuPDF documentation](https://pymupdf.readthedocs.io/)
- [Sentence Transformers documentation](https://sbert.net/)
- [FAISS documentation](https://github.com/facebookresearch/faiss)
- [pandas documentation](https://pandas.pydata.org/docs/)

## Submission

Submit the executable Colab notebook link (or permitted code repository) alongside the required five-page PDF report. Ensure the notebook is accessible to the marker and runs in order. Follow the assignment instruction not to publish the written report to a public GitHub repository before completing the course.
