# AI Core Concepts (Part 20): AI Infrastructure

**AI Infrastructure** refers to the backend systems, tools, and environments that support the development, deployment, and scaling of AI models. For software engineers, mastering AI infra means enabling reliable, efficient, and production-ready AI applications.

---

## 1. Why AI Infrastructure Matters

| Need                    | Infrastructure Role                                      |
|-------------------------|----------------------------------------------------------|
| Model Training          | Access to GPUs/TPUs, data pipelines                      |
| Model Serving           | Fast, scalable inference APIs                            |
| Experimentation         | Version control, reproducibility                         |
| Monitoring              | Logging, performance metrics, alerts                     |
| Scalability             | Deploying across distributed environments                |
| Cost Optimization       | Resource auto-scaling, serverless inference              |

---

## 2. Core Components of AI Infrastructure

### 1. **Compute Resources**
- GPUs (NVIDIA A100, V100), TPUs (Google)
- CPU for lightweight tasks
- On-premise, cloud (AWS, GCP, Azure), or hybrid

```
# Example: Provisioning a GPU instance with AWS CLI
aws ec2 run-instances \
  --image-id ami-xyz \
  --instance-type g4dn.xlarge \
  --key-name your-key \
  --count 1
```

---

### 2. **Data Infrastructure**
- Storage: S3, GCS, Azure Blob
- Preprocessing: Spark, Dask, Pandas
- ETL Pipelines: Airflow, Prefect, Dagster

```
# Example: Airflow DAG for data preprocessing
from airflow import DAG
from airflow.operators.python_operator import PythonOperator

def preprocess():
    # Load, clean, transform data
    ...

dag = DAG('data_pipeline', ...)
preprocess_task = PythonOperator(task_id='preprocess', python_callable=preprocess, dag=dag)
```

---

### 3. **Model Training Infrastructure**
- Frameworks: PyTorch, TensorFlow, JAX
- Distributed Training: Horovod, PyTorch DDP
- Resource Management: Kubernetes, Ray, Slurm

```
# Example: PyTorch DDP (Distributed Data Parallel)
import torch
import torch.distributed as dist
from torch.nn.parallel import DistributedDataParallel as DDP

dist.init_process_group("nccl", rank=rank, world_size=world_size)
model = DDP(model.to(device))
```

---

### 4. **Model Deployment and Serving**
- REST/gRPC APIs: FastAPI, Flask, Triton Inference Server
- Model Servers: TensorFlow Serving, TorchServe, BentoML, MLflow
- Containerization: Docker, Kubernetes

```
# Example: Serving with FastAPI
from fastapi import FastAPI
import joblib

app = FastAPI()
model = joblib.load("model.pkl")

@app.get("/predict")
def predict(x: float):
    return {"prediction": model.predict([[x]])[0]}
```

---

### 5. **Monitoring & Logging**
- Tools: Prometheus, Grafana, Loki, Sentry
- Model Monitoring: Fiddler, WhyLabs, Arize, Evidently
- Track drift, latency, failed requests

```
# Example: Logging inference latency
import time
start = time.time()
prediction = model.predict(input_data)
duration = time.time() - start
logger.info(f"Inference took {duration:.2f} seconds")
```

---

### 6. **Experiment Tracking**
- Tools: MLflow, Weights & Biases, Neptune
- Track: metrics, hyperparameters, model artifacts, code versions

```
# Example: MLflow experiment tracking
import mlflow

with mlflow.start_run():
    mlflow.log_param("lr", 0.001)
    mlflow.log_metric("accuracy", 0.92)
    mlflow.sklearn.log_model(model, "model")
```

---

### 7. **Model Versioning**
- DVC, MLflow Models, Hugging Face Model Hub
- Maintain reproducible snapshots of models + data

```
# Example: DVC model tracking
dvc add models/model.pkl
git add models/model.pkl.dvc
git commit -m "Track model with DVC"
```

---

## 3. Scalable Inference Patterns

| Pattern               | Description                                  |
|------------------------|----------------------------------------------|
| **Batch Inference**    | Run predictions on many records at once      |
| **Online Inference**   | Serve individual predictions via API         |
| **Serverless Inference** | Scales to zero, cheaper                     |
| **Streaming**          | Predict from live data (Kafka, Flink)        |

---

## 4. Security and Compliance

- Data encryption (in transit + at rest)
- Access control (IAM, service accounts)
- Model explainability (for regulated domains)
- Audit logging

---

## 5. Tools by Category

| Category               | Tools                                             |
|------------------------|---------------------------------------------------|
| Training               | PyTorch, TensorFlow, JAX, Hugging Face            |
| Deployment             | BentoML, MLflow, TorchServe, SageMaker            |
| Orchestration          | Airflow, KubeFlow, Prefect                        |
| Monitoring             | Prometheus, Grafana, Arize, WhyLabs               |
| Experiment Tracking    | Weights & Biases, Neptune, MLflow                 |
| Vector DBs             | FAISS, Pinecone, Weaviate, Qdrant                 |

---

## 6. Infrastructure-as-Code Example (Terraform for GCP AI Platform)

```
resource "google_ai_platform_model" "example" {
  name        = "my_model"
  regions     = ["us-central1"]
  labels      = { team = "ai" }
}
```

---

## 📚 Further Resources

- [Full-Stack Deep Learning](https://fullstackdeeplearning.com/)
- [ML Engineering Guide by Goku Mohandas](https://madewithml.com/)
- [Serving LLMs in Production (AnyScale)](https://www.anyscale.com/blog/llm-serving)
- [Google Vertex AI](https://cloud.google.com/vertex-ai)
- [Hugging Face Infinity + Text Generation Inference](https://huggingface.co/docs/infinity/index)

---
