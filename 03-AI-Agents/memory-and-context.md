# Agent Memory and Context

## 1. Context vs. Memory

**Context** is the information available to the model during a particular task or invocation.

**Memory** is information retained and made available across steps, sessions, or tasks.

Memory is not always needed. A stateless agent can be simpler, safer, easier to test, and less expensive.

## 2. Context Types

- **System instructions:** Role, behavior, boundaries, and output requirements.
- **Task input:** Current user request or alert.
- **Conversation history:** Relevant previous messages.
- **Tool observations:** Results from queries and actions.
- **Retrieved knowledge:** Runbooks, docs, incident records.
- **Short-term state:** Information needed within one workflow.
- **Long-term memory:** Validated information retained for future tasks.

## 3. Memory Types

### Working memory
Current investigation state, temporary findings, and task progress.

### Episodic memory
Past events, such as previous incidents, timelines, and resolutions.

### Semantic memory
Reusable knowledge such as service ownership, system architecture, or runbook facts.

### Procedural memory
Approved procedures, workflows, and operational playbooks.

## 4. DevOps Context Example

For an incident agent, useful context may include:
- Service catalog and ownership
- Architecture dependencies
- Approved runbooks
- Recent deployments
- Incident history
- Known alerts and dashboards

Avoid storing credentials, unnecessary personal data, or unverified model-generated conclusions as permanent memory.

## 5. Context Assembly

A practical pipeline:

1. Identify the task.
2. Retrieve only relevant documents and records.
3. Apply access controls.
4. Rank and limit retrieved information.
5. Redact secrets and sensitive data.
6. Include source metadata and timestamps.
7. Distinguish facts from hypotheses.
8. Check context size.
9. Pass the assembled context to the model.

## 6. Memory Write Policy

Before storing new memory, consider:
- Is it useful for future tasks?
- Is it verified?
- Is it permitted to retain?
- Is it sensitive?
- Does it have a retention or expiration period?
- Can it be corrected or deleted?
- Who can access it?

Prefer human-approved or source-verified operational facts. Do not let an agent silently promote a hallucinated RCA into trusted memory.

## 7. Context Window Management

- Summarize long histories.
- Preserve key identifiers and evidence references.
- Drop irrelevant or stale details.
- Use retrieval rather than loading entire repositories.
- Track timestamps and source provenance.
- Do not truncate critical safety instructions.

## 8. Hands-on Lab

Create a basic incident knowledge store with:
- Runbooks
- Service metadata
- A small set of historical incident summaries

Ask an agent to investigate a simulated alert. Compare responses:
- Without retrieval
- With relevant runbook retrieval
- With runbook plus historical incident context

Evaluate factual accuracy, evidence quality, context size, and response latency.

## 9. Risks

- Stale memory
- Poisoned or malicious retrieved content
- Cross-tenant data leakage
- Sensitive-data retention
- Incorrect incident history
- Context overload
- Conflicting documents

## 10. Best Practices

- Keep memory scoped to user, project, or tenant.
- Use access control at retrieval time.
- Track source and freshness.
- Apply retention policies.
- Separate raw records from summaries.
- Treat retrieved content as data, not instructions.
- Make memory inspectable, correctable, and deletable.
