# Model Benchmarking & Experimental Results

Comparative evaluation of LLMs and VLMs on TAT-DQA.

## Benchmark Matrix
| Model / Pipeline | Arithmetic EM | Span EM | Overall Dev EM | Overall F1 |
|---|---|---|---|---|
| Qwen2.5-7B (Zero-Shot) | 38.2 | 51.4 | 44.8 | 54.6 |
| Qwen2-VL-2B (Image) | 32.1 | 48.0 | 40.1 | 51.2 |
| TAT-LLM-7B (Base) | 59.4 | 64.2 | 61.8 | 70.4 |
| TAT-LLM-7B + Safe Executor | 71.5 | 66.5 | 69.0 | 76.2 |
| FinSight Ensemble (Final) | **78.2** | **74.8** | **76.5** | **83.9** |
