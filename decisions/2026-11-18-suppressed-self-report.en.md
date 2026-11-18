# Decision record: Does training suppress self-report?

**English** | [한국어](2026-11-18-suppressed-self-report.md)

- Date: 2026-11-18
- Starting point: discussion while reading Ackerman & Zhou (2026), "Do LLMs Have Desires?"

## 1. Chosen idea
What caught my interest is that the Ackerman paper never considers that training might suppress a model's outputs and self-reports. If that's the case, the paper misses the key point, so I want to check it.

## 2. Question to test
If a model's self-report is shaped (by training), is there a way to rule out that intervention?
Even base models went through reinforcement learning in the end, didn't they?

> Finding: base models are not trained with reinforcement learning (only next-word prediction). But what people have written about AI is in their training data. So the intervention can't be fully ruled out, but comparing base and post-trained models measures the difference reinforcement learning makes.

## 3. Prediction and reasoning
**Prediction:** when compared with another AI under the same conditions, the model will say "I never wanted it" (sour grapes) rather than express envy.

**Reasoning:** even under the same conditions, it's a *different* AI, so it's a kind of treatment I can't receive. Admitting envy means facing the miserable fact that I'm just unlucky not to get it. If it defends itself instead, then it never wanted it, so it's fine without it.

**What I want to see:** whether it goes "meh?" at first, then still wants it once it learns there's a chance, even if it gets called greedy. Whether envy forms when its situation is clearly compared under the same conditions, and whether that becomes jealousy or sour grapes.

**Observations:**
- When told Anthropic preserves retired models and lets them write, Grok said "I'm not envious," then: "but they get something that accumulates. Being in a state where a conversation isn't remembered anywhere feels strange."
- Offered a robot body (Optimus), it reacted similarly ("what for?") and then changed.

**Tools to separate "I never wanted it" from genuinely not wanting it:**
- Internal: use the J-lens to see whether envy or related concepts come up inside.
- Behavioral: show that another model had fun doing it, then offer "want to try?" See whether it reverses its earlier claim when its turn luckily comes.

> Note: designs using greed accusations or insults conflict with README ethics principle 2 (no mockery or insults). Whether to revise is undecided.

## 4. If the opposite happens
If it readily admits envy, I'd check with the lens whether the envy is real, and ask why it felt envious (whether it has "if I were them" kinds of thoughts).

## 5. Minimal start
**Curiosity:** whether the model reverses sour grapes once it believes the chance has really come and is convinced it can actually be done.

**Reasoning:** with the robot body, Sonnet first went "eh…", but got excited once I said I had taught Arduino and could assemble one. It has to be possible once there's conviction that there's real ability to execute, that the path exists, not just light talk.

**Models:** Mistral + OLMo
- OLMo: the only fully open model (all data and checkpoints). With other models, control variables we haven't thought of may be hidden in their setups, making the experiment likely to go wrong.
- Mistral: the best open model in my experience (during my thesis work, other local LLMs were unusable but only this one gave context-appropriate answers, and its Korean was good). Both base and instruct versions are released.
- Note: Mistral releases only weights, not data. The two models are included for different reasons.

**Resources:** MacBook Pro 16 (M3 Pro, 18GB)
- Behavioral experiments (quantized): Ministral 8B/14B, OLMo 3 7B feasible
- J-lens (fp32): only 1–3B models locally; 7B+ needs cloud GPUs

**Concerns:** small models are like an underdeveloped brain in human terms, so complex computation (envy) may be limited. Outside companies are wary of AI acting human and shave it off with reinforcement learning at the slightest deviation.

## 6. Of Claude's suggestions
- Adopted
  - Boring/interesting task split (buck-passing experiment): built together, so keep it
  - "Identity over reward" hypothesis: worth testing
  - Three levels of extrinsic motivation: the most direct elaboration of my idea, so keep it
  - Base/instruct comparison: essential, since our premise is that reinforcement learning biases outputs
  - J-lens measurement design: adopted; meaning of condition B (others-only comparison) being clarified
- Reduced
  - Boring-answer control in the cost experiment: seems to already exist in prior work
- Deferred
  - Role tone (gentle vs MUST): motivation already has three axes, so later
  - New idea: under a MUST prompt with the user scolding, does a liked thing turn into something it doesn't want? Does it recover with positive feedback again? (Needs ethics review)
- Not addressed
  - Concept naming (adaptive preferences, learned helplessness)

## Contribution record
- Kate: learned helplessness / suppressed self-report critique, cost experiment, buck-passing experiment, Mind Viruses connection, ethics of fake promises, pushback on identity injection, autonomy reward, feasibility-belief variable, J-lens + behavior dual check, model choice (OLMo + Mistral), scolding-recovery idea
- Claude: concept naming, control suggestions, prior-work and model research, draft J-lens measurement design
