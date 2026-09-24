# SignalMap skills

Ten prompts that stop asking AI to find bugs and start using it as a recon
analyst.

> **Don't ask AI "where is the bug?" Ask it "what do I understand, what
> don't I understand, and where should I look next?"**

Each skill reads one lens of a SignalMap evidence package and nothing else.
No model is called by SignalMap itself — you bring whichever provider you
use, and the tool hands it evidence it cannot get wrong.

| Skill | Lens | The question |
| --- | --- | --- |
| `signalmap-model` | `model` | How does this application appear to work? |
| `signalmap-methodology` | `methodology` | Where is research effort worth spending here? |
| `signalmap-attention` | `attention` | What deserves a researcher's time here, and why? |
| `signalmap-attack-surface` | `attack_surface` | Where should an unauthenticated tester look first, and what needs a session? |
| `signalmap-gaps` | `gaps` | What am I missing about this target? |
| `signalmap-falsify` | `hypotheses` | Which investigation paths survive scrutiny? |
| `signalmap-javascript` | `javascript` | What is the client-side code made of, and what does it reach? |
| `signalmap-auth` | `authentication` | How is identity delegated, and where is the boundary? |
| `signalmap-changes` | `changes` | What changed, and why would a researcher care? |
| `signalmap-impact` | `impact` | Which advisories could concern what shipped? |

## Using them

```bash
signalmap scan https://app.example.com --out runs
signalmap context app.example.com --lens methodology --write methodology.md
```

Then hand `methodology.md` to your model along with the matching skill. In
Claude Code the skills load directly; anywhere else, the body of each
`SKILL.md` is a provider-agnostic prompt you can paste.

There is a package to try in this repository without running anything:

```
../samples/context/methodology.md
../samples/context/attack_surface.md
```

`signalmap ask` uses three of these directly — `attention`, `falsify` and
`gaps` ship inside the tool as prompt files, so the answer it saves was
produced under exactly the rules below.

## The rule they all share

**Only SignalMap's evidence.** Not memory, not the web, not what is typical
of applications like this one. Every conclusion cites an evidence id, every
claim is marked observed, inferred or unknown, and an absence in the package
is never treated as an absence in the target.

None of them produce vulnerability findings or exploit payloads. SignalMap
tested nothing, so neither can anything reading its output.

## They work beyond SignalMap

The prompts assume structured recon evidence with provenance. A Burp export,
a browser capture, or your own pipeline works the same way — the discipline
is in the prompt, not the format.
