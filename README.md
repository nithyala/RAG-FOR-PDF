# RAG Document Q&A With Groq and Llama3

A Retrieval-Augmented Generation (RAG) app that answers questions about research papers. It loads PDFs, builds a vector database from them, and uses **Groq**-hosted **Llama 3** to answer your questions using only the content of those papers as context.

## 🔗 Live Demo

**[Launch App](https://rag-for-pdf-63qzid76akgu7ufift9w35.streamlit.app)**

> Click **Document Embedding** first to build the vector database, then type your question.

## 📂 Project Files

| File | Embeddings | Notes |
|------|-----------|-------|
| `main.py` | **OpenAI** (`OpenAIEmbeddings`) | Requires an OpenAI API key. This is the deployed version. |
| `app_huggingfaceembedding.py` | **HuggingFace** (`all-MiniLM-L6-v2`) | Free, runs locally — no OpenAI key needed for embeddings. Still needs a Groq key for the LLM. |

Both versions read PDFs from the `research_papers/` folder, which currently contains "Attention Is All You Need" and a Large Language Models survey paper.

## ✨ How It Works

The app follows a standard RAG pipeline: it loads the PDFs (data ingestion), splits them into overlapping chunks with a recursive text splitter, embeds each chunk into a vector, and stores them in a FAISS vector index. When you ask a question, it retrieves the most relevant chunks and passes them to Llama 3 as context, so the answer is grounded in the papers rather than the model's general knowledge. You can expand "Document Similarity Search" to see exactly which chunks were used.

## 🛠️ Tech Stack

- [Streamlit](https://streamlit.io/) — web UI
- [LangChain](https://www.langchain.com/) — RAG pipeline, chaining, prompt templating
- [Groq](https://groq.com/) — fast Llama 3 (`llama-3.1-8b-instant`) inference
- [FAISS](https://github.com/facebookresearch/faiss) — vector similarity search
- [OpenAI](https://openai.com/) or [HuggingFace](https://huggingface.co/) — text embeddings
- `PyPDFDirectoryLoader` — PDF loading

## 🚀 Getting Started

### 1. Clone the repo

\`\`\`bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
\`\`\`

### 2. Install dependencies

\`\`\`bash
pip install -r requirements.txt
\`\`\`

If you don't have a \`requirements.txt\` yet, install the core packages:

\`\`\`bash
pip install streamlit langchain langchain-groq langchain-openai langchain-community langchain-huggingface faiss-cpu pypdf python-dotenv sentence-transformers
\`\`\`

### 3. Set up API keys

Create a \`.env\` file in the project root:

\`\`\`
GROQ_API_KEY=your-groq-api-key
OPENAI_API_KEY=your-openai-api-key
\`\`\`

The HuggingFace version (\`app_huggingfaceembedding.py\`) only needs \`GROQ_API_KEY\`, since it uses a local embedding model.

### 4. Add your PDFs

Place the PDFs you want to query inside a \`research_papers/\` folder in the project root. At least one PDF must be present, or there will be nothing to embed.

### 5. Run the app

**OpenAI embeddings version:**

\`\`\`bash
streamlit run main.py
\`\`\`

**HuggingFace embeddings version (free embeddings):**

\`\`\`bash
streamlit run app_huggingfaceembedding.py
\`\`\`

Then click **Document Embedding** to build the vector store, and start asking questions.

## 📝 Notes

- On **Streamlit Cloud**, store \`GROQ_API_KEY\` and \`OPENAI_API_KEY\` under **Manage app → Settings → Secrets** instead of using a \`.env\` file.
- The \`research_papers/\` folder and its PDFs must be committed to the repo, or the deployed app won't find any documents to embed.
- If you don't have an OpenAI key, use \`app_huggingfaceembedding.py\` — its embeddings are completely free.

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
