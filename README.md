# 🚀 AI Code Review Agent  
### _A Transformer-based Automated Reviewer for GitHub Pull Requests_

The **AI Code Review Agent** is a deep-learning system designed to automatically review GitHub pull requests using a **custom dataset**, **adaptive diff extraction**, and a **fine-tuned FLAN-T5 model**.  
It generates concise, structured review suggestions following this format:

Suggested Review:
<rewritten comment>

Reasoning:
<brief explanation>

Fix:
Replace these lines:
<old code>

With this:
<new code>

---

## ✨ Features

- 🔍 **Diff-aware review generation** (changed-line detection + context extraction)  
- 🧩 **Adaptive hunk grouping** (3–7 lines, stops at next `@@`)  
- ✏️ **Consistent PR-review output structure**  
- 🛠️ **Fine-tuned FLAN-T5 Small** using custom instruction format  
- 📦 **Notebook-driven training pipeline** (local + Kaggle-compatible)  
- 🔧 **Supports `.keras` model saving**  
- 🚦 **Beginner-friendly modular workflow**  

---

## 📁 Project Structure

```text
C:\Users\yogan\OneDrive\Desktop\ai-code-reviewer
│
├── .ipynb_checkpoints/
│   Auto-generated Jupyter metadata
│
├── 1.data Loading.ipynb
│   Raw dataset loading and streaming reader
│
├── 2.data wrangling.ipynb
│   Cleaning, hunk parsing, change detection
│
├── 3.formatting.ipynb
│   Prompt construction and target formatting
│
├── 4.train_val_test_split.ipynb
│   Adaptive splitting and sanity checks
│
├── 5.tokenization_padding_batch_testing.ipynb
│   Tokenizer, padding, collate_fn batch validation
│
├── code-review-model-training (kaggle_trained).ipynb
│   Kaggle GPU/TPU training version
│
├── code-review-model-training(full_scale_local_run).ipynb
│   Local full-scale training loop
│
├── local_model_testing.ipynb
│   Inference tests using saved .keras model
│
├── app/
│   (Future) PR automation endpoint/scripts
│
├── model_download_v3/
│   Saved trained model (.keras)
│
├── venv/
│   Virtual environment
│
├── README.md
│
└── (All dataset files excluded intentionally)

```
## 🧠 Model Architecture

- **Backbone:** FLAN-T5 Small  
- **Type:** Encoder–Decoder (seq2seq)  
- **Training Objective:** Next-token prediction over structured review format  
- **Input:**  
  - Changed code lines  
  - Small context window (before + after lines)  
  - System prompt defining output structure  
- **Output:**  
  - Suggested Review  
  - Reasoning  
  - Fix instructions with minimal diff replacement  

---

## 🏋️ Training Workflow

### 1️⃣ Data Preparation  
Performed across Notebooks 1–4:  
- Stream JSONL → extract PR diffs  
- Skip extremely large hunks  
- Group lines adaptively  
- Build model-ready prompt/label pairs  
- Split into train/val/test

### 2️⃣ Tokenization  
- SentencePiece tokenizer (T5 default)  
- Dynamic padding  
- Masking labels as `-100` for loss to avoid NaN issues  

### 3️⃣ Training  
- Runs on **GPU** (P100) or **TPU v5e-8** (Kaggle)  
- `.keras` checkpointing  
- Gradient clipping, LR warmup, AMP options  

### 4️⃣ Testing  
- Local inference notebook  
- Ensure structure formatting correctness  

---

## 📦 Installation

```bash
git clone https://github.com/<your-username>/ai-code-reviewer
cd ai-code-reviewer
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
▶️ How to Run Training
bash
Copy code
# Launch Jupyter
jupyter notebook
Open one of the training notebooks:

code-review-model-training (kaggle_trained).ipynb

code-review-model-training(full_scale_local_run).ipynb
```
## 🔮 Roadmap
 Real-time PR integration via GitHub Webhooks

 Lightweight FastAPI endpoint for review generation

 Frontend UI for visualizing code diffs

 Expand training to multi-language diff datasets

 Add rule-based fallback reviewer

### 📝 License
This project is open-source under the MIT License.

### 💬 Contact
If you want help expanding the repo, optimizing training, or deploying to production, feel free to reach out!
