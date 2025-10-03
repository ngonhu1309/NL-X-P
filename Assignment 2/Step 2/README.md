# Build Naive RAG

This notebook presents a baseline implementation of a **Retrieval-Augmented Generation (RAG)** pipeline. The goal is to combine information retrieval with generative models so that responses to user queries are grounded in external data. While generative models produce fluent text, they risk hallucinations if not connected to a reliable knowledge source.

### Objectives

The notebook has three primary objectives:

- **Dataset ingestion and preparation:** load a small corpus of documents to serve as the knowledge base.

- **Retriever construction:** build a lightweight method to identify relevant passages for a given query.

- **Naive RAG pipeline assembly:** integrate retrieval with generation to demonstrate the end-to-end workflow.

This simple pipeline serves as a foundation for future enhancements such as query rewriting, reranking, metadata filtering, or multi-vector retrieval.

### Dataset and Preprocessing

The pipeline uses a prebuilt dataset of short passages, drawn from domain-relevant sources (e.g., Wikipedia). The notebook illustrates how to load and inspect the data, then preprocess it into a form suitable for fast lookup (tokenized and indexed).

### Retrieval

The retriever identifies candidate passages most likely to contain an answer. In this baseline version, retrieval is done using simple vector embeddings and similarity search. This method is efficient and works reasonably well, though it cannot capture all nuances of natural language. The retriever returns either the top-k passages or a single best match.

### Generation

The retrieved contexts are passed to a generative model, which produces a final answer based on both the query and the supporting text. Even in this naive form, retrieval reduces hallucinations and grounds responses in factual evidence.

### Evaluation

Initial evaluation is performed using metrics such as Exact Match (EM) and F1 score, comparing outputs against human-annotated answers. These results establish a baseline for later improvements, such as reranking, optimized embeddings, or advanced retrieval strategies.
