# Accepted Design Directions

This register records directions explicitly agreed in design discussion. "Accepted" does not mean implemented or formally verified. Formal decision IDs and final ADR documents should be added only after the source discussion and evidence are checked.

| Topic | Direction | Status |
|---|---|---|
| Essence-first principle | Understand the essential rule behind borrowed concepts; adapt it to Echo Core; avoid complexity without a concrete problem. | Accepted |
| Model / Body boundary | Model owns cognition and capability selection; Body provides runtime foundations and functional capabilities. | Accepted conceptual direction |
| Immune V1 | Preserve its verified protection behavior; do not modify casually during Runtime architecture work. | Frozen |
| Runtime role | Lightweight operating foundation, not cognition, business logic, central controller or scheduler. | Accepted |
| Runtime evolution | Reuse existing environment and evolve incrementally; audit before implementation. | Accepted |
| Host | A logical role owns start/end of a running Echo Core instance; need not be a separate process/module. | Accepted |
| UI relationship | Showing/hiding the interface does not inherently determine Runtime lifecycle. Resident and on-demand policies belong to Host. | Accepted |
| Bootstrap / Assembly | Runtime Bootstrap initializes Runtime-owned foundations; Core Assembly composes and initializes application systems. | Accepted |
| Runtime Context | Lifecycle-scoped Context provides common capabilities through system-specific restricted views. | Accepted |
| Capability authorization | System declares needs; Runtime defines capability types; Host supplies policy; Runtime checks and grants restricted views. | Accepted |
| Configuration | Default, User and Environment layers; precedence Environment > User > Default; effective snapshot immutable per Runtime instance. | Accepted |
| Logging | Runtime defines conventions and manages its own logging infrastructure; systems own business logging decisions and storage choices. | Accepted |
| Error handling | Runtime defines shared error conventions; systems own business recovery and retry policy. | Accepted |
| Resource ownership | Runtime manages only resources it creates and owns; functional systems manage their internal resources. | Accepted |
| Resource release | Release in reverse dependency order; no general-purpose resource registry/framework without demonstrated need. | Accepted |
| Async execution | No general async executor or central task scheduler is required in Runtime V1. | Accepted |

## Open items

- Exact Runtime API and implementation boundaries after R0.
- Exact lifecycle API and how Host observes state and requests shutdown.
- Exact configuration schema and secret handling.
- Logging schema, timestamp standard, sinks and correlation fields.
- Error object schema, classification and propagation rules.
- Capability type definitions, declaration format and authorization representation.
- How Runtime invalidates Context/capability views after terminal states.
- Host's handling of process hangs and external monitoring.
- Whether Ready is used as a long-lived state in a future on-demand mode.
- Exact Core Assembly contract and system startup ordering.

## Decision hygiene

Do not invent formal decision IDs or claim an ADR has been formally ratified unless the project owner explicitly approves it. Preserve the distinction between accepted direction, proposal, implementation and verification.
