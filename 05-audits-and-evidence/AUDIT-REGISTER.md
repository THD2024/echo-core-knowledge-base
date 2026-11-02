# Audit and Evidence Register

## R0 — Source snapshot inspection and reconciliation

- Status: **In progress — static source pass only**.
- Inspection date: 2026-11-01.
- Source repository: [THD2024/echo-core](https://github.com/THD2024/echo-core), branch `main`.
- Inspected snapshot: [`14ecf7deef8f7c4db325888e5056031e730dac1c`](https://github.com/THD2024/echo-core/commit/14ecf7deef8f7c4db325888e5056031e730dac1c).
- Snapshot imports the MiMo-Code working-tree source under `MiMo-Code/` and archives docs/bench evidence.
- Static observations: Effect ManagedRuntime/AppRuntime/BootstrapRuntime and project Instance lifecycle exist; Memory FTS5/file-backed capabilities, TaskRegistry, Bus, Actor/Subagent and tool Execution paths exist; Core-side Immune diagnostic classification is present.
- L2 bash non-zero exit reporting is present in the imported `session/prompt.ts` path.
- Archive gap: no `context-gate/` path was found in the committed repository tree, although the archived reports describe C1 as a deployed external user-config plugin.
- Baseline discrepancy: CLI package metadata and R2 report say 0.1.15; `docs/PHASE-STATUS.md` says 0.1.16.
- Limitation: this was static GitHub source inspection only. No local build, runtime launch or test suite was executed. Current source still needs comparison with historical audit baselines and exact runtime evidence.

## R1 — Existing System and Runtime Audit

- Status: **Verified / Passed (historical audit)**.
- Date: 2026-10-31.
- Purpose: record the existing system structure and runtime behavior.
- Canonical report: `docs/memory-research-01e/arch-audit-final.md` in the source repository.
- Interpretation: evidence of the audited state at the time and source baseline of that audit.
- Limitation: R1 does not automatically prescribe the future Core Runtime architecture. Reconcile against snapshot 14ecf7d before treating line-level findings as current.

## R2 — Local Deployment and Environment Audit

- Status: **Verified / Passed (historical audit)**.
- Date: 2026-10-31.
- Canonical report: `docs/memory-research-01e/arch-audit-r2-deployment.md` and supplement `arch-audit-r2-supplement.md` in the source repository.
- Purpose: record local deployment, environment, startup and platform conditions.
- Interpretation: evidence of the audited local environment at that time; not freshly re-run during this static source pass.
- Limitation: R2 does not automatically prescribe the future Core Runtime architecture.

## Echo Immune System V1 final acceptance

- Status: **Verified / Frozen**.
- Final PID identity check passed:
  - formal executable/process path;
  - SHA256;
  - process creation time;
  - parent process;
  - disk hash checked before startup.
- C4_normal and C2_bad_params stats.jsonl records checked.
- CID association and seven diagnostic fields were confirmed persisted.
- Design and freeze records include `docs/immune-system-v1-design.md`, `docs/IMMUNE-V1-SUPPLEMENTARY-AUDIT.md`, `docs/IMMUNE-V1-E2E-EVIDENCE.md`, `bench/phase1b/IMMUNE-V1-FREEZE-REPORT.md` and `bench/phase2-pre/A-IMMUNE-V1-FREEZE-RECORD.md` in the source repository.
- Protection logic is frozen and must not be casually modified.
- The Core-side diagnostic code is present in the source snapshot. The external C1 `context-gate` consumer implementation is not currently archived in the repository tree and must be separately recovered if full-chain reproducibility is required.

## Evidence storage

Canonical source repository and commit links are recorded above. Raw local process artifacts and machine-specific deployment evidence remain in their original environment unless explicitly archived. Do not fabricate evidence paths, hashes or test results.

## Future audit entries

Each entry should identify:
- audit ID and date;
- exact code commit and environment;
- scope and procedure;
- pass/fail criteria;
- raw evidence location;
- result and unresolved findings;
- reviewer/acceptance status.


## R0 — Runtime ownership findings (local source audit)

- Status: **Scoped audit completed; findings recorded; remediation/design open**.
- Audit ID: `R0-LOCAL-REF-001`.
- Source baseline: main snapshot `14ecf7deef8f7c4db325888e5056031e730dac1c`; local HEAD `432ffd9881587c4502a08c8cf2c1a710f7d787b3` was a descendant containing only Capability Authorization files relative to that baseline.
- Findings:
  - `BootstrapRuntime` has real production call paths.
  - `AppRuntime.dispose()` had no observed production call sites in the inspected source.
  - `Database.close()` had no observed production call sites; it calls `Client()`, so closing an uninitialized Database can trigger initialization before shutdown.
  - Runtime factories expose inconsistent disposal/ownership contracts; some ManagedRuntime wrappers do not expose disposal.
  - Database is a process-level lazy singleton and requires an explicit process-level lifecycle owner.
  - TUI Worker shutdown has a bounded wait before termination; Serve shutdown paths did not show complete AppRuntime/Database release wiring.
  - Detached bootstrap fibers and OTEL layer finalizers remain areas for careful lifecycle integration.
- Limitation: this records the scoped source audit findings, not a completed remediation or production shutdown acceptance. Some report tables had archival blanks; those do not invalidate the core findings.

## R1 — Lifecycle primitive and CoreRuntime facade local verification

- Status: **PASS for the isolated primitive/API slice; R1 overall remains open**.
- Branch: `runtime-core/lifecycle-v1`.
- Verified commit: [`03732e8551b79da177c1fae44273dc00c4604fd9`](https://github.com/THD2024/echo-core/commit/03732e8551b79da177c1fae44273dc00c4604fd9).
- Local report: `R1-LOCAL-TEST-003`.
- Lifecycle state machine and CoreRuntime facade tests: 18/18 passed, repeated six times; original rejection-handling test also passed independently.
- Typecheck: exit 1 due to 12 pre-existing errors also present on main; no reported type errors in the new `echo-runtime` files.
- Scope limitation: no production Host integration, AppRuntime/BootstrapRuntime integration, resource ownership implementation, or full R1 acceptance is implied.

## D-010 — Effect resource lifecycle verification

- Status: **PASS — tested Effect behavior verified; Echo ownership architecture remains open**.
- Date: 2026-11-02.
- Dependency: locally confirmed `effect@4.0.0-beta.48`.
- Local repository HEAD: [`03732e8551b79da177c1fae44273dc00c4604fd9`](https://github.com/THD2024/echo-core/commit/03732e8551b79da177c1fae44273dc00c4604fd9); working tree remained clean.
- Experiment scripts were outside the repository at `C:\Temp\d010-verify\`; no source, tests, configuration, or dependencies were modified.
- Verified:
  - same MemoMap + same Layer object shares one resource Scope; the last observer's disposal triggers the Finalizer once;
  - different Layer objects providing the same Service build/release independently;
  - same-Scope Finalizers execute in reverse registration order; direct Scope and `mergeAll`, `provideMerge`, and explicit `buildWithScope` experiments agreed;
  - repeated/concurrent disposal did not duplicate Finalizer execution in tested scenarios;
  - ManagedRuntime disposal does not interrupt or await business Fibers started with `runFork`.
- All reported local experiment commands exited 0.
- Detailed evidence: [D-010 Effect Resource Lifecycle Verification](D-010-EFFECT-RESOURCE-LIFECYCLE-VERIFICATION.md).
- Limitation: these are Effect dependency behavior findings, not proof that all production Runtime factories or shutdown paths are correct. They do not decide whether Echo needs a resource registry or dependency DAG.
- Next: draft and review the D-010 ownership architecture before implementation.
