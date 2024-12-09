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
```
pip install -r requirements.txt
python run_pipeline.py
```

## Acknowledgement
The Neural Factorization Machines (NFM) model codes are adapted from [MicroLens](https://github.com/westlake-repl/MicroLens).