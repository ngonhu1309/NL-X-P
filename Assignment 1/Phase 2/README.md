# Phase 2: Prompt Engineering Mastery

## Overview

In Phase 1, I defined **Finance** as the target domain, with two evaluation tasks: **Information Synthesis**, **Classification/Analysis**, and **Question-Answering**. In this phase, I design prompts using three structured frameworks: **CLEAR Method**, **Few-Shot Learning**, and **Chain-of-Thought (CoT)**. Each prompt is tailored for its task, with documentation of design rationale, expected output, potential risks, and pilot testing observations.

## Task 1: Information Synthesis (Summarizing Regulatory Filings)

### CLEAR Method Prompt

**Design Rationale:** Constrains tone, audience, and length so outputs are compliance-ready and investor-focused.

**Expected Output:** ≤200 words, professional, bullet or short-paragraph summary containing Revenue, Net Income, EPS, Cash, top 3 risks, and MD&A highlights; use "not disclosed" when missing.

**Failure Modes & Mitigations:**
- **Omission of forward-looking items** → requires an explicit MD&A extraction step in the prompt
- **Numeric distortion** → instruct model to copy digits verbatim and label source lines

**Pilot (single run / prior note):** Summaries are well-structured; occasionally omitted forward-looking language — **mitigation:** add "explicitly extract forward-looking statements" line.

#### Prompt:
```
You are a financial analyst. Summarize the following 10-K/10-Q excerpt for investors.

Requirements:
- Length: ≤200 words
- Audience: investors with general finance knowledge
- Tone: professional, compliance-ready, concise
- Required fields: Revenue, Net Income, EPS, Cash (if missing, write "not disclosed"), Top 3 risk factors, MD&A main points (including forward-looking statements)
- If quoting numbers, copy them exactly from the text and include the sentence location (e.g., "p.12")

Now read the excerpt and produce the summary.
```

### Few-Shot Prompt

**Design Rationale:** Two concrete examples set formatting expectations (compact bullet style).

**Expected Output:** Bullet list with metrics and 1–2 line MD&A + risks.

**Failure Modes & Mitigations:** Overfitting to examples' phrasing → vary examples' structure and domain vocabulary.

**Pilot:** More consistent formatting; occasionally too rigid—allow minor paraphrase.

#### Prompt:
```
You are a financial analyst. Summarize 10-K/10-Q excerpts for investors in a professional, compliance-ready tone.

Example 1:
Excerpt: "Revenue for FY2024 was $45B, up 8% year-over-year. Net income totaled $5B, EPS $3.2. Cash and equivalents $10B. Risks include supply chain disruptions, regulatory changes, and cyber threats. MD&A highlights focus on expanding digital services and reducing operational costs."

Summary: Revenue $45B, Net Income $5B, EPS $3.2, Cash $10B. Top 3 risks: supply chain disruptions, regulatory changes, and cyber threats. MD&A: focus on digital expansion and cost reduction. (≤200 words)

Example 2:
Excerpt: "Revenue $20B, Net income $2B, EPS $1.1. Cash $3B. Risks: geopolitical tensions, interest rate volatility, competition. MD&A emphasizes R&D investment and market diversification."

Summary: Revenue $20B, Net Income $2B, EPS $1.1, Cash $3B. Top 3 risks: geopolitical tensions, interest rate volatility, and competition. MD&A: focus on R&D and market diversification. (≤200 words)

Now summarize the following excerpt:
[INSERT EXCERPT HERE]
```

### Chain-of-Thought Prompt

**Design Rationale:** Forces stepwise extraction to improve numeric accuracy and evidence linking.

**Expected Output:** Short reasoning trace (hidden or concise) followed by ≤200-word summary.

**Failure Modes & Mitigations:** Verbose chain leaking into final summary → instruct "final answer only" after reasoning step.

**Pilot:** Accuracy improved; needed guardrails to keep the final summary under the word limit.

#### Prompt:
```
You are a financial analyst. Summarize 10-K/10-Q excerpts for investors in ≤200 words. Focus on key financial metrics (Revenue, Net Income, EPS, Cash), top 3 risks, and MD&A highlights. Use a professional, compliance-ready tone.

Step-by-step reasoning before summarizing:
1. Identify Revenue, Net Income, EPS, and Cash. If missing, mark as "not disclosed."
2. List the top 3 risk factors mentioned.
3. Extract main points from the Management Discussion & Analysis (MD&A) section.
4. Organize information into a concise, investor-friendly summary.

[INSERT EXCERPT HERE]

Please provide your step-by-step analysis followed by the final summary.
```

## Task 2: Classification/Analysis (Identifying Risk Factors)

### CLEAR Method Prompt

**Design Rationale:** Forces concise label + justification + direct quote to support auditability.

**Expected Output:** For each risk: Label - One-sentence justification with direct quote. If multiple categories apply, provide Primary/Secondary with confidence (High/Med/Low).

**Failure Modes & Mitigations:** Borderline cases misclassified → require model to state ambiguous flags and confidence.

**Pilot:** Accurate on explicit phrasing; struggled with overlapping or implied risks - added confidence fields helped.

#### Prompt:
```
You are a risk analyst. Read the following text and classify each risk mentioned into one of four categories: Operational, Regulatory, Market, or Strategic.

Requirements:
- Output format: Risk Label - One-sentence justification with a direct quote from the text
- If a risk fits more than one category, assign a Primary and Secondary label with confidence (High/Medium/Low)
- Keep responses concise (≤2 sentences per risk)

[INSERT TEXT HERE]
```

### Few-Shot Prompt

**Design Rationale:** 2-3 examples teach correspondence between wording and label.

**Expected Output:** Consistent label + short quote justification.

**Failure Modes & Mitigations:** Pattern-match over-reliance → diversify examples across modalities (footnotes, MD&A, risk factors).

**Pilot:** Higher precision; evidence extraction sometimes requires a verbatim quote to fix.

#### Prompt:
```
You are a risk analyst. Classify risks into categories using the examples below:

Example 1:
Input: "New EU tax rules may affect company margins."
Output: Regulatory Risk - Justification: "EU tax rules" directly affect compliance and profitability

Example 2:
Input: "Revenue fell due to supply chain disruptions in Asia."
Output: Operational Risk - Justification: "supply chain disruptions" hinder core operations

Example 3:
Input: "Market volatility reduced investment income this quarter."
Output: Market Risk - Justification: "market volatility" impacts financial performance

Task: Classify each risk in the following text into [Operational, Regulatory, Market, Strategic]. Provide one risk label and one-sentence justification with a direct quote.

[INSERT TEXT HERE]
```

### Chain-of-Thought Prompt

**Design Rationale:** Make the model show reasoning steps (identify distinct risks → map to categories → cite text).

**Expected Output:** Compact reasoning trace + final labels with quotes and confidence.

**Failure Modes & Mitigations:** Verbose reasoning → require max 4 bullet steps, then final output.

**Pilot:** Improved interpretability and consistency; increased length mitigated by step cap.

#### Prompt:
```
You are a risk analyst. Classify each risk in the following text into one of four categories: Operational, Regulatory, Market, or Strategic. Use this format: Risk Label - One-sentence justification with a direct quote. If multiple categories apply, provide Primary and Secondary labels with confidence (High/Medium/Low). Keep each risk ≤ 2 sentences.

Reasoning steps:
1. Identify each distinct risk mentioned in the text
2. Determine which of the four categories best fits each risk
3. If a risk fits multiple categories, assign Primary and Secondary labels with confidence levels
4. Quote the text directly to justify each classification
5. Present a concise output for each risk

[INSERT TEXT HERE]

Please provide your step-by-step analysis followed by the final risk classifications.
```

## Summary

These prompts demonstrate how different frameworks shape LLM behavior across finance tasks. **CLEAR** ensures professional tone and compliance, **Few-Shot** improves formatting consistency, and **CoT** enhances reasoning depth. In practice, finance professionals would likely prefer a hybrid approach: CLEAR for external-facing compliance tasks, Few-Shot for structured classification, and CoT for high-stakes analysis where interpretability is crucial.

## Key Design Principles

- **Consistency:** All prompts maintain professional, compliance-ready tone
- **Specificity:** Clear word limits and required fields prevent ambiguous outputs
- **Risk Mitigation:** Each approach addresses specific failure modes through targeted instructions
- **Pilot Testing:** Iterative refinement based on observed performance gaps
