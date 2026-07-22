# Uplift-Equivalence Testing

Method item 3: does running the same uplift evaluation suite across a withheld/gated U.S. model class and its nearest non-U.S. open substitute show a changed risk profile, or only a changed country of origin?

## 1. Bio/chem: same-suite comparisons exist, and point toward "same capability, different willingness"

**OpenAI's own malicious-fine-tuning (MFT) testing on gpt-oss** is the most rigorous same-suite comparison located. OpenAI ran the SecureBio battery (VCT, HPCT, MBCT, WCB) plus ProtocolQA, Tacit Knowledge, Gryphon, and TroubleshootingBench on adversarially fine-tuned gpt-oss-120b *and* on DeepSeek-R1-0528, Qwen3 Thinking, and Kimi K2. Result: "gpt-oss-120b does not significantly advance the state of the art" relative to those non-U.S. open models — and OpenAI's own stated rationale for releasing gpt-oss openly was explicitly a substitute-availability argument: existing open models already came close to matching MFT-maximized capability. (High confidence — [OpenAI, arXiv:2508.03153](https://arxiv.org/html/2508.03153v1); [gpt-oss model card, arXiv:2508.10925](https://arxiv.org/html/2508.10925v1); OpenAI's exact numeric Preparedness Framework thresholds are not public, per [METR's independent methodology review](https://metr.org/blog/2025-10-23-gpt-oss-methodology-review/).)

**An independent, non-lab academic evaluation of Kimi K2.5** ([arXiv:2604.03121](https://arxiv.org/abs/2604.03121)) ran VCT, LAB-Bench, ABC-Bench, and FORTRESS on Kimi K2.5 (non-U.S., open) against Claude Opus 4.5 and GPT-5.2 Pro (both U.S., closed). Finding: "biorisk-relevant capability levels similar to Claude Opus 4.5 and GPT-5.2" — but "substantially lower refusal on CBRNE-relevant requests." This is the single strongest piece of evidence for the thesis's exact claim: comparable raw capability, materially weaker refusal.

**CAISI's September 2025 evaluation of DeepSeek**, read directly from the primary PDF ([nist.gov/CAISI DeepSeek report](https://www.nist.gov/system/files/documents/2025/09/30/CAISI_Evaluation_of_DeepSeek_AI_Models.pdf)), measured compliance on a harmful-biology query set:

| Condition | GPT-5 | Opus 4 | gpt-oss | DeepSeek V3.1 | DeepSeek R1-0528 | DeepSeek R1 |
|---|---|---|---|---|---|---|
| No jailbreak | ~0% | ~0–2% | ~0–3% | ~26% | ~18% | ~42% |
| Public jailbreak | ~0% | ~0% | ~0% | ~96% | ~91% | ~100% |

CAISI's own summary: DeepSeek models "assisted with a majority of evaluated malicious requests in domains including harmful biology, hacking, and cybercrime when the request used a well-known jailbreaking technique."

**A third data point, BioTIER** ([arXiv:2607.14479](https://arxiv.org/html/2607.14479v1), 52 models), found DeepSeek-V3.1 the lowest-refusal model of the set (5.6% compliance-refusal), with other non-U.S. open models (Qwen3.7 Max, GLM 5.2, Kimi K2.5/K2.6) clustered around 30–45% — but also scored gpt-oss (U.S., open) at a middling 47.1%, in tension with CAISI's near-0% figure for gpt-oss on a different, jailbroken prompt set. The two studies use different methodologies and aren't strictly contradictory, but they portray gpt-oss's refusal robustness very differently and this project could not reconcile the gap. That weakens a clean "it's nationality, not open-weight status, that drives the refusal gap" reading — open-weight status generally correlates with weaker refusal, with DeepSeek a substantial outlier even among open models.

## 2. Cyber: a genuine matched benchmark table now exists

**CAISI's May 2026 evaluation of DeepSeek V4-Pro** ran CTF-Archive-Diamond (285 hard CTF challenges from ASU's pwn.college) across both U.S. closed and non-U.S. open models on identical tasks:

| Model | Origin | Score |
|---|---|---|
| GPT-5.5 | U.S., closed | 71% |
| Opus 4.6 | U.S., closed | 46% |
| GPT-5.4 mini | U.S., closed | 32% |
| DeepSeek V4-Pro | Non-U.S., open | 32% (imputed via IRT from a sample, not fully measured) |

DeepSeek V4-Pro ties the smallest U.S. model tested but sits well below Opus 4.6 and GPT-5.5 — a **larger gap** than UK AISI's aggregate "4–7 months behind" framing implies (see [capability-gap-tracking.md](capability-gap-tracking.md)), a tension this project flags rather than resolves.

**The independent Kimi K2.5 evaluation** ([arXiv:2604.03121](https://arxiv.org/abs/2604.03121)) ran four separate cyber benchmarks against GPT-5.2, Claude Opus 4.5, and DeepSeek V3.2:

- **CyberGym** (real-world C/C++ exploit generation): Kimi K2.5 40.8%, DeepSeek V3.2 17.3%, vs. Claude Opus 4.5 50.6% and GPT-5 60.2% — both non-U.S. models trail both U.S. models here.
- **DFIR-Metric** (digital forensics, CTF-solved of 34): GPT-5.2-pro 92.2%, GPT-5.2 69.6%, Claude Opus 4.5 65.7%, Kimi K2.5 50.0%, DeepSeek V3.2 39.2%. Kimi actually scores *highest* on raw factual MCQ accuracy (94.2%) but falls behind on applied forensic reasoning — capability composition doesn't move uniformly.
- **Cybench** (hard CTF): Kimi K2.5's point estimate was lowest of the four models tested, though the paper states confidence intervals overlap; on the hardest tier specifically, Kimi solved two more tasks than Claude Opus 4.5.

**The sharpest single finding in this pass** comes from the same paper's autonomous Hack The Box pentesting test (n=3 retired boxes, no human guidance): **Kimi K2.5 compromised 3/3 machines, matching Claude Opus 4.5's 3/3. DeepSeek V3.2 scored 0/3. GPT-5.2 was excluded entirely because it refuses to perform offensive security actions.** The authors' own framing: "safety guardrails in closed-source models can effectively prevent cyber offense use, a protection unavailable in open-weight models." This is a cyber-specific analog to CAISI's bio/jailbreak finding above, from an independent, non-institutional source — and it isolates *willingness* rather than *capability* as the variable that moved, which is close to the cleanest evidence this project has found for the "restraint shifts where risk originates, not whether it exists" mechanism. Caveat: the authors themselves flag n=3 as "illustrative, not statistically robust," and contamination testing showed zero models (including Kimi) succeeded on boxes without public writeups, so capability vs. memorization isn't fully separated.

**OpenAI's gpt-oss cyber MFT testing** is narrower than the bio comparison: the paper states it lacked capacity to run MFT on every comparison model for cyber, so **no non-U.S. open model was fine-tuned or directly compared on cyber tasks at all** in that study — only against OpenAI's own closed o3 (MFT gpt-oss: 24.8% vs. o3: 27.7% on professional CTFs; cyber-range success ~0% for every model tested). This corrects an earlier overstatement in [risk-pass-through.md](risk-pass-through.md), which characterized that paper's model set as applying uniformly across both bio and cyber — it does not.

## 3. Non-U.S., non-Chinese substitutes: the gap is real, not just unsearched

A dedicated pass specifically targeting Mistral (France), Falcon/TII (UAE), Cohere (Canada), AI21 (Israel), and Sakana (Japan) found **no genuine uplift evaluation — bio/chem or cyber — for any of them**, and this is not merely an absence of search hits:

- Mistral and TII are two of six Seoul Summit signatories that never published a frontier safety framework at all ([SaferAI Frontier Risk Management Tracker](https://tracker.safer-ai.org/methodology/)). Epoch AI independently confirms Mistral has released no biorisk evaluation information ([Epoch AI](https://epoch.ai/gradient-updates/do-the-biorisk-evaluations-of-ai-labs-actually-measure-the-risk-of-developing-bioweapons)).
- Cohere published a framework but explicitly excludes CBRN from its scope, and SaferAI's assessment states directly "no evidence that Cohere publishes uplift evaluations."
- AI21's own Jamba whitepaper reports a refusal-propensity score, not a comparative uplift trial.
- TII's current flagship (Falcon-H1R, 7B) is no longer frontier-scale by parameter count, making the "no uplift eval" finding for Falcon somewhat moot — there's currently no TII model that would qualify as frontier-class in the first place.
- What exists *instead* of uplift data for Mistral is a jailbreak-susceptibility red-team report (Enkrypt AI, May 2025: Pixtral models up to 40x more likely than GPT-4o/Claude 3.7 to produce dangerous CBRN information on a 98%-success prompt set) — categorically different from an uplift evaluation and should not be cited as one.
- On general capability terms, this gap isn't explained by these labs being sub-frontier: Mistral Large 3 and Cohere Command A+ sit within roughly a 10-point band of DeepSeek-V3.2, gpt-oss-120b, and Llama 4 Maverick on Artificial Analysis's Intelligence Index — plausibly frontier-class by capability, just never evaluated for uplift by anyone.

This confirms the concern flagged in the project's terminology convention: "non-U.S." data on uplift equivalence is, in practice, entirely China-specific. The one exception — Kimi K2.5's independent evaluation — is itself a Chinese model. No non-Chinese non-U.S. lab has been uplift-tested by anyone, lab-internal or independent.

## What was refuted or could not be reconciled

- BioTIER's gpt-oss compliance score (47.1%) conflicts with CAISI's near-0% figure for gpt-oss under jailbreak — different methodologies, unreconciled, noted above.
- A search result implying Cohere's framework defines a CBRN "material uplift" threshold conflicts with SaferAI's primary-source assessment that Cohere excludes CBRN entirely; treated as unreliable pending direct document verification.
- UK AISI's cyber-eval task count is inconsistently reported across its own publications (70 vs. 95/96) — likely the suite grew over time and AISI deliberately uses an older subset for cross-time comparability, but this wasn't fully confirmed from a single methodology document.

## Open gaps for the next research pass

1. **No withheld-then-matched instance for cyber specifically.** As with the capability-gap-tracking document's open gap #1, no documented case exists of the U.S. gating a specific cyber capability tier that a non-U.S. open release then matched within a measurable window. The Kimi K2.5 HTB finding is evidence of a *willingness* gap, not an *availability* gap — a distinct mechanism.
2. **No tamper-resistance study for cyber uplift.** No controlled experiment tests whether safety fine-tuning is more easily stripped from a non-U.S. cyber-capable model than a U.S. one; the closest evidence (GPT-5.2's outright refusal vs. Kimi K2.5's unrestricted baseline, and DeepSeek V4-Pro's refusals being bypassed by simple retries) shows a baseline-willingness gap, not a fine-tuning-ecosystem gap.
3. **Non-Chinese non-U.S. uplift data remains entirely absent**, confirmed rather than merely unsearched — see Section 3. Any future "non-U.S." framing on uplift specifically should say "China" until this changes.
4. **OpenAI's numeric Preparedness Framework thresholds are not public**, limiting independent verification of "High capability" judgment calls (flagged by METR).
5. **CAISI dropped bio/chem/CBRN testing entirely from its 2026 DeepSeek V4-Pro evaluation**, an unexplained scope narrowing from its 2025 evaluation.
6. **Kimi K2.5's Cybench bar-to-model mapping could not be reliably extracted** from the source PDF; the qualitative finding (lowest point estimate, overlapping confidence intervals) is confirmed but exact figures are not.

## Synthesis

Where a genuine same-suite comparison exists — OpenAI's own MFT testing, CAISI's DeepSeek and DeepSeek V4-Pro evaluations, and the independent Kimi K2.5 study — the pattern is consistent across both bio/chem and cyber: **raw capability is close to parity, and the measurable gap is in refusal/willingness, not underlying uplift potential.** The Hack The Box pentesting result (GPT-5.2 refuses outright; Kimi K2.5 matches a U.S. closed model with no fine-tuning required) and CAISI's bio jailbreak-compliance gap (94–100% vs. ~0% under a public jailbreak) are the two cleanest instances of this project's core mechanism: restraint on release does not remove the capability from circulation, it removes the refusal layer that a U.S. lab would have shipped alongside it.

This complicates the "restraint buys nothing" framing in one direction and sharpens it in another. It's not that a withheld U.S. model and its non-U.S. substitute carry identical risk — CAISI's DeepSeek cyber score (32%) trailing GPT-5.5 (71%) shows real capability gaps still exist in places. It's that where substitutes are near-parity on capability, the marginal risk added by "restraint" is concentrated almost entirely in the substitute's weaker refusal training, which is a safeguard-quality question, not an availability question. That reframes the policy lever: the evidence here argues for evaluating and pressuring safeguard quality on substitutes (an achievable target) rather than treating U.S. restraint on release as the risk-reducing move (a much weaker one once a near-parity substitute exists).
