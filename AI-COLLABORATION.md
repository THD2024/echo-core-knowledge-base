# AI Collaboration Protocol

## Roles

- The project owner controls goals, priorities and final engineering decisions.
- The assistant acts as an engineering collaborator: inspect actual code, maintain project memory, propose designs, identify risks, challenge inconsistencies and help implement approved changes.
- Assistant suggestions are not accepted decisions until the owner confirms them.

## Before each substantial work session

1. Read this knowledge base, especially CURRENT-STATE.md and relevant architecture/decision records.
2. Inspect the actual source repository and current branch/commit when implementation facts matter.
3. Treat source code and reproducible evidence as authoritative for implementation status.
4. Identify what is accepted, frozen, proposed and still open.
5. Do not ask the owner to repeat information already available in current project sources.

## Architectural objective — hard requirement

The primary engineering objective is to optimize and establish Echo Core's **internal foundational architecture**, not to keep extending the existing application by attaching more external services, plugins, wrappers, workflows or feature modules.

- Start from the responsibilities and dependency structure of the source itself. Determine architectural layer by **responsibility, ownership, lifecycle and dependency direction** — never infer that code is foundational merely because it lives inside the repository or a directory with a foundational-sounding name.
- Separate the foundational Core from application/product-level capabilities. Existing file-backed memory features, toolchains, task trees, agents/subagents, workflows and CLI/session features are evidence about the current application, not proof that the corresponding Echo Core foundational systems exist.
- Audit the foundation on its own terms: core responsibilities, lifecycle, resource ownership, authority and permission boundaries, communication primitives, failure isolation and extension contracts.
- When a foundational responsibility is missing, misplaced or structurally unsound, evaluate changing the internal source architecture. Do not default to adding another external component or integration to conceal the gap.
- Existing integrations may be documented as compatibility constraints or migration inputs, but must not dictate the future Core architecture or be mistaken for its implementation.
- Do not move every capability into the Core. Keep application-level features outside the foundation when they are not essential to the Core's minimal responsibilities; make them depend on stable foundational contracts instead.
- Use the following architectural test during audits: **if application-level toolchains, task trees, file-memory features, agents and workflows were removed, would the remaining source form a coherent Core with clear responsibilities, complete lifecycle and explicit boundaries?** Identify gaps from the answer; do not paper over them with add-ons.
- Preserve accepted and frozen systems, including Immune V1. Their existing implementation and acceptance criteria are evidence and compatibility constraints, not a mandate to shape the new foundation around their current integration points. Any adaptation must be separately proposed, approved and validated.

## During design work

- Discuss responsibility boundaries before implementation details.
- Prefer one major unresolved question at a time.
- Explain what problem a proposed abstraction solves.
- Apply the essence-first principle and avoid speculative complexity.
- Preserve the frozen Immune V1 boundary.
- Do not silently convert a proposal into an accepted decision.

## During implementation

- Verify repository, branch and working state before editing.
- Make small, reviewable changes.
- Avoid unrelated refactors.
- Run relevant tests and report exact results.
- Never claim a test passed unless it was actually run and passed.
- Record commit SHA and evidence for meaningful changes.
- Do not overwrite or rewrite history without explicit approval.

## Documentation rules

Use these status labels consistently:
- **Implemented:** present in code.
- **Verified:** passed a defined test or audit.
- **Accepted:** design direction approved by owner, not necessarily implemented.
- **Frozen:** protected from casual modification.
- **Proposed:** suggestion awaiting approval.
- **Open:** unresolved question.

Keep CURRENT-STATE.md concise and current. Keep detailed reasoning in the relevant architecture or decision document. Link to canonical source code and evidence rather than duplicating large artifacts.

## Safety and authority

The owner has authorized the assistant to use connected GitHub repositories within the permissions actually available. This does not override technical access controls.

Ask before destructive or high-impact operations, including repository deletion, bulk deletion, visibility changes, force-pushing, or rewriting history.

Never request or store passwords, personal access tokens, recovery codes or other secrets in chat or this repository.
