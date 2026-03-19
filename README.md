# NLP Project: e-SNLI -> EBR LLM-Likeness Shift

This project trains an explanation-style detector on older e-SNLI explanations, then applies it to newer EBR human feedback to measure how LLM-like the language appears.

## Pipeline Summary
1. Load and inspect e-SNLI train/test.
2. Build balanced e-SNLI subsets (`old-human`).
3. Load synthetic e-SNLI LLM explanations (`old-LLM`) from CSV.
4. Train a main TF-IDF + Logistic Regression detector.
5. Evaluate on held-out e-SNLI split (F1, AUROC).
6. Run style-only and length-matched controls.
7. Load EBR and use `feedback` as explanation text (`new-human`).
8. Score `old-human`, `old-LLM`, and `new-human`.
9. Export summaries, figures, qualitative audit, and reports.

## Setup

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Data Requirements

### e-SNLI
- Offline-safe behavior: the notebook first checks for existing subset CSVs in `data/raw`.
- Hugging Face `datasets.load_dataset("esnli")` is called only when subset CSVs are missing and a rebuild is required.
- Balanced subset files are saved/used at:
  - `data/raw/esnli_subset.csv` (train subset)
  - `data/raw/esnli_test_subset.csv` (test subset)

### Synthetic e-SNLI LLM Explanations
- These are **pre-generated** and loaded from CSV (no API generation in notebook).
- Provenance: generated in a prior step using an OpenAI LLM.
- Expected files:
  - `data/raw/esnli_llm_explanations_train.csv`
  - `data/raw/esnli_llm_explanations_test.csv`
  - optional fallback: `data/raw/esnli_llm_explanations.csv`
- Expected columns:
  - required: `explanation`
  - recommended: `premise`, `hypothesis`, `label`
- Alignment policy:
  - the notebook now enforces strict `premise+hypothesis` alignment by default
  - if files are misaligned, execution fails with an explicit error
  - regenerate LLM explanations from the exact current subset if needed
- Loading priority:
  - `data/processed/esnli_old_llm_train.csv` and `data/processed/esnli_old_llm_test.csv` are used first when present
  - otherwise the notebook loads from `data/raw/...` and writes aligned caches back to `data/processed/...`
  - strict alignment checks are enforced when loading from `data/raw`

### EBR Target Dataset
- Preferred local CSV: `data/raw/ebr_feedback.csv`
- Optional split files:
  - `data/raw/ebr_train.csv`
  - `data/raw/ebr_test.csv`
- If local EBR CSV is missing, notebook can load from Hugging Face (`wadhma/EBR`) if network access is available.
- In the EBR caching cell, `DOWNLOAD_EBR_IF_MISSING=False` by default for offline-safe runs.

## Notebook

Run end-to-end:

- `notebooks/full_project_pipeline.ipynb`

Notes:
1. By default, subset rebuilding is skipped if subset CSVs already exist (to preserve alignment with LLM CSV files).
2. Set `FORCE_REBUILD_SUBSETS = True` only if you also regenerate matching LLM explanation CSVs.
3. The notebook uses `feedback` as the EBR explanation field.
4. Held-out evaluation is strict: both e-SNLI test human and e-SNLI test LLM files are required.
5. Length-matched control now includes minimum per-class count guards after filtering; the run fails fast if filtered class counts are too small.

## Outputs

Artifacts are written to `outputs/`:

- `outputs/scored_explanations_main.csv`
- `outputs/scored_explanations_style_only.csv`
- `outputs/scored_explanations_length_matched.csv`
- `outputs/score_summary_main.csv`
- `outputs/score_summary_style_only.csv`
- `outputs/score_summary_length_matched.csv`
- `outputs/qualitative_audit_samples.csv`
- `outputs/models/explanation_style_models.joblib`
- `outputs/figures/llm_likeness_distributions_main.png`
- `outputs/figures/llm_likeness_distributions_style_only.png`
- `outputs/figures/llm_likeness_distributions_length_matched.png`
- `outputs/reports/final_report.md`
- `outputs/reports/qualitative_pattern_summary.md`
- `outputs/run_manifest.json`
