# FinSight Pipeline Architecture

## Overview

FinSight is a grounded question-answering pipeline for financial documents (TAT-DQA benchmark).
It uses no training — only open models and free GPUs.

## Pipeline Stages

`
Input: PDF page + question
        |
        v
1. READ
   - Extract text layer from PDF
   - Format as numbered rows with ' | ' column breaks
   - Preserves table structure for financial data
        |
        v
2. RAG (Retrieval-Augmented Generation)
   - Retrieve 4 most similar solved TRAINING questions
   - Match by question template + unit type
   - BM25 row retrieval for evidence candidates
   - Provides worked examples for the LLM
        |
        v
3. REASON (LLM Inference)
   - Best open LLM (selected on dev set)
   - Outputs: evidence rows, answer type, program/span, unit
   - Structured output enforced via prompt format
        |
        v
4. COMPUTE (Safe Executor)
   - Python expression evaluator (syntax-tree whitelist)
   - No eval() — safe arithmetic only
   - Handles: +, -, *, /, %, parentheses
   - % reconciliation for percentage answers
        |
        v
5. VERIFY (Grounding + Ensemble)
   - Operands/spans must appear on the page
   - Where disagrees with TAT-LLM-7B: 4 samples + weighted vote
   - Confidence scoring
        |
        v
Output: answer + unit + program + highlighted evidence rows + confidence
        +
        AUTOMATIC RISK REPORT
`

## Component Map

| Stage | Module | Key Function |
|-------|--------|--------------|
| Read | insight/data.py | ender_page() |
| RAG | insight/retrieve.py | etrieve_examples(), etrieve_rows() |
| Reason | insight/tatllm.py | un_tatllm() |
| Compute | insight/program.py | safe_eval() |
| Verify | insight/solve.py | solve(), ensemble() |
| Risk | insight/risk.py | isk_report() |

## Design Decisions

### Why RAG over fine-tuning?
- No GPU required for retrieval
- Works with any LLM backend (Ollama, vLLM, llama.cpp)
- Generalises better across question types

### Why a safe executor over direct LLM arithmetic?
- LLMs hallucinate numerical answers
- Symbolic programs are verifiable and exact
- Error analysis shows executor fixes ~8% of wrong answers

### Why weighted voting?
- TAT-LLM-7B and the final pipeline disagree on ~20% of questions
- Weighted vote with confidence scores outperforms either alone
