# From a failure signature to protocol drift

How three small, independent tools form an evidence-driven reliability loop for
DeepSeek Harness.

[简体中文](CASE_STUDY.zh-CN.md)

## The product question

DeepSeek Harness is built around the idea that almost everything can be a
plugin. That openness creates a large design surface, but it also creates a
reliability question: when a profile fails, where did the failure actually
come from?

It may be:

- a known operating-system or sandbox boundary,
- an interaction among several community plugins, or
- protocol drift inside an official integration.

Treating all three as one generic “doctor” problem would produce a broad tool
with weak evidence. I instead built three narrow projects. Each project owns
one layer and remains useful even when another layer is broken.

## 1. Classify one reproduced failure

The first project began with a real DeepSeek Harness Web result on Windows. A
test command ended with a `spawn EPERM` stack, but the presentation did not
clearly distinguish a sandbox boundary from a failed test assertion.

[`dsh-failure-lens`](https://github.com/ArmyWas/dsh-failure-lens) recognizes
only one high-confidence signature. Four exact markers must occur in the same
tool-result block, and that block must contain durable failure evidence: an
error flag or a non-zero exit code. It never combines evidence across sibling
results, reruns the command, edits the session, or sends telemetry.

That narrow boundary matters. A successful command that merely prints an old
EPERM stack must remain a non-match. External source-level review in the
[official discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/3193)
helped tighten this evidence contract without expanding into unobserved TLS or
temporary-directory signatures.

The result is a real Harness client plugin, but not a universal classifier.
New signatures require new field evidence.

## 2. Minimize a broken plugin composition

A classifier helps after a known signature appears. It does not answer a
different question: which plugin or plugin interaction makes a profile fail?

[`dsh-plugin-reducer`](https://github.com/ArmyWas/dsh-plugin-reducer) applies
delta debugging to that problem. It runs candidates in disposable shadow
profiles and returns a **1-minimal** reproducing set: remove any remaining
candidate and the supplied failure predicate no longer reproduces.

It is deliberately an external CLI. Moving the reducer into the plugin tree it
may need to diagnose would make it disappear with the broken composition. It
also preserves the real profile and emits a redacted machine-readable report
that can be shared without publishing an entire local configuration.

The project does not claim that a 1-minimal set is the globally smallest set,
and its `v0.3.1` release remains on the npm `next` channel. Stable promotion is
gated on three independent, privacy-reviewed real-profile reports; the current
count is `0/3`. This is an explicit product constraint, not unfinished feature
work.

## 3. Detect integration drift before guessing

The third project came from a controlled differential experiment rather than a
feature brainstorm.

On 2026-08-25, the official Harness Codex adapter pinned Codex `0.147.0`, while
the current published Codex target was `0.149.1`. The newer protocol added the
string error category `misalignmentPolicyViolation`. The pinned adapter did not
map that value and degraded it to `unknown`.

Startup, handshake, approval, cancellation, and process cleanup could still
work, so a normal smoke test would miss the loss of diagnostic meaning.

[`dsh-codex-compat-canary`](https://github.com/ArmyWas/dsh-codex-compat-canary)
reads the official adapter without executing Harness source, generates the
pinned and target Codex schemas through the official CLI, and compares the
protocol values the adapter explicitly consumes. It publishes a reproducible
JSON report and exits non-zero when an implemented breaking check fails.

The live finding is recorded in the
[official compatibility discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/4531).
A weekly workflow now reruns the released logic so that an upstream fix or a
new drift becomes observable even if nobody replies to the discussion.

## The reliability loop

| Layer | Question | Project | Evidence |
|---|---|---|---|
| Classify | What failed? | Failure Lens | Reproduced same-block signature |
| Minimize | Which plugins? | Plugin Reducer | Predicate in shadow profiles |
| Monitor | Did it drift? | Codex Canary | Schemas and adapter mappings |

The tools are independent. They are not a bundled platform and do not require
users to adopt all three. The shared product principle is that every conclusion
must point back to durable evidence.

## What I deliberately did not build

- another Codex-to-Harness bridge, because official and community bridges
  already exist;
- a general Harness doctor that guesses across unrelated failure families;
- an automatic fixer that mutates a real user profile;
- speculative classifiers without a reproduced event;
- a claim of complete Codex protocol compatibility based on a few checks.

These boundaries reduce surface area and make failures in the diagnostic tools
themselves easier to understand.

## Release and validation discipline

Across the three projects, the release path includes cross-platform CI,
machine-readable reports where appropriate, clean-install verification, public
release artifacts, and explicit feedback gates. The repositories and official
discussions record what is observed, what is inferred, and what remains
unknown.

This is the lifecycle I want to continue using:

1. reproduce or run a controlled experiment;
2. search the existing ecosystem for maintained solutions;
3. define the smallest useful product boundary;
4. test the evidence contract and false-positive boundary;
5. verify the public artifact from a clean environment;
6. publish the result and let real feedback determine expansion.

## How to help

- If you encounter the exact Windows `spawn EPERM` family naturally, submit a
  privacy-reviewed outcome to the
  [Failure Lens field-report issue](https://github.com/ArmyWas/dsh-failure-lens/issues/10).
- If a community-plugin profile fails, try the Reducer `next` release and share
  a redacted result through its
  [field-report intake](https://github.com/ArmyWas/dsh-plugin-reducer/issues/8).
- If the Codex canary finds a new drift, attach the generated JSON report rather
  than a screenshot or an unpinned description.

All three projects are independent, unofficial community work.
