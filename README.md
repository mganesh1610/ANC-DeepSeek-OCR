# DeepSeek-OCR — Document Understanding via Context Optical Compression

This repo documents how I used **DeepSeek-OCR** in **Google Colab** to:
- Convert **document images → Markdown**
- Run **“Deep Parsing”** for figure-level understanding
- Save structured outputs (`.mmd/.md`) and render them in Colab (plus a repo-style “Deep Parsing” card image)

---

## 🔗 Links
- **Model + inference examples (GitHub):** https://github.com/deepseek-ai/DeepSeek-OCR  
- **Paper (arXiv):** https://arxiv.org/abs/2510.18234  

---

## 🧩 What I Used (Core Idea)
DeepSeek-OCR combines:
- **DeepEncoder** — compresses long contexts into high-resolution visual representations (controlled activations)
- **DeepSeek3B-MoE-A570M decoder** — decodes OCR / document understanding efficiently  
(Source: paper) https://arxiv.org/abs/2510.18234

### 📌 Reported highlights (paper abstract)
- **97% OCR precision** when compression ratio is **< 10×**
- **~60% accuracy** even at **20× compression**
- On **OmniDocBench**, it **surpasses GOT-OCR2.0** using **~100 vision tokens**, and can outperform MinerU2.0 while using **< 800 vision tokens**
- Production throughput claim: **200k+ pages/day on a single A100-40GB**  
(Source: paper) https://arxiv.org/abs/2510.18234

---

## 🛠️ Implementation Summary (What I ran in Colab)
I followed the official HuggingFace/Transformers-style workflow and prompt patterns from the repo.

### ✅ Tasks executed
1. **Document → Markdown conversion**
   - Prompt style:
     ```text
     <image>
     <|grounding|>Convert the document to markdown.
     ```
   - (Repo reference) https://github.com/deepseek-ai/DeepSeek-OCR

2. **Figure “Deep Parsing”**
   - Prompt style:
     ```text
     <image>
     Parse the figure.
     ```
   - (Repo reference) https://github.com/deepseek-ai/DeepSeek-OCR

### ✅ Outputs generated
- The model writes results into an **output directory** (e.g., `outputs/`)
- Outputs are typically saved as **`.mmd` / `.md`** depending on the mode
- I rendered results inside Colab and also created a **“Deep Parsing” card** image to match the repo/paper demo style

---

## 🧪 Engineering Takeaways (Real-world)
### ⚙️ Environment + dependency control
- Pinned compatible versions to match repo expectations (Transformers + Tokenizers alignment)
- Used `_attn_implementation="eager"` when needed to avoid flash-attn / toolkit incompatibilities (common stability fix for research repos)

### 🧯 Runtime stability lessons (Colab)
- **CPU runs are impractical** for this workload (multi-GB model + heavy inference)
- GPU inference can sometimes trigger CUDA asserts → CUDA context may become unstable and require runtime reset
- Practical workflow: **A100 runtime**, controlled image sizes, and stable dependency versions

---

## 📈 Why this matters (Business / Real-world impact)
DeepSeek-OCR enables **searchable + structured document understanding at scale**, useful for:

### 💼 High-value use cases
- 📄 Enterprise ingestion: contracts, SOPs, invoices, claims, compliance PDFs
- 🏥 Healthcare/biomed: lab reports, clinical forms, scanned records
- 💰 Finance: statements + tabular docs (token cost becomes bottleneck)
- 🏗️ Operations: manuals, work orders, audit reports

### 📊 Metrics to track in production
- **Token savings:** reported **7×–20× reduction** via optical compression (depending on stage)  
  (Coverage) https://www.tomshardware.com/tech-industry/artificial-intelligence/new-deepseek-model-drastically-reduces-resource-usage-by-converting-text-and-documents-into-images-vision-text-compression-uses-up-to-20-times-fewer-tokens
- **Throughput:** up to **200k+ pages/day per A100-40GB** (paper claim) https://arxiv.org/abs/2510.18234
- **Quality vs compression:** e.g., **97% at <10×** (paper claim) https://arxiv.org/abs/2510.18234

---



Example embed (replace paths):
```md
![Model download logs](assets/model_download.png)
![Document to Markdown output](assets/doc_to_md.png)
![Deep Parsing card](assets/deep_parsing_card.png)
![CUDA debug](assets/cuda_debug.png)
