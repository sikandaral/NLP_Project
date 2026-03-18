# NLP Project: e-SNLI -> GMEG-EXP LLM-Likeness Shift

This repository implements the full project pipeline in Jupyter notebooks:
1. Build old-human set from e-SNLI
2. Generate synthetic old-LLM explanations on matched e-SNLI inputs
3. Train TF-IDF + Logistic Regression detector (human vs LLM style)
4. Run length-matched and style-only controls
5. Load GMEG-EXP human explanations
6. Score old-human, old-LLM, and new-human explanations
7. Compare distributions, export plots, and run qualitative audit samples

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Set API key for synthetic explanation generation:

```bash
export OPENAI_API_KEY="your_key_here"
```

## Data

- e-SNLI is loaded automatically using `datasets.load_dataset("esnli")`.
- Put GMEG-EXP under:

```bash
data/raw/GMEG-EXP
```

The notebook loader recursively scans CSV/JSON/JSONL and tries to detect explanation and human/source columns. If your column names differ, edit the corresponding loading cell in the notebook.

## Notebook

Run this notebook end-to-end:

- `notebooks/full_project_pipeline.ipynb`

Optional reference notebook:

- `project_steps.ipynb` (step-name-only)

Inside `notebooks/full_project_pipeline.ipynb`:
1. Set `GMEG_ROOT = Path("data/raw/GMEG-EXP")`
2. Set `GENERATE_LLM = True` for first run
3. Run all cells from top to bottom
4. Set `GENERATE_LLM = False` on later runs if cached synthetic e-SNLI outputs already exist

## Outputs

Pipeline artifacts are written to `outputs/`:

- `outputs/scored_explanations.csv`
- `outputs/score_summary.csv`
- `outputs/qualitative_audit_samples.csv`
- `outputs/models/esnli_llm_detector.joblib`
- `outputs/figures/llm_likeness_distributions.png`
- `outputs/reports/final_report.md`
- `outputs/run_manifest.json`
