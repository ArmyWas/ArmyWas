# ArmyWas

I build practical, evidence-driven tooling for the emerging
[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) ecosystem.

我专注于从真实使用问题出发，为 DeepSeek Harness 打磨小而可靠的开发者工具。

## Current work

### [`dsh-plugin-reducer`](https://github.com/ArmyWas/dsh-plugin-reducer)

Finds a 1-minimal set of out-of-tree plugins that still reproduces a broken
Harness profile. It works in disposable shadow profiles, preserves the real
profile, and produces a redacted machine-readable report.

- **Status:** v0.3 early preview
- **Best for:** plugin interaction failures and small, shareable reproductions
- [Release](https://github.com/ArmyWas/dsh-plugin-reducer/releases/tag/v0.3.0)
  · [Official community discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/3133)

### [`dsh-failure-lens`](https://github.com/ArmyWas/dsh-failure-lens)

A deterministic Web client plugin that explains one high-confidence Windows
sandbox `spawn EPERM` failure in context without changing the session, rerunning
commands, or sending telemetry.

- **Status:** v0.2, intentionally narrow
- **Best for:** distinguishing a sandbox boundary from a test assertion failure
- [Release](https://github.com/ArmyWas/dsh-failure-lens/releases/tag/v0.2.0)
  · [Official community discussion](https://github.com/deepseek-ai/deepseek-harness/discussions/3193)

## Working principles

- Start with reproduced behavior and durable evidence.
- Prefer a narrow contract over a broad guess.
- Keep diagnostics reversible, privacy-conscious, and easy to remove.
- Test on Windows, macOS, and Linux where the product boundary allows it.
- Avoid rebuilding tools the community already maintains well.

These are independent, unofficial community projects. Feedback and redacted
field reports are welcome in each repository's issue tracker.
