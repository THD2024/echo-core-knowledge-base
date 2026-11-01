# Echo Core — System Boundaries

## Model

Model owns cognition:
- interpreting input and available context;
- reasoning, planning and decision-making;
- deciding which Body capabilities to invoke;
- organizing multi-step work when needed.

Model is not the Core Runtime and should not be burdened with infrastructure lifecycle responsibilities.

## Body functional systems

Body contains functional systems that expose capabilities to Model. Each system owns its internal business logic, state and resources unless a separate accepted design explicitly says otherwise.

Examples discussed include Memory and Immune. Their implementation status must be checked independently; conceptual descriptions do not imply implementation.

## Core Runtime

Core Runtime is the basic operating environment for Echo Core. It provides a stable, common and manageable foundation for Model and Body systems.

It is infrastructure, not cognition and not a central business controller.

It does not:
- reason or make decisions for Model;
- decide which system Model should call;
- own Memory or Immune business logic;
- coordinate business workflows between systems;
- provide a general task scheduler or universal async executor;
- own resources created internally by functional systems.

## Echo Core Host

Host is a logical role responsible for starting and ending a running Echo Core instance.

Host:
- creates a Runtime instance;
- initiates Runtime startup;
- observes Runtime lifecycle state;
- initiates orderly shutdown when the selected operating mode requires it.

Host need not be a separate process or independently deployed module. Its physical form remains an implementation choice.

Runtime maintains its own state; Host owns the higher-level decision to start or end the running instance.

## Core Assembly

Core Assembly is an application-level responsibility. It organizes the composition and initialization of Model and Body functional systems and connects necessary dependencies.

It is distinct from Runtime Bootstrap:
- Runtime Bootstrap initializes Runtime-owned foundations.
- Core Assembly composes and initializes the application systems.

Functional systems remain responsible for their own internal initialization.

## Echo Interface

Echo Interface is the user interaction entry point. It is not the Runtime itself and should not own Runtime lifecycle merely because a window is shown or hidden.

Two operating modes were discussed:
- **Resident:** the Runtime may continue in the background after the interface closes.
- **On-demand:** when the interaction session ends and the interface is the only entry point, Host may request Runtime shutdown.

Both modes use the same Runtime lifecycle implementation; the Host policy differs.

## Unified Interface

Unified Interface is the communication mechanism between Model and functional systems. It is not Core Runtime.

Runtime may provide infrastructure needed by communication mechanisms, but does not define business routing, decide call targets or centrally orchestrate system interactions.

The detailed protocol, message model and routing rules remain open.

## Boundary summary

| Concern | Owner |
|---|---|
| Cognition and capability selection | Model |
| Runtime lifecycle and base infrastructure | Core Runtime |
| Start/end of a running Echo Core instance | Host |
| Composition of Model and Body systems | Core Assembly |
| User interaction | Echo Interface |
| System-to-Model communication contract | Unified Interface |
| Internal business behavior and resources | Each functional system |
| Autonomous protection behavior | Immune, within user-defined authority boundary |
| Memory retention and retrieval mechanisms | Memory |

These are accepted conceptual boundaries, not proof that each named component already exists in code.
