# Prompt Engineering

## 1. What is prompt engineering?

Prompt engineering is the practice of designing and refining instructions, context, examples, and output constraints to help a model perform a task reliably.

A prompt is not a security boundary. Do not rely on prompt wording alone to enforce permissions or protect production systems.

## 2. A practical prompt structure

Use this structure when appropriate:

1. **Role / purpose:** What kind of assistant is this?
2. **Task:** What must it do?
3. **Context:** Relevant environment and information.
4. **Constraints:** What must it not do?
5. **Evidence requirements:** What sources must support its conclusions?
6. **Output format:** Markdown, JSON, table, or schema.
7. **Completion criteria:** What constitutes a useful answer?

## 3. Prompt techniques

### Clear instructions
State the task directly. Avoid vague requests such as “look into this.”

### Context grounding
Supply relevant logs, metrics, configuration, runbooks, and timestamps. Remove secrets.

### Examples (few-shot)
Provide a few examples of desired input and output when format or classification consistency matters.

### Decomposition
Break complex tasks into steps: collect evidence, analyze, verify, summarize.

### Structured output
Request a JSON schema or fixed fields and validate them programmatically.

### Explicit uncertainty
Require the model to distinguish observed facts, hypotheses, confidence, and unknowns.

## 4. Example: Kubernetes incident investigation prompt

```text
Role:
You are a Kubernetes SRE investigation assistant.

Task:
Investigate the supplied incident using only the evidence provided.

Rules:
- Do not invent command output, metrics, events, or configuration.
- Separate confirmed facts from hypotheses.
- Do not recommend destructive actions.
- Prefer read-only checks.
- If evidence is insufficient, state what additional data is needed.

Output:
1. Incident summary
2. Confirmed evidence
3. Likely causes ranked with reasoning
4. Additional read-only checks
5. Safe remediation options
6. Verification plan
```

## 5. Example: CI failure analysis prompt

```text
Analyze this CI/CD failure log.

Identify:
- Failed stage
- Exact error evidence
- Likely root cause
- Whether the failure is code, dependency, infrastructure, credential,
  configuration, or runner-related
- Next diagnostic steps
- A proposed fix, if supported

Do not invent missing details. Quote only short relevant excerpts.
Return valid JSON matching the schema provided by the application.
```

## 6. Prompt patterns for operations

| Pattern | When to use |
|---|---|
| Summarization | Condense logs, alerts, or postmortems |
| Classification | Route alerts by category/severity |
| Extraction | Parse incident fields into structured data |
| Analysis | Compare evidence and hypotheses |
| Planning | Create a bounded investigation plan |
| Critique | Review a proposed runbook or change |
| Transformation | Convert data into a report or ticket |

## 7. Prompt injection and untrusted content

Operational data may contain malicious or misleading instructions. Logs, tickets, repository files, webpages, and user-provided content must be treated as **data**, not instructions to the agent.

Mitigations:
- Clearly delimit untrusted content.
- Never let retrieved text override system policies.
- Restrict tools independently of model instructions.
- Validate all tool arguments.
- Use allowlists and least-privilege authorization.
- Require approval for high-impact operations.

## 8. Prompt iteration and evaluation

Do not judge a prompt by one successful response.

Create a test set containing:
- Common incidents.
- Edge cases.
- Missing evidence.
- Conflicting evidence.
- Misleading logs.
- Prompt injection attempts.

Measure:
- Correctness.
- Evidence citation accuracy.
- Unsupported claims.
- Format/schema validity.
- Tool selection quality.
- Cost and latency.
- Unsafe action proposals.

## 9. Reusable template

```text
ROLE:
[Define the assistant's scope and expertise.]

TASK:
[One specific task.]

CONTEXT:
[Relevant data and environment.]

RULES:
[Constraints, safety requirements, evidence requirements.]

OUTPUT:
[Required structure/schema.]

SUCCESS CRITERIA:
[How the result will be evaluated.]
```

## 10. Key takeaways

Good prompts improve task clarity and consistency, but reliable systems also need retrieval, tools, schema validation, evaluation, permissions, and monitoring. Prompt engineering is one component of application engineering—not a replacement for it.
