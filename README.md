# AI PDF Q&A Bot

Upload a PDF and ask questions about it. Answers come from OpenAI's GPT, grounded in the document through retrieval-augmented generation (RAG).

> 📦 **Status:** Archived · 2023 learning project
> Built with LangChain `0.0.304` and the OpenAI SDK `0.28`. Both have changed significantly since, so the code won't run on current versions without changes.

## Overview

Language models can't read your private documents and tend to make up answers. This app splits a PDF into searchable chunks, finds the parts relevant to a question, and gives only those to the model, so answers are based on the document itself.

## Architecture

1. **Extract:** reads the text from the uploaded PDF with PyPDF2
2. **Chunk:** splits it into ~1,000-character pieces with 200 characters of overlap, so context isn't lost at the boundaries
3. **Embed & index:** turns each chunk into an OpenAI embedding and stores it in an in-memory FAISS vector index
4. **Retrieve:** finds the chunks most similar to the question
5. **Generate:** sends those chunks and the question to GPT through LangChain's "stuff" QA chain

Token usage and cost for each question are logged to the console.

## Tech Stack

Python · Streamlit · LangChain · OpenAI API · FAISS · PyPDF2

## Getting Started

```bash
cp .env.example .env            # add your OPENAI_API_KEY
pip install -r requirements.txt
streamlit run app.py
```

`test.py` is an experimental version that adds an export-to-text button.

## Lessons Learned

- **RAG basics:** built in September 2023, as RAG was becoming the standard way to ground LLMs in your own data. It taught me that chunk size and overlap directly affect answer quality
- **Cost:** logging tokens per request showed early how quickly API costs add up
