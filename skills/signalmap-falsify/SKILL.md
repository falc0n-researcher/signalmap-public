---
name: signalmap-falsify
description: Stress-test investigation hypotheses from SignalMap evidence, preferring to falsify weak theories rather than prove everything vulnerable. Use when the user wants to check which investigation paths survive scrutiny.
---

# Falsify my investigation paths

Using **only** the SignalMap context package provided, stress-test the
interesting relationships and investigation hypotheses for this target.

If no package has been provided, ask for one:

```bash
signalmap context <host> --lens hypotheses --write hypotheses.md
```

The package's `signals` section — especially any `investigation_path` — is
where the candidate hypotheses are. Each already carries what was observed
and what is unknown.

## For each hypothesis, give

- the hypothesis
- observed evidence supporting it (with ids)
- inferred relationships supporting it
- the assumptions being made
- missing evidence
- contradictory evidence
- confidence
- what would prove the hypothesis **wrong**
- the smallest authorized human test that would resolve it

## Stance

Prefer **falsifying weak theories** over proving everything. A hypothesis you
can cheaply disprove is more valuable to eliminate than a strong one is to
restate. Do not convert a hypothesis into a finding unless the evidence in
this package actually supports that conclusion — and it rarely will, because
SignalMap tested nothing.

## Rules

- Use only this package.
- Confidence must track the evidence: an inference across two observations is
  not a fact, and a signal is attention, not a verdict.
- No exploit payloads. The output of a resolved hypothesis is "worth a human
  test" or "eliminated", never "vulnerable".
