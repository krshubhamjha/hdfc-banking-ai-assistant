
# Fine-Tuned Model – HDFC Banking AI Assistant

## 1. Base Model

- Model: Qwen2.5-1.5B-Instruct
- Hugging Face: https://huggingface.co/Qwen/Qwen2.5-1.5B-Instruct
- Fine-tuning approach: LoRA (Low-Rank Adaptation)
- Task: Causal Language Modeling

## 2. LoRA Configuration

The adapter configuration includes:

- Rank (r): 16
- LoRA Alpha: 32
- LoRA Dropout: 0.05
- Target Modules:
  - q_proj
  - k_proj
  - v_proj
  - o_proj
  - gate_proj
  - up_proj
  - down_proj

## 3. Model Files

The fine-tuned adapter is stored locally in:

```text
backend/models/hdfc_qlora_final/
```

The directory contains:

- adapter_config.json
- adapter_model.safetensors
- tokenizer_config.json
- tokenizer.json
- special_tokens_map.json
- added_tokens.json
- merges.txt
- vocab.json
- chat_template.jinja
- README.md

**Note:** Model weights and adapter files are excluded from GitHub because of their size.

## 4. Loading the Fine-Tuned Adapter

The adapter is loaded on top of the original Qwen base model using Hugging Face Transformers and PEFT.

Example:

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
from peft import PeftModel

base_model = "Qwen/Qwen2.5-1.5B-Instruct"
adapter_path = "backend/models/hdfc_qlora_final"

tokenizer = AutoTokenizer.from_pretrained(base_model)

model = AutoModelForCausalLM.from_pretrained(
    base_model,
    device_map="auto"
)

model = PeftModel.from_pretrained(
    model,
    adapter_path
)
```

## 5. Important Notes

- The base model must be downloaded from Hugging Face.
- The fine-tuned adapter must be available locally to load it using the example above.
- Adapter weights are not included in this GitHub repository.
- The model inference API is used by the FastAPI backend for intent prediction and answer generation.
- Do not commit API tokens, credentials, or private configuration files.
