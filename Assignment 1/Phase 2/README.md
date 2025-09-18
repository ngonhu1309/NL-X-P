# Phase 2: Prompt Engineering Mastery

## Overview
In **Phase 1**, the Finance domain was selected with evaluation tasks including **Information Synthesis**, **Classification/Analysis**, and **Question-Answering**.  

In this phase, prompts are designed using three structured frameworks:  
- **CLEAR Method**  
- **Few-Shot Learning**  
- **Chain-of-Thought (CoT)**  

Each prompt is tailored to its task, with documentation of **design rationale, expected output, potential risks, and pilot testing observations**.

---

## Task 1: Information Synthesis (Summarizing Regulatory Filings)

### CLEAR Method Prompt
- **Design rationale**: Constrains tone, audience, and length so outputs are compliance-ready and investor-focused.  
- **Expected output**: ≤200 words; professional summary with Revenue, Net Income, EPS, Cash, top 3 risks, and MD&A highlights; use “not disclosed” when missing.  
- **Failure modes & mitigations**:
  - Omission of forward-looking items → add explicit MD&A extraction step.  
  - Numeric distortion → instruct model to copy digits verbatim and label source lines.  
- **Pilot**: Summaries well-structured but sometimes omitted forward-looking language → mitigation added.  

**Prompt:**
```text
You are a financial analyst. Summarize the following 10-K/10-Q excerpt for investors.

Requirements:
- Length: ≤200 words
- Audience: investors with general finance knowledge
- Tone: professional, compliance-ready, concise
- Required fields: Revenue, Net Income, EPS, Cash (if missing, write “not disclosed”), Top 3 risk factors, MD&A main points (including forward-looking statements)
- If quoting numbers, copy them exactly from the text and include the sentence location (e.g., "p.12")

Now read the excerpt and produce the summary.
```

---

### Few-Shot Prompt
- **Design rationale:** Two concrete examples set formatting expectations (compact bullet style).  
- **Expected output:** Bullet list with metrics + concise MD&A + risks.  
- **Failure modes & mitigations:** Overfitting to example wording → vary phrasing.  
- **Pilot:** More consistent formatting, but slightly rigid.

**Prompt:**

---

### Chain-of-Thought Prompt
- **Design rationale:** Stepwise extraction improves numeric accuracy and evidence.  
- **Expected output:** Short reasoning trace (hidden/concise) → ≤200-word summary.  
- **Failure modes & mitigations:** Verbose reasoning leaking into summary → require “final answer only.”  
- **Pilot:** Accuracy improved; summaries stayed within word limit after guardrails added.  

**Prompt:**

---

## Task 2: Classification/Analysis (Identifying Risk Factors)

### CLEAR Method Prompt
- **Design rationale:** Concise risk label + justification + quote ensures auditability.  
- **Expected output:** Risk Label – Justification + direct quote. Multiple categories allowed with confidence levels.  
- **Failure modes & mitigations:** Ambiguous cases → require explicit confidence levels.  
- **Pilot:** Strong on explicit risks; overlap better handled with confidence tagging.  

**Prompt:**

---

### Few-Shot Prompt
- **Design rationale:** Use 2–3 examples to align labels with risk phrasing.  
- **Expected output:** Consistent *Label + Quote justification*.  
- **Failure modes & mitigations:** Weak evidence linkage → require verbatim quotes.  
- **Pilot:** Precision improved, evidence clearer with quotes enforced.  

**Prompt:**

---

### Chain-of-Thought Prompt
- **Design rationale:** Exposes reasoning (identify risks → map categories → cite text).  
- **Expected output:** Compact reasoning trace (≤4 bullets) + final classification with confidence.  
- **Failure modes & mitigations:** Reasoning verbosity → capped at 4 steps.  
- **Pilot:** Higher interpretability and classification accuracy after reasoning cap.  

**Prompt:**

---

## Summary
These prompts demonstrate how structured frameworks influence LLM performance on finance tasks:  
- **CLEAR** → ensures professional tone and compliance.  
- **Few-Shot** → standardizes formatting via examples.  
- **CoT** → improves reasoning depth and numeric accuracy.  

In practice, finance professionals may prefer a **hybrid approach**:  
- CLEAR for external-facing compliance outputs,  
- Few-Shot for structured classification tasks,  
- CoT for high-stakes, accuracy-critical analysis.  
