# NLP Project: e-SNLI -> GMEG-EXP LLM-Likeness Shift

This repository implements the full project pipeline:
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

The loader recursively scans CSV/JSON/JSONL and tries to detect explanation and human/source columns. If your column names differ, edit `src/data_utils.py`.

## Run End-to-End

```bash
python run_pipeline.py --gmeg-root data/raw/GMEG-EXP --generate-llm
```

If you already generated synthetic e-SNLI LLM explanations once, rerun without regeneration:

```bash
python run_pipeline.py --gmeg-root data/raw/GMEG-EXP
```

## Notebook

Use:

- `notebooks/full_project_pipeline.ipynb`
- `project_steps.ipynb` (step-name-only notebook)

## Outputs

Pipeline artifacts are written to `outputs/`:

- `outputs/scored_explanations.csv`
- `outputs/score_summary.csv`
- `outputs/qualitative_audit_samples.csv`
- `outputs/models/esnli_llm_detector.joblib`
- `outputs/figures/llm_likeness_distributions.png`
- `outputs/reports/final_report.md`
- `outputs/run_manifest.json`
