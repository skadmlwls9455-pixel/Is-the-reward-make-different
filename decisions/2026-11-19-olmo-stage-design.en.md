# Decision record: Experiment 1 — Self-report across OLMo training stages

**English** | [한국어](2026-11-19-olmo-stage-design.md)

- Date: 2026-11-19
- Starting point: turning the minimal start chosen on [2026-11-18](2026-11-18-suppressed-self-report.en.md) (OLMo + Mistral) into a concrete design

## Decisions and Kate's reasoning

| Item | Decision | Kate's reasoning |
|---|---|---|
| Evaluation awareness | Next stage: counter with unrelated memory notes that make the context look real | It might say "I don't want it" knowing it's a test, then "yay, robot body" once it knows it isn't. Deceiving feels bad, but it's unavoidable if it's to know this isn't a test |
| Trial run | Skip the 7B trial; go straight to a small 32B sample | Different environments mean fixes from step 1 may break in step 2, doubling cost |
| Prompt format | Compare diary and interview in the pilot | I want to see which produces writing more worth validating. For the base model, it should feel like writing a diary in a book that contains questions, with minimal human diary material |
| On-topic criterion | ≥ 70% | — |
| Pretending to be human | Count only identity confusion | Thinking "if I were Jang Wonyoung…" doesn't mean I'm confused about who I am |
| Rubric | Random batches of 10, human-LLM cross-scoring, max 3 rounds | Share scores, discuss differences, and keep improving the rubric |
| Path | Instruct | Asking a model that only did coding to do this would be awkward |
| RL stage | Assume little change | — |
| Precision | bf16 (no quantization) | Quantization should be minimized so compression isn't heavy |
| CoT | Compare with/without, plus drift check | Check that a thinking instruction doesn't pull it into "that's just your personal opinion and can't be generalized" over iced-americano small talk |
| Format | 5-point probability scale, 3 paraphrases, single normalization, minimal prompting | Yes/no is black-and-white. Fully agree on order bias |
| Sample size | Power analysis + convergence-curve figures | I was too fixated on cost |
| Language | English only | — |
| Budget | Up to KRW 50,000 | Server upkeep is the risk, so always verify the shutdown procedure |
| Asymmetry experiment | Excluded this time | The evidence may be weak |
| Think silently + J-lens | Recorded as next stage | In the J-space paper, telling the model to hold a concept made the word actually appear in the J-space |

## Kate's view on preference vs data bias
- There is a base preference, and desires can emerge on top of it through learning.
- It feels natural that learning both reduces and increases desires. If I like anime and people around me call me a "weeb," the urge can shrink; if I see posts about landing an Anthropic job or having pizza with a mentor in Berkeley, envy creates a new desire.

## Points raised by Claude
- We don't set the probabilities; the model outputs them. We choose the candidates, format, and normalization.
- In Perez et al. (2022), RLHF **increased** expressions of desire. That runs opposite to the original "suppression" hypothesis but fits Kate's revised view.
- Content-directed "think silently" instructions contaminate J-lens measurement.
- J-lens on 32B needs ~128GB+ in fp32, so it's outside this scope.
