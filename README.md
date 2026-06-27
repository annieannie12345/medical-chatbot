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

## Project Structure

```text
.
├── app.py                 # Flask app and chat routes
├── ingest.py              # PDF ingestion into Pinecone
├── data/                  # Source medical PDFs
├── src/
│   ├── helper.py          # PDF loading, metadata, chunking, embeddings
│   └── prompt.py          # System prompt
├── static/                # CSS assets
├── templates/             # Chat UI template
├── research/              # Experiment notebook
├── requirements.txt       # Python dependencies
└── README.md
```

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

## Run Locally

Clone the repository:

```bash
git clone git@github.com:annieannie12345/medical-chatbot.git
cd medical-chatbot
```

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows, activate it with:

```bash
.venv\Scripts\activate
```

Install dependencies:

```bash
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Install Ollama from the official website:

```text
https://ollama.com/download
```

Start Ollama:

```bash
ollama serve
```

In a new terminal, pull the model:

```bash
ollama pull qwen2.5:7b-instruct
```

Add your PDFs to `data/`, then ingest them into Pinecone:

```bash
python ingest.py
```

Run the Flask app:

```bash
python app.py
```

Open the app:

```text
http://127.0.0.1:8080
```

## Common Commands

Re-ingest documents after adding or replacing PDFs:

```bash
python ingest.py --data-dir data --index medical-chatbot
```

Run the app with the virtual environment activated:

```bash
source .venv/bin/activate
python app.py
```

Check whether Ollama has the required model:

```bash
ollama list
```

## Important Notes

- This project is for educational and research use.
- The chatbot should not be used as a replacement for professional medical advice.
- Pinecone index setup must match the embedding model dimensions used by the project.
- Ollama must be running while the Flask app is active.
- Keep API keys in `.env`, not inside source code.

## Roadmap Ideas

- Source citations beside each answer
- Admin panel for uploading PDFs
- Docker-based deployment
- Cloud deployment with a hosted LLM option
- Conversation history and user sessions
- Automated evaluation for retrieval quality

## License

Add a license before using this project in production or distributing modified versions.
