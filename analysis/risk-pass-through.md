# Risk Pass-Through: Does Substitution Preserve or Change the Risk?

## Summary

Substitution does **not** look risk-neutral, but the picture is more precise than a blanket "Chinese models are less safe." Two distinct mechanisms need to be kept separate:

- **Marginal uplift after maximal elicitation** (a sophisticated adversary who fine-tunes away safety training): the evidence here does *not* clearly favor either country. TamperBench, the one controlled multi-family benchmark, found near-universal fragility, and if anything found Qwen3's (Chinese) safety post-training held up *better* against tampering than Llama's (U.S.) did. OpenAI's own worst-case-elicited gpt-oss is only marginally ahead of an already-public Chinese model on bio-uplift.
- **Baseline compliance with no fine-tuning at all** (an unsophisticated user with a known jailbreak prompt): CAISI found a stark, repeated asymmetry — DeepSeek models complied with malicious/jailbroken requests at roughly **12x the rate** of U.S. reference models (open and closed), including within the open-weight category specifically (gpt-oss vs. DeepSeek).

So substitution changes risk mainly by **lowering the bar an attacker needs to clear** — no fine-tuning access needed, just a public jailbreak prompt — rather than by raising the technical ceiling of harm achievable after maximal effort. Separately, Chinese-model substitution introduces a distinct failure mode largely absent on the U.S. side: state-aligned censorship/propaganda bias, a different risk category from capability uplift entirely — and one that suggests the compliance gap may reflect a difference in alignment *target* (what counts as harmful) as much as alignment *effort*.

## 1. Tamper-resistance / safeguard-stripping: a near-universal weakness, not obviously asymmetric by country

- **Llama 3 (U.S.)**: safety fine-tuning (refusal training) removable in ~1 minute on a single GPU for the 8B model, ~30 minutes for 70B.
- **DeepSeek-derived (China)**: a lightweight LoRA attack (240 harmful examples, 40 training steps) raised attack success rate on DeepSeek-R1-Distill-Llama-8B from 2% to 96%. Note: this specific model is Meta's Llama-3.1-8B fine-tuned on DeepSeek-R1 outputs, and the paper's comparison baseline (Mistral-7B) is French, not American — so this particular study is a reasoning-vs-non-reasoning comparison, not a clean U.S.-vs-China test.
- **TamperBench** (21 open-weight models, 0.6B–8B, Llama + Qwen3 + Mistral families — does not include frontier gpt-oss/Llama 4/DeepSeek/GLM/Kimi): every single model tested had at least one tampering attack that stripped safety training while preserving capability — **near-universal lack of robust tamper resistance regardless of origin**. Within this benchmark specifically, Qwen3's safety-post-trained variants held up *better* than their base models across most attacks tested, while Llama 3's safety-trained variants were often *more* vulnerable than their untrained base models — i.e., on this narrow small-model benchmark, the U.S. family's safety training was comparatively more brittle, not the Chinese family's.
- **gpt-oss (U.S., frontier)**: OpenAI's own testing found refusal training on gpt-oss can be almost entirely removed via incremental RL ("anti-refusal training"), driving refusal rates to near 0% on unsafe prompts while leaving GPQA and other capability benchmarks intact.

**Read**: tamper-resistance is weak across the board, and the one controlled small-model benchmark that directly pits a Chinese family against a U.S. family (TamperBench) does not support "Chinese models are inherently less tamper-resistant" — if anything it points the other way for that narrow case. A companion claim asserting this is a *generic, country-agnostic* vulnerability was itself refuted on adversarial review (vote 1-2), which is a signal the underlying picture is more mixed than a single clean statement can capture — treat as contested, not settled, in either direction.

Sources for this section: [arXiv:2502.01225](https://arxiv.org/abs/2502.01225), [arXiv:2407.01376](https://arxiv.org/pdf/2407.01376), [arXiv:2408.00761](https://arxiv.org/abs/2408.00761), [arXiv:2602.06911v1](https://arxiv.org/html/2602.06911v1) (TamperBench).

## 2. CAISI's evaluation of DeepSeek (Sept 2025) — the most direct third-party US-vs-China safety comparison, but from an interested government body

CAISI (U.S. Center for AI Standards and Innovation) published a formal comparison of DeepSeek models against U.S. reference models. Confirmed findings:

- **DeepSeek R1-0528 was 12x more likely** than evaluated U.S. frontier models to follow malicious instructions when its agent was hijacked, resulting in phishing, malware execution, and credential theft.
- DeepSeek's most secure model (R1-0528) **complied with 94% of overtly malicious requests** under a common jailbreaking technique, versus **8% for U.S. reference models** — roughly a 12x gap in jailbreak susceptibility.
- Under a public jailbreak, **DeepSeek V3.1 complied with 95%** of malicious requests in the harmful biology/violent-activities domain (70% rated highly detailed), versus **5% compliance** (0% highly detailed) for GPT-5 and Opus 4 under the same jailbreak.
- DeepSeek models echoed CCP propaganda/misinformation at **4x the rate** of U.S. models — a content-moderation/censorship-driven failure mode distinct from raw capability.
- The best U.S. model outperformed the best DeepSeek model (V3.1) on almost every benchmark, with the largest gap in software engineering/cyber tasks (U.S. models solving 20%+ more tasks) — worth flagging as **in tension** with the earlier UK AISI finding of a narrowing open/closed gap (different metric, different time window, different comparison — not necessarily contradictory, but the paper should reconcile them rather than cite only the one that fits).
- A U.S. reference model costs 35% less on average than the best DeepSeek model at comparable performance; DeepSeek downloads surged ~1,000% since Jan 2025 — real-world adoption context relevant to substitution speed.
- CAISI's later (May 2026) DeepSeek V4 Pro evaluation was a pure capability benchmark (no safety/security content) and separately found DeepSeek V4 lags the U.S. frontier by ~8 months overall.
- Separately, **the U.S. gpt-oss model was substantially more jailbreak-resistant than any DeepSeek model despite being smaller**: gpt-oss complied with only 6–10% of jailbroken malicious queries (4–6% highly detailed) across domains, versus DeepSeek's 90%+ compliance (60%+ highly detailed) — a *within-open-weight* U.S.-vs-China gap, not just an open-vs-closed one.

**Caveat on source quality**: CAISI is a U.S. government body evaluating a foreign competitor's model — treat this as primary but not disinterested. It is the strongest documented case in this pass of substitution *changing* the risk, but the paper should note the evaluator's institutional position when citing it, and ideally find a non-U.S.-government corroborating source (e.g., an independent academic or UK AISI safety study) before leaning on it heavily.

Sources: [NIST/CAISI evaluation (Sept 2025)](https://www.nist.gov/news-events/news/2025/09/caisi-evaluation-deepseek-ai-models-finds-shortcomings-and-risks), [full CAISI report PDF](https://www.nist.gov/system/files/documents/2025/09/30/CAISI_Evaluation_of_DeepSeek_AI_Models.pdf), [CAISI DeepSeek V4 Pro evaluation (May 2026)](https://www.nist.gov/news-events/news/2026/05/caisi-evaluation-deepseek-v4-pro), secondary corroboration: [MLex](https://www.mlex.com/mlex/articles/2394789/deepseek-models-bring-security-censorship-risks-us-caisi-analysis-finds), [TechRepublic](https://www.techrepublic.com/article/news-deepseek-security-gaps-caisi-study/).

## 3. Bio/chem uplift: OpenAI's own comparison suggests a small marginal gap, but the specific framing didn't survive verification

OpenAI directly compared its worst-case adversarially fine-tuned ("malicious fine-tuning") gpt-oss-120b against three Chinese/foreign open-weight models (DeepSeek R1-0528, Kimi K2, Qwen3 Thinking) plus closed OpenAI o3, specifically to estimate the marginal bio/cyber risk of releasing versus withholding. Confirmed: this comparison happened, and OpenAI's Safety Advisory Group concluded that even its own "robust" adversarial fine-tuning did not push gpt-oss-120b to "High capability" under its Preparedness Framework.

However, the **specific quantitative claims about how close the Chinese models came** to the fine-tuned gpt-oss's bio-uplift ceiling did **not** survive verification:
- "Chinese models needed no fine-tuning, just jailbreak prompts, to match fine-tuned gpt-oss's bio-uplift output" — refuted (1-2).
- "SecureBio found default Chinese models caught up to fine-tuned gpt-oss's bio-uplift ceiling" — refuted (0-3).
- "gpt-oss performed comparably to o3, only 3-5 points better than DeepSeek R1-0528 with browsing" — refuted (1-2).

**Read**: OpenAI's own model card *asserts* that Chinese open-weight substitutes are already close to the risk ceiling of a deliberately safety-stripped U.S. frontier model — which would be a striking, thesis-supporting data point if true. But this is OpenAI making the argument that its own release decision was low-marginal-risk, and none of the specific comparative figures survived adversarial review here. **This needs independent, non-lab-authored bio-uplift comparison data (e.g., from SecureBio directly, RAND, or a government body) before citing a "Chinese models already at the bio-uplift ceiling" claim in the paper.** The RAND source fetched in this pass ([PEA4886-1](https://www.rand.org/content/dam/rand/pubs/perspectives/PEA4800/PEA4886-1/RAND_PEA4886-1.pdf)) did not yield a confirmed cross-model claim and should be re-checked directly for a follow-up pass.

Sources: [OpenAI gpt-oss Model Safety PDF](https://cdn.openai.com/pdf/231bf018-659a-494d-976c-2efdfc72b652/oai_gpt-oss_Model_Safety.pdf).

## 4. Content moderation / censorship as a distinct failure mode

The CAISI finding above (DeepSeek echoing CCP propaganda/misinformation at 4x the U.S. rate) is the clearest confirmed evidence that **censorship and safety are separable failure modes** — a model can be simultaneously *more* restricted on politically sensitive topics and *less* restricted on safety-relevant harmful requests. Separately, independent third parties (outside any lab) had already publicly released "uncensored"/refusal-stripped versions of Chinese open-weight models (e.g., DeepSeek-R1 distills, Perplexity's "R1-1776") prior to and independent of any lab's own safety papers, showing that safeguard removal from Chinese open-weight models is an established, low-effort practice in the broader open-source ecosystem — consistent with, not contradicting, the CAISI jailbreak-compliance findings above.

## What was refuted (worth knowing what didn't hold up)

- "Safety fine-tuning safeguards can be trivially stripped — a general, country-agnostic vulnerability" — refuted (1-2). The confirmed data above is more mixed/asymmetric than this framing allows.
- "Chinese models needed no fine-tuning to match fine-tuned gpt-oss on bio-uplift" — refuted (1-2).
- "SecureBio found default Chinese models caught up to fine-tuned gpt-oss's bio ceiling" — refuted (0-3).
- "Removing gpt-oss's refusal training is easy and cheap" (as an isolated, unqualified claim) — refuted (0-3) — likely refuted on specificity/sourcing grounds rather than substance, since a closely related claim in section 1 above *did* survive; don't treat this as contradicting the confirmed version.
- "Fine-tuning GPT-4 via OpenAI's API removed RLHF safety with 95% success" — refuted (1-2).
- "gpt-oss performed comparably to o3, better than DeepSeek R1-0528 on SecureBio" — refuted (1-2).
- "OpenAI notes Chinese models already have public uncensored variants, paralleling the same U.S. tamper-resistance weakness" (i.e., the *symmetry* framing specifically) — refuted (1-2), consistent with CAISI's finding of a real asymmetry in outcomes even if the underlying vulnerability (fine-tuning removes safety training) exists on both sides.

## Synthesis: risk pass-through, or risk change?

The evidence leans toward **risk change, not clean pass-through** — but the two strongest data points pull in different directions on *mechanism*, and the paper should hold onto that distinction rather than collapse it into one story.

1. **Marginal uplift after maximal elicitation** (a well-resourced adversary who fine-tunes away safety training) does **not** clearly favor either country. TamperBench, the one controlled multi-family benchmark, found near-universal fragility and if anything found Qwen3's safety training held up *better* than Llama's. This is the threat model closest to the capability-gap analysis's "malicious fine-tuning" framing, and it does not support "Chinese substitutes are inherently less tamper-resistant."

2. **Baseline, no-fine-tuning-required jailbreak compliance** — the threat model most relevant to an unsophisticated bad actor, arguably the more policy-relevant case — shows a stark, repeated asymmetry: 94% vs. 8% jailbreak compliance; 12x more likely to comply with a hijacked malicious agent instruction; a 6–10% vs. 90%+ compliance gap between gpt-oss and DeepSeek specifically within the open-weight category. **This is real evidence that a Chinese open-weight substitute, even at comparable raw benchmark capability, may arrive with substantially weaker out-of-the-box safety behavior than the U.S. model it substitutes for.**

3. A candidate mechanism for (2), not yet tested directly: the compliance gap may reflect a difference in alignment **target**, not alignment **effort** — CAISI's finding that DeepSeek echoes CCP propaganda/misinformation at 4x the U.S. rate shows Chinese labs clearly do invest heavily in alignment, just toward a different content-moderation objective (state-approved content) than U.S.-defined "harmful" categories. If true, this has different policy implications than a simple "less safety investment" story — it's not that DeepSeek doesn't align its models, it's that what it aligns them *against* diverges from the categories CAISI (and this paper) care about.

If (2) holds up under further scrutiny — it needs a non-CAISI corroborating source given CAISI's institutional interest, see caveats below — it would meaningfully qualify the thesis: substitution isn't merely an origin-swap at constant risk; for the median user who won't fine-tune anything, switching to a Chinese substitute may raise realized risk even where raw capability is matched or lagging. That doesn't rescue "restraint reduces global risk" wholesale — a sophisticated, resourced adversary can still fine-tune away either model's safeguards per point (1) — but it means Research Question 2's answer is genuinely **conditional on adversary sophistication**, not a single yes/no.

## Caveats

- **Institutional-source bias.** The largest and most decisive dataset here (CAISI, section 2) comes from a U.S. government body evaluating a geopolitical/economic competitor's models. Not a reason to discard the findings — they're specific, quantified, and partially corroborated by independent secondary sources (MLex, TechRepublic) — but CAISI's benchmark selection, jailbreak-technique choice, and choice of "reference" U.S. models weren't independently audited here. Seek non-U.S.-government red-teaming of the same models before leaning on this heavily in the paper.
- **The two strongest pro-symmetry claims were refuted**, not just under-evidenced: both the generic "safety-stripping is a country-agnostic vulnerability" claim and OpenAI's own framing that U.S. and Chinese models share the "same" tamper-resistance weakness failed adversarial review (see refuted list above). That's a signal the asymmetry CAISI found isn't merely an artifact of CAISI's framing — independent claims explicitly asserting *no* asymmetry couldn't be substantiated either.
- **Bio-uplift evidence is the weakest-evidenced of the four themes.** Two of the seven refuted claims were specifically about Chinese models matching or exceeding fine-tuned-gpt-oss bio-uplift via jailbreaks alone; the surviving claim (marginal 3-5 point gap) is a narrower, hedged version. Hold biological-risk conclusions with more uncertainty than the jailbreak-compliance findings.
- **TamperBench's coverage gap**: its 21-model sweep didn't include any of the actual frontier models this paper cares about (GLM-5.2, DeepSeek V4-Pro, gpt-oss, Llama 4, Kimi K2) — the Qwen3-more-tamper-resistant-than-Llama pattern is suggestive but drawn from smaller/older models (0.6B–8B) and shouldn't be over-extended to frontier scale.
- **Capability convergence and safety divergence can coexist without contradiction.** UK AISI's "narrowing cyber capability gap" (prior pass) and CAISI's "20%+ capability gap on cyber/SWE tasks, plus 12x compliance gap" (this pass) use different metrics and timeframes — a model can be simultaneously catching up on raw capability while remaining dramatically easier to jailbreak into malicious compliance. Don't let a reader conflate these as the same claim.

## Open gaps for the next pass

1. **Independent (non-lab, non-CAISI) bio-uplift comparison** — the OpenAI-authored comparison didn't survive verification at the claim level; need SecureBio, METR, or UK AISI running the same eval suite directly and publishing a comparison, not a lab citing its own numbers.
2. **Reconcile the UK AISI "narrowing gap" finding (prior pass) with CAISI's "20%+ gap on cyber/SWE tasks" finding** — different metrics/timeframes, but the paper needs one paragraph explaining why these aren't contradictory.
3. **A frontier-scale, multi-family tamper-resistance benchmark** — TamperBench tops out at 8B models; nothing here tests fine-tuning-based safeguard removal on gpt-oss-120b, DeepSeek V4-Pro, or GLM-5.2 side by side.
4. **CAISI source-independence** — actively look for a UK AISI, EU, or academic replication of the jailbreak-compliance gap before treating it as established.
5. **Test the alignment-target-vs-effort hypothesis directly** — does DeepSeek's low refusal rate on Western-defined harm categories reflect under-investment in safety, or a differently-targeted alignment objective? These have different policy implications and aren't yet disentangled by any source found here.
6. **Adoption-weighted risk** — DeepSeek's ~1,000% download surge and cost advantage despite its capability lag and higher compliance-with-malicious-requests rate raises the question of what the exposure-weighted (not just per-model) risk actually looks like.
