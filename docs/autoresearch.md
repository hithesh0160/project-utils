# Autoresearch (by Andrej Karpathy)

[Autoresearch](https://github.com/karpathy/autoresearch) is an open-source framework developed by Andrej Karpathy to automate the machine learning research process. It enables an AI coding agent to formulate hypotheses, modify training code, run training runs on GPUs, evaluate results, and keep or revert changes autonomously.

---

## Core Concept: The "Ratchet" Experiment Loop

Instead of a human manually writing code, launching runs, and waiting for metrics, Autoresearch runs a continuous autonomous loop:

```text
 ┌─────────────────────────────────────────────────────────┐
 │                     PROGRAM.MD                          │
 │ (System instructions & research objective for the agent)│
 └────────────────────────────┬────────────────────────────┘
                              │
                              ▼
 ┌─────────────────────────────────────────────────────────┐
 │                      TRAIN.PY                           │
 │ (Agent modifies code, architecture, or hyperparameters) │
 └────────────────────────────┬────────────────────────────┘
                              │
                              ▼
 ┌─────────────────────────────────────────────────────────┐
 │                   EXECUTE & EVALUATE                    │
 │  (Fixed time budget run e.g. 5-min, metric: val_bpb)   │
 └────────────────────────────┬────────────────────────────┘
                              │
             ┌────────────────┴────────────────┐
             │                                 │
             ▼                                 ▼
    Metric Improved                     Metric Worse
   (git commit & keep)              (git checkout & revert)
```

---

## Key Components

1. **`program.md`**: The primary control file where the human researcher specifies the overarching objective, constraints, and instructions. Karpathy describes this as "programming the research organization."
2. **`train.py`**: The central Python script containing the model architecture, training loop, and hyperparameters. The agent is allowed to edit this file arbitrarily.
3. **`prepare.py`**: A read-only evaluation and data-preprocessing script that ensures benchmark metrics (such as validation bits per byte, `val_bpb`) remain uncorrupted and standard across runs.

---

## Why Autoresearch Matters

- **Beyond Standard AutoML:** Unlike hyperparameter optimization frameworks (e.g. Optuna, Ray Tune) that search predefined ranges, Autoresearch allows LLM agents to invent new neural network components, loss functions, and optimization tricks by editing code directly.
- **Overnight Iteration:** Allows researchers to set an objective and let agents run dozens to hundreds of hypothesis-driven experiments overnight.
- **Spec-Driven Guidance:** Keeps AI agents grounded on concrete evaluation metrics (hard loss/metric verification) rather than conversational feedback.

---

## Resources

- **GitHub Repository:** [karpathy/autoresearch](https://github.com/karpathy/autoresearch)
- **License:** Open Source
