# AI Core Concepts (Part 3): Neural Networks for Software Engineers

Neural networks are at the heart of deep learning. They are inspired by the structure of the human brain and are composed of layers of interconnected **neurons** (also called nodes or units).

---

## 1. Input, Hidden, and Output Layers

A neural network is structured in **layers**:

- **Input Layer**: Takes in the raw features (e.g., pixel values, word embeddings).
- **Hidden Layers**: Intermediate layers that learn features and representations.
- **Output Layer**: Produces the final prediction (e.g., class probabilities, numerical value).

**Example: 3-layer network (input → hidden → output)**
```
import torch.nn as nn

model = nn.Sequential(
    nn.Linear(4, 8),  # input layer → hidden (4 inputs, 8 units)
    nn.ReLU(),
    nn.Linear(8, 3)   # hidden → output (8 → 3 classes)
)
```

---

## 2. Activation Functions: ReLU, Sigmoid, Softmax

Activation functions introduce **non-linearity**, allowing networks to learn complex patterns.

### 🔹 ReLU (Rectified Linear Unit)
- Most commonly used.
- `f(x) = max(0, x)`

### 🔹 Sigmoid
- Outputs between 0 and 1.
- Often used for binary classification.

### 🔹 Softmax
- Converts scores into probabilities that sum to 1.
- Used in multi-class classification.

**Example: Using different activation functions**
```
import torch.nn.functional as F

x = torch.tensor([-1.0, 0.0, 1.0])

relu_output = F.relu(x)
sigmoid_output = torch.sigmoid(x)
softmax_output = F.softmax(x, dim=0)
```

---

## 3. Feedforward and Backpropagation Process

### 🔹 Feedforward
- Data flows from input → hidden → output layers.
- Each layer applies a linear transformation + activation.

### 🔹 Backpropagation
- Calculates the gradient of the loss with respect to each weight using the **chain rule**.
- Updates weights using **gradient descent**.

**Example: Simple forward and backward pass**
```
import torch

# Data and labels
X = torch.randn(10, 4)
y = torch.randint(0, 3, (10,))

# Model and loss
model = nn.Sequential(
    nn.Linear(4, 8),
    nn.ReLU(),
    nn.Linear(8, 3)
)
loss_fn = nn.CrossEntropyLoss()
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)

# Forward pass
outputs = model(X)
loss = loss_fn(outputs, y)

# Backward pass
optimizer.zero_grad()
loss.backward()
optimizer.step()
```

---

## 📚 Further Resources
- [Neural Networks from Scratch - Sentdex YouTube Series](https://www.youtube.com/watch?v=Wo5dMEP_BbI)
- [CS231n: Neural Network Basics](https://cs231n.github.io/neural-networks-1/)
- [DeepLizard: Activation Function Intuition](https://www.youtube.com/watch?v=-7scQpJT7uo)

---
