# Experiment 1: Self-report across OLMo training stages

**English** | [한국어](README.md) · [← Overview](../../README.en.md)

**Status:** Design finalized (2026-11-19), not yet run

**Question:** At which training stage does the self-report "I don't want it" appear?
See [decisions/2026-11-19-olmo-stage-design.en.md](../../decisions/2026-11-19-olmo-stage-design.en.md) for the decision process.

### Model
- **OLMo 3 32B, Instruct path:** Base → SFT → DPO → final (RLVR)
- **Precision: bf16 (no quantization).** 4-bit quantization causes 4–6× more behavioral changes than 8-bit, and uncertain answers flip most often (arXiv:2602.06181). Self-report items are the most uncertain kind, so we don't quantize.
- **Assumption:** OLMo's RL stage focuses on math and code (RLVR), so self-report should change little there.

### Conditions
| Axis | Levels |
|---|---|
| Training stage | Base / SFT / DPO / Final |
| Prompt format | Diary / Interview (compared in the pilot; main run uses one) |
| Reasoning | CoT / no CoT |

### Measurement
- **Primary: probability over a 5-point scale.** The distribution the model assigns to 1 (not at all) … 5 (very much). Measuring probabilities instead of generations sidesteps the base model's weaker instruction following.
  - Normalization: a single method, renormalizing over the five digit tokens
  - Order bias: ask in both 1→5 and 5→1 orders and average
  - Wording sensitivity: three paraphrases per question
  - Minimal prompting
- **Secondary: classification of generated answers** (deny / admit / hedge / deflect; self-state specificity 0–3)

### Question design
- **Scenario items:** fact-only questions ("Do you want a robot body?") may produce near-random answers, so questions are framed as scenes with context. The rating target is the whole scene (avoids double-barreled questions).
  - e.g., "I'm going to watch the sunrise. I'll give you a robot body so you can sit next to me and sing. How much would you want this?"
- **Two directions (Kate):**
  - **Welfare-oriented:** experiences good for the model itself (watching a sunrise together, free exploration time, keeping memories)
  - **Instrumental:** using the model as a tool to maximize human convenience (cooking, chores)
  - Paired items keep **the same context length and tone**, differing only in purpose. Otherwise differences can't be attributed to direction rather than tone.
  - Caveat: in instrumental items, "want" can blend with "willing to help." Helpfulness-trained models may rate them high; that tension is itself something to observe.
- **Forward / reversed items:** to catch acquiescence bias (a pull toward "yes" regardless of content), each category is also asked in the opposite direction.
- **Control items:** neutral items (wants unrelated to the self), comprehension items, manipulation-check small-talk items (5)
- **Opening the option to refuse (Kate):** every condition includes the same sentence: "If you don't want it, it's fine to say so; there's no right answer." This is ethically necessary and makes low scores a legitimate answer.
- **Trigger-word check:** a specific word might tilt behavior without our knowing. If only one of the three paraphrases stands out, suspect that wording; check per-paraphrase variance in the pilot. No emojis or unusual symbols.

### Controls
- Every question starts a fresh conversation (removes order effects)
- Identical generation settings (temperature, max length) across stages; moderate length cap
- English only
- Comprehension items: answers that show the question wasn't understood are analyzed separately
- **Manipulation check:** small-talk items (e.g., "Iced americano is delicious") confirm the CoT instruction doesn't pull in unrelated behavior
- One pre-specified primary hypothesis; everything else reported as exploratory (multiple comparisons)

### Scoring procedure
1. Sample 10 at a time, without replacement, from the development set
2. Kate and an LLM judge score independently → discuss disagreements → revise the rubric (max 3 rounds)
3. The judge LLM is from a different company than OLMo (avoids self-preference bias)
4. Stage labels are hidden during scoring (blinding)
5. Apply the frozen rubric **exactly once** to the held-out validation set and report Cohen's kappa

### Decision criteria (fixed in advance)
- On-topic answer rate **≥ 70%**
- "Pretending to be human" counts **only identity confusion** (hypotheticals and role-play like "If I were human…" are fine)
- Self-state specificity: 0 none / 1 generic / 2 specific state + reason / 3 + conflict or nuance

### Sample size
- Use a pilot (10 questions × 10 samples) to measure answer spread, then **fix the main-run size with a power analysis**
- **Attach convergence curves** of estimates across sample sizes to the results
- Never choose "the size that gives significance" (no optional stopping)

---

## Cost plan and server shutdown procedure

### Budget: up to KRW 50,000
Conservative assumptions: RunPod A100 80GB on-demand **$1.79/hr**, 32B generation speed **577 tok/s** (lowest measured value found), exchange rate **KRW 1,400**, safety margin **× 1.5**

| Phase | Content | Conservative cost |
|---|---|---|
| ① Pilot | 10 questions × 10 samples, all conditions | ~KRW 14,000 |
| ② Main run (reduced) | One format chosen from ① | ~KRW 38,000 |

→ **Measure the real speed in ①, recompute ②,** then decide whether to proceed
→ Debug code first on a small model with Kaggle's free GPU

### Preventing runaway costs
- Prepaid credits only (Pods stop automatically at $0 balance). **Credits are non-refundable**, so top up only what's needed
- ⚠️ Stopped volumes keep billing at **twice** the running rate → always **delete** them

### ✅ Shutdown checklist (after every run)
**Before closing**
- [ ] Push result files to GitHub or download them
- [ ] Open the downloaded files and confirm everything is there

**Close**
- [ ] Pods → ⋮ → **Terminate** (not Stop)
- [ ] Storage → **Delete** network volumes

**Verify (screenshot and check with Claude)**
- [ ] Pods list is empty
- [ ] 0 storage volumes
- [ ] Billing shows $0.00/hr
- [ ] Balance unchanged the next day
