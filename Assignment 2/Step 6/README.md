# Automated Evaluation Report

To complement intrinsic metrics such as Exact Match and F1, we conducted a comprehensive automated evaluation of both **Naive RAG** and **Enhanced RAG** pipelines using the **ARES framework**. This framework assesses generated answers along multiple dimensions, including context quality, answer helpfulness, and factual alignment. We evaluated 918 queries against both systems, capturing aggregate averages and distribution statistics.

---
### Metrics Overview

**1. Answer Relevance (Helpfulness)**

This metric measures how well the generated answer aligns with the user’s query, regardless of lexical overlap. The Naive pipeline achieved **0.3393**, while the Enhanced system slightly improved to **0.3440** (+1.39%). This indicates that enhanced prompting and retrieval adjustments improved helpfulness, making responses marginally more relevant to user intent.

**2. Answer Faithfulness**

Faithfulness measures factual alignment of generated answers with retrieved context. Here, the Enhanced system improved from **0.3460 (Naive)** to **0.3495 (+0.99%)**. While the gain is modest, it suggests that enhancements reduced hallucination tendencies and reinforced factual grounding, even if surface scores such as EM and F1 did not fully capture this benefit.

**3. Context Relevance**

Context relevance captures whether retrieved passages were topically aligned with the question. The Naive pipeline slightly outperformed Enhanced **(0.6675 vs 0.6626, –0.73%)**. This suggests that reranking or context filtering steps introduced in the Enhanced pipeline sometimes discarded passages that, while noisy, still contained overlapping keywords or phrasing that the metric rewards.

**4. Context Precision and Recall**

Precision reflects the proportion of retrieved context that was actually useful, while recall measures the proportion of useful context successfully retrieved. Naive scored **0.2152 (precision)** and **0.2692 (recall)**, while Enhanced dropped to **0.2109 precision (–2.01%)** and **0.2336 recall (–13.2%)**. These declines suggest that the Enhanced system traded off retrieval breadth for stricter filtering, leading to fewer relevant passages being surfaced. The corresponding Context F1 also decreased from **0.1813 to 0.1662 (–8.33%)**.

---
### Comparative Analysis

Overall, the evaluation highlights a trade-off between retrieval coverage and answer robustness. The Naive pipeline surfaced broader context and scored higher on recall, but this came at the cost of slightly lower faithfulness and relevance. The Enhanced pipeline, by contrast, narrowed the retrieval scope and emphasized factual grounding. As a result, it achieved higher faithfulness and helpfulness, but at the expense of context coverage.

From a reliability perspective, the Enhanced system is less likely to hallucinate or drift from grounded knowledge. This makes it preferable in high-stakes domains where factual accuracy is critical, even if coverage is narrower. On the other hand, the Naive pipeline may be more suitable in open-domain settings where broad coverage is valuable and slight factual drift is less harmful.

---
### Key Takeaways

1. **Faithfulness and helpfulness** improved in the Enhanced system (+1%), showing better factual consistency and more relevant answers.

2. **Context recall and precision** decreased, highlighting challenges in balancing stricter filtering with maintaining coverage.

3. **Evaluation frameworks like ARES** provide insights beyond EM/F1, revealing subtle trade-offs between retrieval quality and generation reliability.

In conclusion, while the Enhanced pipeline did not outperform Naive across all dimensions, its gains in faithfulness and answer helpfulness suggest meaningful progress toward reducing hallucinations. Future work should explore hybrid retrieval strategies—e.g., reranking multiple candidates but selectively pruning noise—to combine the coverage of Naive retrieval with the faithfulness of Enhanced generation.
