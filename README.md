# The Output-Validation Gap in Agentic AI Standards

**A shape proposal — not a certified conformance suite.**

*2026-09-28 · 正明 (Zhengming)*

---

## TL;DR

The most-referenced AI governance frameworks now **require** validating what a
model outputs. They do **not** say **how**. Three references, read together,
show the same shape:

| Reference | What it actually says | What it leaves open |
|---|---|---|
| **NIST AI 600-1 §2.2 — "Confabulation"** | Names *fabricated outputs presented as factual* as a GAI risk. Suggested actions include comparing output against known ground truth through human **and automated** evaluation (`MP-2.3-001`), documented fact-checking (`MP-2.3-003`), and groundedness metrics such as citation verification (`MEASURE-2.1`). | What "automated evaluation" concretely **is**. No mechanism is specified. |
| **ISO/IEC 42001:2023 — A.6.2.4 (Verification & validation)** | Requires verification and validation of AI system outputs, in the life-cycle control theme. | No reproducible verification **procedure**. The methodology is left to the organisation. |
| **Practitioner checklists built on the OWASP agentic taxonomy** | Hedge explicitly: *"Output validation is implemented **where possible**"* | What "where possible" means in practice. |

Everything says *validate the output*. None of them supplies a **method a third
party can run and get a pass/fail from**.

> **Note on sources.** NIST and ISO claims above are checked against the
> published documents and their crosswalks; action IDs are quoted from
> NIST AI 600-1 §2.2. The "where possible" phrasing is quoted from a published
> vendor compliance checklist for the OWASP agentic taxonomy — **it is the
> checklist's wording, not OWASP's own text**. OWASP's agentic taxonomies are
> themselves still consolidating, which is part of why the method gap is where
> it is.

---

## Why the gap is real (and not just an oversight)

Most "validation" today falls into two buckets, and neither closes the gap:

1. **Ask the model to check itself.** Cheap, but a model that produced a wrong
   output is not a reliable judge of that same output. Self-report is not evidence.
2. **Human review.** Reliable at the individual case, but not reproducible,
   not affordable at scale, and not auditable — two reviewers disagree.

As one practitioner put it, a generic instruction to "verify AI output" is
paperwork dressed as a control: a reviewer needs an authoritative source, a
defined decision threshold, and a place to record disagreement.

What the requirements actually imply is a third bucket: a **mechanical
conformance layer** — checks that decide pass/fail from evidence, without asking
the model anything.

---

## What a mechanical conformance layer must satisfy

If a check is going to count under "output validation," it should have four
properties:

1. **Does not trust model self-report.** The model's own claim is treated as an
   input to be checked, never as the verdict.
2. **Deterministic and reproducible.** Same input → same verdict, on any machine,
   today or next year.
3. **Runnable offline by a third party.** No vendor account, no API key, no
   network. If a stranger cannot run it, it is not a conformance check.
4. **Emits pass/fail, not a score.** Scoring systems drift and invite gaming;
   gates do not.

---

## A taxonomy of *mechanically verifiable* failure classes

These are the shapes of wrongness that a mechanical layer can actually catch.
They are stated generically — the point of publishing them is that anyone can
check their own agent against them.

| # | Failure class | What it looks like |
|---|---|---|
| 1 | **Unverifiable origin** | The agent asserts "I learned X" / "X is true" with no checkable source. |
| 2 | **Truncation reported as completion** | A response ends early — timeout, dropped stream, token cap — and is still labelled complete. |
| 3 | **Action / claim divergence** | The agent says it did something the execution trace does not show. |
| 4 | **Declared vs. actual capability** | A tool or skill claims a capability its code does not implement. |
| 5 | **Silence that was never accounted for** | Expected events did not happen, and nothing reported the absence. |
| 6 | **Unsurfaced presupposition** | A claim depends on a premise nobody asserted and nobody checked. |
| 7 | **Dead reference** | A version, link, or artifact cited as live is no longer real. |

Each of these has the same property: **the wrongness is visible in the artifacts,
not in the model's explanation.** That is what makes them mechanically checkable.

---

## Honest boundary

This note is a **shape proposal**, not a standard. It describes the gap and the
properties a closure would need. It does **not** claim certification, alignment
sign-off, or adoption by any standards body.

A standard is not something you declare — it is something others implement. This
document only tries to make the shape of the missing piece legible.

---

## If you have been bitten by this

If you have shipped an agent that was **silently wrong** — produced output that
looked fine and was not — that failure is data. The most useful thing you can
add is the *shape* of the failure: what looked right, what was actually wrong,
and what evidence would have caught it.

Open an issue or start a discussion. Shapes are more useful than anecdotes.

---

## Related

- [`chenhz01/zhengming-auditors`](https://github.com/chenhz01/zhengming-auditors) —
  seven zero-dependency, single-file CLI auditors, each catching one class of
  silent failure from the taxonomy above.

---

*Released for reuse under CC BY 4.0. No warranty. Verify anything you take from here.*
