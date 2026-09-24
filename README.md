<div align="center">

# SignalMap — artifacts & demos

**Understand a web application as an evidence-backed model — then let an AI
reason over that model instead of over raw recon.**

Recordings, sample reports and demo data for **SignalMap**.
The tool itself is not public yet.

[Watch the demos](#the-demos) · [What SignalMap does](#what-signalmap-does) ·
[Sample outputs](#sample-outputs) · [The skills](#the-skills) · [Boundaries](#boundaries)

</div>

---

## The demos

| | |
| --- | --- |
| **[Burp Webapp Model][vid-burp]** | A Burp export becomes a web-app model: routes, APIs, JavaScript, third parties, credentials and roles — then the interactive graph report. |
| **[Terminal Query][vid-query]** | Asking the model questions. Deterministic, offline, evidence-cited. No language model involved. |

---

## What SignalMap does

Recon tools produce lists. SignalMap produces a **model**: a six-layer graph
where every node cites the observations it came from and every edge is
marked `OBSERVED` or `INFERRED` — never blurred.

It reads an application two ways, and the difference between them is the
point.

**`scan`** observes from outside, signed out, `GET` only.

**`burp`** ingests a Burp Suite export — authenticated, multi-role traffic
including the `POST`s, `PUT`s and `DELETE`s a scanner will never send. It
runs entirely locally, and credential and personal-data *values* are masked
before anything is written to disk.

Same command against both:

```
$ signalmap query shop "code vs runtime"

  scan:  23 API endpoints — 4 static and runtime, 10 static only, 9 documented only
  burp:  18 API endpoints — 10 static and runtime, 6 runtime only, 2 static only
```

Six endpoints exist that **no source file mentions**. Nothing links to them
and nothing names them, so no crawler can find them.

### The question only a capture can answer

Ingest a low-privilege session and an admin session of the same app, and the
model can compare what each identity *reached* against what each identity
was *offered*:

```
$ signalmap burp query shop cross-role

2 endpoints reached by 'user' that only 'admin' is offered

  GET  shop.example.com/api/admin/users       200   779 B   burp_000019
  GET  shop.example.com/api/config/internal   200   248 B   burp_000020

Unknown for each: whether the response was authorised or an unhandled leak.
Next: replay each as 'user' and compare with 'admin'.
```

That is a broken-access-control **hypothesis**, with the exact request as
evidence. It is not a finding, and the tool never calls it one — a `200` can
be an error page, an empty tenant-scoped list, or a denial the client
rendered. What it gives you is the two requests worth replaying.

---

## Why an AI reasons better over a model than over a dump

The evidence package describes the application's **structure**. A raw dump
is the application's **bytes**. A real single-page application has far more
bytes than structure — so the saving grows with the target.

Hold one application fixed, grow only its JavaScript:

| JavaScript shipped | Raw dump | Evidence package | Ratio |
| ---: | ---: | ---: | ---: |
| 0.24 MB | 64,834 tok | **13,493 tok** | 4.8× |
| 1 MB | 264,082 tok | **13,493 tok** | 19.6× |
| 4 MB | 1,050,511 tok | **13,493 tok** | 77.9× |
| **5.2 MB** | **1,365,111 tok** | **13,493 tok** | **101×** |
| 12 MB | 3,147,660 tok | **13,493 tok** | 233× |

The package column does not move. **100× is real once an app ships around
5 MB of JavaScript** — ordinary for a Flutter, Angular or large React build.
On a small site it is 5×, and saying so is what makes the rest credible.

Three caveats travel with every figure:

- the baseline is the **whole** dump — an upper bound on the naive approach,
  not what a careful analyst would actually paste;
- tokens are **estimated** at four bytes each, not counted by a tokenizer;
- the package is **capped** — past forty nodes per type it samples rather
  than grows, and its own coverage section says so.

---

## Sample outputs

| File | What it is |
| --- | --- |
| `samples/report/index.html` | The interactive graph report. One self-contained file — no server, no CDN, works offline. Open it in a browser. |
| `samples/reports/*.md` | The same model as Markdown: `BRIEF.md`, `REACHABILITY.md`, `ATTACK_SURFACE.md`, `UNKNOWNS.md`. |
| `samples/context/methodology.md` | An evidence package — what you hand a language model instead of a dump. |
| `samples/burp-logs.xml` | The synthetic Burp capture everything above was built from — reproduce the whole demo from it. |

Everything in `samples/` is generated from a **fictional application**.
Every host is an RFC 2606 name (`example.com`, `example.net`), and every
credential-shaped string is synthetic and valid nowhere. No real target
appears anywhere in this repository.

> **`burp-logs.xml` deliberately contains credentials and personal data** —
> an OpenAI key, an AWS key, a private key, a card number, an email address —
> base64-encoded inside the request and response bodies, exactly as Burp
> stores them. That is the input, and masking it would remove the thing the
> demo exists to demonstrate. All of it is invented. Every *derived* file in
> this repository has been checked to contain none of it: the model carries a
> truncated hash, a length and an entropy figure, never a value.

---

## The skills

Ten prompts in [`skills/`](./skills) that use a language model as a **recon
analyst** rather than a bug finder.

> Don't ask AI "where is the bug?" Ask it "what do I understand, what don't
> I understand, and where should I look next?"

Each reads one lens of the evidence package and nothing else —
`signalmap-methodology` builds a hunting plan from *this* application's
architecture rather than a generic checklist; `signalmap-gaps` ranks what is
not understood; `signalmap-falsify` stress-tests a hypothesis until it
survives or does not.

Every one enforces the same rule: **only this evidence**. Cite an evidence
id for each conclusion, mark every claim observed, inferred or unknown, and
never treat an absence in the package as an absence in the target. None of
them produce findings or payloads.

Try one without installing anything — there are two ready packages in
[`samples/context/`](./samples/context).

In Claude Code the skills load directly. Anywhere else, the body of each
`SKILL.md` is a provider-agnostic prompt you can paste.

---

## Boundaries

Enforced in code and covered by tests — not flags you can flip.

| SignalMap does | SignalMap never |
| --- | --- |
| `GET` and browser navigation | sends `POST` / `PUT` / `PATCH` / `DELETE` |
| loads a session **you** exported | automates a login or touches a credential |
| reads a capture **you** exported | uploads, forwards or replays one |
| masks a capture's values before the first write | writes authenticated traffic down unmasked |
| reports a credential by hash, length and entropy | records the credential it just found |
| says `PARTIAL` when out of budget | claims coverage it did not have |
| surfaces hypotheses and the evidence to test them | calls anything a vulnerability |

**Attention, not severity.** Nothing SignalMap outputs is a vulnerability
claim. It tested nothing, so neither can anything reading its output.

**A capture shows where somebody browsed.** Absence from it is absence from
the capture, never absence from the application — and the model says so
rather than letting a thin capture read as a small app.

**Authorization is yours.** Point it only at what you own or are permitted
to test, and ingest only traffic you were authorised to record.

---

## Status

SignalMap is not open source yet. This repository is where the artifacts
live while that is decided. Watch it, or reach out if you want to try the
tool against something you own.

*Built by [@falc0n-researcher](https://github.com/falc0n-researcher).*

[vid-burp]: ./Burp%20Webapp%20Model%20Demo.mp4
[vid-query]: ./Terminal%20Query%20Signalmap.mp4
