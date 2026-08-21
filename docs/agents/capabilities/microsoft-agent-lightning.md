# Agent Lightning (`microsoft/agent-lightning`)

[Agent Lightning](https://github.com/microsoft/agent-lightning) (by Microsoft Research) is an open-source framework for training and optimizing AI agents using Reinforcement Learning (RL) and Automatic Prompt Optimization (APO) with zero to minimal modifications to the agent's code.

---

## Core Philosophy: Decoupled Agent Training

Traditional RL or prompt optimization requires tightly coupling the agent loop with training code. Agent Lightning decouples execution from training:

```text
 ┌─────────────────────────┐
 │   AGENT EXECUTION LOOP  │ (LangChain, AutoGen, CrewAI, Custom Python)
 └────────────┬────────────┘
              │  (Captures States, Actions, Rewards, & Traces)
              ▼
 ┌─────────────────────────┐
 │  AGENT LIGHTNING TRAINER│ (RL, Direct Preference Optimization, APO)
 └────────────┬────────────┘
              │  (Updates Policy / Prompt / Weights)
              ▼
 ┌─────────────────────────┐
 │    OPTIMIZED AGENT      │
 └─────────────────────────┘
```

---

## Key Features

- **Framework Agnostic:** Seamlessly hooks into LangChain, AutoGen, CrewAI, Semantic Kernel, and custom Python agents.
- **Harnessed Agentic RL:** Enables efficient RL training (e.g. single-GPU training) for multi-step agentic decision making.
- **Automatic Prompt Optimization (APO):** Refines system prompts based on empirical rewards and task pass/fail feedback.
- **Zero-Invasive Hooking:** Monitors execution traces without breaking the agent's existing codebase architecture.

---

## Installation & Basic Usage

```bash
pip install agent-lightning
```

```python
from agent_lightning import AgentLightningTrainer

# 1. Initialize trainer with target agent runner
trainer = AgentLightningTrainer(
    agent=my_custom_agent,
    reward_fn=evaluate_task_output
)

# 2. Run RL / APO optimization loop
trainer.fit(dataset=training_tasks, epochs=3)
```

---

## Resources

- **GitHub Repository:** [microsoft/agent-lightning](https://github.com/microsoft/agent-lightning)
- **Documentation:** [microsoft.github.io/agent-lightning](https://microsoft.github.io/agent-lightning/)
- **License:** MIT License / Open Source
