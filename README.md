# Synthetic-LSC Pipeline  
*Synthetic datasets for dimensions of Lexical Semantic Change generated using the LSC-Eval framework*

![ACL Findings](https://img.shields.io/badge/ACL%20Findings-Accepted-blueviolet)

---

## Summary

**Aim**: This repository contains the scripts and output files used to generate **diachronic synthetic datasets** for evaluating lexical semantic change (LSC) along specific dimensions. It accompanies the paper introducing the **three-stage evaluation framework, LSC-Eval**:  
📄 https://arxiv.org/abs/2503.08042

**Acknowledgement**: This pipeline was developed by [Naomi Baes](https://naomibaes.github.io/) and [Raphael Merx](https://www.rapha.dev/) under the guidance of Haim Dubossarsky, Ekaterina Vylomova, and Nick Haslam.

**Application**: The current release focuses on the **psychology domain**, providing synthetic datasets for six target concepts manipulated along the **SIB dimensions**:  
- **S**entiment  
- **I**ntensity  
- **B**readth  

📄 Dataset details: [Information Sheet](https://github.com/naomibaes/Synthetic-LSC_pipeline/blob/main/information_sheet_synthetic_SIB_psychology.md)

---

## 📦 Function and Use Cases

These diachronic synthetic datasets allow researchers to evaluate whether LSC detection methods or models are **sensitive to specific semantic dimensions**, rather than treating LSC as a unitary phenomenon.

- Datasets simulate **gradual semantic change** across 5-year periods for each target term.
- Each sample contains up to **1,500 natural sentences** per epoch, which are synthetically manipulated to reflect changes along a targeted dimension.
- Methods can be tested for their ability to **detect varying levels of synthetic change**, allowing for granular benchmarking.

While this pipeline currently targets Sentiment, Intensity, and Breadth, **LSC-Eval** is designed to be extensible to:
- Other types of change (e.g., generalization, specialization)
- Other dimensions (e.g., **Relation**, such as metaphor/metonymy)

---

## Few-Shot Demonstration Examples (In-Context Learning)

Each LLM-based generation task is seeded using few-shot examples crafted by a psychology scholar:

- Path:  
  `domain/[domain]/[dimension]/input/[dimension]_example_sentences.xlsx`  
- Example:  
  `domain/psychology/1_sentiment/input/sentiment_example_sentences.xlsx`

---

## Output Structure

- **5-year samples (synthetic sentences)**:  
  `domain/[domain]/[dimension]/output/5-year/`

- **Merged across all years**:  
  `domain/[domain]/[dimension]/output/all-year/`

---

## Synthetic Dimension Generation Procedures

### 🟣 Sentiment & Intensity

1. **Neutral sentence selection**:  
   - Based on mean valence/arousal scores from the NRC-VAD lexicon (Mohammad, 2018), filtered around a neutral mid-range.

2. **Few-shot prompt crafting**:  
   - Five diverse demonstration examples per target, written by a domain expert.

3. **Prompt refinement**:  
   - Optimized on 10 pilot sentences per target.

4. **Inference**:  
   - GPT-4o generates low/high Sentiment or Intensity versions via the OpenAI API.

5. **Post-processing**:  
   - Some outputs are manually filtered (e.g., due to failure to preserve targets: ~0.25% for Sentiment, ~0.01% for Intensity).  
   - Adjust temperature to optimize creativity vs. fidelity.

Figures in the paper illustrate the breakdown of sampled neutral sentences and success rates.

---

### 🟡 Breadth

Simulated using a **donor context replacement strategy** (adapted from Dubossarsky et al., 2019):

1. Extract co-hyponyms/sibling terms via WordNet 3.0.
2. Filter donor terms based on:
   - Psychological relevance
   - Lin similarity (≥ 0.5)
   - Cosine similarity with BioBERT embeddings (≥ 0.7)
3. Replace sibling terms with the target to simulate **semantic broadening**.
4. Sample evenly using a round-robin approach.

Each 5-year epoch contains up to 1,500 breadth-augmented sentences per injection level.

---

## Domain and Targets

- **Domain**: Academic Psychology (abstracts of scientific articles)
- **Targets**: `abuse`, `anxiety`, `depression`, `mental health`, `mental illness`, `trauma`

Corpus used: [psychology_corpus](https://github.com/naomibaes/psychology_corpus)

Full dataset summary: [Information Sheet](https://github.com/naomibaes/Synthetic-LSC_pipeline/blob/main/information_sheet_synthetic_SIB_psychology.md)

---

## Getting Started

1. Create a virtual environment:
   ```bash
   python -m venv .venv
   ```

2. Activate the environment and install dependencies:
   ```bash
   # Mac/Linux
   source .venv/bin/activate

   # Windows
   .venv\Scripts\activate

   pip install -r requirements.txt
   ```

3. Create a `.env` file with your OpenAI API key:
   ```
   OPENAI_API_KEY=your_key_here
   ```

4. Launch a notebook (e.g.):
   ```bash
   jupyter notebook produce_variations.ipynb
   ```

---

## 📁 File Paths

| Dimension  | Path                                                                 |
|------------|----------------------------------------------------------------------|
| Sentiment  | [`domain/psychology/1_sentiment/output/all-year`](https://github.com/naomibaes/Synthetic-LSC_pipeline/tree/main/domain/psychology/1_sentiment/output/all-year) |
| Intensity  | [`domain/psychology/2_intensity/output/all-year`](https://github.com/naomibaes/Synthetic-LSC_pipeline/tree/main/domain/psychology/2_intensity/output/all-year) |
| Breadth    | [`domain/psychology/3_breadth/output/unique_all-year`](https://github.com/naomibaes/Synthetic-LSC_pipeline/tree/main/domain/psychology/3_breadth/output/unique_all-year) |

---

**Please cite the work as follows:**  
```
@inproceedings{meng-etal-2025-what,
    title = "What is Stigma Attributed to? A Theory-Grounded, Expert-Annotated Interview Corpus for Demystifying Mental-Health Stigma",
    author = "Meng, Han  and
      Chen, Yancan  and
      Li, Yunan  and
      Yang, Yitian  and
      Lee, Jungup  and
      Zhang, Renwen  and
      Lee, Yi-Chieh",
    booktitle = "Proceedings of the 63rd Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)",
    month = jul,
    year = "2025",
    address = "Vienna, Austria",
    publisher = "Association for Computational Linguistics"
}
```
