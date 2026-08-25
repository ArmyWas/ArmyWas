# ArmyWas

I build small, evidence-driven reliability tools for the emerging
[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) ecosystem.

我专注于从真实故障和可复现实验出发，为 DeepSeek Harness 打磨小而可靠的社区工具。

## Reliability loop

Classify → Minimize → Monitor

[Read the case study](CASE_STUDY.md) · [阅读中文案例](CASE_STUDY.zh-CN.md)

## Current work

### 1. Classify — [`dsh-failure-lens`](https://github.com/ArmyWas/dsh-failure-lens)

A deterministic Web client plugin that explains one high-confidence Windows
sandbox `spawn EPERM` signature in context. It does not rerun commands, change
the session, send telemetry, or infer beyond the evidence in one tool-result
block.

- **Status:** stable `v0.2.2`, intentionally limited to one reproduced signature
- **Best for:** distinguishing a sandbox boundary from a test assertion failure
- [npm](https://www.npmjs.com/package/dsh-failure-lens)
  · [Release](https://github.com/ArmyWas/dsh-failure-lens/releases/tag/v0.2.2)
  · [Official community discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/3193)
  · [Field-report gate](https://github.com/ArmyWas/dsh-failure-lens/issues/10)

### 2. Minimize — [`dsh-plugin-reducer`](https://github.com/ArmyWas/dsh-plugin-reducer)

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

### 3. Monitor — [`dsh-codex-compat-canary`](https://github.com/ArmyWas/dsh-codex-compat-canary)

An external compatibility canary that detects Codex App Server protocol drift
the pinned Harness adapter cannot safely interpret. The first live experiment
found a concrete error-union value that degrades to `unknown`.

- **Status:** public `v0.1.0`; weekly live monitoring is active
- **Best for:** turning dependency drift into a reproducible report before it is
  mistaken for a model or user failure
- [npm](https://www.npmjs.com/package/dsh-codex-compat-canary)
  · [Release](https://github.com/ArmyWas/dsh-codex-compat-canary/releases/tag/v0.1.0)
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
