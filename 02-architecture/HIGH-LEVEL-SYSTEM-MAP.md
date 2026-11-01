# Echo Core — Conceptual System Map

Status: **High-level concept only.** This document records the overall vision and conceptual roles discussed for Echo Core. It is deliberately not a detailed system specification, implementation plan, module decomposition or verification report.

For the governing philosophy, see [Design Philosophy and Biological Inspiration](../01-project-overview/DESIGN-PHILOSOPHY-AND-BIOLOGICAL-INSPIRATION.md).

## 1. The overall conception

Echo Core is envisioned as an integrated cognitive system rather than a loose collection of tools.

Its central idea is a **Model supported by a Body**:

- **Model** is the cognitive center: it understands, reasons, plans and makes decisions.
- **Body** provides specialized capabilities and supporting functions through which Model can perceive, remember, act and maintain continuity.
- The systems within Body are not all identical in purpose or autonomy. Each exists to fulfill a distinct need.
- Model remains the central cognitive decision-maker. Specialized systems contribute capabilities and may own bounded autonomous behavior where their role requires it.

This is a conceptual picture, not a prescription for processes, threads, classes, services, packages or deployment topology.

## 2. Conceptual system roles

The names below describe roles in the overall conception. They do not assert that each role must become an independent software module.

### Model — cognition

The central cognitive role:
- interpret information and context;
- reason about situations;
- form plans and make decisions;
- select and use available capabilities;
- evaluate outcomes and adjust behavior.

### Memory — continuity and retained experience

The role responsible for retaining and retrieving information so that the system can make use of prior context and experience.

Ideas discussed:
- retain, associate, organize, maintain and forget information;
- provide relevant information to Model when needed;
- allow Model to query, write, modify and delete information through an agreed interaction approach;
- support different retention durations: transient, short-term, long-term and permanent;
- distinguish retention duration from information type. Long-term information may include episodic, semantic and procedural forms.

These are conceptual dimensions, not a finalized memory schema or policy.

### Immune — protection

The protective role: detect and respond to conditions that threaten the system within a defined protection scope.

The important conceptual point is bounded protective autonomy. Protection should not have to wait for Model to approve every protective action, while Model must not have unrestricted authority to disable protection. Critical protection authority remains subject to the user's explicit control.

The concrete Immune System V1 specification, component design, tests and frozen implementation status are documented separately. They are not defined by this conceptual map.

### Nervous — thought process and information coordination

The conceptual role associated with the system's thinking and analysis process and the flow of information through the cognitive/body system.

Responsibilities discussed include:
- organize and process information relevant to the current situation;
- support analysis, reasoning, planning and decision-making;
- coordinate information flow between cognition and functional capabilities;
- handle relevant events and state feedback;
- help connect the current cognitive state with incoming information and execution results.

The exact division between Model's core cognition and the Nervous role remains an important open design question. This conceptual responsibility must not be interpreted as a finalized internal architecture.

### Sensory — perception and input

The role of receiving information from the user and the external environment and making it available for cognitive processing.

The required modalities and internal organization have not been finalized. Different input sources may eventually be handled by specialized adapters rather than one monolithic component.

### Execution — task realization and action

The role closely connected to Nervous: it turns plans and decisions into actual work and returns the results so the cognitive process can continue.

Responsibilities discussed include:
- create and decompose tasks from an intended action;
- invoke tools and functional capabilities;
- coordinate sub-agent work when needed;
- manage execution progress and relevant task state;
- support synchronous or asynchronous execution where appropriate;
- return results and feedback;
- handle execution failures, recovery decisions and task completion.

The conceptual relationship is: **Nervous thinks, analyzes and plans; Execution carries out the work; the results flow back to Nervous for evaluation.**

Whether task lifecycle management is an internal responsibility of Execution or should eventually be represented as a distinct Task System has not been settled. Do not assume Task System is already a separately approved conceptual subsystem or that Runtime owns task scheduling.

### Metabolism / Energy — resource sustainability

A conceptual role inspired by how living systems manage limited resources. In Echo Core this may relate to awareness and management of constrained resources such as model usage, tokens, compute or other operating budgets.

The analogy is not a commitment to a biological simulation. The concrete resource model and whether this should be a distinct system remain open.

### Homeostasis — overall balance

A conceptual role inspired by the maintenance of stable internal conditions in a changing environment.

For Echo Core, the idea is to maintain awareness of important operating conditions and help preserve an appropriate balance over time. The monitored conditions, authority and relationship with protective behavior have not been specified.

## 3. Conceptual interaction pattern

One broad pattern discussed is:

1. Sensory capabilities bring information into the system.
2. Model interprets the information and may draw on Memory.
3. Model reasons, plans and selects an action.
4. Execution carries out the selected action.
5. The result becomes new information for Model to evaluate.
6. The cycle continues, changes direction or ends.

Nervous participates in thinking, analysis, planning and information coordination across this cycle. Execution is closely coupled to Nervous through task realization and feedback. Immune may act within its protection responsibility when needed. Metabolism / Energy and Homeostasis are potential supporting roles whose actual need and boundaries remain to be established.

This is an explanatory pattern, not a mandatory pipeline. Real interactions may be iterative, partial or event-driven.

## 4. Global design principles

1. **Essence before imitation:** understand why a biological or engineering mechanism exists; borrow its useful principle, not its surface form.
2. **Purpose before module:** define the problem and responsibility before deciding that a separate system or component is needed.
3. **Necessary complexity only:** do not add abstractions, layers or subsystems without a concrete reason.
4. **Cognitive centrality:** Model remains the main reasoning and decision-making center.
5. **Specialization:** systems should have clear, distinct responsibilities rather than duplicating one another.
6. **Different autonomy for different roles:** autonomy is bounded by responsibility; it is not a uniform property every system must have.
7. **Consistent interaction, independent internals:** systems should be understandable and usable through coherent interaction conventions without forcing identical internal implementations.
8. **Clear authority:** critical permissions and user-controlled boundaries must remain explicit.
9. **Continuity:** the overall design should support useful persistence and continuity, without assuming every kind of information has the same lifetime.
10. **Evolution through evidence:** develop the conception incrementally, validate real needs and avoid implementing the entire imagined system prematurely.
11. **Keep levels distinct:** overall vision, conceptual system roles, detailed specifications, implementation and verification are different kinds of knowledge.

## 5. Conceptual development thoughts

A historical development priority was discussed as:

**Immune → Memory → Nervous → Execution**, with Sensory, Metabolism / Energy and Homeostasis considered later as the system matured.

This records a previous sequencing idea only. It is not a binding implementation roadmap or a claim about current engineering priorities. A detailed system may have its own accepted specification and status, documented separately.

## 6. Open conceptual questions

- What should the complete cognitive cycle of Model look like?
- Which conceptual roles genuinely need to be distinct systems?
- What belongs to Model's central cognition versus the Nervous thinking/coordination role?
- What is the exact boundary between Nervous and Execution?
- Should task lifecycle management remain within Execution or become a distinct Task System?
- What forms of autonomy are necessary for each role?
- How should Memory's retention durations and information types relate?
- What should Sensory perceive, and how should different inputs be represented?
- How much task coordination belongs in Execution versus Model?
- What resources should Metabolism / Energy account for?
- What conditions should Homeostasis observe, and what authority should it have?
- How can the system preserve a coherent overall design without copying biological complexity?

Resolve these questions through concrete needs and explicit design decisions. Do not treat an analogy or a name in this map as approval to build a subsystem.
