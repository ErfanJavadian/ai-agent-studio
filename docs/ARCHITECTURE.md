# AI Agent Studio — Architecture

**Status:** Initial target architecture  
**Implementation status:** Proposed; not all components exist.  
**Architecture principle:** Start as a modular application with explicit boundaries; distribute only when operational evidence requires it.

## 1. System overview

AI Agent Studio has two related but distinct responsibilities:

1. **Studio control plane:** Understand user requests, design and change agent definitions, govern capabilities, test changes, and manage versions.
2. **Agent execution plane:** Run approved agent tasks through a bounded runtime and authorized tools.

The conversational interface is the main user-facing entry point. It must not directly bypass validation, permission checks, or the execution boundary.

## 2. High-level flow

`User conversation → Intent & clarification → Change planner → Definition validator → Approval policy → Versioned agent registry → Runtime → Tool gateway → Execution history & evaluation → User report`

This is a logical view, not a claim that every module is implemented.

## 3. Proposed modules

### 3.1 Conversation Interface
- Accept natural-language requests.
- Present clarification questions, previews, approvals, and results.
- Show agent status, versions, and activity in understandable terms.
- Avoid requiring technical configuration.

### 3.2 Intent Understanding
- Convert conversation turns into a typed intent representation.
- Resolve the target agent and requested lifecycle operation.
- Track uncertainty and missing requirements.
- Avoid silently inventing consequential details.

### 3.3 Clarification Manager
- Decide whether more information is required.
- Ask focused questions.
- Preserve conversation context relevant to the current change.
- Produce a resolved request with explicit assumptions.

### 3.4 Change Planner
- Convert resolved intent into a proposed change set.
- Select only capabilities present in the capability registry.
- Estimate resource impact where possible.
- Explain dependencies and unsupported requirements.

### 3.5 Agent Definition Registry
- Store canonical, schema-versioned Agent Definitions.
- Maintain immutable or auditable versions.
- Support retrieval, comparison, and supported rollback.
- Enforce ownership boundaries when multi-user support is introduced.

### 3.6 Capability Registry
- Describe implemented tools, connectors, and internal capabilities.
- Store contracts, permission requirements, risk levels, versions, and availability.
- Distinguish implemented capabilities from planned or unavailable ones.

### 3.7 Validation and Policy Engine
- Validate definition schemas and references.
- Enforce least-privilege authorization.
- Apply risk-based approval rules.
- Enforce budgets, rate limits, and operation boundaries.
- Fail closed when policy or authorization cannot be established.

### 3.8 Human Approval Service
- Present contextual approval requests.
- Bind approval to a specific proposed action and scope.
- Record approver, decision, and time.
- Prevent broad or reusable approvals from being inferred.

### 3.9 Agent Runtime
- Load a validated, approved Agent Definition version.
- Execute bounded tasks.
- Enforce step, time, and tool-call limits.
- Keep model-provider behavior behind a provider abstraction.
- Return structured execution outcomes.

### 3.10 Model Gateway / Provider Adapters
- Offer a consistent interface to supported model providers.
- Handle provider configuration, timeouts, retries, and sanitized errors.
- Track usage and cost where provider data is available.
- Avoid exposing provider secrets to the conversation layer.

### 3.11 Tool Gateway
- Resolve tools through an allowlisted registry.
- Validate inputs and outputs against contracts.
- Check permissions on every invocation.
- Apply timeouts, limits, and audit logging.
- Do not expose arbitrary shell, browser, or network execution by default.

### 3.12 Execution History and Audit
- Persist task status, timestamps, version references, tool events, and safe diagnostic metadata.
- Redact secrets and sensitive payloads.
- Make it possible to explain what the agent did and why.

### 3.13 Evaluation Service
- Run schema, policy, functional, and regression tests.
- Associate results with the exact Agent Definition version.
- Report evidence and limitations.
- Prevent untested changes from being represented as verified.

## 4. Data model concepts

Initial conceptual entities:
- `Workspace`
- `User` (when identity and multi-user support are implemented)
- `Agent`
- `AgentDefinitionVersion`
- `Capability`
- `ConnectorInstallation`
- `PermissionGrant`
- `ApprovalRequest`
- `Execution`
- `ToolInvocation`
- `EvaluationRun`
- `AuditEvent`

These are concepts, not a final database schema. Ownership and tenant isolation must be designed before exposing the system to multiple users.

## 5. Proposed change transaction

For an agent modification:
1. Read the current authorized version.
2. Generate a proposed change set.
3. Validate schema and capability references.
4. Evaluate permissions, risk, and budget.
5. Obtain approval if policy requires it.
6. Persist a new version with provenance.
7. Run targeted and regression tests.
8. Mark the result with an honest status.
9. If validation or tests fail, do not silently promote the change as healthy; preserve evidence and offer recovery.

Where atomicity is possible, definition updates should be transactional. Execution side effects must be treated separately because they may not be reversible.

## 6. Security boundaries

- Treat user input, model output, tool output, and external content as untrusted.
- Never rely on model-generated text alone as authorization.
- Enforce permissions in trusted application code at the tool gateway.
- Keep credentials in a dedicated secret mechanism; do not store them in prompts, definitions, or ordinary logs.
- Validate every tool input and constrain outputs.
- Scope file access to explicitly authorized workspaces.
- Use operating-system or infrastructure sandboxing for untrusted code if such execution is ever introduced.
- Do not describe path checks alone as a hardened sandbox.
- Apply network egress controls if network-capable tools are introduced.
- Add user/workspace isolation before multi-user deployment.
- Sanitize provider and tool errors before persistence or display.
- Maintain audit trails for approvals and consequential actions.

## 7. Deployment and scaling strategy

### Initial
- Modular monolith.
- One application process where practical.
- SQLite for a local/single-user prototype if appropriate.
- Explicit interfaces between modules.
- Deterministic tests and local fixtures.
- No microservices by default.

### Later, when justified
- PostgreSQL for concurrent multi-user workloads.
- Queue/workers for long-running executions.
- Object storage for artifacts.
- Separate services only where isolation, scaling, reliability, or team ownership requires them.
- Centralized observability, secrets management, backups, and operational controls.

## 8. Reuse and migration from the earlier prototype

A previous repository, `ErfanJavadian/Ai_Agent_platform`, may contain reusable implementation work, including a Python package, JSON agent loader, typed domain models, provider protocol, FakeProvider, single-agent runtime, SQLite history, OpenAI-compatible adapter, provider factory, CLI, tool registry/gateway, workspace file tools, bounded tool loop, and failure persistence work.

These are candidate assets, not automatically accepted components. Before copying or adapting any code:
1. Inspect the exact source revision and tests.
2. Verify licensing and dependency compatibility.
3. Run the available tests in a reproducible environment.
4. Review security boundaries and known limitations.
5. Map each component to this architecture.
6. Port incrementally with attribution and documented decisions.

Prior reported test results and static reviews are historical evidence only; they must be independently re-verified in this repository. Do not claim live provider execution, hardened sandboxing, or multi-user isolation without direct verification.

## 9. Architecture decisions to record

Use lightweight Architecture Decision Records (ADRs) for decisions with long-term consequences, such as:
- Agent Definition schema and migration strategy.
- Model provider abstraction.
- Persistence layer.
- Execution isolation.
- Credential storage.
- Approval policy.
- Multi-user ownership and tenant isolation.
- Background execution model.

## 10. Architecture non-goals

- Microservices before there is a demonstrated need.
- Custom model training or hosting as a prerequisite.
- Arbitrary code execution in the initial MVP.
- Direct model-to-tool access that bypasses trusted policy enforcement.
- Treating a successful model response as proof that an operation succeeded.
