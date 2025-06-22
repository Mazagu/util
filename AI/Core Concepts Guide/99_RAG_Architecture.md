# Bonus: Retrieval-Augmented Generation (RAG) Architecture

**RAG (Retrieval-Augmented Generation)** combines **information retrieval** and **language generation** to produce more accurate, context-aware, and up-to-date outputs. It enhances LLMs by grounding their answers in external documents.

---

## 🧠 Why RAG?
```
| Problem with LLMs          | How RAG Helps                                     |
|----------------------------|---------------------------------------------------|
| Hallucinations             | Uses real documents as context                    |
| Limited context window     | Retrieves only the most relevant pieces           |
| Outdated knowledge         | Can pull in current, dynamic data sources         |
| Inefficient fine-tuning    | Uses search instead of retraining the model       |
```
---

## 🔁 High-Level RAG Flow

1. **User Query →**
2. **Retriever**: Finds relevant docs from a knowledge base (vector DB, etc)
3. **Reader / Generator**: LLM uses both query + retrieved docs to generate a grounded answer

```
[User Query]
     ↓
[Retriever] ←→ [Vector DB / Corpus]
     ↓
[Context + Query]
     ↓
[LLM Generator (e.g. GPT, LLaMA)]
     ↓
[Answer]
```

---

## 🛠️ Core Components

### 1. **Document Ingestion Pipeline**

- Load raw docs (PDF, HTML, Notion, DB)
- Chunk + clean + embed them
- Store in vector database (e.g., FAISS, Pinecone)

```
from langchain.document_loaders import WebBaseLoader
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import FAISS

docs = WebBaseLoader("https://example.com/blog").load()
db = FAISS.from_documents(docs, OpenAIEmbeddings())
```

---

### 2. **Retriever**

- Converts query to embedding
- Finds top-k relevant documents

```
query = "What is RAG architecture?"
results = db.similarity_search(query, k=3)
```

---

### 3. **Prompt + Generation**

Combine retrieved documents and user query into a single prompt for the LLM.

```
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI

qa = RetrievalQA.from_chain_type(
    llm=ChatOpenAI(),
    retriever=db.as_retriever()
)

response = qa.run("Explain Retrieval-Augmented Generation")
```

---

## 🧱 RAG Stack Example
```
| Layer       | Tool Example                        |
|-------------|-------------------------------------|
| Embedding   | OpenAI, Cohere, SentenceTransformers |
| Storage     | FAISS, Weaviate, Pinecone, Qdrant   |
| LLM         | GPT-4, LLaMA, Claude, Mistral       |
| Framework   | LangChain, LlamaIndex, Haystack     |
| Frontend    | Streamlit, Next.js, Gradio          |
```
---

## 🔍 Use Cases

- **Chatbots with knowledge base grounding**
- **Legal, financial, or healthcare assistants**
- **R&D document search**
- **Internal Q&A for company wikis**
- **Search-augmented creative writing tools**

---

## ⚠️ Challenges & Tips
```
| Challenge               | Solution                                                |
|-------------------------|---------------------------------------------------------|
| Bad retrieval results   | Improve chunking, embedding model, metadata filtering   |
| Context too large       | Use summarization or reranking                          |
| Slow retrieval          | Use pre-computed, indexed vector DB                     |
| Privacy / access        | Add RBAC or hybrid RAG with permission-aware sources    |
```
---

## 🧠 Advanced RAG Patterns

### ✳️ Multi-hop RAG
Chain multiple queries for reasoning across multiple docs.

### 🧵 RAG with Memory
Add chat history/context to RAG pipeline.

### 🤖 Agentic RAG
Use agents to run tools or fetch external resources beyond vector DB.

---

## 🧪 Evaluation of RAG

- **Groundedness**: Does the answer align with source docs?
- **Faithfulness**: No hallucinated claims?
- **Latency**: Time from query to generation
- **Retrieval quality**: Precision/recall of relevant passages

---

## 📚 Further Resources

- [RAG Paper (Facebook AI)](https://arxiv.org/abs/2005.11401)
- [LlamaIndex RAG Examples](https://gpt-index.readthedocs.io/en/latest/)
- [Prompt Engineering for RAG](https://www.promptingguide.ai/techniques/rag)

---
