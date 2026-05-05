# sources.md

## Primary Sources

### 1. PagedAttention — KV Cache Memory Management

**Title:** Efficient Memory Management for Large Language Model Serving with PagedAttention  
**Authors:** Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng, Cody Hao Yu, Joseph E. Gonzalez, Hao Zhang, Ion Stoica  
**Institutions:** UC Berkeley, Stanford University, UC San Diego  
**Published:** SOSP '23 (October 23–26, 2023, Koblenz, Germany)  
**DOI:** https://doi.org/10.1145/3600006.3613165  
**PDF:** https://arxiv.org/pdf/2309.06180  

**What it contributes to the explainer:**  
This paper is the primary source for the KV cache cost compounding mechanism. The key finding is that existing LLM serving systems store KV cache in contiguous memory, which causes internal and external fragmentation — with only 20.4%–38.2% of KV cache memory actually storing token states in existing systems. The paper demonstrates that KV cache is the dominant memory consumer during inference (close to 30% of GPU memory on a 13B-parameter model on A100), and that its growth is dynamic and unpredictable. PagedAttention solves this by treating KV cache like OS virtual memory — blocks instead of contiguous allocation. The paper's throughput results (2–4× improvement over FasterTransformer and Orca) confirm that KV cache management is a primary lever on inference cost, not just memory efficiency.

**Specific claims supported:**
- KV cache grows dynamically with sequence length during decoding
- Larger KV cache forces each output token to attend over a larger memory footprint, increasing per-token compute cost
- The sequential autoregressive nature of decoding is what makes output tokens expensive — each token depends on all prior tokens in the cache

---

### 2. Green AI — Prefill vs Decode Energy Decomposition

**Title:** Towards Green AI: Decoding the Energy of LLM Inference in Software Development  
**Authors:** Lola Solovyeva, Fernando Castor  
**Institution:** University of Twente, Enschede, the Netherlands  
**Published:** arXiv preprint, February 5, 2026  
**arXiv:** https://arxiv.org/abs/2602.05712  

**What it contributes to the explainer:**  
This is the direct empirical source for the numerical claims in the explainer. The paper conducts a phase-level energy analysis distinguishing prefill from decoding across ten transformer models (six 6B–7B and four 3B–4B). Key findings:

- Increases in prefill cost amplify the energy cost per token during decoding, with amplification ranging from **1.3% to 51.8%** depending on the model
- Three out of ten models demonstrated **babbling behavior** — generating excessive tokens after task completion with no accuracy gain
- Implementing babbling suppression achieved energy savings of **44% to 89%** without affecting generation accuracy
- Decoding dominates energy consumption; prefill costs influence decoding costs but are not the primary driver

**Specific claims supported:**
- The 1.3%–51.8% per-token cost amplification figure used in the explainer
- The babbling suppression 44%–89% energy savings figure
- The framing that decoding dominates total inference cost, and that prefill costs have a secondary amplifying effect on decode costs

---

## Tool / System Used

**vLLM** — the open-source LLM serving system built on PagedAttention, referenced as the production implementation of the memory management principles described in the Kwon et al. paper.  
GitHub: https://github.com/vllm-project/vllm

---

## Follow-On Reading (for deeper understanding)

- **Orca** (Yu et al., 2022) — the iteration-level scheduling system that PagedAttention improves upon; useful for understanding why batching is hard without paged memory
- **FasterTransformer** (NVIDIA) — the other baseline system in the PagedAttention evaluation
- **Pope et al. (2022)** — "Efficiently Scaling Transformer Inference" — formalizes the prefill/decode compute distinction that underlies the pricing asymmetry
