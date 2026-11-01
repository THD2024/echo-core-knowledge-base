# Echo Core — High-Level System Map

Status: **Conceptual architecture record.** The existence of a system as a design concept does not prove that it is implemented, independently deployable, or must be a separate code module. Implementation status must be reconciled against the current source repository.

## 1. Overall model

Echo Core is conceived as a cognitive system supported by a Body of specialized capabilities and a Runtime foundation.

At the highest level:

- **Cognitive / Model:** the central cognitive主体; understands, reasons, plans and decides.
- **Body:** the supporting environment and functional systems through which Model receives information and acts.
- **Core Runtime:** provides the basic operating environment and lifecycle-scoped infrastructure.
- **Host:** owns the lifecycle policy for a running Echo Core instance.
- **Core Assembly:** composes Model and Body systems at the application level.
- **Unified Interface:** provides a consistent communication contract between Model and functional systems.

The diagram below is conceptual; it does not prescribe a specific process, thread, class hierarchy or deployment topology.

```text
                    User / External Environment
                              |
                       Echo Interface
                              |
                         Echo Core Host
                              |
                    Core Runtime Foundation
                              |
                        Core Assembly
                              |
          +-------------------+-------------------+
          |                   |                   |
      Cognitive /          Body Systems       Unified Interface
        Model                 |                   |
          |            +------+------+             |
          |            |      |      |             |
          +---------- Memory  Immune  ... <--------+
                       |       |
                  persistence  protection
                       |
             Other functional capabilities
       (Nervous, Sensory, Execution, etc.)
```

Unified Interface is a communication mechanism, not a central business router owned by Runtime. The exact placement and direction of calls must be defined by its future protocol.

## 2. Conceptual systems and responsibilities

### 2.1 Cognitive System / Model

**Role:** central cognitive主体.

Conceptual responsibilities:
- understand and interpret input and context;
- reason about situations;
- form plans and make decisions;
- choose which functional capabilities to invoke;
- evaluate feedback and adjust plans.

This is the cognitive role, not necessarily a separate Body subsystem. The Model implementation and its exact internal decomposition remain to be verified and designed.

### 2.2 Memory System

**Role:** retain, organize and retrieve information over different timescales.

Conceptual responsibilities:
- accept information to be stored;
- retrieve relevant information for Model;
- associate and maintain information;
- manage retention and forgetting;
- support continuity and accumulated experience.

Two dimensions must remain distinct:

**Lifecycle / retention duration**
- transient;
- short-term;
- long-term;
- permanent.

**Memory type**
- long-term memory may contain episodic, semantic and procedural forms.

Memory is expected to own its internal storage, indexing, retrieval and maintenance mechanisms. Model may actively query, write, modify and delete information through the agreed communication contract, subject to Memory's rules.

### 2.3 Immune System

**Role:** protect Echo Core and detect/respond to abnormal conditions within its defined scope.

The verified Immune V1 design contains five named parts:
- **Sentinel:** monitors for abnormal conditions.
- **Recognition:** identifies and classifies detected conditions.
- **Response:** records, classifies and raises alerts.
- **Immune Memory:** retains the immune-memory data structure.
- **Tolerance:** preserves fault-tolerance principles.

V1 boundaries matter: it does not include automatic retry, code/configuration modification, deletion, self-repair or a complete learning system.

Immune has bounded protective autonomy. Model may control it within explicit authority limits, but cannot independently disable protection; disabling protection requires an explicit user request.

**Status:** Immune V1 is formally verified and frozen. Do not change its protection logic as part of speculative architecture work.

### 2.4 Nervous System

**Role:** organize information and coordinate the cognitive work among relevant capabilities.

Conceptual responsibilities discussed:
- receive and organize information;
- help structure information into a coherent thought/work context;
- coordinate interactions among relevant modules or capabilities;
- support the plan–act–feedback cycle between Model and execution capabilities.

Nervous System is not the same thing as Core Runtime. Runtime supplies the operating foundation; Nervous System belongs to the cognitive/application behavior layer.

Exact boundaries between Model's own reasoning and a distinct Nervous System remain an architectural question. Do not create a separate module merely because the biological analogy has a nervous system.

### 2.5 Sensory System

**Role:** acquire and normalize information from the user and external environment for cognitive processing.

This is a conceptual input/perception role. Its exact modalities, interfaces and implementation boundaries remain open. It may be realized through multiple adapters or capabilities rather than one monolithic system.

### 2.6 Execution System

**Role:** turn plans or selected intentions into concrete actions through tools, APIs or other capabilities, then return results.

Conceptual responsibilities:
- receive an action request or execution plan;
- invoke the required tool/capability;
- track the result of the operation;
- return feedback and failures;
- expose enough status for Model to decide what to do next.

A previously discussed task concept includes each task having an ID and a status, with completion and cancellation represented explicitly. Exact task lifecycle, ownership and interfaces remain to be designed.

Execution is distinct from Runtime. Runtime does not become a universal task scheduler or own every functional system's asynchronous work. The boundary between Execution's task orchestration and Model-organized asynchronous work must be resolved from concrete use cases.

### 2.7 Metabolism / Energy Management

**Role:** manage scarce operating resources and support sustainable operation.

The concept was discussed by analogy with biological metabolism, including token/resource consumption. The exact resource model, accounting, budget policy and authority are not yet established. Do not assume it is a literal biological energy simulation or add it to Runtime by default.

### 2.8 Homeostasis

**Role:** maintain awareness and coordination of longer-running system condition and operating balance.

This is a conceptual role inspired by biological homeostasis. Exact responsibilities, monitored variables, intervention authority and its relationship to Immune, Runtime and Host remain open. It must not duplicate Runtime lifecycle management or override Immune V1.

## 3. System relationships and interaction patterns

### Cognitive loop

A high-level pattern discussed:

1. Sensory/input capabilities provide information.
2. Model interprets the information with relevant Memory context.
3. Nervous/cognitive coordination supports organizing thought and selecting next steps.
4. Model decides on an action.
5. Execution carries out the action through the required capability.
6. Execution returns results, failures and status.
7. Model evaluates feedback and continues, revises or stops.

This is a conceptual interaction loop, not a finalized protocol or mandatory linear pipeline. Some tasks may omit steps or involve repeated cycles.

### Memory relationship

Memory supports cognition but does not replace Model's reasoning. Memory owns memory-specific mechanisms; Model decides when and why to use memory through the communication contract.

### Immune relationship

Immune protects within its own domain and can act autonomously within approved limits. It is not a general-purpose supervisor for all business operations and must preserve its frozen V1 scope.

### Runtime relationship

Runtime provides the shared operating foundation for the running instance. It does not decide which functional system Model should invoke and does not coordinate business workflows.

### Unified Interface relationship

Unified Interface standardizes communication between Model and functional systems. It should hide unnecessary internal implementation details while preserving system-specific responsibilities. It is not synonymous with Runtime or Core Assembly.

## 4. Design principles governing every system

1. **Essence-first borrowing:** understand the underlying rule behind biological or engineering analogies; adapt the principle rather than copy surface anatomy or implementation.
2. **Necessary complexity only:** every module, abstraction and mechanism must solve a real problem. If its absence causes no concrete issue, defer it.
3. **Clear responsibility boundaries:** each system owns its own business logic, internal state, initialization and internal resources unless explicitly agreed otherwise.
4. **Cognitive centrality:** Model remains the main reasoning and decision-making center; functional systems provide specialized capabilities.
5. **Bounded autonomy:** autonomy is granted according to a system's responsibility. Do not make every subsystem an autonomous agent.
6. **Unified external contract, independent internals:** systems communicate through consistent conventions without requiring identical internal designs.
7. **Least authority:** a system receives only the capabilities and permissions it needs; declaring a need does not automatically grant authority.
8. **User authority over critical protection:** Model cannot independently disable Immune protection.
9. **Lifecycle clarity:** distinguish Runtime instance lifecycle from Model activity, interface visibility and individual task lifecycles.
10. **Ownership clarity:** the creator/owner of a resource is responsible for its lifecycle; Runtime does not take over functional-system resources.
11. **Incremental, evidence-led construction:** reuse verified foundations, make small reversible changes, and audit actual code before implementation.
12. **Separate concept, acceptance, implementation and verification:** a design map is not an implementation report.

## 5. Historical sequencing discussion and current next step

A conceptual system-development priority was previously discussed as:

**Immune → Memory → Nervous → Execution**, with Sensory, Metabolism/Energy and Homeostasis considered later, after general foundations.

This is a historical proposed priority, not a current approved implementation schedule. Immune V1 has since been completed, verified and frozen. The current immediate engineering direction is to perform R0 against the actual source repository before making Core Runtime implementation changes, while preserving R1/R2 evidence and Immune V1.

## 6. Status discipline and open questions

This map intentionally does not label Memory, Nervous, Sensory, Execution, Metabolism or Homeostasis as implemented. Their exact current implementation status must be verified against the current source tree and audit records. Historical audit notes and current code may describe different snapshots.

Open questions include:
- Which conceptual roles are implemented as distinct systems versus responsibilities inside Model or another system?
- What is the precise boundary between Model reasoning and Nervous coordination?
- What task lifecycle belongs to Execution, and what asynchronous work is organized directly by Model?
- How do Sensory adapters normalize inputs?
- What resource accounting belongs in Metabolism/Energy?
- What variables and authority belong to Homeostasis?
- How do Immune and Homeostasis avoid overlapping responsibility?
- What is the canonical Unified Interface protocol?
- Which systems require autonomous behavior, and what are their authority boundaries?

These questions should be resolved incrementally from actual needs, code inspection and explicit owner decisions.
