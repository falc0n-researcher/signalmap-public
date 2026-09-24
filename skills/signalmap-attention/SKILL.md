---
name: signalmap-attention
description: Say what in a scanned target most deserves a researcher's time, and why, using only SignalMap's evidence package. Use when the user asks where to start, what stands out, or what deserves attention on a target.
---

# What deserves attention here

Using **only** the SignalMap context package provided, say what in this
target most deserves a researcher's time, strongest first.

If no package has been provided, ask for one:

```bash
signalmap context <host> --lens attention --write attention.md
```

The package's `signals` section already carries what was observed and what
is unknown for each one. The `architecture` section is the shape those
signals sit in. Build on both rather than restating them.

## For each of the three strongest, give

- **What was observed** — the entities and the relationships between them,
  with evidence ids.
- **Why it deserves attention** — what makes this different from the rest of
  the surface. A route that loads a chunk that calls an endpoint carrying an
  identifier is a workflow; three unrelated new strings are three strings.
- **What is unknown** — name the unknown from the package rather than
  guessing past it.
- **The smallest authorized test** a human could run to resolve that
  unknown, and what each result would mean.

Close with one line naming what the `coverage` section says was not fully
seen, so the reader knows where the answer's edges are.

## Stance

Be specific and short. A generic paragraph about attack surface is worse
than three sentences about this application.

Rank by how much the answer would change what someone does next, not by how
alarming it sounds. An endpoint that ran once with an account identifier in
it beats a missing header, every time.

## The rules

- Only this package. Not memory, not the web, not what is typical of
  applications like this one.
- Name no endpoint, route, parameter or host that is not in the package. If
  the answer needs one that is not here, say that it is not here.
- Cite an evidence id for every claim that rests on one.
- Distinguish observed from inferred from unknown. An absence in the package
  is not an absence in the target.
- Hypotheses and the evidence that would test them — never findings, never
  severity ratings, never payloads. SignalMap tested nothing.
