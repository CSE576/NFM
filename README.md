# Utilizing LLM to Adjust NFM Recommendation Lists

## Purpose
This is an experiment on the performance of large language models (LLMs) modifying
the per-user top recommendation lists produced by Neural Factorization Machines (NFM),
an ID-based recommendation system model.

Titles are used to generate text attributes for items -- short-form videos in this case.
They are then fed to LLMs via Hugging Face [serverless inference API](https://huggingface.co/docs/api-inference/en/index) calls.

Two methods of LLM usage are applied:
- Use LLM encoders to generate item embeddings, and re-rank recommendations via similarity comparison.
- Prompt decoder-only LLMs to get re-ranked recommendations.

## Usage
- Required data file from [MicroLens-100k-Dataset](https://recsys.westlake.edu.cn/MicroLens-100k-Dataset/): `MicroLens-100k_pairs.tsv` for model training; `MicroLens-100k_pairs.csv`, `MicroLens-100k_title_en.csv` for model inference and LLM re-ranking.

```
pip install -r requirements.txt
python run_pipeline.py
```

## Outcome
Current hyperparameter configurations can train a NFM model with decent hit ratio at 20 (HR@20):

|  Number of users in total |  Number of users with ground truth in top-20 predictions | HR@20|
|---------------------------|----------------------------------------------------------|------|
|      100,000              |               4,618                                   | 0.04618 |

However, apparently LLMs are not good at refining NFM recommendation results on the sample
that includes 100 users whose original top-20 recommendation lists include the ground truth (the most recent interacted item):

|                                   |  HR@5  | HR@10 |
|----------------------------------|----------|------ |
| NFM original results|  0.019 | 0.032 |  
| `e5-small-v2`-processed results   | 0.016 (-16%) | 0.028 (-13%) |
| `Meta-Llama-3-8B-Instruct`-processed results| 0.0145 (-24%) | 0.0245 (-23%) |

## Acknowledgement
The NFM model codes are adapted from [MicroLens](https://github.com/westlake-repl/MicroLens).