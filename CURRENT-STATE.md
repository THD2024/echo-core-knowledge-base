# Current State

Last updated: 2026-11-01

## Source baseline

- Source/research repository: [THD2024/echo-core](https://github.com/THD2024/echo-core)
- Branch: `main`
- Latest source snapshot inspected: [`14ecf7deef8f7c4db325888e5056031e730dac1c`](https://github.com/THD2024/echo-core/commit/14ecf7deef8f7c4db325888e5056031e730dac1c)
- The commit imports the MiMo-Code working-tree source under `MiMo-Code/`, alongside reports and benchmark evidence.
- Root `MiMo-Code/package.json`: `packageManager: bun@1.3.14`; CLI package version in `packages/cli/package.json`: `0.1.15`.
- Version discrepancy to resolve: `docs/PHASE-STATUS.md` reports MiMoCode v0.1.16, while the archived R2 report and committed CLI package metadata report 0.1.15.
- Repository visibility at inspection: public. Review committed documentation for local machine paths before any wider redistribution.

## Status at a glance

| Area | Status | Notes |
|---|---|---|
| Echo Immune System V1 | **Verified / Frozen** | Formal acceptance completed. Preserve the frozen protection boundary. |
| R1 system/runtime audit | **Verified historical audit** | Audit dated 2026-10-31; its source baseline must be reconciled against snapshot commit 14ecf7d. |
| R2 deployment/environment audit | **Verified historical audit** | Audit dated 2026-10-31; local-environment findings are historical, not freshly re-run here. |
| Existing MiMoCode runtime infrastructure | **Implemented / source-observed** | Effect ManagedRuntime, AppRuntime, BootstrapRuntime and per-project Instance lifecycle exist. |
| Echo Core Runtime V1 | **Accepted design direction; not implemented as a distinct Echo architecture** | Must be mapped onto existing runtime infrastructure after R0. |
| Memory capability | **Implemented / partially mature** | Markdown file storage, SQLite FTS5 index, BM25 search, reconcile and write gates exist; no Memory Service write API or semantic retrieval. |
| Nervous capability | **Partially implemented** | Main prompt/run loop, Effect Bus, task coordination, plan/goal mechanisms exist; reasoning/planning remains largely model- and prompt-driven. |
| Execution capability | **Implemented / partially mature** | Tool execution, gate/settlement, actor/subagent and recovery paths exist; known feedback/reliability gaps remain. |
| Task Registry | **Implemented / source-observed** | SQL-backed task tree and event history exist; distinct Task System boundaries remain a design question. |
| Unified Interface | **Not established in source** | A coherent cross-system protocol has not been identified in this source pass. |
| Core Assembly | **Accepted conceptual responsibility; no distinct implementation identified** | Existing Effect Layer assembly is not automatically equivalent to the proposed Echo Core Assembly. |

## Overall conception

The high-level vision, biological inspiration and conceptual system roles are recorded separately from detailed system specifications. See [Conceptual System Map](02-architecture/HIGH-LEVEL-SYSTEM-MAP.md). This conceptual map is not an implementation report.

## Current architectural direction

- Model is the core cognitive主体: reasoning, understanding, planning and decisions belong here.
- Body provides the runtime foundation and functional capabilities that Model uses.
- Core Runtime is lightweight infrastructure, not a central controller or business orchestrator.
- A logical Echo Core Host owns the lifecycle of a running Echo Core instance.
- Runtime Bootstrap initializes Runtime-owned foundations only; Core Assembly organizes Model and Body system composition.
- Runtime Context exposes only per-system authorized capability views.
- Runtime manages only resources it creates and owns.
- Echo Immune System V1 remains frozen.
- Existing MiMoCode runtime primitives must be assessed for reuse; do not build a parallel runtime before understanding their lifecycle and ownership.

## Current focus / next action

1. Complete R0 by comparing snapshot commit 14ecf7d with the archived R1/R2 and Phase 2 reports.
2. Resolve the 0.1.15 / 0.1.16 baseline discrepancy and identify the exact local source revision represented by the imported working tree.
3. Inspect current startup/shutdown, ManagedRuntime disposal, database closure, Instance lifecycle and resource ownership.
4. Reconcile Memory, Nervous, Execution and Task implementation status against current source and tests.
5. Locate and archive the external C1 `context-gate` implementation: no `context-gate/` path is present in the committed repository tree, although reports reference it as a deployed user-config plugin.
6. Reconfirm local deployment/runtime facts before treating R2 environment results as current.
7. Only after R0 is closed, compare findings with accepted Core Runtime V1 direction and propose a small, testable next step.

## Important caveats

- The current pass is a static GitHub source inspection. No local build, runtime launch or test suite was executed in this session.
- R1/R2 reports are valuable historical evidence, but they predate or use a different baseline from the newly imported source snapshot. Do not silently treat them as proof that every current file has been re-tested.
- `docs/PHASE-STATUS.md` and the R2 report disagree on the MiMoCode version. Resolve before freezing a source baseline.
- The R0–R6 sequence remains a suggested planning structure, not a formally approved acceptance plan.
