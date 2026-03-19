# Explanation-Style Shift Report (e-SNLI -> EBR)

## Main Detector Performance (e-SNLI held-out test)
- F1: 1.0000
- AUROC: 1.0000

## Control Performance (e-SNLI)
- Style-only: F1=0.9858, AUROC=0.9938
- Length-matched: F1=0.9999, AUROC=1.0000, band=[7.00, 39.00]

## Main Distribution Summary (old-human, old-LLM, EBR-human)
- new_human: mean=0.1150, median=0.0679, pct>=thr=0.66%, n=12650
- old_human: mean=0.0232, median=0.0181, pct>=thr=0.00%, n=8000
- old_llm: mean=0.9892, median=0.9939, pct>=thr=99.92%, n=8000