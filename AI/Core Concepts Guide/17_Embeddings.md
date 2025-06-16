# AI Core Concepts (Part 17): Embeddings

**Embeddings** are vector representations of data (text, images, audio, etc.) that capture **semantic meaning** in a machine-readable format. They're foundational to many AI tasks like search, recommendation, and similarity comparison.

---

## 1. What Are Embeddings?

An **embedding** maps complex input (e.g., a sentence or image) into a **dense vector of real numbers** in a high-dimensional space.

- Similar inputs → similar vectors
- Different inputs → distant vectors

For example:
- "cat" and "kitten" → vectors close together
- "cat" and "car" → vectors farther apart

---

## 2. Embedding Spaces

Embeddings exist in **high-dimensional vector spaces** (e.g., 512 or 1536 dimensions), where:
- **Distances** indicate semantic similarity
- **Dot product / cosine similarity** is used to compare

```
from sklearn.metrics.pairwise import cosine_similarity

# Vectors (pretend these came from a model)
vec_cat = [0.2, 0.8, 0.1]
vec_kitten = [0.25, 0.75, 0.15]

similarity = cosine_similarity([vec_cat], [vec_kitten])
print(similarity)
```

---

## 3. Text Embeddings with OpenAI API

```
import openai

response = openai.Embedding.create(
    input=["artificial intelligence", "machine learning"],
    model="text-embedding-3-small"
)

embedding_1 = response["data"][0]["embedding"]
embedding_2 = response["data"][1]["embedding"]
```

---

## 4. Visualizing Embeddings (Optional)

Use **t-SNE**, **UMAP**, or **PCA** to project high-dimensional embeddings into 2D/3D:

```
from sklearn.manifold import TSNE
import matplotlib.pyplot as plt

X = [embedding_1, embedding_2, ...]
labels = ["AI", "ML", "Pizza"]

X_proj = TSNE(n_components=2).fit_transform(X)
plt.scatter(X_proj[:, 0], X_proj[:, 1])
for i, label in enumerate(labels):
    plt.annotate(label, (X_proj[i, 0], X_proj[i, 1]))
plt.show()
```

---

## 5. Use Cases of Embeddings

| Task                        | Description                                     |
|-----------------------------|-------------------------------------------------|
| **Semantic Search**         | Find similar docs/questions                    |
| **Recommendations**         | Match users and content based on behavior      |
| **Clustering/Topic Modeling** | Group similar items without supervision        |
| **Text Classification**     | Use embeddings as features for ML models       |
| **RAG (Retrieval-Augmented Generation)** | Combine vector search with LLMs       |

---

## 6. Embedding Models (Text/Image/Audio)

| Type     | Examples                                  |
|----------|-------------------------------------------|
| Text     | `text-embedding-3-small`, `all-MiniLM-L6` |
| Image    | CLIP, BLIP-2                              |
| Audio    | Wav2Vec, Whisper embeddings               |
| Multi    | Gemini, GPT-4o                            |

---

## 7. Example: Sentence Similarity with SentenceTransformers

```
from sentence_transformers import SentenceTransformer, util

model = SentenceTransformer('all-MiniLM-L6-v2')
sentences = ["The cat sits outside", "A dog barks loudly"]

embeddings = model.encode(sentences)
similarity = util.cos_sim(embeddings[0], embeddings[1])

print("Similarity score:", similarity.item())
```

---

## 8. How Are Embeddings Used in LLM Pipelines?

LLMs often use embeddings in **RAG systems**, **tool use**, and **memory**:

```
# Pseudocode
query_vector = embed("What is a transformer?")
docs = vector_store.similarity_search(query_vector)
context = combine(docs)
response = llm.generate(prompt=context)
```

---

## 9. Common Vector Databases for Storing Embeddings

- **Pinecone**
- **Weaviate**
- **FAISS (Meta)**
- **Qdrant**
- **Milvus**
- **Chroma**

These allow fast similarity search across millions of vectors.

---

## 📚 Further Resources

- [OpenAI Embedding Guide](https://platform.openai.com/docs/guides/embeddings)
- [Hugging Face Sentence Transformers](https://www.sbert.net/)
- [Pinecone Docs](https://docs.pinecone.io/)
- [Vector Search with FAISS](https://github.com/facebookresearch/faiss)
- [Embeddings in RAG Systems](https://www.pinecone.io/learn/retrieval-augmented-generation/)

---
