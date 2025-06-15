# AI Core Concepts (Part 4): Natural Language Processing (NLP)

Natural Language Processing (NLP) enables machines to understand, interpret, and generate human language. It's used in applications like chatbots, translation, sentiment analysis, and search.

---

## 1. Tokenization, Stemming, Lemmatization

### 🔹 Tokenization
Splits text into individual elements like words or subwords.

**Example: Word tokenization**
```
from nltk.tokenize import word_tokenize

text = "Deep learning models are powerful."
tokens = word_tokenize(text)
print(tokens)  # ['Deep', 'learning', 'models', 'are', 'powerful', '.']
```

---

### 🔹 Stemming
Reduces words to their base/root form by chopping off ends. Often less accurate.

**Example: Stemming**
```
from nltk.stem import PorterStemmer

stemmer = PorterStemmer()
print(stemmer.stem("running"))  # run
print(stemmer.stem("flies"))    # fli
```

---

### 🔹 Lemmatization
Reduces words to their base dictionary form (lemma), considering context.

**Example: Lemmatization**
```
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()
print(lemmatizer.lemmatize("running", pos="v"))  # run
print(lemmatizer.lemmatize("flies", pos="n"))    # fly
```

---

## 2. POS Tagging, Named Entity Recognition

### 🔹 POS (Part-of-Speech) Tagging
Assigns grammatical labels (noun, verb, adjective, etc.) to each token.

**Example: POS Tagging**
```
import nltk
nltk.download("averaged_perceptron_tagger")

tokens = word_tokenize("The quick brown fox jumps over the lazy dog.")
pos_tags = nltk.pos_tag(tokens)
print(pos_tags)
```

---

### 🔹 Named Entity Recognition (NER)
Identifies entities like names, organizations, locations in text.

**Example: NER with spaCy**
```
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("Apple is looking to buy a startup in the UK for $1 billion.")

for ent in doc.ents:
    print(ent.text, ent.label_)
```

---

## 3. Transformers like BERT, GPT

Transformers are state-of-the-art models in NLP, based on self-attention. They understand context by attending to every word in a sentence simultaneously.

### 🔹 BERT (Bidirectional Encoder Representations from Transformers)
- Trained using **masked language modeling**.
- Great for classification, NER, and Q&A tasks.

**Example: Sentiment Analysis using BERT**
```
from transformers import pipeline

classifier = pipeline("sentiment-analysis", model="distilbert-base-uncased-finetuned-sst-2-english")
result = classifier("This course is absolutely amazing!")
print(result)
```

---

### 🔹 GPT (Generative Pre-trained Transformer)
- Decoder-only model trained to **generate** text.
- Powers models like ChatGPT.

**Example: Text generation using GPT-2**
```
from transformers import pipeline

generator = pipeline("text-generation", model="gpt2")
response = generator("Once upon a time in a world of AI,", max_length=30)
print(response[0]["generated_text"])
```

---

## 📚 Further Resources
- [NLTK Book (Free)](https://www.nltk.org/book/)
- [spaCy Official Tutorials](https://spacy.io/usage)
- [HuggingFace Transformers Course](https://huggingface.co/course)
- [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/)

---
