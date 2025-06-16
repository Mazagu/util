# AI Core Concepts (Part 16): Multimodal Models

**Multimodal models** can process and generate **multiple types of data** — including **text, images, audio, and video** — either as inputs, outputs, or both. These models integrate information from different modalities for richer understanding and reasoning.

---

## 1. What Is a Multimodal Model?

A model that accepts or produces **more than one type of input/output** modality. Common modalities:

- 📝 Text
- 🖼️ Image
- 🔊 Audio
- 🎥 Video
- 🧠 Sensor data

---

## 2. Examples of Multimodal Use Cases

| Task                        | Modalities             | Example                          |
|-----------------------------|------------------------|----------------------------------|
| Visual Question Answering   | Image + Text → Text    | “What’s in this picture?”        |
| Image Captioning            | Image → Text           | Describe what's in an image      |
| Speech Recognition          | Audio → Text           | Transcribe audio recordings      |
| Text-to-Image Generation    | Text → Image           | “Generate a cat riding a bike”   |
| Video Summarization         | Video → Text           | Summarize a YouTube video        |
| Audio-Visual Emotion Recognition | Audio + Video → Label | Detect mood in a video call     |

---

## 3. Architecture: Combining Modalities

Multimodal models typically use:
- Separate **encoders** per modality (e.g., image encoder, text encoder)
- A **fusion layer** to combine modality embeddings
- A **decoder** to generate output in a target modality

```
# Pseudocode for architecture
text_embedding = TextEncoder(text_input)
image_embedding = ImageEncoder(image_input)

combined = FusionLayer([text_embedding, image_embedding])
output = Decoder(combined)
```

---

## 4. Famous Multimodal Models

| Model       | Capabilities                    |
|-------------|---------------------------------|
| **CLIP**    | Connects images and text        |
| **DALL·E**  | Text-to-image generation        |
| **GPT-4o**  | Omni-modal (text, image, audio) |
| **Flamingo**| Visual-language QA              |
| **Gemini**  | Multimodal across web and tools |
| **BLIP-2**  | Image captioning + QA           |
| **SpeechT5**| Unified audio-text model        |

---

## 5. Example: CLIP (Contrastive Language–Image Pretraining)

CLIP learns to match images with relevant text by training on image-caption pairs.

```
from transformers import CLIPProcessor, CLIPModel
from PIL import Image
import torch

model = CLIPModel.from_pretrained("openai/clip-vit-base-patch32")
processor = CLIPProcessor.from_pretrained("openai/clip-vit-base-patch32")

image = Image.open("dog.jpg")
texts = ["a dog", "a cat", "a human"]

inputs = processor(text=texts, images=image, return_tensors="pt", padding=True)
outputs = model(**inputs)
logits_per_image = outputs.logits_per_image
probs = logits_per_image.softmax(dim=1)

print(probs)  # Probability that the image matches each caption
```

---

## 6. Building a Simple Text + Image Model

```
# Encode text and image, then combine
text_emb = TextEncoder("a red apple")
image_emb = ImageEncoder("apple.jpg")

combined = torch.cat([text_emb, image_emb], dim=1)
output = Classifier(combined)  # e.g., for matching or classification
```

---

## 7. Tools & Libraries for Multimodal AI

- 🤗 Hugging Face Transformers + Datasets
- 🧨 Diffusers – for generative models (DALL·E, Stable Diffusion)
- 🐍 PyTorch + torchvision
- 🧠 OpenAI’s CLIP and Whisper
- 🔥 Google DeepMind Flamingo / Gemini APIs (cloud-based)

---

## 8. Challenges of Multimodal Models

- **Alignment**: Matching info across modalities (e.g., image ↔ caption)
- **Latency**: Higher inference costs due to multiple encoders
- **Data imbalance**: Text data is more available than video/audio
- **Complexity**: Difficult to fine-tune or deploy on edge devices

---

## 9. Prompting Multimodal LLMs

For models like GPT-4o (text + image):

```
Prompt: "What's happening in this image?"
Attachment: 📷 [image of a flooded street]
```

For tools like LangChain:

```
from langchain.chains import MultiModalChain
# Combine a text prompt and image input for richer querying
```

---

## 📚 Further Resources

- [CLIP Paper](https://arxiv.org/abs/2103.00020)
- [DALL·E 2](https://openai.com/dall-e-2)
- [GPT-4o (OpenAI)](https://openai.com/gpt-4o)
- [BLIP-2 on Hugging Face](https://huggingface.co/Salesforce/blip2)
- [Multimodal Transformers Survey](https://arxiv.org/abs/2301.04847)

---
