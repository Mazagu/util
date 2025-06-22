# AI Core Concepts (Part 18): Vector Search

**Vector search** allows you to find items that are semantically similar using **vector embeddings**, rather than exact keyword matches. It powers modern search engines, recommendation systems, and Retrieval-Augmented Generation (RAG) pipelines.

---

## 1. What Is Vector Search?

It’s a technique to **search by meaning** rather than literal text. It uses **cosine similarity**, **dot product**, or **L2 distance** to find vectors closest to a query vector.

- "AI revolution" → retrieves articles like “machine learning progress”
- "Heart attack symptoms" → finds related medical advice, even if phrased differently

---

## 2. Process Overview

1. **Embed your content** (e.g., documents) into dense vectors
2. **Store** them in a vector database
3. **Embed the user query**
4. **Search** using similarity (e.g., top-k nearest vectors)
5. **Use results** in your app (search, answer, recommend, etc.)

---

## 3. Use Cases

| Task                     | Description                                      |
|--------------------------|--------------------------------------------------|
| **Semantic Search**      | "Explain gravity" ≠ exact match, still relevant |
| **Recommendations**      | Find similar users/items by behavior            |
| **RAG**                  | LLM retrieves relevant context chunks           |
| **Similarity Detection** | Near-duplicate detection                        |
| **Multimodal Search**    | Search images by text description               |

---

## 4. Example: Using FAISS for Local Vector Search

```
from sentence_transformers import SentenceTransformer
import faiss
import numpy as np

# Step 1: Create embeddings
model = SentenceTransformer('all-MiniLM-L6-v2')
docs = ["What is AI?", "Benefits of machine learning", "Pizza recipe"]
doc_embeddings = model.encode(docs)

# Step 2: Index with FAISS
index = faiss.IndexFlatL2(len(doc_embeddings[0]))
index.add(np.array(doc_embeddings))

# Step 3: Query
query = "Explain artificial intelligence"
query_embedding = model.encode([query])

# Step 4: Search
D, I = index.search(np.array(query_embedding), k=2)
print("Top results:", [docs[i] for i in I[0]])
```

---

## 5. Vector Similarity Metrics

| Metric          | Use Case                              |
|------------------|----------------------------------------|
| **Cosine**       | Measures angle, not magnitude (text)   |
| **L2 Distance**  | Euclidean distance (images, audio)     |
| **Dot Product**  | Often used in neural retrieval         |

```
from sklearn.metrics.pairwise import cosine_similarity

cosine_similarity([vec1], [vec2])  # value between -1 and 1
```

---

## 6. Popular Vector Databases

| Tool        | Features                                           |
|-------------|----------------------------------------------------|
| **FAISS**   | Local search, fast, Facebook                      |
| **Pinecone**| Cloud-native, scalable, metadata filtering        |
| **Weaviate**| Semantic + symbolic search, GraphQL API           |
| **Qdrant**  | Open source, hybrid filters                       |
| **Chroma**  | Lightweight, built for LLM apps                   |
| **Milvus**  | High-volume, production-grade                     |

---

## 7. Example: Pinecone + OpenAI

```
import openai
import pinecone

# Init Pinecone
pinecone.init(api_key="your-key", environment="us-west1-gcp")
index = pinecone.Index("my-index")

# Create embedding
query = "How does a transformer work?"
query_vec = openai.Embedding.create(
  input=[query], model="text-embedding-3-small"
)["data"][0]["embedding"]

# Query vector DB
results = index.query(vector=query_vec, top_k=3, include_metadata=True)
for match in results['matches']:
    print(match['metadata']['text'])
```

---

## 8. Integration with LLMs (RAG)

```
# Retrieve relevant documents using vector search
query_vector = embed("What is supervised learning?")
docs = vector_db.similarity_search(query_vector, k=3)

# Inject into LLM prompt
context = "\n\n".join([doc.text for doc in docs])
prompt = f"Answer based on the context below:\n\n{context}\n\nQuestion: What is supervised learning?"
response = llm.generate(prompt)
```

---

## 9. Tips for Better Vector Search

✅ Use domain-specific embedding models  
✅ Normalize vectors (unit length)  
✅ Chunk text intelligently (e.g., 200–300 words)  
✅ Store metadata (title, tags, source)  
✅ Filter results with hybrid search (text + metadata)  

---

## 📚 Further Resources

- [FAISS GitHub](https://github.com/facebookresearch/faiss)
- [Pinecone Docs](https://docs.pinecone.io/)
- [Weaviate Docs](https://weaviate.io/developers/weaviate)
- [RAG + Vector Search Overview](https://www.pinecone.io/learn/retrieval-augmented-generation/)

---
