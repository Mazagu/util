# AI Core Concepts (Part 19): Model Evaluation

**Model Evaluation** is the process of assessing how well your machine learning or deep learning model performs on unseen data. This ensures your model generalizes beyond the training data and performs reliably in production.

---

## 1. Why Model Evaluation Matters

- Prevents **overfitting** to training data
- Ensures model is **robust** and **generalizable**
- Helps choose between models and **hyperparameter settings**
- Measures **real-world impact** using meaningful metrics

---

## 2. Key Concepts
```
| Term                 | Description                                          |
|----------------------|------------------------------------------------------|
| **Training Set**     | Used to train the model                             |
| **Validation Set**   | Used for tuning (cross-validation, early stopping)  |
| **Test Set**         | Final performance check, never touched during training |
```
---

## 3. Common Evaluation Metrics

### For **Classification**
```
| Metric       | Use Case                                   |
|--------------|---------------------------------------------|
| Accuracy     | % correct predictions                       |
| Precision    | Correct positive predictions / all predicted positives |
| Recall       | Correct positive predictions / all actual positives  |
| F1 Score     | Harmonic mean of precision and recall       |
| ROC-AUC      | Area under the ROC curve (binary classifiers) |
```
```
from sklearn.metrics import classification_report

y_true = [1, 0, 1, 1, 0, 1]
y_pred = [1, 0, 0, 1, 0, 1]

print(classification_report(y_true, y_pred))
```

---

### For **Regression**
```
| Metric          | Description                               |
|------------------|-------------------------------------------|
| MAE (Mean Absolute Error) | Average absolute difference        |
| MSE (Mean Squared Error)  | Penalizes large errors            |
| RMSE                   | Square root of MSE                   |
| R² Score               | % variance explained by model        |
```
```
from sklearn.metrics import mean_squared_error, r2_score

y_true = [3.0, -0.5, 2.0, 7.0]
y_pred = [2.5, 0.0, 2.1, 7.8]

mse = mean_squared_error(y_true, y_pred)
r2 = r2_score(y_true, y_pred)

print(f"MSE: {mse:.2f}, R²: {r2:.2f}")
```

---

## 4. Cross-Validation

Cross-validation helps assess how the model generalizes. Most common is **k-fold CV**:

- Split data into `k` parts
- Train on `k-1`, test on the 1 left out
- Repeat `k` times and average results

```
from sklearn.model_selection import cross_val_score
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris

X, y = load_iris(return_X_y=True)
model = RandomForestClassifier()

scores = cross_val_score(model, X, y, cv=5)
print("Cross-validation scores:", scores)
print("Mean CV score:", scores.mean())
```

---

## 5. Evaluation for Deep Learning Models

Use validation accuracy and loss curves during training to spot overfitting.

```
import matplotlib.pyplot as plt

# Assume you have history object from Keras
history = model.fit(...)

plt.plot(history.history['accuracy'], label='Train Acc')
plt.plot(history.history['val_accuracy'], label='Val Acc')
plt.legend()
plt.title("Training vs Validation Accuracy")
plt.show()
```

Also consider:
- Confusion matrix
- Early stopping
- Learning rate scheduling

---

## 6. Evaluation in NLP / LLMs
```
| Task          | Metric                     |
|---------------|-----------------------------|
| Text classification | Accuracy, F1            |
| Translation   | BLEU score                   |
| Summarization | ROUGE score                  |
| Q&A / Chatbot | Human evals, BLEU, GPT-based grading |
```
```
from rouge_score import rouge_scorer

scorer = rouge_scorer.RougeScorer(['rouge1', 'rougeL'], use_stemmer=True)
scores = scorer.score("The cat sat on the mat.", "The cat is sitting on the mat.")

print(scores)
```

For generative tasks, automatic metrics often fall short. Human evaluation is still gold standard.

---

## 7. A/B Testing in Production

Once a model is deployed, evaluate with **live metrics**:

- Conversion rate
- User engagement
- Latency
- Feedback scores

A/B tests help compare models in real environments.

---

## 8. Tools for Evaluation

- `scikit-learn.metrics`: traditional ML metrics
- `Keras` and `PyTorch`: training history, validation loss
- `MLflow`, `Weights & Biases`: track experiment performance
- `TruLens`, `Helicone`: LLM-specific evaluation
- Human eval platforms: Label Studio, Scale, Surge AI

---

## 📚 Further Resources

- [Scikit-learn Metrics Docs](https://scikit-learn.org/stable/modules/model_evaluation.html)
- [Keras Model Evaluation](https://keras.io/api/models/model_training_apis/)
- [MLflow for experiment tracking](https://mlflow.org/)
- [HumanEval benchmark for LLM coding tasks](https://github.com/openai/human-eval)

---
