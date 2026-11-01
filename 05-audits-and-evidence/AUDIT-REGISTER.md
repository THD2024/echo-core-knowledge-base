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
