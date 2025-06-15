# AI Core Concepts (Part 6): Reinforcement Learning for Software Engineers

**Reinforcement Learning (RL)** is a type of machine learning where an agent learns to make decisions by interacting with an environment, receiving **rewards** or **penalties** for its actions. It’s used in robotics, game playing (e.g., AlphaGo), and autonomous systems.

---

## 1. Key Concepts in RL

- **Agent**: The learner or decision-maker.
- **Environment**: The world the agent interacts with.
- **Action**: Choices the agent can make.
- **State**: Current situation of the agent.
- **Reward**: Feedback signal after an action.
- **Policy**: Strategy that the agent uses to decide actions.
- **Episode**: A complete sequence of states, actions, and rewards.

**Goal**: Maximize cumulative long-term reward.

---

## 2. Example: Q-Learning (Tabular RL)

Q-learning is a value-based RL algorithm that learns a policy by estimating the **Q-values** (expected future rewards).

**Q-learning update rule:**

```
Q(s, a) ← Q(s, a) + α * [r + γ * max(Q(s', a')) - Q(s, a)]
```

Where:
- `α` is the learning rate
- `γ` is the discount factor
- `r` is the reward
- `s'` is the new state

**Simple Q-Learning Example using `gym`**
```
import gym
import numpy as np

env = gym.make("FrozenLake-v1", is_slippery=False)
q_table = np.zeros((env.observation_space.n, env.action_space.n))

alpha = 0.8    # learning rate
gamma = 0.95   # discount factor
epsilon = 0.1  # exploration rate

for episode in range(1000):
    state = env.reset()
    done = False

    while not done:
        if np.random.rand() < epsilon:
            action = env.action_space.sample()
        else:
            action = np.argmax(q_table[state])

        next_state, reward, done, truncated, info = env.step(action)

        old_value = q_table[state, action]
        future = np.max(q_table[next_state])
        q_table[state, action] = old_value + alpha * (reward + gamma * future - old_value)

        state = next_state

print("Trained Q-Table:\n", q_table)
```

---

## 3. Deep Reinforcement Learning

When state/action spaces are too large for a table, we use **Deep Q-Networks (DQN)** or **Policy Gradient methods** with neural networks.

**Example: DQN Components**
- Replay buffer to store experiences
- Target network for stable learning
- CNN or MLP for Q-function approximation

**Popular libraries**:
- `Stable Baselines3`
- `Ray RLlib`
- `OpenAI Gymnasium`

---

## 4. Common Algorithms

| Algorithm        | Type       | Description                                      |
|------------------|------------|--------------------------------------------------|
| Q-Learning       | Value-based| Learn Q-values for state-action pairs           |
| SARSA            | Value-based| Similar to Q-learning but on-policy             |
| DQN              | Value-based| Deep learning version of Q-learning             |
| REINFORCE        | Policy-based| Uses Monte Carlo policy gradient                |
| PPO (Proximal Policy Optimization) | Policy-based | Stable and widely used in practice            |
| A3C              | Actor-Critic | Parallelized training for faster learning       |

---

## 5. Real-World Applications

- Game AI (e.g., AlphaGo, OpenAI Five)
- Robotics (e.g., robot locomotion)
- Finance (portfolio optimization)
- Recommendation Systems (optimizing user engagement)
- Traffic signal control and autonomous vehicles

---

## 📚 Further Resources

- [Spinning Up in Deep RL by OpenAI](https://spinningup.openai.com/)
- [RL Course by David Silver (DeepMind)](https://www.davidsilver.uk/teaching/)
- [CleanRL (lightweight RL algorithms)](https://github.com/vwxyzjn/cleanrl)
- [Stable Baselines3 Docs](https://stable-baselines3.readthedocs.io/)

---
