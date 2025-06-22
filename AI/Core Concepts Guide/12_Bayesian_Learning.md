# AI Core Concepts (Part 12): Bayesian Learning

**Bayesian Learning** is a probabilistic approach to modeling uncertainty in machine learning. Instead of finding a single best model, it estimates a **distribution over possible models** given the data.

---

## 1. Core Idea: Bayes’ Theorem

Bayesian learning updates our beliefs about model parameters using **Bayes' theorem**:

```
P(θ | D) = [ P(D | θ) * P(θ) ] / P(D)
```

Where:
- `θ` = model parameters  
- `D` = observed data  
- `P(θ)` = prior belief about parameters  
- `P(D | θ)` = likelihood of data given parameters  
- `P(θ | D)` = posterior: updated belief after seeing data

---

## 2. Why Bayesian?

- **Uncertainty Quantification**: Provides confidence intervals, not just point estimates.
- **Small Data Friendly**: Prior knowledge helps when data is scarce.
- **Regularization by Design**: Priors act as built-in regularizers.

---

## 3. Example: Bayesian Linear Regression

Instead of estimating one best line, Bayesian regression gives a **distribution of possible lines**.

```
from sklearn.linear_model import BayesianRidge
import numpy as np

# Fake data
X = np.random.randn(100, 1)
y = 3 * X[:, 0] + np.random.randn(100) * 0.5

# Bayesian Regression
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y)

model = BayesianRidge()
model.fit(X_train, y_train)

# Predict with uncertainty
y_mean, y_std = model.predict(X_test, return_std=True)
```

The model outputs not only predictions (`y_mean`), but also the **uncertainty** (`y_std`) per point.

---

## 4. Priors and Posteriors in Practice

- **Priors** reflect what we assume before seeing data.
- In Bayesian regression, a Gaussian prior on weights favors small values (like L2 regularization).

```
# Prior: w ~ N(0, α⁻¹)
# Likelihood: y ~ N(Xw, β⁻¹)
# Posterior: Computed via closed-form update or sampling
```

---

## 5. Approximate Bayesian Inference

Exact computation of posteriors is intractable in most deep models. Alternatives:

- **MCMC (Markov Chain Monte Carlo)**: Samples from the posterior
- **Variational Inference**: Optimizes a simpler distribution to approximate the posterior
- **Monte Carlo Dropout**: Use dropout at inference to simulate uncertainty

```
# Monte Carlo Dropout Example (Keras)
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout

model = Sequential([
    Dense(64, activation='relu'),
    Dropout(0.5),  # Dropout active during test
    Dense(1)
])
```

---

## 6. Bayesian vs Frequentist Learning

| Concept         | Bayesian                             | Frequentist                         |
|----------------|--------------------------------------|-------------------------------------|
| Parameters      | Distributions (random variables)     | Fixed but unknown                   |
| Inference       | Posterior from prior + data          | Maximum likelihood / optimization  |
| Output          | Probabilistic predictions + variance | Point estimates                     |

---

## 7. When to Use Bayesian Learning

✅ Use when:
- You need **uncertainty estimates**
- Working with **small datasets**
- Decisions have **high cost of error**

⚠️ Less practical when:
- You need fast inference (Bayesian models can be slow)
- You work with massive datasets and scale is critical

---

## 8. Libraries for Bayesian Learning

- **PyMC** – Probabilistic programming in Python  
- **TensorFlow Probability** – Probabilistic layers and distributions  
- **Stan** – Powerful Bayesian modeling language  
- **Edward2** – Probabilistic modeling with TensorFlow  
- **GPyTorch** – Bayesian Gaussian Processes (PyTorch-based)

---

## 📚 Further Resources

- [Bayesian Methods for Hackers (free book)](https://camdavidsonpilon.github.io/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/)
- [PyMC Documentation](https://www.pymc.io/)
- [TensorFlow Probability](https://www.tensorflow.org/probability)
- [Andrew Gelman – Bayesian Data Analysis](http://www.stat.columbia.edu/~gelman/book/)

---
