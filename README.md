# Echo Core Knowledge Base

> Engineering memory for the Echo Core project. This repository records project intent, accepted design directions, implementation facts, audit evidence, and open questions so work can continue consistently across conversations.

## Source-of-truth rules

1. **Code is the source of truth for implementation.** Documentation must not claim a feature is implemented or verified unless the repository and evidence support it.
2. **Status labels matter.** Use:
   - `Implemented`: present in code.
   - `Verified`: passed a defined test or audit.
   - `Accepted`: design direction explicitly agreed by the project owner, not necessarily implemented.
   - `Frozen`: accepted and protected from casual modification.
   - `Proposed`: suggestion awaiting approval.
   - `Open`: unresolved question.
3. R1/R2 audits describe the existing system; they do not automatically dictate the future architecture.
4. Echo Immune System V1 is the only system currently recorded as formally accepted and verified. It is frozen.
5. Keep personal, health, emotional, account, and secret information out of this repository.

## Current project status

- **Verified / Frozen:** Echo Immune System V1.
- **Verified historical audits:** R1 existing-system/runtime audit and R2 local deployment/environment audit.
- **R1 implementation slice verified:** Lifecycle state machine and Host-facing CoreRuntime facade on isolated branch `runtime-core/lifecycle-v1`; not production-integrated and not full R1 acceptance.
- **D-010 Effect behavior verified:** shared MemoMap/Layer lifetime, Layer identity, Scope LIFO, tested disposal idempotency, and ManagedRuntime/Fiber separation for `effect@4.0.0-beta.48`.
- **Still under design:** Echo-level resource ownership architecture and shutdown protocol; no ResourceManager implementation started.
- **Not yet implemented as complete intended architectures:** Core Runtime V1, Memory, Unified Interface, Task System, Nervous System, and Core Assembly.
- **Current focus:** draft and review D-010 resource ownership boundaries before implementation.

## Documentation map

The knowledge base deliberately separates the project's overall conception from detailed engineering specifications.

### A. Overall vision, conception and philosophy

These documents describe what Echo Core is intended to become, why its conceptual systems exist, and the principles that guide future design. They are not detailed specifications or implementation claims.

- [Project Mission](01-project-overview/PROJECT-MISSION.md) — purpose and central Model / Body conception.
- [Design Philosophy and Biological Inspiration](01-project-overview/DESIGN-PHILOSOPHY-AND-BIOLOGICAL-INSPIRATION.md) — long-term goal, biological inspiration, autonomy and essence-first principles.
- [Conceptual System Map](02-architecture/HIGH-LEVEL-SYSTEM-MAP.md) — conceptual system roles, broad relationships, interaction pattern and open questions.

### B. Specific system designs and engineering architecture

These documents specify particular systems or concrete architectural boundaries. They must be read independently from the high-level conception.

- [System Boundaries](02-architecture/SYSTEM-BOUNDARIES.md) — concrete boundaries among Model, Runtime, Host, Assembly, Interface and functional systems.
- [Core Runtime V1](02-architecture/CORE-RUNTIME-V1.md) — specific accepted Runtime design; not implemented.
- [Accepted Directions](03-design-decisions/ACCEPTED-DIRECTIONS.md) — accepted engineering decisions and their status.

**Archive gap:** Echo Immune System V1 is verified and frozen, but this knowledge base currently contains only its summary/status and audit-register notes, not the complete detailed V1 specification or raw audit artifacts. These should be linked or imported from the canonical source project without changing the frozen implementation.

### C. Current state and evidence

- [Current State](CURRENT-STATE.md) — concise status and next action.
- [Implementation Status](04-implementation-status/STATUS.md) — implemented vs. planned.
- [Audit Register](05-audits-and-evidence/AUDIT-REGISTER.md) — R0/R1/D-010, R1/R2 and Immune V1 evidence register.
- [D-010 Effect Resource Lifecycle Verification](05-audits-and-evidence/D-010-EFFECT-RESOURCE-LIFECYCLE-VERIFICATION.md) — local source and experiment evidence for Effect Scope, Layer, MemoMap and ManagedRuntime behavior.
- [AI Collaboration](AI-COLLABORATION.md) — working protocol for future assistant sessions.

**Classification rule:** Overall philosophy and conceptual system roles belong to section A. A concrete system specification such as Immune System V1 or Core Runtime V1 belongs to section B. Implementation facts and verification evidence belong to section C. A document may cross-reference another layer, but must not merge their status or level of detail.

## Maintenance

Update `CURRENT-STATE.md` whenever the project state materially changes. Record decisions with their status and rationale. Do not turn a discussion or proposal into an accepted decision without the owner's confirmation.
