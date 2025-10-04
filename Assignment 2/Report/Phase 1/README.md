# Architecture Document for RAG System

### Introduction
This document describes the architecture of a Retrieval-Augmented Generation (RAG) system implemented as part of the assignment. The system was developed in Python within a Jupyter environment, combining large language model (LLM) generation with embedding-based retrieval using Milvus. The purpose of this architecture is to enable accurate, context-grounded question answering over a corpus of passages. Two variants were implemented: a Naive baseline and an Enhanced system with optimizations in retrieval and evaluation.

### High-Level Architecture
The RAG pipeline follows a modular design, consisting of five major stages:
1. **Data Ingestion and Preprocessing:** preparing passage data for retrieval.
2. **Embedding Generation:** converting text into dense vector representations.
3. **Vector Indexing and Storage:** storing embeddings in Milvus for efficient similarity search.
4. **Retrieval and Generation:** fetching relevant passages and generating answers with an LLM.
5. **Evaluation and Metrics:** benchmarking results using both QA-style metrics and advanced evaluation frameworks such as ARES and RAGAS.

Each stage is designed to be replaceable and extensible, allowing experimentation with alternative embedding models, retrieval configurations, or evaluation strategies.

### System Components
**1. Data Layer**

The data layer begins with loading and cleaning passages from a provided dataset. Data is stored in Parquet format and preprocessed by removing null values and calculating basic length statistics (character and word counts). This ensures a consistent, high-quality input corpus for embedding.

**2. Embedding Layer**

The embedding component uses the SentenceTransformers library to encode passages into dense vectors. Multiple embedding models were tested, primarily:

- `all-MiniLM-L6-v2` (384 dimensions, lightweight and efficient)

- `all-mpnet-base-v2` (768 dimensions, higher capacity)

A configuration class (`Config`) manages embedding metadata, including model names, dimensions, and collection naming conventions. This enables flexible experimentation with different embedding backbones.

**3. Vector Storage and Indexing**

The system employs Milvus, a specialized vector database, to store and retrieve passage embeddings. Milvus Lite mode was used for local development. Key aspects include:

- **Collection Schema:** Defined with fields for passage ID, text, and vector embedding.

- **Indexing:** IVF_FLAT with Inner Product similarity for efficient top-k search.

- **Collections:** Separate collections for 384d and 768d embeddings to prevent conflicts.

This component ensures sub-second retrieval latency over thousands of vectors while supporting experimentation with index parameters (nlist, nprobe).

**4. Retrieval and Generation Layer**

At query time, the pipeline:

1. Embeds the input question using the same SentenceTransformer model.

2. Performs top-k retrieval in Milvus to fetch candidate passages.

3. Concatenates retrieved passages with the query into a structured prompt.

4. Uses a Seq2Seq LLM (flan-t5-base) to generate an answer conditioned on retrieved evidence.

Three prompting strategies were implemented:

- Instructional Prompting (direct Q&A).

- Chain-of-Thought Prompting (reasoning steps).

- Persona Prompting (stylistic constraints).

This layer is central to the Naive and Enhanced comparison:

- Naive system: Direct retrieval + instruction prompting.

- Enhanced system: Includes prompt refinements, top-k variations, and embedding dimension comparisons.

**5. Evaluation Layer**

Evaluation was conducted using two complementary frameworks:

- Traditional QA Metrics:

  - Exact Match (EM) for strict correctness.

  - F1 score for token overlap.

- ARES & RAGAS Metrics:

  - Answer relevance and faithfulness.

  - Context relevance, precision, and recall.

  - Context F1 as a balance measure.

This dual-layer evaluation captures both factual accuracy and faithfulness to retrieved evidence, ensuring that improvements are not limited to superficial lexical overlap.

### Naive vs Enhanced Architecture

The Naive pipeline serves as a minimal baseline, using a single embedding model (384d MiniLM), top-1 retrieval, and direct instruction prompting. While lightweight, it demonstrates the feasibility of the approach and provides a benchmark.

The Enhanced pipeline introduces several architectural refinements:

- **Multiple Embedding Models and Dimensions:** Compared 384d MiniLM vs 768d MPNet.

- **Variable Retrieval Depth:** Evaluated top-k settings (1, 3, 5, 10).

- **Prompting Strategies:** Tested Instruction, Chain-of-Thought, and Persona prompts.

- **Broader Evaluation:** Used ARES to capture faithfulness and grounding.

These enhancements allowed for a richer analysis of system trade-offs, showing how different components affect precision, recall, and answer quality.

### Data Flow

The pipeline can be summarized as:

1. Input Query → embedded as vector.

2. Vector Search in Milvus → retrieves top-k relevant passages.

3. Prompt Construction → combines query and retrieved context.

4. LLM Answer Generation → produces response.

5. Evaluation → compares with ground truth using EM/F1 and ARES.

This flow highlights the centrality of embeddings and retrieval in influencing final answer quality.

### Strengths of the Architecture

- **Modularity:** Each stage (embedding, retrieval, generation, evaluation) is loosely coupled, allowing for component swapping.

- **Scalability:** Milvus enables large-scale vector storage and fast search.

- **Flexibility:** Config-driven embedding setup simplifies experimentation.

- **Comprehensive Evaluation:** Dual framework provides deeper insights than accuracy alone.

### Limitations

- **Context Utilization Trade-offs:** Enhanced system achieved modest gains in faithfulness but reduced recall.

- **Limited Corpus Size:** Larger, more diverse datasets would provide stronger validation.

- **LLM Constraints:** Flan-T5-base may not fully leverage retrieved context compared to larger LLMs.

### Future Improvements

- **Hybrid Retrieval:** Combine broad recall (Naive) with reranking (Enhanced).

- **Adaptive Prompting:** Tailor strategies by query type.

- **Cross-Encoder Rerankers:** Improve passage selection beyond embedding similarity.

- **Scaling Embeddings:** Explore 512d balanced models or multi-vector retrieval.

### Conclusion

This architecture successfully demonstrates a modular RAG system with both Naive and Enhanced variants. By layering Milvus-based retrieval with LLM generation, the system achieves grounded answers while providing a platform for experimentation. Evaluation shows that enhancements improved answer faithfulness and relevance, but at the cost of retrieval diversity. The design lays the foundation for hybrid approaches that better balance precision, recall, and contextual grounding.
