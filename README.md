AI Code Review Agent

A transformer-based system that automatically generates high-quality code review comments from GitHub pull request diffs.
The project builds a clean training dataset from PR comments, formats them into a structured review prompt, and fine-tunes a FLAN-T5 model on GPU using a 300k-example subset for memory efficiency.

🚀 Project Overview

This project focuses on generating automated, human-like code review comments.
Due to memory constraints, the full dataset was downsampled to 300k curated examples for training.

Key Goals

Extract structured training pairs from GitHub PR diffs.

Fine-tune FLAN-T5-Small on GPU.

Generate structured review comments with reasoning + minimal patch fixes.

Maintain an efficient data pipeline suitable for consumer GPUs.

📦 Features Implemented
✅ Diff Parsing & Extraction

A custom streaming diff-parser that performs:

Changed-line detection

Adaptive grouping (3–7 lines)

1-line context before & after each changed block

Scoped extraction stopping at next @@ or limit

Indentation normalization only when irregular

Skipping large diff hunks for speed

✅ Final Prompt Format

Each training sample follows:

Suggested Review:
<rewritten PR comment>

Reasoning:
<brief explanation>

Fix:
Replace these lines:
<old>

With this:
<new>


Formatting adapts:

Single-line fixes → plain text

Multi-line fixes → code blocks

✅ Dataset Pipeline

Consists of several notebook stages:

Raw → cleaned comment files

Diff pairing and formatting

Train/val/test split

Tokenization, padding, batching

Batch-shape verification

Final training performed on 300k samples, not full dataset, to fit GPU memory.

✅ Training Pipeline (GPU)

HuggingFace Transformers + PyTorch

Custom collate_fn

Label masking via -100

Gradient clipping

AMP disabled when debugging NaN loss

Model saved in .keras format (preferred)

📁 Project Structure

Below is the uploaded repo structure, with large dataset files removed, and no environment, vector DB, or database folders listed.

ai-code-reviewer/
│
├── 1.data Loading.ipynb
├── 2.data wrangling.ipynb
├── 3.formatting.ipynb
├── 4.train_val_test_split.ipynb
├── 5.tokenization_padding_batch_testing.ipynb
│
├── code-review-model-training (kaggle_trained).ipynb
├── code-review-model-training(full_scale_local_run).ipynb
├── local_model_testing.ipynb
│
├── app/                     # App folder (UI/API for inference)
│
├── model_download_v3/       # Saved model weights/checkpoints
│
├── venv/                    # Local virtual environment
│
├── README.md                # This file
└── rev-rec-projects.pdf     # Research reference (kept small)


NOTE:
All huge .jsonl, .csv, .feather, and .zip dataset files were removed from this structure because they are too large for GitHub.

⚙️ Technologies Used

Python

PyTorch

HuggingFace Transformers (FLAN-T5)

Jupyter Notebooks

Virtualenv (not Conda)

GPU training (local + Kaggle)

🧪 Model Output Format

The trained model generates:

1️⃣ Suggested Review

A clearer version of the PR comment.

2️⃣ Reasoning

Why the change is needed.

3️⃣ Fix Patch

Minimal actionable diff:

Replace these lines:
<old>

With this:
<new>


Designed for easy integration with future GitHub automation.

🔮 Future Enhancements

Add retrieval-augmented context (vector DB)

Larger model variants (base/large)

Integrate GitHub webhooks for real-time PR review

Web dashboard for analytics

👤 Author

A developer building a real, production-grade ML portfolio with hands-on experimentation, GPU-based training, and structured dataset design.
