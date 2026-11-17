# Appendix C. RouterBench check of the router choice

Draft by Person 1 (Aryan). Numbers come from `routerbench/results/` (0-shot). Letter "C" depends on the final appendix order in the paper.

**Purpose.** To check that BERT is a reasonable probe among the three RouteLLM routers (BERT, MF, SW-ranking). It is not meant to show that BERT is the best router.

**Set-up.** We used 1,000 prompts from RouterBench (GSM8K and ARC-Challenge) with its stored GPT-4 and Mixtral answers and costs, so no API calls were needed. Three ARC prompts had missing scores in the 5-shot file and were dropped under both formats (997 prompts for the format comparison), and 14 duplicate sample IDs with identical scores were removed. The weak model in RouterBench is mixtral-8x7b-chat, while our router set-up names Mixtral-8x7B-Instruct-v0.1. They are related but not identical. Bootstrap intervals use 500 resamples.

**Results (0-shot).**

- **Accuracy.** All three routers beat random routing by only about +0.007 to +0.008 pooled, and their differences are within the bootstrap intervals (BERT 0.0071, MF 0.0083, SW-ranking 0.0074).
- **Ranking.** Pooled AUC for choosing the prompt where GPT-4 helps: BERT 0.626 [0.592, 0.659], MF 0.604 [0.569, 0.641], SW-ranking 0.620 [0.583, 0.656]. A rule that only uses the dataset label scores 0.65 [0.613, 0.685], so the routers mostly separate GSM8K from ARC.
- **Within dataset.** On GSM8K no router shows signal (AUC 0.47 to 0.51). On ARC all three are above chance (0.66 to 0.70).
- **Spread and speed.** Score SD is 0.117 (BERT), 0.073 (MF) and 0.003 (SW-ranking). BERT takes 0.305 s per prompt on CPU with identical scores on a repeat run. [TODO: confirm MF 0.52 s and SW-ranking 3.69 s from the notebook output; both include an embedding call.]

**Limitations.** One strong/weak pair; two English datasets; 500 bootstrap resamples with no multiple-comparison correction; routers score the question while the stored costs cover the full few-shot prompt; results depend on prompt format (0-shot and 5-shot give different ARC prompts), and we report 0-shot here; unchecked training-data overlap. We do not use RouterBench to make cost claims.
