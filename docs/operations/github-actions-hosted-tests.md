# GitHub Actions automatic test shutdown — 2026-10-29

## Decision and scope

The workspace owner requested that GitHub-hosted test execution be stopped in the repositories they operate. In this **Cyclizer fork**, automatic multi-platform CI (`.github/workflows/ci.yml`) is removed, and the `push` trigger for the rolling **Canary** test build is removed. Canary remains manual via `workflow_dispatch` for deliberate release candidate testing; it will no longer run from a regular merge or push to main.

The following are deliberately **not** disabled because they are not routine hosted PR/push tests: `codeql.yml` (security analysis, currently job-scoped to the upstream repository), `publish.yml` and `release.yml` (explicit release packaging), `verify-release.yml` (published artifact integrity), and issue/release administration workflows. Nothing in this change modifies the upstream `totec448-spec/chat-on-steroids` repository.

## Verification requirements and recovery

- Baseline fork main: `27ee321cf02ab737b70604a748288814b1c80159`.
- Code tests, quality checks and native acceptance remain required in appropriate independent verification environments; removing a hosted runner never constitutes a PASS.
- Do not execute full tests/builds on the owner's Windows workstation without separate authorization. Do not disturb its dirty feature checkout.
- A required branch protection CI status, if still configured, must be explicitly reconciled by an authorized administrator rather than bypassed or marked green.
- Re-enable the old automatic CI by restoring `ci.yml` from `git show 27ee321cf02ab737b70604a748288814b1c80159:.github/workflows/ci.yml`; restore the original Canary triggers from `git show 27ee321cf02ab737b70604a748288814b1c80159:.github/workflows/canary.yml`. Integrate through an ordinary reviewed PR.

## Evidence

Before merge, review the exact workflow diff. After merge, re-fetch the fork's PR and main ref and confirm `ci.yml` is absent, Canary has no `push` trigger, and this note remains. Tests were **NOT RUN** for this workflow-configuration change.
