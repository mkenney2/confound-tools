---
name: confound-check
description: Stress-test an experiment's design or results for confounds before believing, writing up or sharing them. Use when a result is about to go into a doc, README, post or message; when a new method "beats" a baseline; when planning an experiment; or when the user asks "is this legit?". Args: the claim(s) to check, or a doc/README path; empty = the most recent headline result in the conversation.
---

# Confound check

Goal: find the most plausible way each headline result could be true **without** its claimed cause, then settle the
serious candidates with cheap checks before anyone repeats the claim.

## 1. Pin down the claims

From the args (or the latest headline in the conversation / the named doc), list each claim as:

> **Claim:** X improves Y. **Metric:** M (how computed, file). **Compared against:** B. **Data:** which items, n.
> **Produced by:** script paths and result files.

Note which numbers are final vs partial, and which judge/model/prompt produced each arm's numbers. If the user is
planning an experiment rather than reporting one, state the planned comparison the same way.

## 2. Independent review

Spawn the `confound-tools:confound-reviewer` agent (listed as `confound-reviewer` if installed outside the plugin; fresh context, so it does not inherit the framing that produced the result).
Give it the claim cards, the paths to the scripts and result files, and **not** your opinion of whether the result
holds. For a large set of claims, one reviewer per claim, in parallel.

While it runs, do your own pass over the high-yield list below; these are the ones that have actually bitten:

- **Retention / length artifacts:** if the metric is "any error per output" or "target appears anywhere", any filter
  that shortens outputs moves it. Compare arms at matched retention and against a random-drop baseline.
- **Mismatched judges or prompts across arms:** every arm must be scored by the same judge, prompt and settings.
- **Selection by one noisy measure, evaluation by another:** regression to the mean favours the second measure on a
  subset chosen by the first. Select and evaluate with the same measure, or report both selections.
- **Answer leakage:** any field the method sees that encodes the label (answer keys, source text, gold spans).
- **Tuned on test:** thresholds or weights fit on the reported data; use a held-out split.
- **Partial or mixed numbers:** half-finished runs, caches mixing old and new settings, double-counted records.
- **Missing baselines:** "more of the same" (more samples, a stricter own-only threshold) and random.
- **Mechanism by aggregate:** a story about *why* that nobody checked on raw examples.
- **Small critical n:** the decisive subset has only dozens of positives.

## 3. Verify before reporting

For each high or medium finding, run the cheapest check that settles it, preferring existing data:
re-slice at matched retention, recompute with one judge, add a random baseline, re-select the subset, count the
positives, read 5-10 raw examples. Ask the user before anything that costs money (paid API judging, GPU pods) and give
a cost estimate.

Drop findings the check refutes. Do not report a confound as real without either a check or an explicit
"unverified".

## 4. Report

Lead with the verdict per claim: **survives**, **weakened** (say what the corrected claim is), **artifact**, or
**unresolved** (say what would settle it and its cost). Then the confirmed findings, each with the numbers that
settled it, ranked by how much they change the conclusion. Give the corrected one-sentence headline to use instead.

If a correction is needed in something already written (doc, README, post), say exactly which statements change and
offer to update them; credit whoever raised the issue. Do not edit shared or published material without the user's
go-ahead.
