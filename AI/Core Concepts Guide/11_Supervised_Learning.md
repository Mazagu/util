# AI Core Concepts (Part 11): Supervised Learning

**Supervised Learning** is a type of machine learning where models are trained on **labeled data**—each input is associated with a known output (label). The model learns to map inputs to outputs by minimizing a loss function.

---

## 1. Core Idea

- Learn a function `f(x) ≈ y` from examples of `(x, y)` pairs.
- The model predicts outputs on new unseen inputs.
- Loss is computed based on how close predictions are to true labels.

---

## 2. Types of Supervised Learning

### 🔹 Classification

- Goal: Predict a **discrete label**.
- Examples: Spam detection, image classification, sentiment analysis

```
from sklearn.linear_model import LogisticRegression

model = LogisticRegression()
model.fit(X_train, y_train)
predictions = model.predict(X_test)
```

### 🔹 Regression

- Goal: Predict a **continuous value**.
- Examples: Price prediction, temperature forecasting, stock values

```
from sklearn.linear_model import LinearRegression

regressor = LinearRegression()
regressor.fit(X_train, y_train)
y_pred = regressor.predict(X_test)
```

---

## 3. Common Algorithms

| Task         | Algorithms                            |
|--------------|----------------------------------------|
| Classification | Logistic Regression, SVM, Random Forest, k-NN, Neural Networks |
| Regression      | Linear Regression, SVR, XGBoost, Decision Trees, Ridge/Lasso |
| Universal       | Gradient Boosting, Neural Nets, Transformers (with labels) |

---

## 4. Supervised Learning Pipeline

1. **Data Preparation** – clean, normalize, encode
2. **Train/Test Split** – evaluate generalization
3. **Model Selection** – pick based on data & task
4. **Training** – learn parameters on training data
5. **Evaluation** – use metrics (accuracy, MSE, F1)
6. **Tuning** – hyperparameter optimization
7. **Deployment** – inference on new data

---

## 5. Example: End-to-End Classification

```
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score

# Load and split data
data = load_iris()
X_train, X_test, y_train, y_test = train_test_split(data.data, data.target, test_size=0.2)

# Train
clf = RandomForestClassifier()
clf.fit(X_train, y_train)

# Predict and evaluate
y_pred = clf.predict(X_test)
print("Accuracy:", accuracy_score(y_test, y_pred))
```

---

## 6. Key Evaluation Metrics

| Task          | Metrics                             |
|---------------|--------------------------------------|
| Classification| Accuracy, Precision, Recall, F1     |
| Regression    | Mean Squared Error (MSE), R² score  |
| All Tasks     | Cross-validation, confusion matrix  |

---

## 7. When to Use Supervised Learning

✅ Use when:
- You have **labeled data**
- The problem is well-defined
- You want to **predict** something specific

❌ Avoid when:
- You only have unlabeled data (consider **unsupervised learning**)
- Task involves sequential decisions (consider **reinforcement learning**)

---

## 📚 Further Resources

- [Supervised Learning - Scikit-learn Guide](https://scikit-learn.org/stable/supervised_learning.html)
- [Hands-On ML with Scikit-Learn, Keras, & TensorFlow](https://www.oreilly.com/library/view/hands-on-machine-learning/9781492032632/)
- [Google ML Crash Course](https://developers.google.com/machine-learning/crash-course)

---
