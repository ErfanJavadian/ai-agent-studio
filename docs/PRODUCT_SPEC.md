# AI Agent Studio — Product Specification

**Status:** Initial product specification  
**Scope:** Product behavior and MVP requirements  
**Implementation status:** Not yet verified; this document describes intended behavior.

## 1. Core experience

The primary interface is a conversation with the platform. Users express goals and changes in everyday language. The platform translates these requests into structured internal operations and reports their real status.

The conversation is the control surface for the agent lifecycle, not a substitute for execution.

### User should not need to provide
- Source code or scripts.
- JSON, YAML, or schema definitions.
- API calls or raw HTTP details.
- System prompts or prompt templates.
- Agent architecture diagrams.
- Tool implementation details.

The platform may expose optional advanced controls for expert users in the future, but they must not be required for the standard journey.

## 2. Core user journeys

### Journey A — Create an agent

1. User describes the outcome and intended users.
2. Platform extracts the goal, actors, inputs, outputs, constraints, and success criteria.
3. Platform identifies missing information and asks focused questions.
4. Platform summarizes its understanding in plain language.
5. Platform creates a proposed Agent Definition.
6. Platform checks whether requested capabilities exist and whether permissions are sufficient.
7. Platform previews the plan, including data access, external effects, and expected costs where estimable.
8. User confirms creation and any required permissions.
9. Platform creates a version and runs safe initial tests.
10. Platform reports the result and clearly labels anything not implemented.

### Journey B — Add or change a capability

Example: “Add a weekly report.”

1. Identify the target agent and intended change.
2. Clarify report recipients, schedule, data sources, and delivery channel as needed.
3. Check the capability registry and connector availability.
4. Prepare a change proposal without silently granting broader access.
5. Validate permissions, configuration, and budget.
6. Preview the change and request approval when needed.
7. Apply as a new version.
8. Run targeted and regression tests.
9. Report pass/fail results and the new version identifier.

### Journey C — Diagnose and improve

1. User describes a symptom or asks the platform to investigate.
2. Platform inspects permitted execution history, configuration, and evaluation results.
3. It identifies evidence, uncertainty, and likely causes.
4. It proposes a bounded fix.
5. It validates and tests the fix in a safe environment.
6. It asks approval before applying changes with material impact.
7. It reports the diagnosis, evidence, applied change, and remaining risks.

### Journey D — Pause, inspect, or roll back

Users can ask to pause an agent, explain its permissions, show recent activity, or restore a prior supported version. High-impact operations require clear confirmation. Rollback must state which state is restored and whether external side effects can be reversed; external side effects generally cannot be undone merely by restoring configuration.

## 3. Conversation and intent handling

The platform should extract, at minimum:
- User's desired outcome.
- Target agent or whether a new agent is requested.
- Trigger or schedule.
- Inputs and data sources.
- Outputs and recipients.
- Allowed and forbidden actions.
- Required integrations.
- Human approval requirements.
- Budget or usage constraints.
- Success criteria.
- Ambiguities and confidence.

### Clarification policy

Ask a question when an unresolved detail could materially change behavior, permissions, cost, or external impact. Prefer one clear question at a time when possible.

Do not ask users to choose technical implementation details if the platform can make a safe, reversible default choice. State the default in plain language.

If intent remains ambiguous, do not apply a consequential change. Offer a safe interpretation and request confirmation.

### Intent classes

Initial intent categories:
- Create agent.
- Modify agent.
- Add/remove capability.
- Connect/disconnect integration.
- Test/evaluate.
- Diagnose/improve.
- Explain agent/configuration/activity.
- Pause/resume.
- Version/rollback.
- Delete/archive.

Unknown requests should be handled explicitly rather than mapped to an unsafe action.

## 4. Agent Definition

The platform needs a canonical, structured, versioned internal representation of an agent. This representation is not a user-facing coding requirement.

An Agent Definition should eventually represent:
- Stable agent identity and metadata.
- Purpose and operating boundaries.
- Instructions/behavior policy generated and maintained by the platform.
- Input/output contracts.
- Trigger and schedule configuration.
- Model/provider preferences and fallback policy.
- Enabled capabilities and tool references.
- Data sources and connector references.
- Permission grants and denied actions.
- Approval rules.
- Budget and rate limits.
- Memory/knowledge configuration.
- Evaluation and acceptance criteria.
- Version metadata and provenance.

The exact schema is an engineering decision and must be validated against actual runtime needs. Do not freeze a large schema prematurely. Use explicit schema versions and migration rules.

## 5. Change lifecycle

For consequential configuration changes, use this conceptual lifecycle:

**Understand → Plan → Validate → Preview → Approve → Apply → Test → Report**

- **Understand:** Resolve user intent and target.
- **Plan:** Produce a bounded change set.
- **Validate:** Check schema, available capabilities, permissions, dependencies, and budgets.
- **Preview:** Explain the proposed change in ordinary language.
- **Approve:** Obtain explicit user approval where policy requires it.
- **Apply:** Persist a new immutable or auditable version.
- **Test:** Run targeted and regression evaluations.
- **Report:** Distinguish applied changes from tested behavior and unresolved issues.

A plan is not an applied change. A configured tool is not proof that it works. A passing test is not a guarantee of future performance.

## 6. Capability and integration discovery

The platform should maintain a capability registry describing what it can actually do.

Each capability entry should eventually include:
- Stable identifier and human-readable description.
- Implementation and version.
- Input/output contract.
- Required permissions and scopes.
- Risk classification.
- Network/data boundaries.
- Availability and health status.
- Cost characteristics or limits where known.
- Test coverage and known limitations.

The conversation layer must query this registry before promising a capability. If a requested capability is missing, explain that it is not currently available. Do not represent a planned integration as connected.

Connector onboarding should be understandable to non-technical users: explain what account is being connected, what data/actions will be accessible, and how to revoke access. Never ask users to paste secrets into ordinary chat. Use a secure credential flow when implemented.

## 7. Permissions and safety

Use least privilege. Permissions should be scoped by agent, capability, data resource, and action where practical.

Suggested risk classes:
- **Read-only:** inspect explicitly authorized information.
- **Reversible internal change:** update agent configuration or draft an artifact.
- **External communication:** send messages or contact people.
- **Financial or legally meaningful action:** payment, refund, purchase, contract, or commitment.
- **Destructive or high-impact action:** delete data, publish publicly, change access, or perform irreversible operations.

Higher-risk actions require explicit, contextual approval. Approval must identify the action, target, relevant data, and expected consequence. Approval for one action must not become blanket permission for future unrelated actions.

Enforce budgets, rate limits, timeouts, retry limits, and maximum tool-call counts. Fail closed when authorization cannot be established.

## 8. Testing and evaluation

Testing should cover:
- Agent Definition schema and migration validity.
- Tool contract and permission enforcement.
- Expected behavior for representative tasks.
- Negative cases and prohibited actions.
- Ambiguous requests and clarification behavior.
- Budget and rate-limit enforcement.
- Regression against prior accepted behavior.
- Failure, timeout, retry, and recovery behavior.

Use synthetic fixtures for early development. Never run tests against real customer data or send real external communications without explicit authorization.

Evaluation results should include test identity, version tested, timestamp, pass/fail, evidence, and limitations.

## 9. Versioning, history, and rollback

Every applied Agent Definition change should be attributable to:
- The user request or system event that initiated it.
- The generated plan.
- The approving actor, if approval was required.
- The resulting version.
- Validation and evaluation results.

Support comparison between versions and rollback to a prior supported definition. Preserve audit history. Do not imply that rollback reverses external actions already taken.

## 10. Failure handling and human handoff

When the platform cannot safely complete a request:
- Stop before unauthorized or uncertain consequential actions.
- Explain what failed in plain language.
- Identify whether anything was changed.
- Preserve useful diagnostic evidence without exposing secrets.
- Offer a safe next step or human handoff.
- Avoid claiming success on partial execution.

Errors shown to users should be actionable and should not expose credentials, internal stack traces, or sensitive provider payloads.

## 11. MVP scope

The first MVP should prove one complete, narrow conversational lifecycle:
1. Create a single agent from natural-language description.
2. Ask clarifying questions.
3. Generate and validate a minimal Agent Definition.
4. Configure only a small, explicitly allowlisted set of safe capabilities.
5. Preview and apply a versioned change.
6. Execute a bounded task through the existing runtime where verified.
7. Persist execution history.
8. Run basic deterministic tests.
9. Report what is implemented, what passed, and what is unavailable.

A text-only prototype may be used to validate conversation design, but it must be labeled as a prototype and must not imply that it can execute changes.

## 12. Explicit MVP exclusions

- General-purpose autonomous access to shell, browser, or arbitrary network.
- Unrestricted payment, messaging, publishing, or destructive actions.
- Public agent marketplace.
- Multi-agent organization orchestration.
- User-managed prompt engineering or raw JSON editing as a requirement.
- Custom model training or model hosting.
- Complex enterprise billing and multi-region deployment before need is demonstrated.

## 13. Acceptance criteria

The MVP is acceptable only when:
- A non-technical tester can create the target agent using plain language.
- The platform asks for missing consequential details.
- The generated definition passes schema validation.
- Unsupported capabilities are clearly identified as unavailable.
- Permission checks reject unauthorized tool use.
- The user can preview a proposed change before it is applied.
- Applied changes create traceable versions.
- Tests run against the resulting version and report actual results.
- Failures do not produce false success messages.
- Sensitive values are not exposed in logs or user-facing errors.
- A prior supported definition can be restored without claiming to reverse external side effects.

## 14. Open product questions

- Which first user segment should the MVP optimize for?
- Which exact task should define the first end-to-end success test?
- Which model providers are affordable and accessible in the initial deployment environment?
- What is the minimum secure credential-connection experience?
- Which capabilities can be implemented and tested without paid external services?
- What retention and deletion controls are required for early users?
