# Restraint Without Return: Reassessing Unilateral Withholding of Frontier Open-Weight Models

## Thesis

U.S. policy debate over whether frontier labs should release open-weight models still implicitly treats American restraint as risk reduction. That assumption no longer holds. As Chinese open-weight models approach capability parity with U.S. closed frontier systems and now carry the majority of open-weight usage volume, unilateral U.S. withholding functions less as a brake on global risk and more as a transfer of market and standard-setting position in the open-weight space without a corresponding drop in the marginal capability available to a bad actor. This proposal is designed to test that claim empirically and to replace the current single-country restraint framework with one that prices in substitution.

## Problem statement

Frontier labs and their overseers (export control authorities, CAISI, internal safety teams) currently evaluate open-weight release decisions largely in isolation, asking whether *this model* clears a risk threshold, not whether withholding it changes the risk faced by the world. That framing made sense when U.S. labs were the only plausible source of near-frontier open weights. It is a weaker assumption now that Chinese providers account for a large and growing share of open-weight inference volume and sit within a few points of U.S. closed models on real world coding and reasoning workloads. If a withheld U.S. capability is available within months from a foreign substitute with comparable or weaker safeguards, restraint may be shifting *where* risk originates rather than *whether* it exists.

*Whether risk exists* = the total amount of dangerous capability available in the world.
*Where risk originates* = which country's model is the source of that capability.

## Research questions

1. **Substitution speed:** When a U.S. lab withholds or delays an open-weight release at a given capability tier, how quickly and completely does a foreign open-weight model fill the resulting gap in accessible capability?
2. **Risk pass-through:** Does the substitute preserve the same marginal risk profile as the withheld model, or does substitution change the risk itself — e.g., weaker filtering, a different fine tuning ecosystem, different failure modes?
3. **Strategic residual:** What does withholding actually buy the U.S. if not reduced global risk (like standard-setting influence, safety research leadership, alliance leverage) and does that residual value outweigh its measurable cost in ceded open-weight share?

## Method

- **Comparative capability-gap tracking.** Build a paired release-timeline dataset of U.S. and Chinese (and other) open-weight models on matched benchmarks (such as coding, reasoning, and available bio/cyber uplift evaluations) to measure the actual lag or parity window following a U.S. withholding decision.
- **Substitution case studies.** Select 3–4 instances where a U.S. lab held back or delayed a specific capability tier, and track adoption data (OpenRouter volume, Hugging Face downloads, developer surveys) to measure how quickly usage shifted to substitutes.
- **Uplift-equivalence testing.** Run the same uplift evaluation suite across a withheld model class and its nearest foreign substitute to test whether restraint changes the risk profile or only its country of origin.
- **Formal model.** Frame the release decision as a repeated game between labs/states with asymmetric costs of restraint, to identify the conditions under which unilateral restraint remains rational — chiefly, cases with no near-parity substitute, or where safeguard quality genuinely diverges.
- **Expert elicitation.** Structured interviews with export-control and frontier-safety officials (CAISI, BIS, UK AISI) to surface non-risk rationales for restraint and stress-test them against the substitution data (to come).

## Data sources

OpenRouter and Hugging Face usage telemetry; model cards and safety reports for comparably-tiered U.S. and Chinese models; BIS export control determinations (ECCN 4E091) and public comments; existing uplift evaluation suites (SecureBio, METR, UK AISI); structured interviews.

## Anticipated contribution

If substitution proves fast and near complete (as current OpenRouter share data suggest at the coding and reasoning tiers) the finding would reframe the policy question from "should the U.S. hold this release back" to "under what  *conditions* (such as safeguard quality, tamper-resistance, absence of an adequate substitute) is restraint still risk-reducing, versus merely status-ceding." That distinction bears directly on how a policy evaluation mandate is scoped and on how the compute threshold should be calibrated going forward.

## Scope and limitations

This is a positive, empirical question, not a normative claim that release should therefore go unrestricted. It isolates the risk-reduction rationale for restraint from other rationales (alliance management, industrial policy, soft power) that require separate treatment. The project also depends on usage and safety telemetry that is proprietary or incomplete for Chinese platforms; this constraint will be addressed through triangulation (such as through app-store rankings, cloud-provider disclosures, and citation counts) of capability proxies where direct telemetry is unavailable.
