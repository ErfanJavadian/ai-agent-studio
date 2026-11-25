# AI Agent Studio — Project State

**Last updated:** 2026-11-25  
**Repository:** `ErfanJavadian/ai-agent-studio`  
**Branch for foundation documents:** `foundation/product-foundation`

## 1. Current verified state

- A new public GitHub repository has been created.
- Default branch is `main`.
- The repository initially contained a minimal `README.md`.
- GitHub integration reports read/write access for this repository.
- Product foundation documents are being added on a separate branch.
- No application code, runtime, user interface, database schema, or production deployment has been verified in this repository.

## 2. Product direction

AI Agent Studio is intended to be a general-purpose, conversation-first platform for creating and evolving AI agents.

The user describes desired outcomes in natural language. The platform should handle intent understanding, clarification, internal configuration, validation, approvals, testing, versioning, and reporting.

Users should not need coding, JSON, API, architecture, or prompt-engineering knowledge for the standard experience.

**Product promise:** *Just describe. Your Agent evolves.*

The AI Revenue Recovery Agent is the initial proving-ground agent, not the limit of the platform's long-term scope.

## 3. Resource constraints and planning assumptions

Current planning constraints supplied by the project owner:
- Available capital: zero.
- Current hardware: phone.
- Current project income: none.
- Development experience: limited.

These constraints should shape the initial plan:
- Prioritize product design, documentation, and small verifiable steps.
- Prefer free/open-source tooling where suitable.
- Do not assume free hosting, model inference, or integrations will remain free.
- Avoid paid infrastructure until a justified budget exists.
- Plan for access to a computer for serious software development and testing; do not assume it is currently available.

## 4. Prior repository

A separate repository, `ErfanJavadian/Ai_Agent_platform`, contains candidate prior work. Reported components include a Python package, JSON agent loader, typed domain models, provider protocol, FakeProvider, single-agent runtime, SQLite history, OpenAI-compatible adapter, provider factory, CLI, tool registry/gateway, workspace file tools, bounded tool loop, and failure persistence.

This inventory is based on prior project review and is not a current verification of the code in this repository. No code has yet been copied into this repository as part of the foundation setup.

Before reuse, inspect a specific source commit, run tests, review dependencies and license, examine security boundaries, and document accepted changes.

## 5. Known risks and constraints

- **Scope risk:** A general platform is too broad for a first release. Prove one complete workflow first.
- **Security risk:** Model-generated plans must never be treated as authorization.
- **Isolation risk:** Multi-user ownership and tenant isolation must be designed before public multi-user operation.
- **Execution risk:** Path validation alone is not a hardened OS sandbox.
- **Provider risk:** Live model connectivity, reliability, and cost must be tested in the target environment.
- **Integration risk:** A planned connector is not an implemented connector.
- **Cost risk:** Model inference, hosting, storage, and external APIs can create recurring costs.
- **Verification risk:** Documentation and successful mock tests do not prove real-world execution.
- **Age and access constraints:** Development planning should be practical and should account for the project owner's available equipment, time, and need for appropriate adult support for accounts or paid services.

## 6. Status vocabulary

Use these labels in future project updates:
- **VERIFIED:** Directly inspected or tested in the current repository/environment.
- **PARTIAL:** Some implementation or evidence exists, but acceptance criteria are incomplete.
- **PLANNED:** Documented intention only.
- **BLOCKED:** Cannot proceed until a dependency or decision is resolved.
- **UNKNOWN:** Not yet inspected or tested.

Never label a planned feature as implemented.

## 7. Immediate next actions

1. Review the foundation documents and resolve product/architecture questions.
2. Choose one narrow MVP user journey and define its acceptance tests.
3. Audit the prior repository at a fixed commit before deciding what to reuse.
4. Define the minimal Agent Definition schema.
5. Establish a small test-first implementation plan.

## 8. Change log

- **2026-11-25:** Initial project state recorded for the new AI Agent Studio repository. Product direction and foundation documentation established as planning artifacts; no application implementation claimed.
