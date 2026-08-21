# nanochat (`karpathy/nanochat`)

[nanochat](https://github.com/karpathy/nanochat) is an open-source, full-stack educational harness developed by Andrej Karpathy to train a ChatGPT-style LLM from scratch on a single GPU node (the "best ChatGPT that $100 can buy").

It succeeds Karpathy's previous `nanoGPT` repository and serves as the capstone codebase for Eureka Labs' `LLM101n` course.

---

## Core Pipeline Stages

nanochat covers the complete end-to-end LLM lifecycle in clean PyTorch (~8,500 lines of code):

1. **Tokenization:** Custom Byte-Pair Encoding (BPE) tokenizer training and vocabulary compilation.
2. **Pretraining:** Distributed single-node pretraining using modern tricks (Flash Attention 3, FP8 mixed-precision, Muon optimizer).
3. **Supervised Fine-Tuning (SFT):** Instruction tuning on conversation datasets for chat alignment.
4. **Reinforcement Learning (RL):** Post-training reasoning alignment using Group Relative Policy Optimization (GRPO).
5. **Evaluation & Web UI:** Automated benchmark evaluation and lightweight web inference server.

```text
 ┌──────────────┐     ┌──────────────┐     ┌──────────────┐     ┌──────────────┐
 │ TOKENIZATION │ ──> │ PRETRAINING  │ ──> │     SFT      │ ──> │   RL (GRPO)  │
 └──────────────┘     └──────────────┘     └──────────────┘     └──────────────┘
                                                                        │
                                                                        ▼
                                                             ┌──────────────────┐
                                                             │ INFERENCE & EVAL │
                                                             └──────────────────┘
```

---

## Key Improvements Over nanoGPT

- **Full Stack:** Unlike nanoGPT (which focused primarily on pretraining), nanochat includes SFT, GRPO reinforcement learning, evaluation benchmarks, and web serving.
- **Modern Optimization:** Features the **Muon optimizer**, Flash Attention 3, RoPE, SwiGLU activations, and FP8 training.
- **Foundation for Autoresearch:** Used as the lightweight, single-file model backbone powering Karpathy's `autoresearch` project.

---

## Quickstart

```bash
# Clone repository
git clone https://github.com/karpathy/nanochat.git
cd nanochat

# Install dependencies (PyTorch 2.x, FlashAttention)
pip install -r requirements.txt

# Run single-node training pipeline
python train.py
```

---

## Resources

- **GitHub Repository:** [karpathy/nanochat](https://github.com/karpathy/nanochat)
- **Eureka Labs LLM101n:** Educational capstone project
- **License:** MIT License / Open Source
