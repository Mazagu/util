# AI Core Concepts (Part 2): Deep Learning for Software Engineers

## Deep Learning Overview

Deep learning is a subfield of machine learning based on **neural networks with many layers** (a.k.a. deep neural networks). It excels in tasks involving **unstructured data** such as images, text, and audio.

---

## 1. Uses GPUs for Training Efficiency

Training deep learning models involves millions of matrix operations. GPUs accelerate this by performing parallel computations on thousands of cores.

### 🔹 Why GPUs?
- Matrix multiplications (key to neural nets) are massively parallelizable.
- Libraries like **CUDA**, **cuDNN**, **TensorFlow**, and **PyTorch** take advantage of GPU hardware.

**Example: Enabling GPU in PyTorch**
```
import torch

device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
print("Using device:", device)

# Example model
model = MyModel().to(device)
```

---

## 2. Excels in Image, Audio, and NLP Tasks

Deep learning shines when working with **high-dimensional**, complex data like:

- **Images** (e.g., CNNs for object detection)
- **Audio** (e.g., RNNs for voice recognition)
- **Text** (e.g., Transformers for language models)

### 🔹 Example: Image Classification with CNN
```
import torch.nn as nn

class SimpleCNN(nn.Module):
    def __init__(self):
        super().__init__()
        self.conv = nn.Conv2d(1, 16, kernel_size=3)
        self.pool = nn.MaxPool2d(2)
        self.fc = nn.Linear(16 * 13 * 13, 10)

    def forward(self, x):
        x = self.pool(torch.relu(self.conv(x)))
        x = x.view(-1, 16 * 13 * 13)
        x = self.fc(x)
        return x
```

### 🔹 Example: NLP with Transformers (HuggingFace)
```
from transformers import pipeline

classifier = pipeline("sentiment-analysis")
print(classifier("I love using deep learning for NLP!"))
```

---

## 3. Learns Hierarchically from Raw Data

Unlike traditional ML, deep learning reduces need for manual feature engineering. Instead, it learns **hierarchical features** from raw input.

### 🔹 Hierarchical Learning Explained
- **Early layers** learn basic features (e.g., edges in images).
- **Middle layers** detect patterns (e.g., textures or shapes).
- **Later layers** combine into abstract concepts (e.g., faces, objects).

### 🔹 Example: Layers in CNN
```
model = nn.Sequential(
    nn.Conv2d(3, 32, kernel_size=3),    # Learn edges
    nn.ReLU(),
    nn.Conv2d(32, 64, kernel_size=3),   # Learn textures
    nn.ReLU(),
    nn.Flatten(),
    nn.Linear(64 * 6 * 6, 10)           # Combine into high-level features
)
```

---

## 📚 Further Resources
- [Deep Learning Specialization (Coursera)](https://www.coursera.org/specializations/deep-learning)
- [FastAI Practical Deep Learning](https://course.fast.ai/)
- [PyTorch Tutorials](https://pytorch.org/tutorials/)
- [HuggingFace Transformers Course](https://huggingface.co/course)

---