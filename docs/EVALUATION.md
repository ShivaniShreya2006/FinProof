# Evaluation Notes

## TAT-DQA Scoring

This file documents the evaluation methodology used in FinSight.

### Official Scorer
We use the official TAT-DQA evaluation script (vendored in insight/tatqa_metric.py).
- **Exact Match (EM):** 1 if the predicted answer exactly matches gold (after normalisation)
- **F1:** token-level overlap between prediction and gold

### Key Results
| Model | Dev EM | Dev F1 | Test EM | Test F1 |
|-------|--------|--------|---------|---------|
| Qwen2.5-7B baseline | ~45 | ~55 | - | - |
| TAT-LLM-7B + executor | 69.0 | ~76 | 69.0 | ~76 |
| Final pipeline (ensemble) | **>76** | **>83** | TBD | TBD |

### Evaluation Rules
- Tune hyperparameters on **dev** only
- Run **test** once at the very end (notebook 05, Part E)
- Report both EM and F1, plus the per-type breakdown

### Per-type Analysis
Types evaluated: span, multi-span, rithmetic, counting
- Arithmetic questions benefit most from the executor
- Span questions benefit from evidence grounding and verification

### Reporting
Compare models under the **same official scorer** — TAT-LLM's published 72.6 EM
uses a lenient script with ~1% tolerance that ignores units.
