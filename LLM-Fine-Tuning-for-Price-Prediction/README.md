# Product Price Prediction using LLMs

This project explores various machine learning and deep learning techniques to predict the price of a product based on its text description. The goal is to compare traditional ML models, zero-shot performance of frontier LLMs, and the effectiveness of fine-tuning a frontier model for a specific regression task.

The entire workflow, from data collection and cleaning to model evaluation, is documented in the Jupyter notebooks.

## Key Steps & Methodologies

*   **Data Exploration & Curation:** The project starts by exploring the "Amazon Reviews 2023" dataset from the McAuley Lab. Raw data is cleaned, processed, and curated into a balanced dataset of 400,000 products across 8 categories.
*   **Baseline Model Benchmarking:** Traditional machine learning models (Linear Regression, SVM, Random Forest) are trained using `scikit-learn` and `gensim` to establish a performance baseline.
*   **Frontier Model Evaluation:** The zero-shot performance of leading frontier models like GPT-4o and Claude 3.5 Sonnet is evaluated on the price prediction task.
*   **Fine-Tuning Experiment:** OpenAI's `gpt-4o-mini` is fine-tuned with a small subset of the curated data to test if this specialization improves performance on this regression task.

## Setup and Installation

### Prerequisites
*   Python 3.10+
*   Conda (or another virtual environment manager like `venv`)

### 1. Clone the Repository
```bash
git clone <your-repo-url>
cd product-price-prediction