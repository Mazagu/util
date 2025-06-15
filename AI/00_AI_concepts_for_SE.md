# AI Concepts for Software Engineers

## 1. Machine Learning
Machine Learning (ML) is the study of algorithms that improve automatically through experience. Core components include:

- Supervised, unsupervised, and reinforcement learning
- Regression, classification, and clustering algorithms
- Gradient descent and backpropagation
- Cross-validation and performance metrics

**Example: Linear Regression**
```python
from sklearn.linear_model import LinearRegression

model = LinearRegression()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

---

## 2. Deep Learning
Deep Learning is a subset of ML that uses neural networks with multiple layers (deep architectures) to model complex representations.

- **Uses GPUs for training efficiency**
- **Excels in image, audio, and NLP tasks**
- **Learns hierarchically from raw data**

**Example: Simple Deep Neural Network with Keras**
```python
from keras.models import Sequential
from keras.layers import Dense

model = Sequential([
    Dense(64, activation='relu', input_shape=(input_dim,)),
    Dense(64, activation='relu'),
    Dense(1)
])
model.compile(optimizer='adam', loss='mse')
model.fit(X_train, y_train, epochs=10)
```

---

## 3. Neural Networks
Artificial Neural Networks (ANNs) consist of layers of neurons that transform input data using weights and activation functions.

- **Input, hidden, and output layers**
- **Activation functions: ReLU, sigmoid, softmax**
- **Feedforward and backpropagation process**

**Example: Manual forward pass**
```python
import numpy as np

def relu(x):
    return np.maximum(0, x)

def forward_pass(x, weights):
    return relu(np.dot(x, weights))
```

---

## 4. Natural Language Processing (NLP)
NLP focuses on enabling machines to understand and generate human language.

- **Tokenization, stemming, lemmatization**
- **POS tagging, named entity recognition**
- **Transformers like BERT, GPT**

**Example: Tokenizing and stemming**
```python
from nltk.tokenize import word_tokenize
from nltk.stem import PorterStemmer

tokens = word_tokenize("ChatGPT is amazing!")
stems = [PorterStemmer().stem(word) for word in tokens]
```

---

## 5. Computer Vision
Computer Vision (CV) enables machines to interpret and process visual data like images and video.

- **Object detection, segmentation, classification**
- **CNNs (Convolutional Neural Networks)**
- **OpenCV, TensorFlow, PyTorch support**

**Example: Image classification with OpenCV and CNN**
```python
import cv2
image = cv2.imread('cat.jpg')
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
```

---

## 6. Reinforcement Learning
RL involves agents learning optimal behavior through rewards and penalties by interacting with environments.

- **Markov Decision Processes**
- **Q-learning, DQN, PPO**
- **Exploration vs. exploitation tradeoff**

**Example: Q-Learning algorithm sketch**
```python
Q[state, action] = reward + gamma * max(Q[next_state])
```

---

## 7. Generative Models
Generative models learn data distributions and can create new, similar data.

- **GANs (Generative Adversarial Networks)**
- **VAEs (Variational Autoencoders)**
- **Applications: image synthesis, text generation**

**Example: GAN architecture outline**
```python
# Generator and Discriminator models are trained in a loop
# Generator tries to fool the Discriminator
```

---

## 8. Large Language Models (LLMs)
LLMs are deep neural networks trained on massive text corpora to generate human-like language.

- **Examples: GPT, PaLM, LLaMA**
- **Used in chatbots, summarization, code generation**
- **Can be fine-tuned for specific domains**

**Example: Generate text with HuggingFace Transformers**
```python
from transformers import pipeline
generator = pipeline("text-generation", model="gpt2")
print(generator("Once upon a time", max_length=30))
```

---

## 9. Transformers
Transformers use self-attention to weigh importance of input tokens, allowing for parallel processing.

- **Encoder-decoder architecture**
- **Used in LLMs, translation, vision**
- **Highly scalable and performant**

**Example: Self-attention mechanism**
```python
Attention(Q, K, V) = softmax(QK^T / sqrt(d_k)) * V
```

---

## 10. Feature Engineering
The process of selecting, transforming, and creating variables that help ML models perform better.

- **Domain knowledge crucial**
- **Scaling, encoding, polynomial features**
- **Automated Feature Engineering tools exist**

**Example: Feature scaling**
```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
```

---

## 11. Supervised Learning
Training models using labeled data to learn input-output mappings.

- **Regression, classification**
- **Requires large labeled datasets**
- **Clear performance metrics**

**Example: Classification with Decision Tree**
```python
from sklearn.tree import DecisionTreeClassifier

clf = DecisionTreeClassifier()
clf.fit(X_train, y_train)
```

---

## 12. Bayesian Learning
Incorporates probability distributions and uncertainty into modeling.

- **Bayes’ Theorem**
- **Probabilistic graphical models**
- **Useful in low-data scenarios**

**Example: Naive Bayes classifier**
```python
from sklearn.naive_bayes import GaussianNB

model = GaussianNB()
model.fit(X_train, y_train)
```

---

## 13. Prompt Engineering
Designing effective inputs (prompts) to guide generative AI models.

- **Few-shot or zero-shot learning**
- **Prompt templates improve performance**
- **Useful in LLMs like GPT**

**Example: Prompt template**
```python
prompt = "Translate the following English text to French: '{}'"
```

---

## 14. AI Agents
Autonomous software systems that perceive environments, make decisions, and act.

- **Use planning, reasoning, learning**
- **Embodied agents (robots) or digital agents (bots)**
- **Popular in games, virtual assistants**

**Example: Simple rule-based agent**
```python
def agent(state):
    if state == "hungry":
        return "eat"
    return "wait"
```

---

## 15. Fine-Tuning Models
Customizing a pre-trained model on domain-specific data to improve accuracy.

- **Few epochs on new data**
- **Requires less data than training from scratch**
- **Common in NLP, CV tasks**

**Example: Fine-tuning BERT for classification**
```python
from transformers import BertForSequenceClassification

model = BertForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=2)
```

---

## 16. Multimodal Models
Models that process multiple data types simultaneously (e.g., text + image).

- **CLIP, Flamingo, GPT-4 (multimodal)**
- **Better understanding of real-world inputs**

**Example: Multimodal input sketch**
```python
# Inputs: image and caption
# Output: relevant tag or response
```

---

## 17. Embeddings
Dense vector representations of data (text, image, audio) for ML models.

- **Text: word2vec, BERT embeddings**
- **Useful in similarity, clustering**

**Example: Sentence embedding**
```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer('all-MiniLM-L6-v2')
embedding = model.encode("AI is transforming software engineering.")
```

---

## 18. Vector Search
Search using vector embeddings to find semantically similar items.

- **Used in semantic search, recommendations**
- **Libraries: FAISS, Pinecone**

**Example: FAISS search**
```python
import faiss
index = faiss.IndexFlatL2(embedding_dim)
index.add(vectors)
D, I = index.search(query_vector, k)
```

---

## 19. Model Evaluation
Assess model performance using metrics and validation techniques.

- **Accuracy, precision, recall, F1**
- **Cross-validation, confusion matrix**
- **AUC-ROC for classifiers**

**Example: Evaluation metrics**
```python
from sklearn.metrics import accuracy_score, classification_report

print(accuracy_score(y_test, y_pred))
print(classification_report(y_test, y_pred))
```

---

## 20. AI Infrastructure
Systems and tools that support development, deployment, and scaling of AI models.

- **Model training on distributed hardware**
- **Deployment: Docker, Kubernetes, CI/CD**
- **Monitoring and retraining pipelines**

**Example: Model deployment with Flask**
```python
from flask import Flask, request
app = Flask(__name__)

@app.route('/predict', methods=['POST'])
def predict():
    data = request.json
    prediction = model.predict([data['features']])
    return {'prediction': prediction.tolist()}
```

---
