# Chain of Thought Experiments

This folder contains Jupyter notebooks that implement and explore Chain-of-Thought (CoT) prompting methods and simple baseline comparisons. The notebooks are intended as an experimental playground to reproduce and inspect behaviors reported in the CoT literature.

The work in these notebooks is informed by the following papers:

- Wei et al., "Chain of Thought Prompting Elicits Reasoning in Large Language Models." arXiv:2201.11903
- Kojima et al., "Large Language Models are Zero-Shot Reasoners." 

## What the notebooks do

- `Comparing_Few_Shot_,_Zero_Shot_and_Baseline_Models.ipynb` — Implements three prompting strategies and a small evaluation harness:

  - Few-shot Chain-of-Thought (CoT): prompts containing step-by-step example solutions.
  - Zero-shot CoT: prompts that request the model to "think step by step" without providing worked examples.
  - Few-shot No-CoT (Baseline): few-shot prompts that provide only direct question→answer pairs (no intermediate reasoning).
  - The notebook collects model outputs, applies a simple heuristic to extract the final answer from generated text, and reports per-question correctness against a small ground-truth set.
- `Inference_Time_Compute_Scaling_Effect_of_Model_Size_on_Accuracy_with_Chain_of_Thought_Reasoning.ipynb` — An experimental notebook scaffold for studying how inference time, compute usage, and accuracy change with model size when using CoT prompts. It is set up to measure inference characteristics and compare accuracy across model configurations.

## Relation to the cited papers

- Chain-of-Thought prompting (arXiv:2201.11903) demonstrates that providing chains of intermediate reasoning—either as few-shot examples or by directly asking models to provide step-by-step reasoning—can enable large language models to produce multi-step reasoning and improve performance on reasoning benchmarks. The main notebook implements both few-shot and zero-shot variants to observe these effects on small, illustrative examples.
-- "Large Language Models are Zero-Shot Reasoners" demonstrates that adding a short instruction or trigger (for example, the phrase "Let's think step by step") can enable zero-shot chain-of-thought style reasoning and substantially improve performance on certain reasoning tasks without few-shot examples. The notebooks implement both few-shot and zero-shot variants so you can inspect how prompt engineering and zero-shot triggers affect generated reasoning and final answers.

## Suggested experiments and uses

- Compare accuracy of few-shot CoT, zero-shot CoT, and few-shot No-CoT across larger, standard benchmarks (e.g., GSM8K, MultiArith) to quantify gains.
- Explore zero-shot CoT prompting and trigger phrases (for example, the "Let's think step by step" prompt), and measure how zero-shot instructions compare to few-shot CoT and No-CoT baselines.
- Run scaling experiments to measure how model size affects CoT benefits and inference cost; record inference time, memory, and accuracy trade-offs.

## Beam Search / Test-time compute experiments

- Reference: Kaplan et al., "Scaling LLM Test-Time Compute Optimally can be More Effective than Scaling Model Parameters" (arXiv:2408.03314)

  - Brief summary of the paper: this work studies the trade-off between scaling model parameters (making models larger) versus allocating additional test-time compute (for example by using search, sampling, or ensembling at inference time). The authors show that, under certain conditions, investing compute at test-time (for example via more extensive decoding or ensemble-like strategies) can yield better cost-performance than solely increasing model size. The paper analyzes optimal allocation of compute budget and proposes practical techniques to improve performance given a fixed compute budget.

  - What `Beam_Search.ipynb` does: the notebook explores test-time compute strategies using beam search and related decoding techniques. It implements experiments that compare decoding strategies (beam search variations, beam width, and scoring/length-normalization variants) and records their effect on final-answer extraction and simple accuracy metrics in the context of chain-of-thought style prompts. The notebook is intended as a starting point to study whether allocating more compute at inference (wider beams, more hypotheses) improves reasoning accuracy compared to single-sample greedy/temperature-based decoding.
