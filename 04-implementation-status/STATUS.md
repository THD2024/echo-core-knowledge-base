# Implementation Status

Last reconciled: 2026-11-02; source inspection baseline remains `14ecf7deef8f7c4db325888e5056031e730dac1c`.

## Verified and frozen

### Echo Immune System V1
- Status: **Verified / Frozen**.
- Formal acceptance completed.
- Final PID identity verification passed, including formal path, SHA256, creation time, parent process and pre-start disk hash validation.
- C4_normal and C2_bad_params stats.jsonl raw records, CID association and seven diagnostic-field persistence checks passed.
- R1/R2 and supplementary acceptance records are archived in the source repository.
- Protection logic must not be changed as part of speculative Runtime architecture work.

The committed source contains the Core-side diagnostic schema/classifier and event publication paths. The C1 `context-gate` consumer is referenced as a deployed user-config plugin in reports, but no `context-gate/` implementation path is present in the committed repository tree. Its implementation/evidence must be separately archived before claiming the entire deployed chain is reproducible from this repository.

## Implemented capabilities observed in source

These are existing MiMoCode-derived capabilities, not proof that the full conceptual Echo systems have reached their intended final form.

### Runtime foundations
- Effect `ManagedRuntime` is used by `AppRuntime` and `BootstrapRuntime`.
- Effect Layers assemble application services.
- `Instance.provide` caches and scopes project instances, tracks active requests/executions and disposes per-directory state.
- `InstanceBootstrap` loads configuration, initializes plugins and supporting services, starts Memory reconciliation in the background, and subscribes Metrics.
- The proposed Echo Core Runtime V1 is **not** integrated as a complete production architecture with its accepted Host, Runtime Context/Capability Views and resource-ownership contract.
- An isolated branch, `runtime-core/lifecycle-v1`, contains the Lifecycle state machine and Host-facing `CoreRuntime` facade. Commit [`03732e8`](https://github.com/THD2024/echo-core/commit/03732e8551b79da177c1fae44273dc00c4604fd9) passed focused local verification: 18/18 tests, repeated six times. This is a verified primitive/API slice, not full R1 acceptance or production integration.
- The focused typecheck still exits 1 due to 12 historical errors also present on the main baseline; the new `echo-runtime` files introduced no reported type errors.
- D-010 local experiments against Effect `4.0.0-beta.48` verified shared MemoMap observer lifetime, Layer identity behavior, same-Scope LIFO Finalizers, disposal idempotency in tested scenarios, and the fact that ManagedRuntime disposal does not drain/interrupt business Fibers. See [D-010 verification report](../05-audits-and-evidence/D-010-EFFECT-RESOURCE-LIFECYCLE-VERIFICATION.md).
- The D-010 resource ownership architecture is still **proposed / under design**; no Echo ResourceManager or ownership registry has been implemented.

### Memory
- Markdown files are the durable memory source; SQLite FTS5 is a search index.
- `Memory.Service` exposes `root`, `reconcile` and `search`; it has no direct write/delete API.
- Search uses FTS5 BM25, with configurable filters and result floor; no vector/embedding semantic retrieval was found in the inspected implementation.
- Memory writes occur through file/tool paths and agent/checkpoint workflows, guarded by memory path/write-gate logic.
- Status: **Implemented, with known architectural and retrieval limitations**; not “not implemented”.

### Nervous / coordination
- Main cognition/coordination is concentrated in the SessionPrompt run loop and associated session/processor logic.
- Effect Bus, TaskRegistry, Actor/Subagent, plan mode, goal checks and context/checkpoint mechanisms are implemented.
- Planning and decomposition remain substantially model/prompt-driven; Bus is best-effort event delivery, not a guaranteed durable message bus.
- Status: **Partially implemented capability set**, not a complete independent Nervous system specification.

### Execution
- Tool registration and execution flow, ToolGate, ToolSettlement, actor/subagent execution, result feedback and recovery mechanisms exist.
- Archived Phase 2 assessment identifies known limitations, including local-tool timeout gaps and failure-reporting issues.
- L2 bash non-zero exit handling is present in the imported source: it publishes an error ToolCall with diagnostic data instead of recording every bash result as success. This should still be covered by the archived regression evidence and revalidated against the snapshot.
- Status: **Implemented capability set with known gaps**, not a complete autonomous execution architecture.

### Task Registry
- SQL-backed task tree, task events and task lifecycle operations exist.
- Task Registry existence does not imply automatic planning, DAG scheduling or a distinct Task System approved as part of the future Echo architecture.

## Not established as implemented in the intended Echo architecture

- Core Runtime V1 as a distinct Echo-specific architecture;
- accepted Runtime lifecycle state machine;
- Runtime Bootstrap contract as specified in CORE-RUNTIME-V1;
- Core Assembly as a distinct application assembly responsibility;
- Runtime Context and per-system Capability Views;
- unified configuration snapshot and accepted Runtime-wide logging/error contracts;
- accepted Echo-level resource ownership and shutdown protocol (Effect Scope/MemoMap primitives have been verified, but Echo Owner policy is not implemented);
- Unified Interface as the agreed Model↔Body communication protocol;
- complete Model/Nervous cognitive architecture;
- semantic Memory retrieval, verified memory confidence lifecycle and complete memory governance.

## Required R0 reconciliation

Before implementation:
- compare current source snapshot with R1/R2 and Phase 2 report baselines;
- resolve CLI package version 0.1.15 vs PHASE-STATUS v0.1.16;
- verify ManagedRuntime disposal and Database.close wiring;
- inspect process-level shutdown and Instance disposal paths;
- identify C1 external plugin source and exact deployed revision;
- recheck Memory write-path security, including shell/bypass boundaries;
- validate L2 bash error reporting and Immune freeze regression evidence;
- separate static code presence from tests actually run against this exact snapshot.

No local build or test suite has been run as part of this static source inspection.

## Change recording template

For each future change, record:
- date;
- source repository and commit;
- scope and reason;
- files/modules changed;
- tests or audit commands executed;
- results and evidence;
- status label (Implemented / Verified / Accepted / Proposed / Open);
- rollback path or known risk.


## 2026-11-02 progress boundary

The D-010 audit is evidence about the behavior of the pinned Effect dependency, not an acceptance of a specific Echo resource-management architecture. The next step is design review of Host / Process Owner / Core Runtime / Feature Owner authority and shutdown responsibilities. Do not start implementation until that design is approved.
