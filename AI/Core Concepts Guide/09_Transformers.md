# AI Core Concepts (Part 9): Transformers

Transformers are the backbone of modern deep learning models in NLP, vision, and multimodal AI. They power models like **BERT**, **GPT**, **T5**, **Vision Transformers**, and more.

---

## 1. What Is a Transformer?

A Transformer is a neural network architecture introduced in the paper **"Attention is All You Need" (Vaswani et al., 2017)**. It replaces recurrence with **self-attention**, enabling parallelization and long-range context understanding.

### Key Components:
- **Self-Attention Mechanism**: Weighs relationships between all tokens in a sequence.
- **Multi-Head Attention**: Learns multiple attention patterns.
- **Feedforward Layers**: Position-wise MLPs applied after attention.
- **Layer Normalization** and **Residual Connections**
- **Positional Encoding**: Adds order to token embeddings.

---

## 2. Self-Attention Explained

Given input tokens, attention calculates a weighted sum of all tokens based on **query, key, and value vectors**.

**Attention Formula:**
```
Attention(Q, K, V) = softmax(QKᵀ / √dₖ) * V
```

This lets each token "attend" to others, capturing dependencies regardless of distance.

---

## 3. Encoder vs Decoder
```
| Component | Role                          | Example Use           |
|----------|-------------------------------|------------------------|
| Encoder  | Bidirectional understanding    | BERT, RoBERTa          |
| Decoder  | Autoregressive generation      | GPT, LLaMA             |
| Encoder-Decoder | Translation, Summarization | T5, BART, mT5         |
```
---

## 4. Example: Minimal Transformer Block (PyTorch)
```
import torch
import torch.nn as nn

class TransformerBlock(nn.Module):
    def __init__(self, embed_dim, heads):
        super().__init__()
        self.attn = nn.MultiheadAttention(embed_dim, heads)
        self.ff = nn.Sequential(
            nn.Linear(embed_dim, embed_dim * 4),
            nn.ReLU(),
            nn.Linear(embed_dim * 4, embed_dim)
        )
        self.norm1 = nn.LayerNorm(embed_dim)
        self.norm2 = nn.LayerNorm(embed_dim)

    def forward(self, x):
        attn_out, _ = self.attn(x, x, x)
        x = self.norm1(x + attn_out)
        ff_out = self.ff(x)
        return self.norm2(x + ff_out)
```

---

## 5. Real-World Transformer Models
```
| Model   | Type         | Purpose                    |
|---------|--------------|----------------------------|
| BERT    | Encoder       | Classification, QA         |
| GPT     | Decoder       | Text generation            |
| T5      | Encoder-Decoder| Summarization, translation|
| ViT     | Encoder       | Vision tasks               |
| SAM     | Encoder-Decoder | Image segmentation       |
```
---

## 6. Applications of Transformers

- Language modeling (GPT series)
- Machine translation (T5, MarianMT)
- Document classification (BERT)
- Visual tasks (ViT, SAM)
- Multimodal reasoning (Flamingo, GPT-4, Gemini)

---

## 7. Strengths and Limitations

✅ Strengths:
- Handles long-range dependencies
- Highly parallelizable
- Adaptable across data modalities

⚠️ Limitations:
- Quadratic time/memory complexity with sequence length
- Prone to hallucination in generation
- Requires large training data and compute

---

## 📚 Further Resources

- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)
- [Transformers from Scratch (Peter Bloem)](https://peterbloem.nl/blog/transformers)
- [Attention is All You Need – Original Paper](https://arxiv.org/abs/1706.03762)
- [Hugging Face Transformers Docs](https://huggingface.co/docs/transformers/index)

---
