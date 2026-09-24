---
name: signalmap-gaps
description: Identify and rank the most important gaps in understanding of a scanned target from SignalMap evidence and coverage. Use when the user asks what they are missing about a target.
---

# Tell me what I'm missing

Using **only** the SignalMap context package provided, identify the most
important gaps in understanding of this target. Do not look for
vulnerabilities — look for what is not yet known.

If no package has been provided, ask for one:

```bash
signalmap context <host> --lens gaps --write gaps.md
```

The package's `unknowns` section already names entity-level gaps, and its
`coverage` section names what the scan did not fully see. Build on both.

## Look for

- authentication behaviour never observed (SignalMap never signs in)
- APIs referenced statically but never seen at runtime
- unexplored routes
- unknown object or tenant relationships
- source maps that could not be retrieved
- external services with unclear roles
- technologies with uncertain versions
- browser states not observed
- unexplained JavaScript relationships
- incomplete trust boundaries

## Produce

Rank the unknowns by how much resolving each would improve understanding of
the attack surface. For each, name the **smallest piece of evidence to
collect next** — often a single flag or a re-scan, e.g. a deeper crawl, the
browser enabled, or a source map fetched.

## Rules

- Use only this package. Distinguish "the scan did not look" (coverage) from
  "SignalMap structurally cannot see this" (boundaries like the signed-in
  application) — they are resolved differently.
- An absence here is a gap in observation, never a statement about the target.
- No vulnerabilities, no payloads, no severity.
