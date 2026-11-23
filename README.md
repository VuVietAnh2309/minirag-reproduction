# Dissecting MiniRAG: Graph RAG for Small Language Models

A course research project that reproduces and critically analyzes **MiniRAG**, a graph-based retrieval-augmented generation (RAG) system designed for small language models.

> The repository name is `Rec_System`, but its content is a RAG reproduction study and not a recommender system.

## Overview

The working research question is: *does MiniRAG's advantage come from better retrieval, or from being less willing to answer "I don't know"?*

On MultiHop-RAG, the results reported in the MiniRAG paper show higher accuracy than LightRAG. They also show an error rate roughly 2–2.4× higher and far fewer abstentions. This suggests the two systems may sit at different points on the accuracy–coverage trade-off. The project plans to test this on the LiHua-World benchmark, which ships with ground-truth `Evidence` documents and 65 "Null" (unanswerable) queries.

The current repository contains the **Stage 1 proposal** and paper shortlist, the reference PDFs, and pinned references to the upstream code repositories. It contains no experiment code or results yet.

## Planned study (from the proposal)

- **RQ1 – Reproduction:** NaiveRAG, LightRAG and MiniRAG with small LMs (Phi-3.5-mini, Qwen2.5-3B; optionally GLM-Edge-1.5B and MiniCPM3-4B) on LiHua-World, plus MultiHop-RAG.
- **RQ2 – Where the gain comes from:** split accuracy into correct, wrong and abstained answers. Measure retrieval Recall@k directly from the `Evidence` column, so no LLM judge is needed.
- **RQ3 – Breakdown by query type:** Single-hop (506), Multi-hop (66) and Null (65).
- **RQ4:** check whether GraphRAG really "fails completely" with small LMs, or only fails to parse their output.
- **RQ5:** reproduce the paper's ablations (`-I`, `-R_edge`, `-R_chunk`).
- **Proposed extension:** add a confidence-based abstention threshold to MiniRAG, then compare systems on accuracy–coverage curves instead of a single accuracy number. The method follows *Sufficient Context* (ICLR 2025).
- **Evaluation stance:** prefer judge-free metrics (Recall@k, string match against the gold answer). The motivation is *Unbiased Evaluation for GraphRAG* (2025).

## Repository structure

```
Rec_System/
├── research/
│   ├── report/proposal/
│   │   ├── 00_shortlist_papers.md    # Comparison of 9 RAG papers (2024–2025) under a single-GPU constraint
│   │   ├── 01_proposal_stage1.md     # Stage 1 proposal (Vietnamese)
│   │   └── 01_proposal_stage1.pdf
│   └── paper/                        # Reference PDFs (MiniRAG, HippoRAG 1/2, LightRAG, FlashRAG, Adaptive-RAG, CRAG, Search-R1, ...)
└── source/
    ├── README.md                     # Map of upstream repos ↔ papers, environment notes
    └── MiniRAG/, LightRAG/, HippoRAG/, FlashRAG/, Adaptive-RAG/,
        CRAG/, Search-R1/, sufficientcontext/, GraphRAG-Benchmark/   # Upstream repos, recorded as git links only
```

The `source/` subfolders are committed as gitlinks without a `.gitmodules` file. A fresh clone therefore gets them as empty directories, and you need to clone them manually (see below).

## Setup

The target hardware in the proposal is a single RTX 5090 (32 GB, sm_120 / Blackwell). That GPU needs **PyTorch ≥ 2.7 with CUDA 12.8**, so repositories that pin older `torch`, `vllm` or `flash-attn` versions need their pins removed. `source/README.md` lists each one.

Clone the upstream repositories into `source/`:

```bash
cd source
git clone --depth 1 https://github.com/HKUDS/MiniRAG.git
git clone --depth 1 https://github.com/HKUDS/LightRAG.git
# TODO (owner): add the clone URLs for HippoRAG, FlashRAG, Adaptive-RAG, CRAG, Search-R1,
# sufficientcontext and GraphRAG-Benchmark, or convert them into proper submodules.
```

LiHua-World ships inside the MiniRAG repository (`MiniRAG/dataset/LiHua-World/`).

## Usage

TODO (owner): no experiment scripts exist yet. Add run commands once the reproduction code is in place.

## Status

- [x] Paper shortlist and Stage 1 proposal
- [ ] Environment and MiniRAG end-to-end run on LiHua-World
- [ ] RQ1–RQ5 experiments and accuracy–coverage analysis

## References

- Fan et al., *MiniRAG: Towards Extremely Simple Retrieval-Augmented Generation*, arXiv:2501.06713, 2025. Code: [HKUDS/MiniRAG](https://github.com/HKUDS/MiniRAG)
- Guo et al., *LightRAG*, arXiv:2410.05779
- Gutiérrez et al., *HippoRAG 2: From RAG to Memory*, arXiv:2502.14802
- Jin et al., *FlashRAG*, arXiv:2405.13576
- Joren et al., *Sufficient Context*, arXiv:2411.06037
- *Unbiased Evaluation for GraphRAG*, arXiv:2506.06331

## License

There is no license yet. The PDFs in `research/paper/` and the upstream repositories belong to their respective authors.
