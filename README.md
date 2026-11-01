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
- **Verified:** R1 existing-system/runtime audit; R2 local deployment/environment audit.
- **Accepted design direction:** build Core Runtime incrementally by reusing the existing environment, after a fresh R0 code and environment audit.
- **Not yet implemented:** Core Runtime V1, Memory, Unified Interface, Task System, Nervous System, and Core Assembly as described in design discussions.
- **Current focus:** establish a reliable engineering knowledge base, then inspect the actual Echo Core repository and perform R0 before changing runtime code.

## Documentation map

- [Current State](CURRENT-STATE.md) — concise status and next action.
- [Project Mission](01-project-overview/PROJECT-MISSION.md) — purpose and design principles.
- [Design Philosophy and Biological Inspiration](01-project-overview/DESIGN-PHILOSOPHY-AND-BIOLOGICAL-INSPIRATION.md) — long-term goal, organism-inspired principles, bounded autonomy and essence-first design.
- [System Boundaries](02-architecture/SYSTEM-BOUNDARIES.md) — Model, Body, Runtime, Host, Interface and system responsibilities.
- [Core Runtime V1](02-architecture/CORE-RUNTIME-V1.md) — accepted Runtime design direction.
- [Accepted Directions](03-design-decisions/ACCEPTED-DIRECTIONS.md) — decisions and their status.
- [Implementation Status](04-implementation-status/STATUS.md) — implemented vs. planned.
- [Audit Register](05-audits-and-evidence/AUDIT-REGISTER.md) — R1/R2 and Immune V1 evidence register.
- [AI Collaboration](AI-COLLABORATION.md) — working protocol for future assistant sessions.

## Maintenance

Update `CURRENT-STATE.md` whenever the project state materially changes. Record decisions with their status and rationale. Do not turn a discussion or proposal into an accepted decision without the owner's confirmation.
