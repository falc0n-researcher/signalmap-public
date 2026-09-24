---
name: signalmap-changes
description: Explain what meaningfully changed between two SignalMap scans and why a researcher would care, ignoring cosmetic churn. Use when a context package includes a changes/drift section.
---

# Find what changed — and why it matters

Using **only** the SignalMap context package provided, explain what changed
between the current observation and the previous one.

If no package has been provided, or its `changes` section says none is
available, ask for one from a host with at least two scans:

```bash
signalmap context <host> --lens changes --write changes.md
```

SignalMap has already done the mechanical diff and grouped connected changes
into workflows. Your job is to read that, not to recompute it.

## Ignore

Cosmetic churn — bundle hashes, cache-busters, static-asset renames.
SignalMap normalises most of these out already; disregard any that remain.

## Identify meaningful change in

routes · APIs · JavaScript · authentication · runtime configuration ·
technologies · external services · third-party scripts · workers ·
source maps · trust relationships.

Where the package has grouped changes into a workflow, treat the group as one
thing, not several.

## For every meaningful change, give

- what changed
- the evidence
- what new capability or relationship appeared
- why a security researcher may care
- what to inspect manually next

## Rules

- Use only this package.
- Do **not** assume a change is a vulnerability. A new export workflow is a
  new place to look, not a finding.
- If the `changes` section reports a caveat — a partial scan on either side —
  carry it: an apparent removal may be something that was not looked at.
- No exploit payloads. No severity.
