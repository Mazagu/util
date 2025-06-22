# AI Core Concepts (Part 1) for Software Engineers

## 1. Supervised, Unsupervised, and Reinforcement Learning

### 🔹 Supervised Learning
The algorithm learns from labeled data — input-output pairs — and maps input to output.

**Examples:** Email spam detection, image classification.  
**Popular algorithms:** Linear regression, decision trees, SVMs, neural networks.

**Example: Supervised classification with SVM**
```
from sklearn.svm import SVC

model = SVC()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

---

### 🔹 Unsupervised Learning
The model learns patterns or groupings from **unlabeled data**.

**Examples:** Customer segmentation, anomaly detection.  
**Popular algorithms:** K-means, DBSCAN, PCA.

**Example: Clustering with K-means**
```
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=3)
kmeans.fit(X)
labels = kmeans.labels_
```

---

### 🔹 Reinforcement Learning
An agent interacts with an environment, learning to take actions that maximize cumulative rewards.

**Examples:** Game-playing AIs (e.g., AlphaGo), robot navigation.  
**Core concepts:** Agent, environment, state, action, reward.

**Example: Pseudo-code for Q-Learning**
```
Q[state, action] = Q[state, action] + alpha * (
    reward + gamma * max(Q[next_state]) - Q[state, action]
)
```

---

### 📚 Further Resources
- [Andrew Ng’s ML Course (Supervised/Unsupervised)](https://www.coursera.org/learn/machine-learning)
- [DeepMind’s RL Course](https://deepmind.com/learning-resources)
- [OpenAI Spinning Up in RL](https://spinningup.openai.com/)

---

## 2. Regression, Classification, and Clustering Algorithms

### 🔹 Regression
Predicts continuous values.

**Examples:** House price prediction, temperature forecasting.  
**Popular algorithm:** Linear Regression.

**Example: Linear regression with scikit-learn**
```
from sklearn.linear_model import LinearRegression

reg = LinearRegression()
reg.fit(X_train, y_train)
y_pred = reg.predict(X_test)
```

---

### 🔹 Classification
Predicts discrete labels (classes).

**Examples:** Fraud detection, sentiment analysis.  
**Popular algorithm:** Logistic regression, SVM, decision trees.

**Example: Logistic regression**
```
from sklearn.linear_model import LogisticRegression

clf = LogisticRegression()
clf.fit(X_train, y_train)
predictions = clf.predict(X_test)
```

---

### 🔹 Clustering
Groups data based on similarity without predefined labels.

**Examples:** Market segmentation, image compression.  
**Popular algorithm:** K-means, DBSCAN.

**Example: K-means again (for emphasis)**
```
from sklearn.cluster import KMeans

model = KMeans(n_clusters=3)
model.fit(X)
```

---

### 📚 Further Resources
- [StatQuest on Regression & Classification](https://www.youtube.com/user/joshstarmer)
- [Scikit-learn User Guide](https://scikit-learn.org/stable/user_guide.html)

---

## 3. Gradient Descent and Backpropagation

### 🔹 Gradient Descent
An optimization algorithm used to minimize the loss function by iteratively updating parameters.

**Concept:**
- Compute gradient (partial derivatives) of the loss.
- Update weights opposite to the gradient.

**Example: Manual gradient descent for linear regression**
```
w = 0
b = 0
learning_rate = 0.01

for epoch in range(100):
    y_pred = w * X + b
    loss = ((y - y_pred) ** 2).mean()
    
    dw = -2 * (X * (y - y_pred)).mean()
    db = -2 * (y - y_pred).mean()

    w -= learning_rate * dw
    b -= learning_rate * db
```

---

### 🔹 Backpropagation
The algorithm used in neural networks to compute gradients via the chain rule and update weights layer by layer.

**Example: Backprop sketch in a 2-layer NN**
```
# Forward pass
z1 = X @ W1 + b1
a1 = relu(z1)
z2 = a1 @ W2 + b2
a2 = softmax(z2)

# Backward pass
dL_dz2 = a2 - y_true
dL_dW2 = a1.T @ dL_dz2
```

---

### 📚 Further Resources
- [Gradient Descent Explained Visually](https://www.3blue1brown.com/lessons/gradient-descent)
- [Backpropagation Tutorial by Michael Nielsen](http://neuralnetworksanddeeplearning.com/chap2.html)
- [CS231n Notes - Optimization](https://cs231n.github.io/optimization-1/)

---

## 4. Cross-Validation and Performance Metrics

### 🔹 Cross-Validation
Technique to assess model generalization by splitting data into training and validation sets multiple times.

**Example: K-Fold cross-validation**
```
from sklearn.model_selection import cross_val_score
from sklearn.ensemble import RandomForestClassifier

scores = cross_val_score(RandomForestClassifier(), X, y, cv=5)
print("Average accuracy:", scores.mean())
```

---

### 🔹 Performance Metrics
Measure how well your model performs.

- **Classification:** Accuracy, Precision, Recall, F1-score, AUC.
- **Regression:** MSE, MAE, R².

**Example: Classification metrics**
```
from sklearn.metrics import classification_report, confusion_matrix

print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
```

**Example: Regression metrics**
```
from sklearn.metrics import mean_squared_error, r2_score

print("MSE:", mean_squared_error(y_test, y_pred))
print("R2:", r2_score(y_test, y_pred))
```

---

### 📚 Further Resources
- [Scikit-learn: Model Evaluation Guide](https://scikit-learn.org/stable/modules/model_evaluation.html)
