# Medical Chatbot

An intelligent medical question-answering chatbot powered by Retrieval-Augmented Generation. It reads trusted PDF medical material, stores semantic embeddings in Pinecone, retrieves relevant context with hybrid search, and generates concise answers through a local Ollama LLM.

![Medical Chatbot UI](assets/ui.png)

## What Makes It Different

This is not a plain chatbot. It is a compact RAG system designed to answer from curated source documents instead of relying only on model memory.

- **Document-grounded answers** using PDF content from `data/`
- **Hybrid retrieval** with Pinecone vector search plus BM25 keyword search
- **Multi-query rewriting** to improve recall for vague or complex questions
- **Local LLM inference** through Ollama using `qwen2.5:7b-instruct`
- **Streaming chat responses** for a smoother user experience
- **Embedding and response caching** to reduce repeated work
- **Clean Flask interface** with a lightweight browser chat UI

## Architecture

```text
PDF files
   |
   v
Text loading and chunking
   |
   v
SentenceTransformer embeddings
   |
   v
Pinecone vector index
   |
   v
Hybrid retriever: BM25 + vector search
   |
   v
Ollama LLM: qwen2.5:7b-instruct
   |
   v
Flask chat interface
```

## Tech Stack

| Layer | Tools |
| --- | --- |
| Backend | Python, Flask |
| RAG framework | LangChain |
| Vector database | Pinecone |
| Local LLM | Ollama |
| Model | `qwen2.5:7b-instruct` |
| Embeddings | SentenceTransformers |
| PDF processing | PyPDF |

## Environment Variables

Create a `.env` file in the project root:

```env
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_INDEX=medical-chatbot
```

Optional tuning values:

```env
DATA_DIR=data
VECTOR_K=4
BM25_K=4
ENSEMBLE_BM25_WEIGHT=0.35
ENSEMBLE_VECTOR_WEIGHT=0.65
RESPONSE_CACHE_MAX=128
```

Never commit `.env`. It is already included in `.gitignore`.

## How It Works

The chatbot follows a retrieval-first workflow. Medical PDFs are converted into smaller text chunks, embedded into numerical vectors, and stored in Pinecone. When a user asks a question, the system searches for the most relevant document passages using both semantic similarity and keyword matching.

The retrieved context is passed to a local Ollama model, which generates an answer grounded in the available medical material. This keeps responses focused on the uploaded knowledge base instead of depending only on the model's built-in training data.

## User Experience

- Ask natural medical questions in a simple chat interface
- Receive short, focused answers based on retrieved document context
- Stream responses as they are generated
- Benefit from cached answers for repeated questions
- Use curated PDF sources as the knowledge base

## Ideal Use Cases

- Medical learning assistants
- Healthcare document exploration
- First-pass information retrieval from large PDFs
- RAG experimentation with LangChain and Pinecone
- Local LLM prototyping with Ollama

## Important Notes

- This project is for educational and research use.
- The chatbot should not be used as a replacement for professional medical advice.
- Pinecone index setup must match the embedding model dimensions used by the project.
- Ollama must be running while the Flask app is active.
- Keep API keys in `.env`, not inside source code.

## Author

Created by [annieannie12345](https://github.com/annieannie12345)
