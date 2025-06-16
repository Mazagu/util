# AI Core Concepts (Part 15): Fine-Tuning Models

**Fine-tuning** is the process of taking a **pre-trained model** and continuing its training on a **smaller, task-specific dataset**. This lets you adapt powerful general-purpose models (like BERT or GPT) to specialized domains (e.g., legal, medical, financial).

---

## 1. Why Fine-Tune?

- **Leverages pretrained knowledge**
- **Reduces training cost** (compared to training from scratch)
- **Improves accuracy on domain-specific data**
- **Customizes tone, format, or behavior of LLMs**

---

## 2. Fine-Tuning vs Prompting

| Feature             | Prompting                     | Fine-Tuning                        |
|---------------------|-------------------------------|------------------------------------|
| Customization Level | Shallow control               | Deep behavior adaptation           |
| Speed               | Instant                       | Takes time/resources               |
| Use Case            | General tasks                 | Domain-specific or sensitive tasks |
| Cost                | Low                           | Medium to High                     |

---

## 3. Fine-Tuning Example: BERT for Sentiment Analysis

```
from transformers import BertForSequenceClassification, Trainer, TrainingArguments
from transformers import BertTokenizerFast
from datasets import load_dataset

# Load dataset and tokenizer
dataset = load_dataset("imdb")
tokenizer = BertTokenizerFast.from_pretrained("bert-base-uncased")

# Tokenize
def preprocess(example):
    return tokenizer(example["text"], truncation=True, padding="max_length")

encoded = dataset.map(preprocess, batched=True)

# Load model
model = BertForSequenceClassification.from_pretrained("bert-base-uncased")

# Training setup
args = TrainingArguments(
    output_dir="./bert-finetuned",
    evaluation_strategy="epoch",
    per_device_train_batch_size=8,
    num_train_epochs=3,
    save_strategy="epoch",
)

trainer = Trainer(
    model=model,
    args=args,
    train_dataset=encoded["train"].shuffle(seed=42).select(range(2000)),
    eval_dataset=encoded["test"].select(range(500)),
)

trainer.train()
```

---

## 4. Fine-Tuning LLMs (GPT-2 / GPT-3.5)

LLMs can be fine-tuned with:
- Instruction-following data (prompt → completion)
- Domain-specific Q&A, document writing, code, etc.

```
# Example JSONL format (OpenAI fine-tuning)
{"prompt": "Translate to French: Hello", "completion": "Bonjour"}
{"prompt": "Translate to French: Good night", "completion": "Bonne nuit"}
```

---

## 5. Techniques for Fine-Tuning

- **Full fine-tuning**: Update all weights (costly)
- **LoRA (Low-Rank Adaptation)**: Add trainable adapters, efficient
- **PEFT (Parameter-Efficient Fine-Tuning)**: Family of techniques like LoRA
- **QLoRA**: Quantized version of LoRA, more memory efficient

```
# Hugging Face example (LoRA with PEFT)
from peft import get_peft_model, LoraConfig, TaskType
from transformers import AutoModelForCausalLM

base_model = AutoModelForCausalLM.from_pretrained("gpt2")
config = LoraConfig(task_type=TaskType.CAUSAL_LM, r=8, lora_alpha=32, lora_dropout=0.1)
model = get_peft_model(base_model, config)
```

---

## 6. When Not to Fine-Tune

❌ Don’t fine-tune if:
- You just need one-off responses → use prompting
- You have too little data (use embeddings + RAG instead)
- You can control behavior with system prompts or structured input

✅ Do fine-tune if:
- You need precise domain adaptation (e.g. legal contracts)
- You want specific tone, style, or behavior
- You’re building a production model with reusable logic

---

## 7. Tools & Platforms

- **Hugging Face Transformers** – Full fine-tuning + LoRA support
- **OpenAI Fine-tuning API** – Fine-tune GPT-3.5 on your data
- **Google Vertex AI / AWS Sagemaker** – Managed training environments
- **Weights & Biases** – Training tracking and experiments

---

## 📚 Further Resources

- [OpenAI Fine-Tuning Guide](https://platform.openai.com/docs/guides/fine-tuning)
- [Hugging Face Course – Fine-Tuning](https://huggingface.co/course/chapter3/3)
- [LoRA Paper (2021)](https://arxiv.org/abs/2106.09685)
- [PEFT by Hugging Face](https://huggingface.co/docs/peft/index)

---
