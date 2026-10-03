# ArmyWas

I build small, evidence-driven reliability tools for the emerging
[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) ecosystem.

我专注于从真实故障和可复现实验出发，为 DeepSeek Harness 打磨小而可靠的社区工具。

## Reliability loop

Preflight → Classify → Minimize → Verify → Monitor

[Read the case study](CASE_STUDY.md) · [阅读中文案例](CASE_STUDY.zh-CN.md)

## Current work

### 1. Preflight — [`dsh-provider-passport`](https://github.com/ArmyWas/dsh-provider-passport)

A review-first Web plugin that probes the request dialect of explicitly
configured OpenAI-compatible providers before proposing the smallest supported
Harness compatibility change. It verifies the real runtime after an approved
write and keeps one-click rollback available.

- **Status:** public `v0.1.0-preview.2`; intentionally not promoted to stable
- **Best for:** custom gateways that reject otherwise valid requests because
  their accepted request shape differs from Harness defaults
- [npm preview](https://www.npmjs.com/package/dsh-provider-passport/v/0.1.0-preview.2)
  · [Release](https://github.com/ArmyWas/dsh-provider-passport/releases/tag/v0.1.0-preview.2)
  · [Official community discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/5356)
  · [Public tester guide](https://github.com/ArmyWas/dsh-provider-passport/blob/main/docs/TESTER_GUIDE.md)

### 2. Classify — [`dsh-failure-lens`](https://github.com/ArmyWas/dsh-failure-lens)

A deterministic Web client plugin that explains one high-confidence Windows
sandbox `spawn EPERM` signature in context. It does not rerun commands, change
the session, send telemetry, or infer beyond the evidence in one tool-result
block.

- **Status:** stable `v0.3.0`, intentionally limited to one reproduced signature
- **Best for:** distinguishing a sandbox boundary from a test assertion failure
- [npm](https://www.npmjs.com/package/dsh-failure-lens)
  · [Release](https://github.com/ArmyWas/dsh-failure-lens/releases/tag/v0.3.0)
  · [Official community discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/3193)
  · [Field-report gate](https://github.com/ArmyWas/dsh-failure-lens/issues/10)

### 3. Minimize — [`dsh-plugin-reducer`](https://github.com/ArmyWas/dsh-plugin-reducer)

An external CLI that finds a 1-minimal set of out-of-tree plugins that still
reproduces a broken Harness profile. It works in disposable shadow profiles,
preserves the real profile, and produces a redacted machine-readable report.

- **Status:** stable `v0.3.0`; `v0.3.1` is available on the npm `next` channel
- **Best for:** plugin-interaction failures and small, shareable reproductions
- **Validation gate:** `0/3` independent real-profile reports;
  no speculative stable promotion
- [npm](https://www.npmjs.com/package/dsh-plugin-reducer)
  · [v0.3.1 prerelease](https://github.com/ArmyWas/dsh-plugin-reducer/releases/tag/v0.3.1)
  · [Official community discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/3686)
  · [Field-report intake](https://github.com/ArmyWas/dsh-plugin-reducer/issues/8)

### 4. Verify — [`dsh-subagent-contract`](https://github.com/ArmyWas/dsh-subagent-contract)

A deterministic post-run verifier for relationships that aggregate evaluation
metrics lose: parent/child lineage, admission cardinality, continuation
identity, FIFO delivery, settlement provenance, and explicit report relays.

- **Status:** research `v0.1.0-preview.1`; the offline verifier works, while
  the live runner remains an evidence-producing experiment rather than a
  supported npm path
- **Best for:** checking durable multi-agent behavior after a Harness run
- [Release](https://github.com/ArmyWas/dsh-subagent-contract/releases/tag/v0.1.0-preview.1)
  · [Official community discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/5136)
  · [Research evidence](https://github.com/ArmyWas/dsh-subagent-contract/blob/main/docs/RESEARCH_EVIDENCE.md)

### 5. Monitor — [`dsh-codex-compat-canary`](https://github.com/ArmyWas/dsh-codex-compat-canary)

An external compatibility canary that detects Codex App Server protocol drift
the pinned Harness adapter cannot safely interpret. The first live experiment
found a concrete error-union value that degrades to `unknown`.

- **Status:** public `v0.2.0`; weekly live monitoring is active
- **Best for:** turning dependency drift into a reproducible report before it is
  mistaken for a model or user failure
- [npm](https://www.npmjs.com/package/dsh-codex-compat-canary)
  · [Release](https://github.com/ArmyWas/dsh-codex-compat-canary/releases/tag/v0.2.0)
  · [Official compatibility report](https://github.com/deepseek-ai/deepseek-harness/discussions/4531)
  · [Current canary finding](https://github.com/ArmyWas/dsh-codex-compat-canary/issues/1)

## Working principles

- Start with reproduced behavior or a controlled experiment.
- Search for maintained solutions before creating another project.
- Prefer a narrow, testable contract over a broad diagnostic guess.
- Keep diagnostics reversible, privacy-conscious, and easy to remove.
- Validate public artifacts from a clean install, not only from source.
- Expand only when new evidence demonstrates that the existing boundary is too narrow.

These are independent, unofficial community projects. Feedback and
privacy-reviewed field reports are welcome in each repository.
