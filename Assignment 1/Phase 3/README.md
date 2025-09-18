# Phase 3: Systematic Evaluation

This document describes the methodology and rubric used to evaluate prompt performance for finance tasks. The goal of Phase 3 is a rigorous, reproducible assessment of how different prompt frameworks (CLEAR, Few-Shot, Chain-of-Thought) interact with several LLMs on finance-specific tasks: **Information Synthesis** and **Risk Classification/Analysis**.

---

## Systematic Testing

Each task was executed using three prompt frameworks across three models:

**Prompt frameworks**
- CLEAR Method  
- Few-Shot Learning  
- Chain-of-Thought (CoT)

**Models**
- GPT-5.0  
- Gemini 2.5 Flash  
- Claude Sonnet 4.0

This results in **9 model–prompt combinations per task** (3 prompts × 3 models), allowing coverage of both advanced and baseline models and enabling cross-framework comparisons.

---

## Rubric-Based Scoring

Outputs were scored on a 4-point scale across four evaluation metrics. Reviewers used the rubric below to ensure consistent, evidence-based assessment.

| Metric | 1 (Poor) | 2 (Fair) | 3 (Good) | 4 (Excellent) |
|---|---:|---:|---:|---:|
| **Accuracy** | Major factual errors. | Some errors; key facts missing. | Mostly correct; minor errors. | Completely correct; factually precise. |
| **Relevance** | Off-topic; doesn’t address the task. | Partially relevant. | Addresses most of the task. | Directly and fully addresses the task. |
| **Completeness** | Major gaps; missing required components. | Covers some but not all components. | Covers most required components. | Covers all specified components fully. |
| **Domain Appropriateness** | Uses informal or inappropriate language; shows no finance awareness. | Some domain terms present but inconsistent or inaccurate. | Mostly correct terminology and tone; minor slips. | Consistently professional, precise, and context-aware; fully aligned with finance conventions. |

**Notes on application**
- **Accuracy** checks always referenced the authoritative source (the filing excerpt) for numeric and factual verification.  
- **Relevance** penalized extraneous content and rewarded tight adherence to the task prompt.  
- **Completeness** required presence of all mandated fields (e.g., metrics + MD&A + top risks for Task 1).  
- **Domain Appropriateness** weighed professional tone, correct financial terminology, and formatting appropriate for investor or compliance audiences.

---

## Blind Evaluation Process

To reduce evaluator bias:
1. Outputs were randomized and stripped of model/prompt labels.  
2. Independent reviewers scored each output against the rubric.  
3. Scores were aggregated, then re-associated to the originating model–prompt pair for comparative analysis.  

Inter-annotator agreement was tracked; disagreements were resolved via a third adjudicator when necessary.

---

## Failure Case Documentation

For every model and task, at least one significant failure case was identified and documented. Common failure types included:

- **Omission of forward-looking statements (Task 1):** MD&A guidance, strategy or projections were frequently missed unless prompts explicitly requested forward-looking extraction.  
- **Inconsistent risk labeling (Task 2):** Similar or overlapping risk text was categorized differently across runs or prompts.  
- **Numeric distortion:** Rounding, paraphrasing, or miscopying of numeric values introduced material errors.  
- **Verbosity / constraint violations:** Chain-of-Thought prompts occasionally produced outputs that exceeded the ≤200-word target or leaked internal reasoning.  
- **Weak evidence anchoring:** Outputs sometimes lacked direct quotes or precise sentence/page citations, reducing auditability.

**Root-cause analysis approach**
- Categorize each failure as **Prompt Design** (ambiguous instructions, missing constraints) or **Model Limitation** (context truncation, hallucination, weaker domain adaptation).  
- Document remediation steps: prompt hardening, explicit evidence-anchoring, numeric post-checks, or human review for high-impact outputs.

---

## Comparative Analysis

Post-scoring comparisons focused on identifying patterns and trade-offs:

- **Accuracy vs. Interpretability:** Which frameworks (CLEAR, Few-Shot, CoT) favored raw metric fidelity versus transparent reasoning?  
- **Model tier performance:** Do advanced models (GPT-5.0, Claude Sonnet 4.0) consistently outperform the standard model (Gemini 2.5 Flash) across all metrics?  
- **Prompt trade-offs:** CLEAR often enforces professional tone and numeric fidelity but can miss implied/forward-looking nuance; Few-Shot stabilizes formatting; CoT improves reasoning but can increase verbosity.  
- **Reproducibility:** Which model + prompt pairs produce the most stable outputs across repeated runs and across different filing styles (tables, footnotes, long narratives)?

Key outputs from the comparative analysis inform recommendations for prompt hardening, automated checks, and human-in-the-loop safeguards.

---

## Next Steps

1. Implement **automated numeric validation** and label-consistency checks in the evaluation pipeline.  
2. Expand the test set to include diverse filing structures (footnotes, tables, multi-column PDFs).  
3. Iterate prompts with adversarial examples to reduce hallucination and omission rates.  
4. Enforce evidence anchoring by requiring verbatim quotes and sentence/page citations for each extracted metric or risk.  
5. Operationalize a **failure log schema** to track recurrence, severity, remediation, and reviewer notes.

---

## Appendix (optional)
- Scoring spreadsheets, example outputs, and failure logs can be appended here for reproducibility and audit trails.
