# paperspace-rag-tools-rolling
Provide a rolling release notebook development environment with transformers, PGVector that runs the latest models

Development Container for use with paperspace-gradient. Inspired by https://github.com/gradient-ai/base-container
Contains whats needed to run the latest small llm models from huggingface, a vectordatabase, and enough to run an agent
Should be in active dev until january 2025

I noticed that the default container is not up to date to the latest branch 
and thus cannot run cutting edge models like LLAMA 3.2 or QWEN 2.5 that require transformers 4.47
This container is thus offered as a "stable" rolling release around transformers for development around RAG and small models
around the latest and most trendy tools

# Removed :
tensorflow. This repo focuses on pytorch

# Contains :
- NVIDIA drivers, cuda and whats needed to run a GPU.
- Python basic librairies for machine learning.
- The following Extras :
  - PGVector

The PGVector bind is located in the same folder as the one with the files. So data is saved across sessions
