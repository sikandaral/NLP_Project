# Explanation-Style Shift Report (e-SNLI -> EBR)

## Main Detector Performance (e-SNLI held-out test when available)
- F1: 1.0000
- AUROC: 1.0000

## Control Performance (e-SNLI)
- Style-only: F1=0.9853, AUROC=0.9932
- Length-matched: F1=1.0000, AUROC=1.0000, band=[7.00, 39.00]

## Main Distribution Summary (old-human, old-LLM, EBR-human)
- new_human: mean=0.1074, median=0.0678, pct>=thr=0.33%, n=12650
- old_human: mean=0.0249, median=0.0198, pct>=thr=0.00%, n=8000
- old_llm: mean=0.9865, median=0.9921, pct>=thr=99.91%, n=8000