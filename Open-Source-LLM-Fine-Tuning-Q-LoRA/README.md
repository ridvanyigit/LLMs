# Open-Source LLM Fine-Tuning with Q-LoRA

This project demonstrates the process of fine-tuning an open-source Large Language Model (Llama 3.1 8B) for a specific regression task: predicting product prices from text descriptions. The key objective is to show how a moderately sized open-source model can be specialized to outperform its baseline and become competitive with larger, general-purpose frontier models using efficient fine-tuning techniques like Q-LoRA.

## Key Technologies & Concepts

*   **Model:** `meta-llama/Meta-Llama-3.1-8B`
*   **Efficient Fine-Tuning:** Q-LoRA (Quantized Low-Rank Adaptation) to train the model on a single consumer-grade GPU (e.g., Google Colab T4).
*   **Frameworks:** Hugging Face `transformers`, `peft` (Parameter-Efficient Fine-Tuning), `bitsandbytes` (for quantization), and `trl` (Transformer Reinforcement Learning for the SFTTrainer).
*   **Monitoring:** `wandb` (Weights & Biases) for real-time tracking of training metrics and loss.

## Setup and Installation

### 1. Clone the Main Repository
This project should be placed inside the main `LLMs` repository.
```bash
git clone <your-repo-url>
cd LLMs