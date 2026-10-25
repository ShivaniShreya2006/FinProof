# FinSight — Frequently Asked Questions

## General

**Q: What is FinSight?**  
A: FinSight is a grounded question-answering system for financial documents. It answers numerical and span-based questions over PDF financial reports and tables, evaluated on the TAT-DQA benchmark.

**Q: Does it require any model training?**  
A: No. FinSight uses only pre-trained open models (TAT-LLM-7B-FFT, Qwen2.5-7B) with no fine-tuning. The RAG retrieval uses only training question templates — no gradient updates.

**Q: What dataset does it use?**  
A: [TAT-DQA](https://github.com/NExTplusplus/TAT-DQA) — a benchmark of ~16,000 QA pairs over real financial documents requiring arithmetic and discrete reasoning.

---

## Running the Pipeline

**Q: Do I need a GPU?**  
A: For full inference with TAT-LLM-7B, yes — a free T4 GPU on Google Colab is enough. The sanity check (scripts/sanity_check.py) runs on CPU only.

**Q: How do I run on Colab?**  
A: Use the direct links in the README. Select Runtime ? Change runtime type ? T4 GPU, then run the cells. Results save to Google Drive automatically.

**Q: How do I run the final pipeline?**  
A: Import notebook 05 into Kaggle (File ? Import notebook ? GitHub). Enable GPU T4 x2 and Internet. Run Parts A–D on dev, then set RUN_TEST = True for Part E.

---

## Evaluation

**Q: What metrics are reported?**  
A: Exact Match (EM) and F1, using the official TAT-DQA scorer. Both are reported per question type (span, multi-span, arithmetic, counting).

**Q: Why does TAT-LLM's published 72.6 EM differ from ours?**  
A: TAT-LLM's authors use a lenient script with ~1% numerical tolerance that also ignores units. Our results use the stricter **official** scorer for fair comparison.

**Q: What is the target score?**  
A: The best published result on TAT-DQA test is TAT-LLM-70B at 76.5 EM / 83.9 F1. FinSight's final pipeline targets this with only 7B-scale open models.

---

## Risk Report

**Q: What does the automatic risk report cover?**  
A: Six dimensions — liquidity, leverage, interest cover, profitability, growth, and cash backing. Each is rated Low / Medium / High risk. Large year-on-year changes (>20%) are flagged separately.

**Q: Are the risk values verified against the document?**  
A: Yes — all values used in risk ratios are cross-checked against their source rows in the original page text.

---

## Data and Licence

**Q: Can I use this for commercial purposes?**  
A: The code is MIT licensed. TAT-DQA data is CC BY 4.0. TAT-LLM weights are under the Llama 2 Community Licence (non-commercial for large-scale use).

**Q: Where is the dataset?**  
A: Already packed in data/ as .json.gz files (~19 MB total). No separate download needed.
