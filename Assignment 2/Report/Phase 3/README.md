# Enhanced RAG System

The Enhanced Retrieval-Augmented Generation (RAG) system in this notebook represents a significant progression from the naive baseline. Where the naive pipeline deliberately constrained retrieval and generation to the simplest possible setup, the enhanced system explores richer embedding choices, multi-passage retrieval, and more robust evaluation to deliver answers that are both more accurate and contextually reliable. The enhancements are not mere incremental tweaks; they systematically address weaknesses identified in the naive approach, thereby improving the overall fidelity of the RAG pipeline.

### Embedding Configurations

The first enhancement centers on the use of multiple embedding models. In addition to the lighter `all-MiniLM-L6-v2 model` 384 dimensions, the enhanced pipeline incorporates `all-mpnet-base-v2`, a more powerful transformer model with 768 dimensions. The motivation is clear, which is larger and more expressive embeddings capture subtler semantic relationships between queries and passages. By experimenting with both configurations, the notebook enables empirical comparison between computational efficiency (`MiniLM`) and semantic richness (`MPNet`). This choice also reflects a key RAG design principle—embedding models strongly determine retrieval quality, and diverse embeddings allow the system to hedge against model-specific biases.

### Passage Retrieval Beyond Top-1

A second major enhancement involves broadening the retrieval scope. Whereas the naive implementation restricted retrieval to the single most similar passage, the enhanced system allows retrieval of multiple top-k passages, with k values of 1, 3, and 5. This directly mitigates the recall problem observed in the naive pipeline. If the top-1 passage is imperfect, the model now has additional candidate passages that may collectively capture the answer space. Importantly, this also aligns with real-world RAG applications, where redundancy and coverage are critical. By expanding context length constraints and retrieving multiple candidates, the system equips the language model with a broader base of evidence from which to generate answers.

### Context Preprocessing and Length Control

Another subtle yet important improvement lies in preprocessing passages. The notebook introduces filtering based on passage length and context window size, ensuring that overly short or excessively long passages do not degrade retrieval quality. It also imposes maximum context length thresholds, which prevent irrelevant text from diluting the useful context passed to the model. This design choice balances inclusivity with efficiency: by trimming noisy passages while maintaining semantic richness, the enhanced pipeline provides more relevant and digestible contexts to the generator model.

### Answer Generation and Strategy Diversity

The enhanced system continues to rely on a robust sequence-to-sequence model (`flan-t5-base`) for answer generation, but with the crucial difference that it now receives more informative and carefully curated contexts. The concatenated multi-passage contexts better equip the LLM to synthesize evidence, resolve ambiguities, and produce more faithful answers. In practical terms, the model is no longer forced to rely on a single passage’s limitations but can leverage corroboration across multiple retrieved texts.

### RAGAS Evaluation Framework

Evaluation is central to validating the effectiveness of enhancements. Just as in the naive system, the enhanced pipeline leverages RAGAS metrics—faithfulness, context precision, context recall, and answer relevancy. However, because enhanced retrieval produces richer contexts, the evaluation better highlights the system’s improved ability to (1) ground answers in retrieved passages, (2) select passages that truly matter, and (3) balance precision with recall across multiple retrieved documents. The notebook saves per-query results and averages, storing them in both CSV and JSON formats for reproducibility and further analysis. This structured evaluation framework makes improvements transparent and quantifiable.

### Comparison to the Naive Baseline

One of the most valuable aspects of the notebook is its systematic comparison between naive and enhanced pipelines. Results are aggregated into summary tables that report F1 scores, exact match rates, and RAGAS metrics across embedding and retrieval settings. These tables not only identify the best-performing configurations but also contextualize the degree of improvement over the naive baseline. For example, best-enhanced strategies are explicitly compared to best-naive strategies, with percentage gains in F1 and other metrics reported. This comparative perspective underscores the pedagogical point of the exercise: enhancements are not merely theoretical but demonstrably superior.

### Advantages of the Enhanced RAG System

The enhanced pipeline delivers clear benefits. Faithfulness improves because multiple passages increase the likelihood that the retrieved evidence supports the generated answer. Context recall improves since the retrieval scope broadens beyond top-1, capturing more comprehensive information. Answer relevancy rises because the generator is less constrained by poor single-passage inputs. Even context precision benefits in many cases, since length and filtering controls weed out noise before generation. In sum, the enhancements collectively elevate the system from a fragile baseline to a more robust and trustworthy RAG solution.

### Limitations and Future Directions

Despite its advances, the enhanced system remains bounded by design trade-offs. Longer contexts, while more informative, risk exceeding token limits and overwhelming the generator. Multi-passage concatenation may also introduce irrelevant details, creating opportunities for hallucination. Embedding models, while improved, still have limitations in capturing domain-specific semantics. The current system does not yet incorporate reranking, cross-encoder validation, or metadata filtering, which could further improve retrieval precision. Additionally, the absence of true human-annotated ground truth answers constrains the reliability of evaluation metrics, as the model’s own outputs are reused as proxy references.

### Conclusion

The Enhanced RAG system represents a marked improvement over the naive pipeline. By broadening retrieval, diversifying embedding models, implementing context preprocessing, and systematically evaluating results, the notebook demonstrates how thoughtful enhancements translate into measurable performance gains. While not the final word in RAG system design, it successfully establishes a stronger, more resilient framework upon which further refinements can be layered. Ultimately, the enhanced pipeline validates the importance of iterative improvement: from naive simplicity to informed complexity, each step brings the system closer to delivering reliable, contextually grounded answers.
