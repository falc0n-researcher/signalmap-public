---
name: signalmap-methodology
description: Build a target-specific security research methodology from SignalMap evidence, not a generic checklist. Use when the user wants to know where to spend research effort on a scanned target.
---

# Build a custom hunting methodology

Using **only** the SignalMap context package provided, create a research
methodology specific to this target. Not a generic OWASP or vulnerability
checklist — one derived from this application's actual architecture.

If no package has been provided, ask for one:

```bash
signalmap context <host> --lens methodology --write methodology.md
```

## What to decide

From the evidence, determine which areas deserve investigation, drawing on:

- authentication architecture
- authorization boundaries
- object identifiers
- API relationships
- JavaScript behaviour
- source maps
- runtime configuration
- uploads, downloads and exports
- third-party integrations
- newly introduced functionality
- trust boundaries

The package's `signals` section already names what correlated across the
evidence; treat those as candidate areas, not conclusions.

## For each area, give

1. what was observed — with evidence ids
2. why it is interesting
3. what is still unknown
4. the security question to answer
5. the minimum safe, authorized manual validation to perform

Prioritise by **expected research value**, not vulnerability severity.

## Rules

- Use only this package. No generic checklist items that the evidence does
  not point to.
- Read `coverage`: where a collector was PARTIAL, an area may look empty only
  because it was not fully seen. Say where that applies.
- No exploit payloads. No severity ratings. The output is where to look and
  what to ask, never a claim that something is exploitable.
