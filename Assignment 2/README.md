# RAG System Evaluation in Colab

This project implements and evaluates a Retrieval-Augmented Generation (RAG) system using both **Naive** and **Enhanced** approaches. It supports automated evaluation of question-answering performance with metrics like Exact Match (EM), F1, and ARES-based faithfulness and relevance scoring.

## Overview

- **Naive RAG:** Basic retrieval and generation pipeline.  
- **Enhanced RAG:** Improved context handling and prompt engineering.  
- **Evaluation:** Compare system outputs to ground truth using standard metrics.  
- **Colab-friendly:** Run the entire pipeline without local setup.

## Getting Started in Colab

### 1. Open Notebook

Download the notebook in the Code folder.

### 2. Install Dependencies

All required packages are installed in the notebook.

### 3. Set API Keys

The system uses OpenAI APIs for generation. Set your key in Colab:
```
import os
os.environ["OPENAI_API_KEY"] = "YOUR_OPENAI_API_KEY"
```
### Notes

Use GPU runtime for faster model inference.

