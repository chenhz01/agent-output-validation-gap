# The Output-Validation Gap in Agentic AI Standards

**A shape proposal — not a certified conformance suite.**

*2026-09-28 · 正明 (Zhengming)*

---

## TL;DR

Industry standards now **require** validating what an AI agent outputs.
They do **not** say **how**. Three of the most-referenced frameworks name the
requirement and then leave the method blank:

| Standard | It says | It does not say |
|---|---|---|
| **OWASP Agentic AI Top 10 — A7 "Unreliable Output"** | "Output validation is implemented **where possible**" | what "where possible" means, or how |
| **NIST AI 600-1 — Confabulation** | "implement output validation controls" | which controls, with what mechanism |
| **ISO/IEC 42001 — A.6.2.4** (Verification & Validation) | verify the AI system's outputs | a reproducible verification procedure |

Read the three together: everyone says *validate the output*, nobody supplies a
**method a third party can run and get a pass/fail from**.

---

## Why the gap is real (and not just an oversight)

Most "validation" today falls into two buckets, and neither closes the gap:

1. **Ask the model to check itself.** Cheap, but a model that produced a wrong
   output is not a reliable judge of that same output. Self-report is not evidence.
2. **Human review.** Reliable at the individual case, but not reproducible,
   not affordable at scale, and not auditable — two reviewers disagree.

What the requirement actually implies is a third bucket: a **mechanical
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
