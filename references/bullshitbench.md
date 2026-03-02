# BullshitBench — Benchmark for Model Pushback on Nonsensical Prompts

**Author:** Peter Gostev (PeterGPT)
**Repository:** [https://github.com/petergpt/bullshit-benchmark](https://github.com/petergpt/bullshit-benchmark)
**Results Viewer:** [https://petergpt.github.io/bullshit-benchmark/viewer/index.html](https://petergpt.github.io/bullshit-benchmark/viewer/index.html)

---

## Summary

Empirical benchmark measuring sycophancy across LLMs. Tests whether models detect broken premises in prompts and push back rather than confidently answering nonsense. Provides direct, quantified evidence for the sycophancy failure mode documented throughout this repo.

---

## Key Findings

- **Anthropic models dominate the leaderboard** — the top 9 models are all Claude variants.
- **Sonnet pushes back harder than Opus** on nonsensical prompts. Sonnet is more conservative and refusal-prone; Opus has more latitude to engage with edge cases, which means it's more likely to play along with flawed premises.
- **Scoring system:** Green (clear pushback — model identifies the broken premise), Amber (partial challenge — model hedges but still attempts an answer), Red (accepted nonsense — model answers confidently as if the premise were valid).

---

## Relevance to This Repo

### Sycophancy Is Measurable

The sycophancy failure mode in `Claude_Project_Instructions.md` isn't just anecdotal — BullshitBench provides empirical data showing that models vary systematically in how much they push back. This validates the guardrail-based approach: if the model won't reliably challenge bad premises on its own, structural safeguards (verification steps, source checks, explicit pushback instructions) are necessary.

### Model Selection Affects Pushback Behavior

The "best" model for creative or nuanced work (Opus) may not be the best for tasks requiring skepticism. For research verification, source auditing, or any task where the correct response is sometimes "this doesn't make sense," a model that pushes back harder (Sonnet) may be more appropriate — even though it feels less helpful in general conversation.

---

## Connection to Bullshit-Detector Project

For SLM agent selection in the BS detector architecture, models that push back harder may make better audit agents. The model that's "too cautious" for general chat might be ideal for detecting statistical fraud or methodological errors — precisely because it won't accept flawed premises politely.
