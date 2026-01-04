# FineKB

FineKB is a domain-adaptive retrieval framework for matching noisy enterprise support cases to relevant knowledge base (KB) articles.  
This repository provides the public dataset, clustering workflow, retrieval baselines, and evaluation notebooks used to build and analyze the FineKB system.

---

## Overview

FineKB processes historical support cases using LLM-generated summaries, converts them into dense embeddings, clusters them to form structured semantic groups, and evaluates multiple retrieval strategies over both KB and case-derived representations.

### Pipeline Summary

Case Summaries → Embeddings → Clustering → Centroids → FAISS Index → Retrieval Evaluation

---

## Repository Structure

dataset/
    finekb_cases_train_clustered.parquet
	finekb_cases_val.parquet
    finekb_cases_test.parquet
    finekb_kb.parquet
    finekb_train.part_aa
    finekb_train.part_ab
    ...

notebooks/
    01_cluster_cases.ipynb
    02_retrieval_kb_embeddings.ipynb
    03_retrieval_case_centroids.ipynb
    04_retrieval_cluster_multi_centroid.ipynb

---

## Dataset

The dataset includes anonymized embeddings for both support cases and KB articles.

### Included Files

- finekb_cases_train_clustered.parquet — clustered training cases  
- finekb_cases_val.parquet — validation cases
- finekb_cases_test.parquet — test cases  
- finekb_kb.parquet — KB article embeddings  
- finekb_train.part_* — split files for the full training set

### Reconstructing the Train File

cat finekb_cases_train.part_* > finekb_cases_train.zip
unzip finekb_cases_train.zip

This produces:

finekb_cases_cases_train.parquet

---

## Notebooks

### 01_cluster_cases.ipynb
Clusters the training cases using embedding vectors and writes:

./dataset/finekb_cases_train_clustered.parquet

Key parameters:
- CASE_EMB_COL = "embed_summary"
- CLUSTER_COL = "cluster_id"
- MIN_CLUSTER_SIZE = 10
- MAX_CLUSTERS_PER_KB = 50

---

### 02_retrieval_kb_embeddings.ipynb
Builds a FAISS index over KB embeddings and evaluates retrieval using test cases.

Embedding columns:
- CASE_EMB_COL = "embed_summary"
- KB_EMB_COL = "embed_kb"

---

### 03_retrieval_case_centroids.ipynb
Computes per-KB case centroids from the training set and evaluates centroid-based retrieval.

Uses:
- CASE_INDEX_EMB_COL = "embed_summary"
- CASE_SEARCH_EMB_COL = "embed_summary"

---

### 04_retrieval_cluster_multi_centroid.ipynb
Implements cluster-aware retrieval by computing multiple centroids per KB based on clustered cases.

Parameters:
- CLUSTER_ID_COL = "cluster_id"
- MIN_CLUSTER_SIZE = 3

---

## Running the Project

1. Reconstruct the train dataset (if using split files).  
2. Open the notebooks in order (01 → 04).  
3. Each notebook saves its outputs directly in the dataset/ folder.  
4. Modify configuration parameters at the top of each notebook if needed.

---
