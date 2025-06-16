# AI Core Concepts (Part 14): AI Agents

**AI Agents** are autonomous systems that can **perceive** their environment, **make decisions**, and **take actions** toward achieving goals. Unlike simple models, agents often act in a loop, learn from outcomes, and adjust behavior over time.

---

## 1. What Is an AI Agent?

An AI Agent typically follows the **perception → decision → action** cycle.

- **Perception**: Observes data from the environment.
- **Decision-making**: Plans what to do using a model or policy.
- **Action**: Executes steps in the environment.
- **Feedback Loop**: Observes effects and learns from them.

---

## 2. Types of AI Agents

| Agent Type       | Description                                              |
|------------------|----------------------------------------------------------|
| Reactive Agent   | Responds to current input (no memory/learning)           |
| Reflex Agent     | Uses rules to map inputs to actions                      |
| Goal-Based Agent | Chooses actions to achieve specific goals                |
| Learning Agent   | Improves over time using data/experience                 |
| Planning Agent   | Builds plans by simulating outcomes                      |

---

## 3. Basic AI Agent Structure (Pseudocode)

```
while True:
    observation = agent.observe(env)
    action = agent.decide(observation)
    env.update(action)
    agent.learn(observation, action, env.feedback())
```

---

## 4. AI Agents vs Standard Models

| Feature              | Standard ML Model           | AI Agent                        |
|----------------------|-----------------------------|----------------------------------|
| Passive or Active    | Passive (predict-only)       | Active (acts in environment)     |
| One-shot inference   | Yes                          | No, typically multi-step         |
| Learning approach    | Supervised, Unsupervised     | Often Reinforcement Learning     |
| Autonomy             | Low                          | High                             |

---

## 5. Example Use Cases

- **Robotics** – Robots navigating a room, picking objects
- **Game Agents** – AI that learns to play video games (e.g., AlphaStar, MuZero)
- **Conversational Agents** – Multi-turn chatbots with memory and reasoning
- **Automation Bots** – Web scraping agents, workflow automation (RPA)
- **Trading Bots** – Agents making buy/sell decisions in markets

---

## 6. Example: Agent with LangChain + OpenAI

```
from langchain.agents import initialize_agent, Tool
from langchain.llms import OpenAI
from langchain.tools import DuckDuckGoSearchRun

search = DuckDuckGoSearchRun()
tools = [Tool(name="Search", func=search.run, description="Useful for answering factual questions")]

agent = initialize_agent(
    tools,
    llm=OpenAI(temperature=0),
    agent="zero-shot-react-description",
    verbose=True
)

agent.run("What's the weather in Berlin today?")
```

---

## 7. Memory & Planning (Advanced Agents)

Agents can use memory to store conversation history or past observations.

- **Short-term memory**: Limited context window (chat context)
- **Long-term memory**: Vector store or database
- **Planning**: Create multi-step strategies (e.g., AutoGPT)

```
# Pseudocode for a planning agent
goal = "Book a flight and hotel to Paris"
plan = planner.decompose(goal)
for step in plan:
    agent.execute(step)
```

---

## 8. Frameworks for Building AI Agents

- **LangChain** – Chains of LLMs, tools, memory, and agents
- **AutoGen by Microsoft** – Agents collaborating via LLMs
- **Haystack** – Conversational pipelines with retrievers
- **CrewAI** – Define multi-agent collaborations
- **AgentLite / Semantic Kernel** – Lightweight frameworks for agent workflows

---

## 📚 Further Resources

- [LangChain Documentation](https://docs.langchain.com/)
- [AutoGen by Microsoft](https://github.com/microsoft/autogen)
- [AutoGPT](https://github.com/Torantulino/Auto-GPT)
- [ReAct: Reasoning + Acting in Language Models](https://arxiv.org/abs/2210.03629)
- [OpenAI Agents Cookbook](https://github.com/openai/openai-cookbook)

---
