# Domain Documents

The **RAG Mini Wikipedia dataset** is a simplified question-answering (QA) corpus derived from Wikipedia, designed for retrieval-augmented generation (RAG) research and evaluation. It was accessed via the Hugging Face datasets library in a Google Colab environment, confirming correct setup of the development platform and tools.

The dataset is packaged as a `DatasetDict` with a single `test` split containing 918 rows. Each record has three fields:

- `question` (string): the input query, phrased as a factual or historical question

- `answer` (string): the expected response

- `id` (int64): a unique identifier for indexing entries

Sample entries illustrate the dataset’s concise, factual style. For example, the first question, *"Was Abraham Lincoln the sixteenth President of the United States?"* is answered with *"yes"*, while the fourth question, *"How long was Lincoln’s formal education?"* is answered with *"18 months."*

The corpus is well-structured and free of missing values. Answers vary in types, binary, numeric, or textual, providing useful diversity for evaluating model behavior across QA categories. While relatively small compared to production-scale QA datasets, its compact size makes it efficient for prototyping and controlled experiments.

For infrastructure, the dataset was explored in Colab using Python, Hugging Face datasets, and pandas. Colab provides sufficient compute for handling this dataset, with optional GPU support for downstream training tasks. This establishes a reliable foundation for subsequent RAG pipeline development and evaluation.
