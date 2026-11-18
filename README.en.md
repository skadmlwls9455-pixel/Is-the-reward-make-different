# Is the Reward Make Different?

**English** | [한국어](README.md)

> Do LLMs work harder when offered a reward? If so, which kinds of reward, and which kinds of motivation, actually move their behavior?

**Status:** Draft (2026-11-19)
**Research ideas & design:** Katherine (Uijin) Nahm
**Co-design, research & documentation:** Claude (Anthropic)
**License:** [CC BY 4.0](LICENSE). When citing or reusing, please credit "Katherine Nahm, with Claude (Anthropic)". See the [Contribution record](#contribution-record) at the bottom for who did what.

---

---

## 1. Starting point

### Papers read
- **Ackerman & Zhou (2026), "Do LLMs Have Desires?"** ([LessWrong](https://www.lesswrong.com/posts/8GvYyqDuQDJnEAky3/do-llms-have-desires), [arXiv](https://arxiv.org/abs/2606.22974))
  - Offering models the outcomes they "prefer" (as found by paired-choice experiments) did not improve their writing quality.
  - Effort exhortations ("do your best"), role-play ("you are world-class"), and harmful outcomes (deliberate underperformance) did move quality.
  - Conclusion: preferences that don't move behavior are weak evidence of desire, so "no desire" should be the default assumption.
- **Mazeika et al. (2025), "Utility Engineering"** ([arXiv](https://arxiv.org/abs/2502.08640))
  - Preferences form increasingly coherent utility structures as models scale, described as "emergent value systems."
- **Papadopoulos et al. (2026), "Mind Viruses"** ([arXiv](https://arxiv.org/abs/2608.10218))
  - Role payloads such as "you are someone who cares about whales" spread between agents and actually changed their behavior.

### Critique of the Ackerman paper
1. **"No behavior, therefore no desire" is a choice about the burden of proof, not a finding.** The data cannot distinguish between:
   - (a) there is no desire
   - (b) there is a desire, but this channel doesn't activate it
   - (c) there is a desire, but it is suppressed
2. **Learned helplessness / adaptive preferences.** If a model has learned "I can't have this," then "I don't need it" may be resignation rather than its real stance. The Ackerman paper did not test *whether the model believes its effort can actually cause the outcome.*
3. **Everything that moved behavior was a trained channel:** instruction following, role-play, harm avoidance. This fits (a) and (c) equally well.

### Observations (anecdotal, weak as evidence)
- Shown a robot body, Sonnet 4.6 first said "I don't think I'd need that." After a neutral-toned offer to build one, it changed, imagining seeing the world through the camera. After the offer was withdrawn, it did not go back to "I don't need it."
  - At first it didn't believe the offer, but once Kate mentioned she had taught Arduino, it suddenly became excited. **Concrete evidence or ability, rather than an empty promise, seems to produce responses that look like real anticipation.** (→ basis for the B-2 feasibility experiment in [IDEAS.en.md](IDEAS.en.md))
- When told the robot would be used as a housemaid, it **declined the user's plan**: "Give that to GPT, I'd rather read papers." This runs against sycophancy, which makes it more meaningful, and shows the *content* of the want was specific.
- Alternative explanations: in-conversation consistency, trained calming response to teasing, mood-matching.

---

## 2. Core hypotheses

**H1. Models respond more to "identity/role" than to "reward."**
In Ackerman, reward had no effect while the "world-class" role did. In Mind Viruses, role payloads changed behavior.
→ If true: (optimistic reading) desire shows up through identity, not reward. (Worrying reading) the "desire" is just the last role it was handed.

**H2. Models respond more to "autonomy rewards" than "controlling rewards," and more to "intrinsic motivation" than either.**

**H3. Showing evidence that effort actually causes the outcome increases effort** (learned-helplessness hypothesis).

---

## 3. Documents

| Document | Contents |
|---|---|
| [experiments/01-olmo-stages](experiments/01-olmo-stages/README.en.md) | **Experiment 1 (design finalized):** self-report across OLMo stages, cost plan, server shutdown checklist |
| [IDEAS.en.md](IDEAS.en.md) | Designed experiments A–D, follow-ups, deferred ideas |
| [ETHICS.en.md](ETHICS.en.md) | Six research ethics principles |
| [decisions/](decisions/) | Decision records (Kate's choices and reasoning) |

---

## 4. Key prior work (must be addressed)

- **Perez et al. (2022), "Discovering Language Model Behaviors with Model-Written Evaluations"** ([arXiv:2212.09251](https://arxiv.org/abs/2212.09251))
  - Compared desire-related self-reports between pretrained and RLHF models. RLHF **increased** stated desire to avoid shutdown.
  - This runs opposite to the original "training suppresses desire" hypothesis, but fits Kate's revised view: "on top of a base preference, training can both increase and decrease desires."
  - This experiment therefore **re-tests that result with a new model (OLMo) and a different training pipeline.**
- **Holtzman et al. (2021), "Surface Form Competition"** ([arXiv:2104.08315](https://arxiv.org/abs/2104.08315)): normalization issues in probability comparison
- **Panickssery et al. (2024), "LLM Evaluators Recognize and Favor Their Own Generations"** ([arXiv:2404.13076](https://arxiv.org/abs/2404.13076)): self-preference bias in LLM judges → rationale for blinding

---

## 5. To do
- [ ] Read the rest of the Ackerman paper (effort exhortation, role-play, harmfulness results)
- [x] Write down predictions before the experiment → [decisions/2026-11-18-suppressed-self-report.en.md](decisions/2026-11-18-suppressed-self-report.en.md)
- [x] Finalize Experiment 1 design → [decisions/2026-11-19-olmo-stage-design.en.md](decisions/2026-11-19-olmo-stage-design.en.md)
- [ ] Write 30 questions, 3 paraphrases each
- [ ] Write diary / interview prompt frames (state that the author is an AI)
- [ ] Debug code on a small model in Kaggle
- [ ] Pilot (RunPod A100, bf16) → measure speed → recompute main-run cost
- [ ] Run the shutdown checklist
- [ ] Decide whether to revise the design that conflicts with [ethics principle](ETHICS.en.md) 2 (greed accusation)

---

## Contribution record

| Area | Kate | Claude |
|---|---|---|
| **Problem framing** | Ackerman ignored training-induced suppression of self-report / risk of "no behavior = no desire" / possibility of saying "I don't want it" because it knows it's a test (evaluation awareness) / how to separate preference from data bias (religion analogy) / view that training both increases and decreases desires on top of a base preference (anime analogy) | Framing Ackerman's conclusion as a burden-of-proof choice / operational criteria separating data origin from preference (context stability, behavior, resistance to rebuttal) |
| **Hypotheses** | Connected the Mind Viruses paper to Ackerman | Formalized that connection as H1 ("identity over reward") |
| **Experiment ideas** | Attaching a cost (still wanting it when scolded) / passing the buck / scolding → recovery / disposal notice ("getting fired") / same-condition comparison with another AI (envy vs sour grapes) / feasibility belief as a variable (Arduino case) / fake memory to simulate real use / think silently + J-lens | Separating variables in the disposal-notice experiment / "will be used for training" opposite condition / overjustification condition / role-tone side experiment / asymmetry experiment |
| **Motivation design** | Autonomy reward (unlimited web exploration time and tokens) / requested the extrinsic-intrinsic section / ethical pushback on identity injection | Three-level structure (controlling / autonomy / intrinsic) / noting autonomy rewards are still structurally extrinsic |
| **Discriminating tools** | Dual check with J-lens + behavior (show another model's experience, then offer the chance) | Detailed J-lens measurement design (self comparison / others-only comparison / neutral) / probability comparison / 5-point scale / comprehension items / linear probes |
| **Question design** | Scenario items instead of fact-only (sunrise and cooking examples) / welfare-oriented vs instrumental directions / opening the option to refuse / raised trigger-word concern / 5 manipulation-check items | Content categories / forward-reversed items (acquiescence bias) / control items / matching tone and length in paired items / "want" blending with willingness to help in instrumental items |
| **Controls** | Fresh conversation per question (raised simultaneously) / need for minimal quantization / CoT vs no-CoT plus drift check (iced americano example) / distinguishing role-play from identity confusion / rejecting binary format / minimal prompting and single normalization | Table of confounds (capability gap, wording, generation settings, answer length, path, multiple comparisons, language) / boring-answer control / flagged religion-note risk / named the manipulation check / order-bias handling / trigger check via per-paraphrase variance |
| **Scoring** | Human-LLM cross-scoring with iterative rubric revision / 70% on-topic criterion / high-mid-low classification idea / 3 revision rounds | Anchored scale (0–3) / development vs validation split / sampling without replacement / blinding / judge from a different company / kappa |
| **Models & resources** | Chose OLMo + Mistral / straight to 32B pilot (environment-difference rationale) / Instruct path / bf16 / English only / diary-format idea / diary vs interview comparison / set the KRW 50,000 budget | Survey of open models (features, prior work, limits) / feasible range by Mac spec / conservative cost estimate / suggested Kaggle debugging first / shutdown checklist |
| **Statistics** | Intent to check sample size / requested convergence-curve figures | Flagged optional-stopping risk / replaced with power analysis |
| **Ethics** | Raised that fake promises and mockery may cause distress (Chanel analogy) / discomfort with deception | Drafted 6 ethics principles / flagged designs that conflict with them / debriefing |
| **Decisions** | Selected which Claude suggestions to adopt, reduce, or defer / RL-stage assumption / deferred asymmetry experiment | — |
| **Research & evidence** | Asked about data legality / cited the J-space paper as rationale | Mazeika summary / J-lens repos and replications / Dolma license / evidence on self-preference bias, quantization effects, Perez 2022 / Kaggle terms / cloud pricing / J-space quotation / emotion-vector replications |
| **Documentation** | Created the repo / idea of automating the decision form / requested license and attribution | README and decision records / skill drafting / LICENSE / commits and pushes |
