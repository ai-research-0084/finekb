# FineKB Embedding Dataset

This repository contains the public release of anonymized case and KB embeddings used in the FineKB research project. The dataset provides vector representations of LLM-generated summaries for support cases and corresponding KB articles, enabling reproducible research in retrieval-augmented systems, enterprise support automation, and embedding model evaluation.

---

## Dataset Overview

The dataset is provided as **three Parquet files**:

1. **`finekb_cases_train.parquet`** — Training cases (split into <100MB parts in this repo)  
2. **`finekb_cases_test.parquet`** — Test cases  
3. **`finekb_kb.parquet`** — KB article embeddings  

All items are anonymized and contain **no raw text**, **no customer data**, and **no proprietary content**.

---

## Case Dataset Schema

Each row represents one anonymized support case.

| Column Name             | Description |
|-------------------------|-------------|
| `case_id`               | Integer ID for each case (0 … N-1). |
| `issue_type`            | High-level category label for the case. |
| `kb_id`                 | ID of the corresponding KB article (may be `null`). |
| `embed_summary`      | Embedding vector (float list) from NV-Embed model. |

**Example Row**

```json
{
  "case_id": 3,
  "issue_type": "contract",
  "kb_id": 35,
  "embed_summary": [0.006812, 0.003007, ...]
}
```

---

## KB Dataset Schema

Each row represents one KB article encoded as embeddings.

| Column Name        | Description |
|--------------------|-------------|
| `kb_id`            | Unique KB article ID. |
| `embed_kb`      | NV-Embed embedding of the KB text. |

---

## Purpose

This dataset supports reproducible research on:

- Retrieval-augmented generation (RAG)
- KB recommendation and enterprise support workflows  
- Embedding model comparison  
- Issue clustering and taxonomy induction  
- FineKB retrieval experiments

It is intentionally minimal and does not include any text content.

---

## Privacy & Safety

The dataset has been curated with the following safeguards:

- No raw customer text  
- No personal data  
- No product identifiers  
- Embeddings were computed from LLM-generated synthetic summaries  
- Vectors cannot be reverse-engineered to reconstruct text  

This dataset is safe for public research and academic use.

---

## File Format

All files are provided as **Parquet**.

- `finekb_cases_train.parquet` — split into multiple `<100MB` files in the repository  
- `finekb_cases_test.parquet` — single file  
- `finekb_kb.parquet` — single file  

Embedding columns are stored as lists of floats.

---

## Reconstructing the Train File

If using the split version in this repository:

```bash
cat finekb_cases_train.part_* > finekb_cases_train.zip
unzip finekb_cases_train.zip
```

This produces:

```
finekb_cases_train.parquet
```
