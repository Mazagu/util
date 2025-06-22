# AI Core Concepts (Part 13): Prompt Engineering

**Prompt Engineering** is the practice of crafting effective inputs (prompts) to guide the outputs of **generative models**, especially large language models (LLMs) like GPT or Claude. It’s essential for improving quality, accuracy, and usefulness of responses.

---

## 1. What Is a Prompt?

A **prompt** is the input you give to a model. It can be:

- A question: “What is the capital of France?”
- An instruction: “Summarize this article in bullet points.”
- A task context: “Translate the following to French: Hello, how are you?”

Prompt design determines how well the model performs a task.

---

## 2. Types of Prompting

### 🔹 Zero-shot Prompting

Ask a model to complete a task without examples.

```
Prompt: "Summarize this paragraph in two sentences:

Artificial Intelligence is revolutionizing various industries..."
```

### 🔹 One-shot Prompting

Provide **one example** of the desired behavior.

```
Prompt: "Correct the grammar in this sentence:
Input: 'He go to school every day.'
Output: 'He goes to school every day.'

Correct this:
Input: 'She walk home yesterday.'
Output:"
```

### 🔹 Few-shot Prompting

Provide **a few examples** for better results.

```
Prompt:
"Convert these instructions to JSON:

Instruction: 'Turn off the lights and close the blinds.'
JSON: { 'action1': 'turn off lights', 'action2': 'close blinds' }

Instruction: 'Lock the door and arm the alarm.'
JSON:"
```

---

## 3. Prompting Patterns

### ➤ Chain-of-Thought (CoT)

Ask the model to **explain reasoning** step-by-step.

```
Prompt: "If there are 3 apples and you take away 2, how many do you have?
Let's think step by step."
```

### ➤ Role Prompting

Assign a persona or expertise to the model.

```
Prompt: "You are a senior software engineer. Review this Python code for performance issues:"
```

### ➤ Format Prompting

Request specific formats like lists, JSON, tables, Markdown.

```
Prompt: "List three pros and cons of remote work. Format as a Markdown table."
```

---

## 4. Common Use Cases

| Use Case             | Prompt Example |
|----------------------|----------------|
| Text generation      | “Write a sci-fi story in 5 sentences” |
| Code generation      | “Write a function to sort a list in Python” |
| Translation          | “Translate ‘Good night’ to German” |
| Summarization        | “Summarize this email for a manager” |
| Style Transfer       | “Rewrite this in a more formal tone” |

---

## 5. Tips for Better Prompt Engineering

- Be **clear and explicit**
- Specify **output format**
- Use **step-by-step thinking** for complex tasks
- Add **examples** for better consistency
- Use **system-level instructions** (e.g., "You are an expert editor")

---

## 6. Advanced Techniques

- **Prompt Chaining**: Output of one prompt feeds into another
- **Auto Prompting**: Algorithms generate the optimal prompt
- **ReAct Prompting**: Combine reasoning and action (popular in AI agents)
- **Retrieval-Augmented Generation (RAG)**: Add context to prompts from external sources

---

## 7. Tools for Prompt Engineering

- [OpenAI Playground](https://platform.openai.com/playground)
- [LangChain](https://www.langchain.com/)
- [PromptLayer](https://promptlayer.com/)
- [Flowise](https://flowiseai.com/) (Visual prompt chaining)
- [LlamaIndex](https://www.llamaindex.ai/)

---

## 📚 Further Resources

- [Awesome Prompt Engineering GitHub Repo](https://github.com/dair-ai/Prompt-Engineering-Guide)

---
