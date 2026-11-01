# Core Runtime V1 — Accepted Design Direction

Status: **Accepted design direction; not implemented.**

## Definition

Core Runtime is Echo Core's basic operating environment. It provides a unified, stable and manageable foundation for cognition and Body functional systems. It does not itself perform cognition, business operations or system-level decision-making.

## Responsibilities

- initialize Runtime-owned foundations;
- maintain Runtime lifecycle state;
- manage the Runtime instance lifecycle;
- provide common infrastructure such as configuration access, logging conventions, error conventions and basic resource support;
- create a Runtime Context and system-scoped capability views;
- release Runtime-owned resources safely during shutdown or failed initialization.

The named responsibilities are logical concerns, not a requirement to create one class, service or process per concern.

## Explicit non-responsibilities

- Model reasoning, planning or decisions;
- Memory organization, retrieval or forgetting;
- Immune protection decisions or policy;
- functional-system initialization internals;
- business coordination or routing between systems;
- deciding which capability Model should invoke;
- task queues, task state, cancellation or central scheduling;
- a general-purpose async executor;
- lifecycle management of resources owned by functional systems.

## Lifecycle

States:
- `Created`: instance exists; initialization has not begun.
- `Initializing`: Runtime-owned foundations are being initialized.
- `Ready`: Runtime initialization is complete and the basic environment is ready.
- `Running`: Runtime is active and provides its operating services, including waiting for and receiving commands.
- `Stopping`: orderly shutdown is in progress.
- `Stopped`: orderly shutdown completed.
- `Failed`: an unrecoverable Runtime lifecycle failure occurred.

Normal path:
`Created → Initializing → Ready → Running → Stopping → Stopped`

An unrecoverable lifecycle failure transitions to `Failed`.

Rules:
- `Ready` and `Running` are distinct. In the ordinary startup flow, Ready is a transition after initialization and before Running; waiting for user input belongs to Running.
- One Runtime instance has one lifecycle and may be started only once.
- A stopped instance cannot be restarted. A new start creates a new Runtime instance.
- `Stopped` and `Failed` are terminal and irreversible.
- `Stopping` represents orderly shutdown. An unrecoverable failure during shutdown results in Failed.
- `Ready` may be useful as a stable state in some future on-demand arrangements; ordinary use is expected to proceed into Running. Exact policy remains to be refined.

## Failure boundary

Potential Runtime-fatal conditions identified:
- Runtime initialization cannot complete;
- critical Runtime state is corrupted;
- the program is stuck/hung;
- the process cannot shut down normally.

Ordinary business failures do not automatically fail Runtime. Examples include one Memory retrieval failure, one Model API failure, one functional-system fault or one ordinary async operation failure.

A true process hang may require Host or an external process monitor to detect; no complex Supervisor is currently approved.

## Host relationship

Runtime lifecycle belongs to a running Echo Core instance, not to Model, the UI window or an individual functional system.

Host creates and starts Runtime and may request orderly shutdown. Runtime maintains its state but does not decide whether the overall instance should continue to exist.

Closing or hiding Echo Interface does not inherently mean Runtime must stop.

## Bootstrap and Core Assembly

### Runtime Bootstrap

Initializes Runtime-owned foundations in four conceptual stages:
1. **Configuration:** load, merge and validate Runtime base configuration.
2. **Infrastructure:** establish logging, error handling and other Runtime foundations.
3. **Runtime Context:** create Context and prepare system-scoped capability views.
4. **Readiness Check:** check Runtime's own basic environment only; do not turn this into a health audit of Memory, Immune or other business systems.

A failed stage stops later stages. Runtime-owned resources already created must be cleaned up safely. Cleanup failure must not overwrite the original initialization failure.

### Core Assembly

Application-level composition of Model and Body systems, their initialization and necessary dependency connections. Functional systems own their internal initialization.

Runtime Ready means the Runtime foundation is ready; it does not mean the entire Echo Core application is fully operational.

## Runtime Context and capability views

Runtime Context is the shared lifecycle-scoped source of common Runtime capabilities.

- It is not a central controller.
- It contains no business logic, task scheduler or system-to-system coordinator.
- Systems do not all receive unrestricted access to the complete Context.
- Each system receives a restricted capability view containing only capabilities it has been authorized to use.
- A system declaring a capability requirement does not automatically grant itself that capability.
- Runtime defines capability types; Host supplies authorization policy; Runtime checks authorization and returns the restricted view.
- Capability names discussed so far are examples only, not a finalized API.

Lifecycle:
- Context is created for a Runtime instance.
- Capability views are prepared and authorized during initialization.
- They remain valid while the Runtime instance is active.
- They become invalid when Runtime reaches Stopped or Failed.
- A new Runtime instance receives a new Context; old Context objects must not be reused across instances.

## Configuration

Three sources:
1. Default Configuration
2. User Configuration
3. Environment Configuration

Priority:
`Environment > User > Default`

Runtime loads, parses, merges and validates the base configuration during initialization to create the effective configuration snapshot.

The effective configuration is immutable for the lifetime of one Runtime instance. Changing it requires a new Runtime instance. This does not mean values must be hard-coded.

Functional systems define and validate the semantics and constraints of their own configuration. A functional-system configuration error is handled by that system and does not automatically fail Runtime. Failure to load or parse required Runtime base configuration is a Runtime initialization failure.

Secret/environment handling needs a secure implementation when required; no specific Secret Provider is selected.

## Logging

Runtime provides common logging conventions and, if Runtime creates it, the lifecycle of the shared logging infrastructure.

Conventions may include:
- levels such as DEBUG, INFO, WARN and ERROR;
- timestamp representation;
- structured fields;
- system identifier;
- Runtime instance identifier;
- error details and correlation identifiers.

Each system decides which business events to log and when. A common logging convention does not require one common storage destination.

Exact schema and format remain open.

## Error handling

Runtime provides common error representation and basic handling conventions, not business recovery logic.

Potential shared conventions include error structure, category/severity, context and correlation identifiers, propagation and delivery rules.

Functional systems identify their own business failures, decide recoverability, retry or degrade when appropriate, and report errors using the common conventions. For example, Memory owns retry policy for a failed Memory write. Such a failure does not automatically transition Runtime to Failed.

Exact error schema and propagation rules remain open.

## Resource lifecycle

Runtime manages only resources it creates and owns.

- **Owned Resources:** created and owned by Runtime; Runtime is responsible for their lifecycle.
- **Borrowed Resources:** used by Runtime but owned elsewhere; Runtime must not destroy them.

Runtime-owned lifecycle-level resources identified:
1. Logging Infrastructure
2. Error Handling Infrastructure
3. Runtime Context / Capability Views
4. Configuration Snapshot

External configuration files and environment variables are borrowed inputs; the resolved configuration snapshot is Runtime-owned.

Context and capability views become invalid at Runtime termination; this does not require every object to implement an explicit destroy method.

Functional systems manage their own connections, threads, temporary files, caches, intermediate data and business resources.

No general Resource Registry or elaborate allocation/reclamation framework is approved. Release resources in reverse dependency order: dependents first, dependencies last. Keep logging available as long as practical during cleanup.

No general async executor is a required Runtime resource. Model may organize asynchronous work; functional systems manage any internal execution resources they need.

## Incremental construction

Accepted direction: reuse the existing environment and evolve incrementally.

- Preserve R1/R2 verified foundations.
- Do not modify frozen Immune V1 protection logic.
- Audit actual code and dependencies before choosing implementation boundaries.
- Prefer reuse over replacement.
- Each change should be testable and reversible.
- Add no abstraction without a demonstrated need.

Suggested stages (planning proposal, not an approved acceptance plan):
- R0: audit existing code/runtime/environment.
- R1: Runtime lifecycle.
- R2: Bootstrap and configuration.
- R3: logging and error handling.
- R4: Runtime Context and capability views.
- R5: resource lifecycle and shutdown validation.
- R6: Runtime V1 integrated acceptance.

R0 findings may change the sequence.
