# Hugging Face Upskill (`huggingface/upskill`)

[Hugging Face Upskill](https://github.com/huggingface/upskill) is an open-source tool developed by Hugging Face to automatically generate, refine, distillation-train, and evaluate **Agent Skills** (`SKILL.md` + scripts) for coding agents.

---

## Core Concept: Teacher-to-Student Skill Distillation

Upskill captures the execution trace of a powerful "teacher" model (e.g. Claude Opus or GPT-4o) completing a complex domain task, distills the reasoning, edge-case handlers, and validation rules into a structured skill file (`SKILL.md`), and tests it to ensure smaller "student" models (e.g. Claude Haiku, Llama, Qwen) can execute the exact same task reliably.

```text
 ┌─────────────────────────┐
 │      TEACHER MODEL      │ (Runs complex task & generates execution trace)
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │     UPSKILL ENGINE      │ (Extracts rules, scripts, & builds SKILL.md)
 └────────────┬────────────┘
              │
              ▼
 ┌─────────────────────────┐
 │      STUDENT MODEL      │ (Executes complex task reliably using SKILL.md)
 └─────────────────────────┘
```

---

## Key Features

- **Automated Skill Generation:** Distills raw agent execution logs into standardized Agent Skills.
- **Skill Benchmarking & Evaluation:** Measures student model performance with vs. without the generated skill.
- **Companion Ecosystem (`huggingface/skills`):** Marketplace repository containing pre-built, community-contributed skills (e.g., `hf-cli`, `datasets`, `transformers`).
- **Compatible Formats:** Outputs standard `SKILL.md` files with YAML frontmatter, markdown instructions, and helper scripts.

---

## Installation & CLI Usage

```bash
# Install via uv / pip
uv pip install upskill

# Generate a skill from an execution trace
upskill generate --trace ./trace.json --output ./skills/my-skill/

# Evaluate a skill against a target model
upskill eval --skill ./skills/my-skill/ --model haiku
```

---

## Resources

- **GitHub Repository:** [huggingface/upskill](https://github.com/huggingface/upskill)
- **Skills Library:** [huggingface/skills](https://github.com/huggingface/skills)
- **License:** Apache 2.0 / Open Source
