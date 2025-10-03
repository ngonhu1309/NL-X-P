# Evaluation Phase I

To establish a baseline for our Retrieval-Augmented Generation (RAG) system, we evaluated three prompting strategies: **Instructional Prompting**, **Chain-of-Thought (CoT) Prompting**, and **Persona Prompting**.

Evaluation relied on two complementary metric sets:

1. **SQuAD metrics (Exact Match, F1):** measuring lexical overlap with gold answers.

2. **RAGAS metrics (faithfulness, answer relevancy, context recall, context precision):** assessing semantic alignment and grounding in retrieved evidence.

This dual framework captures both extractive accuracy and semantic quality.

----
### Instructional Prompting

- **Results:** EM: **34.64%**, F1: **42.13%**
- **Interpretation:** Strongest on extractive overlap, but weak semantic grounding. Answers often matched gold spans but lacked faithfulness to retrieved evidence, suggesting reliance on memorized or generic text.

---
### Chain-of-Thought (CoT) Prompting

- **Results:** EM: **0.00%**, F1: **10.22%**
- **Interpretation:** Poor lexical alignment, but semantically stronger. Multi-step reasoning led to hallucinations or verbose answers misaligned with concise gold annotations, yet grounded responses improved on faithfulness and relevance.

---
### Persona Prompting

- **Results:** EM: **26.91%**, F1: **34.09%**
- **Interpretation:** Balanced approach. Comparable to Instruction in extractive accuracy but far stronger in semantic grounding. Persona framing encouraged faithful, contextually relevant responses without sacrificing accuracy.

---
### Summary Table
| Strategy     | Exact Match (%) | F1 Score (%) | 
|--------------|-----------------|--------------|
| Instruction  | 34.64           | 42.13        | 
| CoT          | 0.00            | 10.22        | 
| Persona      | 26.91           | 34.09        |

---
### Conclusion
Persona Prompting emerged as the most balanced strategy, combining competitive accuracy with strong semantic grounding. Instructional prompts maximize surface-level precision, while CoT improves semantic grounding but at the expense of extractive accuracy. Persona strikes a middle ground, making it more reliable for downstream use cases requiring both factual correctness and contextual faithfulness.
