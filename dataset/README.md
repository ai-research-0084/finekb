# FineKB Embedding Dataset

This repository contains the public release of anonymized case and KB embeddings used in the FineKB research project. The dataset provides vector representations of LLM-generated summaries for support cases and corresponding KB articles, enabling reproducible research in retrieval-augmented systems, enterprise support automation, and embedding model evaluation.

---

## Dataset Overview

The dataset is provided as **four Parquet files**:

1. **`finekb_cases_train.parquet`** — Training cases (split into <100MB parts in this repo)  
2. **`finekb_cases_val.parquet`** — Validation cases  
3. **`finekb_cases_test.parquet`** — Test cases  
4. **`finekb_kb.parquet`** — KB article embeddings  

All records are anonymized and contain **no raw text**, **no customer data**, and **no proprietary content**.

---

## Case Dataset Schema

Each row corresponds to a single anonymized support case embedding.

| Column Name        | Description |
|--------------------|-------------|
| `case_id`          | Integer ID for each case (0 … N-1). |
| `issue_type`       | High-level classification label for the case. |
| `kb_id`            | ID of the correct KB article (may be `null`). |
| `embed_summary`    | Embedding vector (list of floats) from the NV-Embed model. |

**Example Row**

```json
{
  "case_id": 3,
  "issue_type": "contract",
  "kb_id": 35,
  "embed_summary": [0.006812, 0.003007, ...]
}
