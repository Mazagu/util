# AI Core Concepts (Part 7): Generative Models

**Generative Models** learn the underlying patterns of input data to **generate new data** that resembles the training distribution. They are used in image synthesis, text generation, audio creation, drug discovery, and more.

---

## 1. What are Generative Models?

Unlike discriminative models (which learn `P(y|x)`), generative models try to model the **data distribution itself** (`P(x)` or `P(x|z)`), allowing them to sample **new instances**.

---

## 2. Common Types of Generative Models

### 🔹 Variational Autoencoders (VAEs)

- Learn to encode input data into a **latent space**, then decode it back.
- Use **probabilistic encoders** and **KL-divergence** in loss.

**Example: VAE loss**
```
loss = reconstruction_loss + KL_divergence
```

**VAE Applications**: Denoising, image generation, latent space exploration.

---

### 🔹 Generative Adversarial Networks (GANs)

- Consist of a **generator** and **discriminator** in a zero-sum game.
- Generator tries to fool the discriminator into thinking fake samples are real.

**Training loop overview**
```
# Train discriminator
loss_D = loss(real) + loss(fake)

# Train generator
loss_G = loss(fooling_discriminator)
```

**Popular GAN variants**: DCGAN, StyleGAN2, CycleGAN.

---

### 🔹 Autoregressive Models

- Generate output **one token at a time**, conditioned on previous tokens.
- Examples: PixelRNN, WaveNet, GPT

**Example: Autoregressive text generation with GPT-2**
```
from transformers import pipeline

generator = pipeline("text-generation", model="gpt2")
output = generator("Once upon a time", max_length=30)
print(output[0]["generated_text"])
```

---

### 🔹 Diffusion Models

- Learn to **reverse a noise process** to generate high-quality data.
- Dominating image generation (e.g., DALL·E 3, Stable Diffusion).

**Core idea**: Train model to gradually remove noise from random input.

**Frameworks**:
- `diffusers` by Hugging Face
- `stable-diffusion-webui`

---

## 3. Applications of Generative Models

- 🎨 Art and Design (DALL·E, Midjourney)
- 🧬 Drug Discovery (generating novel molecules)
- 🧠 Data Augmentation (synthesizing training examples)
- 🗣️ Text-to-Speech (e.g., Tacotron, WaveNet)
- 📈 Anomaly Detection (model normal data and flag unusual ones)

---

## 4. Common Challenges

- Mode collapse (GANs generate limited diversity)
- Training instability (especially with adversarial setups)
- Evaluating quality of generated data
  - Inception Score (IS)
  - Frechet Inception Distance (FID)
  - Human judgment for language or creative tasks

---

## 📚 Further Resources

- [GANs in 50 Lines of Code (PyTorch)](https://github.com/eriklindernoren/PyTorch-GAN)
- [HuggingFace Diffusers](https://huggingface.co/docs/diffusers/index)
- [Lil’Log: GANs Explained](https://lilianweng.github.io/lil-log/)
- [VAE Intuition – Towards Data Science](https://towardsdatascience.com/understanding-variational-autoencoders-vaes-f70510919f73)

---
