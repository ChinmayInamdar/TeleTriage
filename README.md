# TeleTriage: Hierarchical Multi-Tiered Telecom Troubleshooting System

> 🚀 AI-powered fault resolution engine for telecom networks using static caching, semantic retrieval, and generative transformers.

---

## Table of Contents

- [Overview](#overview)  
- [Motivation](#motivation)  
- [Features](#features)  
- [System Architecture](#system-architecture)  
- [Technology Stack](#technology-stack)  
- [Tier Descriptions](#tier-descriptions)  
  - [Cache-Augmented Generation (CAG) Tier](#cache-augmented-generation-cag-tier)  
  - [Retrieval-Augmented Generation (RAG) Tier](#retrieval-augmented-generation-rag-tier)  
  - [Fallback Generative Tier](#fallback-generative-tier)  
- [Training & Fine-Tuning](#training--fine-tuning)  
- [Evaluation Methodology](#evaluation-methodology)  
- [Results](#results)  
- [Key Insights](#key-insights)  
- [Limitations & Future Work](#limitations--future-work)  
- [Authors](#authors)  

---

## Overview

**TeleTriage** is a hierarchical, multi-tiered troubleshooting system designed to automatically diagnose and resolve faults in telecom networks. By combining static caching, retrieval-based search, and generative language models, TeleTriage delivers:

- **Ultra-low latency** for common queries  
- **High accuracy** for documented issues  
- **Robust fallback** for novel/unseen problems  

Queries are dynamically routed to the most appropriate tier based on confidence thresholds, optimizing both speed and resource usage.

---

## Motivation

Telecom networks are critical infrastructure but suffer from:

- Frequent hardware/software faults  
- High operational costs due to manual intervention  
- Downtime impacting businesses and services  

Automating fault resolution reduces mean time to repair (MTTR), cuts costs, and improves reliability.

---

## Features

- **Three-Tier Architecture**: CAG, RAG, Generative  
- **Smart Query Routing** based on confidence scores  
- **Cache-Augmented Generation** for instant answers  
- **Semantic Retrieval** with Sentence-BERT + FAISS  
- **Generative Fallback** using fine-tuned T5  
- **Comprehensive Metrics**: ROUGE-L, latency, CPU/memory profiling  
- **Modular & Extensible** for future tiers or modalities  

---

## System Architecture

![System Architecture](fig1.png)

> **Fig 1:** TeleTriage query flow. Queries first check the cache, then semantic retrieval, and finally the generative model if needed.

---

## Technology Stack

- **Python 3.10+**  
- **PyTorch** & **Transformers** (T5-small)  
- **Sentence-BERT** (`all-MiniLM-L6-v2`)  
- **FAISS** (similarity search)  
- **psutil** (resource monitoring)  
- **GitHub Actions** (CI/CD, optional)  

---
## Tier Descriptions

### Cache-Augmented Generation (CAG Tier)
- **Mechanism:** Static JSON dictionary of high-frequency Q&A  
- **Latency:** ~0.01 s  
- **Accuracy:** ROUGE-L F1 = 0.98  

### Retrieval-Augmented Generation (RAG Tier)
- **Mechanism:**  
  1. Encode query with Sentence-BERT (768-dim)  
  2. Search FAISS index (L2 distance)  
  3. Return top-1 if similarity ≥ 0.7  
- **Latency:** ~0.15 s  
- **Accuracy:** ROUGE-L F1 = 0.85  

### Fallback Generative Tier
- **Mechanism:** Fine-tuned T5-small model on telecom QA  
- **Latency:** ~1.23 s  
- **Accuracy:** ROUGE-L F1 = 0.72  

---

## Training & Fine-Tuning

- **Dataset:** 104 domain-specific QA pairs  
- **Model:** `t5-small`  
- **Optimizer:** AdamW (lr = 3e-5, β₁=0.9, β₂=0.999, ε=1e-8)  
- **Batch Size:** 8 (gradient accumulation every 4 steps)  
- **Epochs:** 5 (early stopping on BLEU, patience=2)  

---

## Evaluation Methodology

- **Accuracy:** ROUGE-L F1  
- **Latency:** Wall-clock time per query  
- **Confidence:** Tier heuristics  
- **Resource Usage:** CPU (%) & Memory (MB) via `psutil`  

---

## Results

| Tier         | ROUGE-L F1 | Latency (s) | Confidence | CPU (%) | Memory (MB) |
|--------------|------------|-------------|------------|---------|-------------|
| **CAG**      | 0.98       | 0.01        | 0.95       | 5.2     | 23.4        |
| **RAG**      | 0.85       | 0.15        | 0.80       | 15.6    | 120.5       |
| **Generative** | 0.72     | 1.23        | 0.60       | 45.1    | 678.2       |

_Test set: 1,000 queries from 3GPP (common, documented, novel)_

---

## Key Insights

- **CAG Tier** resolves ~63% of queries with minimal overhead.  
- **RAG Tier** covers ~28% of less-frequent documented issues.  
- **Generative Tier** handles ~9% novel queries, ensuring full coverage.  
- **Smart routing** reduces average CPU usage by ~62% vs. generative-only.  

---

## Limitations & Future Work

- **Memory footprint:** T5 requires ~678 MB RAM. Consider distillation/quantization.  
- **Static thresholds:** Explore adaptive confidence thresholds.  
- **Cache updates:** Automate cache expansion using query logs.  
- **Multilingual support:** Extend to handle non-English queries.  

---

## Authors

- **Chinmay Inamdar** — chinmay.inamdar22@vit.edu  
- **Arya Doshi** — arya.doshi22@vit.edu  
- **Tanmay Gote** — tanmay.gote22@vit.edu  
- **Prof. Dr. Vaishali Jabade** — vaishali.jabade@vit.edu  

Vishwakarma Institute of Technology, Pune, India

