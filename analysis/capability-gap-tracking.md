# Comparative Capability-Gap Tracking

### 1. gpt-oss: OpenAI's release, not withholding, decision (high confidence)

OpenAI released **gpt-oss-120b** (116.8B total / 5.1B active params) and **gpt-oss-20b** (20.9B total / 3.6B active params) on **August 5, 2025** under Apache 2.0 plus a usage policy, with full chain-of-thought access — its first open-weight release since GPT-2 (2019).

The decision point is directly documented: OpenAI ran internal **"malicious fine-tuning" (MFT)** testing — deliberately fine-tuning gpt-oss-120b to maximize bio/chem and cyber uplift — and released openly only after concluding the adversarially-tuned model still stayed below the Preparedness Framework's "High" capability threshold in both domains, and stayed below OpenAI's own o3 model on the same evaluations. In this case, **the company resolved toward release** after a safety gate, not toward withholding.

Sources: [OpenAI gpt-oss model card](https://openai.com/index/gpt-oss-model-card/), [arXiv:2508.03153](https://arxiv.org/pdf/2508.03153) — vote: unanimous (3-0, 2-1, 3-0, 3-0, 3-0 across merged sub-claims).

### 2. Llama 4: U.S. release-timeline anchor, but not fully open (high confidence)

Meta released **Llama 4 Scout** (17B active / 109B total, 16-expert MoE) and **Llama 4 Maverick** (17B active / 400B total, 128-expert MoE) on **April 5, 2025**, weights on GitHub/Hugging Face same day. Both ship under the custom **Llama 4 Community License** — not Apache 2.0/MIT — which bars unlawful use, use beyond supported capabilities, and restricts EU-domiciled entities on multimodal use. 

Llama 4's openness tier sits meaningfully below fully-permissive licenses, which complicates a clean "U.S. open vs. China open" comparison — the U.S. side of the pair is itself only partially open.

Sources: [Meta Llama 4 model card](https://github.com/meta-llama/llama-models/blob/main/models/llama4/MODEL_CARD.md), [HF release blog](https://huggingface.co/blog/llama4-release) — vote: unanimous (3-0 across 7 merged sub-claims).

### 3. DeepSeek-V3: released, not withheld, custom license (medium confidence)

DeepSeek released **DeepSeek-V3** (671B total / 37B active, MoE), technical report dated **Dec 27, 2024**, checkpoints published openly on GitHub/HF. Weights sit under a custom **DeepSeek License Agreement v1.0**, not MIT/Apache (the code, separately, is MIT). DeepSeek's own paper claims performance parity with GPT-4o and Claude 3.5 Sonnet — self-reported, flagged accordingly.

**DeepSeek-V3-0324** (~March 2025), was released under a *different*, fully MIT-licensed repo.

Sources: [arXiv:2412.19437](https://arxiv.org/pdf/2412.19437), [DeepSeek-V3 license](https://huggingface.co/deepseek-ai/DeepSeek-V3/blob/main/LICENSE-MODEL) — vote: 3-0 (license), 2-1 (specs/release), 2-1 (parity framing).

### 4. Qwen3-235B-A22B: lone surviving Chinese benchmark datapoint (medium confidence)

On Lambda's independent leaderboard, **Qwen3-235B-A22B** (Alibaba) scores **69.5% LiveCodeBench**, **80.6% MMLU-Pro**. This is the *only* Chinese-model benchmark figure that survived verification — no paired U.S. model score (Llama 4, gpt-oss) survived alongside it, so a genuine side-by-side capability table isn't yet supportable from verified claims alone. A companion DeepSeek-R1-Distill-Llama-70B claim on the same leaderboard was refuted (0-3).

Source: [Lambda LLM Benchmarks Leaderboard](https://lambda.ai/llm-benchmarks-leaderboard) — vote: 2-1.

### 5. UK AISI: the substitution gap is narrowing on cyber capability (high confidence)

UK AISI's **July 2026** evaluation found the open/closed capability gap is compressing over time, and Chinese labs are the ones compressing it:

- Open-weight models released **Jan–Sept 2025** lagged frontier closed models by **6–10 months** on cyber-risk evaluations.
- By **mid-2026**, recent open-weight models — specifically China's **GLM-5.2** (June 2026, Zhipu/Z.ai) and **DeepSeek V4-Pro** — lag by only **4–7 months**.
- On narrow cyber tasks: GLM-5.2 matches Anthropic's Opus 4.6 (Feb 2026, ~4-month lag) and OpenAI's GPT-5.3-Codex; DeepSeek V4-Pro matches Opus 4.5 (Nov 2025, ~5-month lag).
- A wider ~7-month gap holds on longer-horizon Cyber Ranges tasks.

Source: [UK AISI — How Far Behind the Frontier are Leading Open Weight Models on Cyber?](https://www.aisi.gov.uk/blog/how-far-behind-the-frontier-are-leading-open-weight-models-on-cyber) — vote: unanimous (3-0 across 3 merged sub-claims).

## What did not survive verification

- OpenAI claiming existing open-weight models already match MFT-tuned gpt-oss's worst-case capability (would have undercut the "withholding matters" framing directly) — refuted 0-3.
- DeepSeek license "bans military use" framing as comparable to RAIL-style restrictions — refuted 1-2.
- DeepSeek-R1-Distill-Llama-70B's specific Lambda leaderboard scores — refuted 0-3.
- DeepSeek-V3's low training-compute claim (2.788M H800 GPU-hours) as evidence of cost efficiency — refuted 0-3.
- DeepSeek-V3's MMLU/GPQA scores as claimed in its own paper — refuted 1-2 (self-reported, didn't hold up to scrutiny).
- OpenAI's direct MFT-gpt-oss vs. Chinese-model benchmark comparison — refuted 1-2.

## Open gaps for the next research pass

1. **No confirmed U.S. withholding-without-release case.** The dataset only substantiates gpt-oss as release-after-safety-gating, not an instance of actual withholding. No verified GPT-2-precedent claim, BIS export-control withholding claim, or Meta capability-gating claim. This is a real gap against Research Question 1 as scoped in the README — need a dedicated search pass on this specifically (secondary sources like the-decoder.com and transformernews.ai were fetched but their claims didn't survive the top-25 verification cut; worth re-running with them prioritized).
2. **No genuine matched benchmark table.** Only one Chinese datapoint (Qwen3) survived, no U.S. counterpart alongside it. No confirmed SWE-bench Verified, GPQA Diamond, AIME, or HumanEval figures for any model.
3. **No cross-lab bio-uplift comparison.** Bio/cyber evidence exists only for gpt-oss (OpenAI's own MFT testing) and the AISI cyber-gap finding — no equivalent bio-risk comparison survived.
4. **Usage/adoption data unverified** (see section above).


UK AISI's cyber-capability lag measurement — points the same direction as the thesis: the gap between what a bad actor can get from an open-weight substitute and the closed frontier **shrank from 6–10 months to 4–7 months in under a year**, and the labs closing that gap are Chinese. This finding supports the claim that restraint is shifting where risk originates rather than whether it exists; if a substitute keeps arriving faster, withholding buys progressively less time. Whether the AISI-measured cyber-capability gains come with weaker safeguards, different fine tuning ecosystems, or different failure modes than the withheld U.S. model would have had is untested here.

Two of the three U.S./China model pairs found (Llama 4 vs. DeepSeek-V3) both carry **custom, non-fully-open licenses**, not the permissive open-source "open-weight." Both the United States and other countries gate their releases to some degree. I focused on the AISI finding and DeepSeek V4-Pro case because it compares actual open, no-restriction releases.
