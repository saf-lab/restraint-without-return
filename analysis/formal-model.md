# Formal Model: Restraint as a Repeated Release-Timing Game

Method item 4: frame the release decision as a repeated game between labs/states with asymmetric costs of restraint, to identify when unilateral restraint remains rational.

## Relation to existing literature

No paper found in a dedicated literature check builds this specific model, but three lines of prior work bound it. Armstrong, Bostrom & Shulman's "Racing to the Precipice" (2013/2016) is the canonical AI-race model: n teams draw a capability level and choose a safety level, the highest capability-minus-safety score wins, and disaster is realized once, at the moment of deployment. That model is about whether to *finish first*; this one is about whether to *disclose weights* once a capability already exists, and it replaces ABS's single realized-risk event with a discounted, indefinite-horizon risk stream — closer in spirit to Han et al.'s (2020) repeated/evolutionary extension of race dynamics, though that paper is also about development speed, not release timing. Jensen, Emery-Xu & Trager's "Safety Tax" (2023) and Xu, Wang, Chen & Xie's "Economics of AI Foundation Models" (2025) are the closest formal treatments of safety-spending and openness, respectively, as strategic choices — but neither models a cross-actor *safeguard-quality asymmetry* conditional on similar capability, which is the variable this project's empirical work (see [uplift-equivalence.md](uplift-equivalence.md)) shows actually distinguishes U.S. and non-U.S. releases. Bostrom, Douglas & Sandberg's unilateralist's curse (2016) supplies the normative reason individually-rational restraint by one actor can be collectively suboptimal, but has not, as far as this check found, been applied formally to AI open-weight release. Gomes (2026), "The Open-Weight Paradox," argues the same directional thesis as this project qualitatively (via the DeepSeek-R1 case) but without a threshold model. This section fills that specific gap: a closed-form condition for when a substitution lag is long enough to make restraint risk-reducing, calibrated against this project's own empirical findings.

## Setup

A U.S. lab decides, for each newly-feasible capability tier, whether to release its open weights (**R**) or withhold them (**W**). A competitive, largely non-strategic "rest of world" (RoW) fringe — the case studies in [substitution-case-studies.md](substitution-case-studies.md) show multiple non-U.S. labs shipping in parallel (Kimi K2, Qwen3, GLM-4.5 all within the same month) rather than acting as a single coordinated player — will eventually reach the same capability tier regardless of what the U.S. lab does, after some lag **L**.

- If the U.S. releases: the tier becomes available immediately, carrying the U.S. lab's own safeguard quality, **s_US** ∈ [0,1] (refusal robustness, tamper-resistance — the fraction of otherwise-available risk that safety training actually suppresses).
- If the U.S. withholds: the tier is unavailable for **L** periods, then becomes available via the RoW substitute at safeguard quality **s_RoW** < s_US — a gap this project's empirical work (CAISI's DeepSeek jailbreak-compliance data, the Kimi K2.5 Hack The Box result) has documented directly, not assumed.

Once weights are public, in either case, they cannot be recalled — a point the Anthropic Fable/Mythos case made concretely (open-weight models "cannot be shut off" once downloaded, per PIIE's Chorzempa). So both the release and withhold paths generate an *indefinite* risk stream once triggered; the question is only when each stream starts and at what rate.

## The core result: a lag threshold, not a binary condition

Let **v** be the tier's dangerous-capability value and **r** a discount rate reflecting that near-term risk exposure matters more than distant exposure (more actors have time to exploit it; or conversely, defenses have time to catch up — either justification supports r > 0). Discounted total risk under each policy:

- **Release**: risk starts at t=0 at rate v(1−s_US), forever → discounted total = v(1−s_US)/r
- **Withhold**: risk starts at t=L at rate v(1−s_RoW), forever → discounted total = v(1−s_RoW)·e^(−rL)/r

Restraint reduces discounted risk exactly when v(1−s_RoW)e^(−rL) < v(1−s_US), which rearranges to:

**L > L\* = (1/r) · ln[(1−s_RoW) / (1−s_US)]**

This is the paper's central formal result. Two things follow immediately from it that sharpen — and in one case correct — the README's original framing of "chiefly, cases with no near-parity substitute, or where safeguard quality genuinely diverges":

**1. "No near-parity substitute" is the special case L → ∞.** If no substitute is expected to arrive at all, the threshold is trivially cleared regardless of the safeguard gap. This is the cleanest, least ambiguous case for restraint — and also the rarest one this project's case studies actually found. In every documented instance (OpenAI's own paper on gpt-oss, the AISI cyber-gap trend, the Kimi K2.5 evaluation), a near-parity substitute existed or was close.

**2. Safeguard divergence is not, by itself, pro-restraint — this is a real correction to the README's framing.** L\* is *increasing* in the safeguard gap: the larger (1−s_RoW) is relative to (1−s_US), the *longer* a lag is required before restraint pays off, not shorter. The intuition: a wider gap means the eventual substitute is more dangerous, so deferring toward it is a worse proposition, not a better one — restraint only pays off if the delay is long enough to offset that eventual worse outcome. A large, well-documented safeguard gap paired with a short lag is the single worst combination in this model: it means restraint mostly just delays the arrival of the more dangerous version, without meaningfully reducing total exposure. This directly matters for how the paper should read its own uplift-equivalence findings — a big CAISI-style compliance gap is not on its own evidence that restraint helps; it raises the bar restraint has to clear.

## Calibration against this project's empirical findings

Plugging in real numbers from the project's own research, rather than leaving L\* abstract:

**Safeguard gap** — from [risk-pass-through.md](risk-pass-through.md) and [uplift-equivalence.md](uplift-equivalence.md), CAISI's blended cross-domain figure for DeepSeek under a public jailbreak is roughly 94% compliance vs. ~8% for U.S. closed/open models, i.e. s_US ≈ 0.92, s_RoW ≈ 0.06. Ratio (1−s_RoW)/(1−s_US) = 0.94/0.08 ≈ 11.75, ln(11.75) ≈ 2.46.

**Substitution lag** — UK AISI's cyber-specific figure: 4–7 months as of mid-2026, narrowing from 6–10 months in 2025; Epoch AI's broader capability-gap tracker: 4–14 months, averaging ~7 months, since 2023.

| Discount rate r (per year) | Required L\* | Empirical L (AISI cyber, mid-2026) | Restraint risk-reducing? |
|---|---|---|---|
| 0.5 (low urgency) | ~4.9 years | 4–7 months | No — L\* far exceeds observed lag |
| 1.0 | ~2.5 years | 4–7 months | No |
| 2.0 (high urgency) | ~1.2 years | 4–7 months | No |

Across a wide range of plausible discount rates, the empirically observed substitution lag (months) falls well short of the threshold the model says restraint needs to clear (well over a year, even at aggressive discounting). This is not a claim that restraint never pays off in risk terms — at sufficiently low r and a sufficiently narrow safeguard gap, L\* falls into the empirically observed range — but it shows the current combination of a wide, documented safeguard gap and a short, documented lag is specifically the regime where this model says restraint underperforms release on pure risk-reduction terms. The one clean exception the case studies found (Case 1 in [substitution-case-studies.md](substitution-case-studies.md)) is gpt-oss itself: OpenAI's own paper found L ≈ 0 — the substitute already existed before release — which trivially fails the L > L\* test regardless of r or the safeguard gap, consistent with OpenAI's actual decision to release rather than continue withholding.

## Repeated-game extension: restraint can erode its own rationale

The single-period model treats L as exogenous, but the case-study evidence suggests it isn't. UK AISI's own finding — the cyber lag narrowed from 6–10 months to 4–7 months within about a year — is consistent with RoW's incentive to close the gap increasing precisely because the U.S. side keeps signaling it will eventually release or that a gap is worth capturing. A disclosed 2022 internal OpenAI email (reported via 2026 litigation disclosures, not independently verified — see [substitution-case-studies.md](substitution-case-studies.md)) makes the mechanism explicit from the other direction: Altman reportedly argued for releasing an open model specifically to *discourage* competitors from investing in matching it. Read together, these suggest L_t is a decreasing function of the cumulative history of U.S. withholding: each additional period of restraint is itself a market signal that increases RoW's expected payoff to closing the gap, shortening L for the *next* tier. This creates a dynamic-consistency problem for a pure risk-minimization strategy: restraint that is justified today (L_t > L\*) can undermine the empirical basis for its own future justification by accelerating the very substitution it was trying to outlast. A policy of *sustained* restraint across many periods is a stronger, harder-to-satisfy claim than restraint being justified in any single period, because it requires L\* to stay below a shrinking L_t — not just today, but on a trajectory that current evidence (AISI's narrowing-gap finding) says is moving in the wrong direction.

## Bringing in the strategic residual (RQ3)

The risk-only model above answers RQ1–RQ2 (substitution speed and risk pass-through) but not RQ3: what restraint buys beyond risk reduction, and whether that's worth its cost. Extend the objective to a U.S. policymaker's own interest, not global welfare: let **B** be the non-risk residual value of withholding (standard-setting influence, safety-research leadership, alliance leverage) and **C** be the cost of ceding open-weight market/mindshare (the OpenRouter and Hugging Face share data in [substitution-case-studies.md](substitution-case-studies.md) gives this a real empirical anchor). Restraint is rational from the U.S.'s own interest-weighted perspective when:

**B − C > w · [Risk(withhold) − Risk(release)]**

where w is the weight placed on global risk reduction relative to national interest. This nests the risk model cleanly: if L > L\* (the risk term is negative — restraint reduces risk), the right-hand side is negative and restraint is rational for almost any B ≥ C, even a small one. If L < L\* (today's empirically calibrated case), the right-hand side is positive, and restraint is rational for the U.S. only if the standard-setting/alliance/leadership residual genuinely exceeds *both* the ceded-market cost and the risk-weighted cost of the extra global risk created by waiting for a worse-safeguarded substitute to arrive. This is precisely the empirical burden of proof RQ3 was designed to test, now given a specific threshold rather than an open question — and it means the strategic-residual case for restraint gets *harder* to clear, not easier, exactly when the risk case for restraint is weakest.

## Limitations

- **r is a free parameter, not an empirically estimated one.** No source in this project's research pins down a defensible discount rate for AI-risk exposure specifically; the sensitivity table above is offered as a range, not a point estimate, and the qualitative conclusion (restraint underperforms at current L and safeguard-gap values) is robust across that range but would flip at low enough r combined with a narrower safeguard gap.
- **v (capability value) is assumed constant across the comparison**, but general capability progress means v is actually rising over the delay window — this would tend to *increase* the case against long withholding (a longer wait sees a more dangerous tier released either way), a direction not incorporated here.
- **The model treats RoW as a competitive, non-strategic fringe.** The case studies (multiple non-U.S. labs releasing within the same weeks) support this for the current empirical period, but a single dominant non-U.S. actor with its own strategic release-timing decision would turn this into a genuine two-sided repeated game rather than a one-sided optimal-stopping problem against an exogenous-ish arrival process — a natural extension, not attempted here.
- **Safeguard quality s is treated as a scalar**, collapsing refusal robustness, tamper-resistance, and capability-uplift-specific behavior into one number; [uplift-equivalence.md](uplift-equivalence.md) shows these can diverge within a single model (e.g., gpt-oss's inconsistent refusal scores across different evaluation methodologies), which a fuller model would need to disaggregate.
- **No paper was found applying the unilateralist's curse formally to AI open-weight release** (per the literature check) — this model's L\* threshold is one way to operationalize that concept for this specific decision, not a citation of prior formal work doing so.

## Synthesis

The formal result reframes the paper's three research questions as three inputs to one threshold rather than three separate questions: RQ1 (substitution speed) supplies L, RQ2 (risk pass-through) supplies the safeguard-gap ratio that sets L\*, and RQ3 (strategic residual) supplies B and C, which only need to clear a bar once the risk term is already unfavorable. Calibrated against this project's own empirical findings — short lags (months, per AISI and Epoch AI) and a wide, well-documented safeguard gap (CAISI's compliance data, the Kimi K2.5 HTB result) — the model's own logic says restraint is currently operating in the regime where it underperforms release on pure risk-reduction grounds, and where the strategic-residual case has to do correspondingly more work to be worth the ceded position. The repeated-game extension adds a further complication: continued restraint is not just failing to clear today's threshold, it may be actively shortening the lag that any future case for restraint would need to rely on.
