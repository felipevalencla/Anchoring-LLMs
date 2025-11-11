# Anchors in the Machine: Behavioral and Attributional Evidence of Anchoring Bias in Large Language Models

[![arXiv](https://img.shields.io/badge/arXiv-Placeholder-red)](https://arxiv.org/abs/2511.05766)

This repository contains the code, datasets, and replication notebooks for the preprint [*Anchors in the Machine: Behavioral and Attributional Evidence of Anchoring Bias in LLMs*](https://arxiv.org/abs/2511.05766).
It replicates and extends Tversky & Kahneman’s classic anchoring experiment across six open-source LLMs.

---

## Contents

- **`anchoring_bias_code.ipynb`**
  Main replication notebook with a minimal setup (defaults to GPT-2).
  Includes:
  - Log-probability scoring of numeric targets (`0..100%`)
  - Exact Shapley attribution over structured prompt fields: `scene`, `anchor`, `comparative`, `absolute`
  - Example workflow for switching to other models

- **`requirements.txt`**
  Environment dependencies (heavy due to `transformers`, `CUDA`, etc.).

- **`Data/Simulation/`**
  - Standard anchor simulation results
  - Different anchors simulation results
  - *Note:* V0 replication is repeated in both datasets.

- **`Data/Stats/`**
  - Statistical summaries for each simulation and variation (standard and different anchors).
  - *Note:* V0 replication is repeated in both datasets.

- **`Data/ABSS/`**
  Anchoring Bias Sensitivity Score (ABSS) results:
  - Per model
  - Per variation
  - Includes generated plots.

---

## Replication Instructions

1. **Set up environment**
   ```bash
   git clone https://github.com/felipevalencla/Anchoring-LLMs.git
   ```
   ```bash
   pip install -r requirements.txt
   ```

2. **Run the notebook**
   Open `anchoring_bias_code.ipynb` in Jupyter or VS Code.
   The default replication runs on **GPT-2**, but you can change `MODEL_NAME` to any of the following:

   - `EleutherAI/gpt-neo-125M`
   - `tiiuae/falcon-rw-1b`
   - `microsoft/phi-2`
   - `google/gemma-2b` *(gated; requires Hugging Face token)*
   - `meta-llama/Llama-2-7b-hf` *(gated; requires Hugging Face token)*

---

## Hardware & VRAM Guidance

- **Lightweight models**
  GPT-2, GPT-Neo-125M → run on CPU or modest GPUs (<1 GB VRAM).
- **Mid-size models**
  Falcon-RW-1B, Phi-2, Gemma-2B → require ~6–10 GB VRAM (FP16).
- **Large models**
  LLaMA-2-7B → requires ≥14–20 GB VRAM (FP16).

If your GPU does not meet these requirements:
- Use **8-bit/4-bit quantization*** (`BitsAndBytesConfig`, `device_map="auto"`) to fit large models.
- Or run on a **cloud GPU** (A10, A100, etc.).

**Replication with quantization/cloud inference may yield *small numerical differences* in log-probabilities and Shapley values.*

---

## Gated Model Setup

To use Gemma-2B or LLaMA-2-7B, request access on Hugging Face and set your token:

```bash
export HUGGINGFACE_TOKEN=hf_xxxxxxxxxxxxx
```

## Results

- **Behavioral analysis:** log-prob evidence that anchors shift full output distributions
- **Attributional analysis:** Shapley values showing anchors influence reweighting
- **Unified metric:** Anchoring Bias Sensitivity Score (ABSS) integrating both views

## Discussion and Conclusion

- Anchoring bias is a **robust and reproducible property** of multiple open-source LLMs, with stronger effects in larger models (Gemma-2B, Phi-2, LLaMA-2-7B).
- Shapley-based attribution provides **deterministic evidence** that anchor field influence model scoring, complementing behavioral distribution shifts.  
- The Anchoring Bias Sensitivity Score (ABSS) integrates behavioral and attributional evidence, offering a **framework for systematic bias measurement**.
- Attributional fragility across prompt variations indicates that LLMs **cannot be treated as substitutes for human subjects**, even when biases appear human-like.
- For LLMs as decision systems, anchoring highlights risks: **arbitrary cues can systematically shift outputs**, raising governance and safety concerns.  
- Mitigation strategies (e.g., filtering, chain-of-thought prompting) require further research for **generalization across models**.
- Extensions include benchmarking across instruction-tuned and frontier models, **integrating attributional, concept-based, and mechanistic methods**, and mapping a taxonomy of cognitive biases in LLMs.
- Overall, attributional approaches serve as a **middle ground between behavioral observation and causal interpretability**, bridging behavioral science and AI safety research.


Full details in the [preprint](https://arxiv.org/abs/2511.05766).

