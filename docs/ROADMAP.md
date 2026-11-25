# AI Agent Studio — Roadmap

**Status:** Directional roadmap  
**Rule:** A phase is complete only when its acceptance criteria are demonstrated. A planned item is not an implemented feature.

## North-star sequence

**Prove the conversation → Prove safe agent creation → Prove reliable execution → Prove agent evolution → Prove repeatable user value → Scale the platform.**

The roadmap is intentionally staged. Dates and scale targets should be added only when capacity, cost, and delivery evidence are known.

## Phase 0 — Product and repository foundation

### Goals
- Establish product vision, specification, architecture, and project state.
- Define scope and non-goals.
- Establish documentation and engineering conventions.
- Inspect candidate code from the earlier repository without copying blindly.

### Deliverables
- Product foundation documents.
- Initial repository structure proposal.
- Decision log / ADR template.
- Baseline risk register.
- A verified inventory of reusable prior code, if any.

### Exit criteria
- Product direction is internally consistent.
- MVP use case and acceptance tests are selected.
- Existing code reuse decisions are evidence-based.
- No undocumented assumption is treated as a delivered feature.

## Phase 1 — Conversational creation prototype

### Goals
Prove that a non-technical user can describe an agent and reach a clear, valid proposed definition.

### Deliverables
- Natural-language intake.
- Intent extraction.
- Clarification flow.
- Plain-language plan and preview.
- Minimal versioned Agent Definition schema.
- Deterministic validation.

### Exit criteria
- Test users can complete the creation flow without writing code or JSON.
- Ambiguous consequential requirements trigger clarification.
- Unsupported capabilities are identified honestly.
- Definitions validate and can be inspected by developers.

**Boundary:** This phase may be a non-executing prototype. Do not imply that proposed agents can perform real work.

## Phase 2 — Safe single-agent runtime integration

### Goals
Connect approved definitions to a bounded execution runtime.

### Deliverables
- Runtime adapter for validated Agent Definitions.
- Small allowlisted capability set.
- Tool gateway authorization and input validation.
- Execution records and safe error handling.
- Explicit budgets, timeouts, and tool-call limits.
- Synthetic test fixtures.

### Exit criteria
- A created agent can complete a narrowly scoped test task.
- Unauthorized tool calls are rejected.
- Execution outcomes are persisted and accurately reported.
- Provider failures do not leak secrets.
- No unrestricted shell or network access is exposed.

## Phase 3 — Agent evolution and version control

### Goals
Allow users to modify existing agents through conversation and safely manage changes.

### Deliverables
- Conversational change requests.
- Change planner and validation.
- Contextual approvals for sensitive changes.
- Version history and comparison.
- Targeted and regression tests.
- Supported rollback.

### Exit criteria
- A user can request a change without editing technical configuration.
- The platform previews and validates the change.
- Applied changes create traceable versions.
- Failed tests are visible and do not become false success claims.
- Rollback restores a prior supported definition.

## Phase 4 — First real-world workflow: Revenue Recovery

### Goals
Use the AI Revenue Recovery Agent as a proving ground for platform capabilities.

### Candidate workflow
- Read explicitly authorized invoice data.
- Identify overdue accounts using transparent rules.
- Prioritize follow-up opportunities.
- Draft communications for human review.
- Produce a recovery activity report.

### Safety boundaries
- Begin with synthetic or manually supplied test data.
- Do not send external messages automatically in the early release.
- No payment collection, refunds, or financial commitments.
- Require human approval for external communications and material record changes.

### Exit criteria
- Workflow quality is measured against a defined test set.
- Data access is scoped and auditable.
- Drafts and recommendations are reviewable.
- Any external side effect is gated by explicit approval.
- Value and operating costs are measured.

## Phase 5 — Usability, integrations, and operational readiness

### Goals
Make the platform dependable for a small group of real users.

### Deliverables
- Secure connector onboarding for selected integrations.
- Clear permission and revocation UX.
- Better monitoring and evaluation.
- Backup, recovery, and data-retention controls.
- Usage and cost visibility.
- Improved accessibility and onboarding.

### Exit criteria
- Users understand what each integration can access.
- Permissions can be reviewed and revoked.
- Failures are diagnosable without exposing secrets.
- Reliability and cost are measured under realistic usage.

## Phase 6 — Multi-user platform foundations

### Goals
Introduce collaboration and isolation only after the single-user lifecycle is reliable.

### Deliverables
- Authentication and workspace ownership.
- Tenant isolation.
- Role and permission model.
- Workspace-level audit history.
- Quotas and abuse controls.
- Production database and background execution where needed.

### Exit criteria
- Cross-workspace access tests pass.
- Sensitive operations are attributable to an authorized actor.
- Backups and recovery are tested.
- Operational limits and costs are understood.

## Phase 7 — Capability ecosystem

### Goals
Expand the number of useful agents and integrations without weakening governance.

### Possible deliverables
- Reusable capability packages.
- Connector catalogue.
- Agent templates.
- Evaluation suites and compatibility metadata.
- Carefully governed sharing or distribution.

### Exit criteria
- Capabilities are versioned, documented, permission-scoped, and tested.
- Installation and revocation are understandable.
- Untrusted extensions cannot bypass policy enforcement.

## Phase 8 — Scale and platform maturity

### Goals
Grow toward a robust general-purpose agent platform.

### Possible investments
- Distributed execution where justified.
- Advanced observability and reliability engineering.
- Enterprise administration and compliance capabilities where required.
- Multi-provider routing and cost optimization.
- More sophisticated evaluation and governance.
- Ecosystem and partner strategy.

### Exit criteria
Set measurable reliability, security, customer-value, and unit-economics targets before committing to scale investments.

## Current status

At the creation of this roadmap:
- The repository is newly created and contains only a minimal README.
- The documents in this foundation branch are planning artifacts.
- No product runtime, UI, agent builder, connector, or production service is claimed to be implemented in this repository.
- Previous prototype code exists in a separate repository and has not yet been audited or migrated here.

## Prioritization rules

1. Prefer the smallest end-to-end user outcome over broad feature lists.
2. Do not build a feature before defining how it will be tested.
3. Security and permission enforcement are core product requirements, not later polish.
4. Reuse working code only after verification.
5. Keep infrastructure and model costs within a realistic budget.
6. Do not add multi-agent orchestration, marketplace, or microservices until user evidence supports them.
7. Update this roadmap when evidence changes priorities; preserve completed history.

## Immediate next engineering steps

1. Review these foundation documents together and resolve contradictions.
2. Select the exact MVP user and workflow.
3. Inspect the earlier repository at a known commit and produce a reuse/security audit.
4. Define the smallest Agent Definition schema.
5. Write acceptance tests before implementing the conversation-to-definition path.
