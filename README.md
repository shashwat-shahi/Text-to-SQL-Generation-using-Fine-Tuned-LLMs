# Text-to-SQL Generation using Fine-Tuned LLMs

This project explores parameter-efficient fine-tuning of large language models (LLMs) for text-to-SQL generation using the Gretel Synthetic Text-to-SQL dataset. Two models are fine-tuned using different techniques: Llama-3.2-1B with LoRA and Mistral-7B-Instruct-v0.3 with QLoRA.

## Overview

Converting natural language questions into executable SQL queries is a critical problem for database accessibility. This project demonstrates how to fine-tune LLMs to generate accurate SQL queries from natural language inputs with minimal computational resources using parameter-efficient techniques.

## Models and Techniques

- **Models**:
  - Meta's Llama-3.2-1B: Fine-tuned with LoRA
  - Mistralai's Mistral-7B-Instruct-v0.3: Fine-tuned with QLoRA (4-bit quantization)

- **Parameter Efficiency**:
  - Only 0.1-1% of total parameters trained
  - Significant reduction in memory requirements
  - Enables fine-tuning of large models on consumer hardware

## Dataset

- **Dataset**: [Gretel Synthetic Text-to-SQL](https://huggingface.co/datasets/gretelai/synthetic_text_to_sql)
- **Size**: 
  - 100,000 training examples
  - 5,851 test examples
- **Features**: 
  - Multiple SQL complexity levels
  - Diverse domain coverage
  - Various SQL task types (SELECT, JOIN, WHERE, GROUP BY, etc.)

## Training Setup

- **Hyperparameters**:
  - Learning rate: 3e-4
  - Batch size: 4
  - Epochs: 3
  - Gradient accumulation: 16
  - Warm-up steps: 100
  - Weight decay: 0.01

- **LoRA/QLoRA Configuration**:
  - Rank (r): 16
  - Alpha: 32
  - Target modules: q_proj, v_proj
  - LoRA dropout: 0.05

## Results

Performance comparison between base models and fine-tuned models:

| Model | Base Model | Fine-tuned Model |
|-------|------------|------------------|
| **Llama-3.2-1B** | Exact Match: 0.0200<br>Similarity: 0.1315 | Exact Match: 0.3200<br>Similarity: 0.4808 |
| **Mistral-7B-Instruct-v0.3** | Exact Match: 0.0600<br>Similarity: 0.1449 | Exact Match: 0.3200<br>Similarity: 0.5246 |

The fine-tuning process resulted in significant improvements:
- 16x improvement in exact match accuracy for Llama-3.2-1B
- 5.3x improvement in exact match accuracy for Mistral-7B-Instruct-v0.3
- Substantial increases in character-level similarity

## Demo

The project includes a Gradio-based UI for interactive SQL generation, allowing:
- Input of custom database schemas
- Natural language queries
- Selection between the two fine-tuned models
- Display of generated SQL and explanations

## Requirements

- Python 3.8+
- PyTorch
- Transformers (Hugging Face)
- PEFT (Parameter-Efficient Fine-Tuning)
- Datasets
- Gradio (for UI)
- Accelerate (for distributed training)
- bitsandbytes (for quantization)

## Usage

### Setup Environment
```bash
pip install datasets evaluate gradio huggingface_hub peft bitsandbytes transformers accelerate
```

### Fine-tuning Models
```bash
python fine_tune.py --model llama-3.2-1b --technique lora --output_dir ./llama3_sql_model
```


## Future Work

- Implement additional fine-tuning techniques (Prefix Tuning, P-Tuning)
- Explore SQL query optimization and error correction
- Improve schema understanding for complex database relationships
- Create a pipeline for direct database connection and query execution
- Extend to multilingual query support
