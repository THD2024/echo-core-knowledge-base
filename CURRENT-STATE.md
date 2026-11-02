# Current State

Last updated: 2026-11-02

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
| Echo Core Runtime V1 | **Partially implemented on an isolated branch; not production-integrated** | Lifecycle state machine and Host-facing CoreRuntime facade exist and passed focused local tests; production Host/Runtime integration and the remaining R1 scope are still open. |
| D-010 resource lifecycle audit | **Verified for Effect 4.0.0-beta.48 tested scenarios** | MemoMap sharing, Layer identity, Scope LIFO, disposal/Fiber behavior and idempotency were locally verified; Echo ownership architecture is not yet designed or implemented. |
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

1. Formally draft and review the D-010 Resource Ownership Architecture: Host, Process Owner, Core Runtime and Feature Owner responsibilities; resource admission, task draining, sharing and release.
2. Decide whether the first implementation needs an explicit dependency graph/registry or can rely on scoped ownership plus Effect Scope/MemoMap.
3. After the architecture is approved, define the next small implementation slice and its local acceptance tests.
4. Keep the existing Lifecycle/CoreRuntime API work isolated until its integration boundary and branch strategy are explicitly decided.
5. Preserve the existing R0 findings: AppRuntime disposal is not wired into observed production shutdown paths; Database is a process-level lazy singleton with unsafe close semantics; Runtime factory ownership is inconsistent.
6. Keep Echo Immune System V1 frozen. Do not merge Capability Authorization PR #1 or treat it as integrated into Runtime.

## Important caveats

- The current pass is a static GitHub source inspection. No local build, runtime launch or test suite was executed in this session.
- R1/R2 reports are valuable historical evidence, but they predate or use a different baseline from the newly imported source snapshot. Do not silently treat them as proof that every current file has been re-tested.
- `docs/PHASE-STATUS.md` and the R2 report disagree on the MiMoCode version. Resolve before freezing a source baseline.
- The R0–R6 sequence remains a suggested planning structure, not a formally approved acceptance plan.


## 2026-11-02 — Runtime and D-010 progress

### Core Runtime Lifecycle implementation branch

- Source repository branch: `runtime-core/lifecycle-v1`.
- Latest locally verified commit: [`03732e8551b79da177c1fae44273dc00c4604fd9`](https://github.com/THD2024/echo-core/commit/03732e8551b79da177c1fae44273dc00c4604fd9).
- Lifecycle state machine plus Host-facing `CoreRuntime` facade are implemented on this isolated branch.
- Local verification report `R1-LOCAL-TEST-003`: 18/18 tests passed, repeated six times; target failing test passed independently; no unhandled rejection.
- Typecheck still exits 1 because of 12 pre-existing errors also present on the main baseline; the new `echo-runtime` files introduced no reported type errors.
- Scope limitation: this verifies the Lifecycle primitive and CoreRuntime Instance API only. It does not mean all of R1 or Core Runtime V1 is complete or integrated into production Host paths.

### D-010 Effect resource lifecycle audit

- Effect dependency confirmed locally as `4.0.0-beta.48`.
- Same MemoMap + same Layer object: one build; first Runtime disposal leaves the shared resource alive; last observer disposal runs its Finalizer once.
- Different Layer objects providing the same Service build and release independently.
- Scope Finalizers are stored in insertion order and traversed in reverse order; direct Scope and three Layer composition experiments all confirmed C → B → A for A/B/C registration/acquisition.
- ManagedRuntime disposal does not interrupt or await business Fibers started through `runFork`; task draining must be an explicit owner responsibility.
- Repeated/concurrent disposal did not duplicate Finalizer execution in the tested scenarios.
- Full evidence: [D-010 Effect Resource Lifecycle Verification](05-audits-and-evidence/D-010-EFFECT-RESOURCE-LIFECYCLE-VERIFICATION.md).

### Current D-010 status

- Effect implementation behavior: **Verified for the tested version and scenarios**.
- Echo resource ownership architecture: **Not yet designed/approved**.
- Resource manager implementation: **Not started**.
- Next step: GPT prepares the concrete architecture proposal; project owner reviews key authority and ownership decisions before implementation.
