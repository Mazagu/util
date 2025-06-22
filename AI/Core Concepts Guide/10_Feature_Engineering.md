# AI Core Concepts (Part 10): Feature Engineering

**Feature Engineering** is the process of transforming raw data into meaningful inputs that improve model performance. Even with powerful models like deep neural networks, quality features remain crucial—especially in classical ML.

---

## 1. What Is a Feature?

A **feature** is an individual measurable property or characteristic of the phenomenon being observed. In datasets, features are columns (excluding the target in supervised learning).

---

## 2. Goals of Feature Engineering

- Make patterns more **learnable** by models
- Encode domain **knowledge** into features
- Reduce **noise** and improve **signal**
- Enable **better generalization**

---

## 3. Common Techniques

### 🔹 Numerical Transformations

- Normalization (min-max scaling)
- Standardization (z-score)
- Log, square root, polynomial expansion

```
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
scaled_features = scaler.fit_transform(numeric_data)
```

---

### 🔹 Categorical Encoding

- **One-Hot Encoding** for nominal categories
- **Label Encoding** for ordinal categories
- **Target Encoding** (mean of target per category)

```
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder()
encoded = encoder.fit_transform(data[['category']])
```

---

### 🔹 Binning and Bucketing

Group continuous variables into bins (e.g., age → young, middle-aged, old)

```
data["age_group"] = pd.cut(data["age"], bins=[0, 18, 40, 65, 100],
                           labels=["Teen", "Adult", "MiddleAge", "Senior"])
```

---

### 🔹 Feature Crosses and Interactions

- Combine multiple features to capture interactions (e.g., `user_region x product_category`)
- Polynomial feature combinations

```
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(degree=2, interaction_only=True)
X_poly = poly.fit_transform(X)
```

---

### 🔹 Date & Time Features

Extract:
- Day of week, hour, holiday indicator
- Seasonality patterns
- Time since last event

```
df["day_of_week"] = df["timestamp"].dt.dayofweek
df["hour"] = df["timestamp"].dt.hour
```

---

### 🔹 Text Features

- Bag-of-words, TF-IDF
- Embeddings (word2vec, BERT)
- Character n-grams

```
from sklearn.feature_extraction.text import TfidfVectorizer

vectorizer = TfidfVectorizer(max_features=1000)
X_text = vectorizer.fit_transform(df["text"])
```

---

## 4. Automated Feature Engineering

Libraries like **Featuretools**, **tsfresh**, and **AutoFeat** can automatically generate new features based on data relationships.

```
import featuretools as ft

es = ft.EntitySet(id="data")
es = es.entity_from_dataframe(entity_id="df", dataframe=df, index="id")
feature_matrix, features = ft.dfs(entityset=es, target_entity="df")
```

---

## 5. Feature Selection

After engineering, not all features help. Selection methods include:

- Filter: Variance threshold, correlation
- Wrapper: Recursive Feature Elimination (RFE)
- Embedded: Lasso, Tree-based importance

```
from sklearn.feature_selection import SelectKBest, f_classif

selector = SelectKBest(score_func=f_classif, k=10)
X_selected = selector.fit_transform(X, y)
```

---

## 6. Best Practices

- Avoid **leakage** (don't use future information)
- Understand **data distribution**
- Use **domain knowledge**
- Validate using **cross-validation**
- Don’t overengineer: More features ≠ better performance

---

## 📚 Further Resources

- [Feature Engineering for ML – Book by Alice Zheng](https://www.oreilly.com/library/view/feature-engineering-for/9781491953235/)

---
