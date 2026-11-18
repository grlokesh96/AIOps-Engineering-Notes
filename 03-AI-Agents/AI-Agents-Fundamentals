# AI Agents Fundamentals

## 1. What Is an AI Agent?

An AI agent is a software system that uses a model to interpret a goal, decide what to do next, and interact with tools or systems to accomplish a task. Unlike a basic chatbot that primarily generates text, an agent can use a defined set of tools, observe their results, and continue a workflow.

**Typical loop:** Observe → Reason/Plan → Act → Observe Results → Continue or Stop.

An agent is not automatically autonomous or safe. Its behavior is determined by its instructions, model, tools, permissions, runtime, and controls.

## 2. Agent vs. Chatbot vs. Automation

| System | Main behavior | Example |
|---|---|---|
| Chatbot | Responds to user messages | Explain a Kubernetes error |
| Script/automation | Executes predefined steps | Restart a service when a condition is met |
| AI agent | Selects among allowed actions based on context and observations | Investigate a failed deployment and recommend a next step |

Agents may combine deterministic automation with model-based decision-making. Use ordinary scripts for predictable operations; use an agent when interpretation, context, or selection among actions is useful.

## 3. Core Components

- **Model:** Interprets context, plans, and generates responses or tool requests.
- **Instructions:** Defines the goal, boundaries, and behavior.
- **Tools:** APIs, CLI wrappers, queries, or functions the agent may invoke.
- **State/context:** Relevant inputs and previous observations.
- **Orchestration:** Controls steps, retries, timeouts, and workflow transitions.
- **Guardrails:** Validates requests, results, and actions.
- **Evaluation/observability:** Measures quality, latency, errors, and cost.

## 4. DevOps & AIOps Use Cases

- Kubernetes incident investigation
- CI/CD failure diagnosis
- Cloud service troubleshooting
- Log and trace summarization
- Security finding triage
- Infrastructure drift investigation
- Cost anomaly explanation
- GitOps synchronization analysis
- Drafting incident timelines and postmortems

## 5. Example: Kubernetes Incident Agent

**Goal:** Investigate a pod in `CrashLoopBackOff` and provide evidence-backed diagnosis.

Possible read-only tools:
- `kubectl get pods`
- `kubectl describe pod`
- `kubectl logs --previous`
- `kubectl get events`
- Prometheus query API
- Deployment history or Git commit metadata

Expected output:
1. Incident summary
2. Affected workload and namespace
3. Evidence collected
4. Probable root cause (with confidence and uncertainty)
5. Suggested remediation
6. Risk and rollback considerations

The initial agent should not directly mutate production resources.

## 6. When Should You Use an Agent?

Use an agent when a task:
- Requires interpretation of unstructured data
- Needs several tool calls chosen dynamically
- Benefits from contextual correlation
- Has a clear goal and verifiable completion criteria

Prefer a simple script or workflow when:
- The steps are fixed
- Decisions are deterministic
- The operation is safety-critical and can be expressed as rules

## 7. Agent Lifecycle

1. Receive task and validate input.
2. Load only relevant context.
3. Determine next step.
4. Request an allowed tool.
5. Validate tool arguments and permissions.
6. Execute tool and capture output.
7. Evaluate observation.
8. Continue, ask for clarification, or stop.
9. Produce result and evidence.

## 8. Hands-on Lab: Incident Investigation Prompt

Try an agent with read-only Kubernetes tools and a test namespace.

**Task prompt:**

> Investigate the reported workload failure. Collect pod status, recent events, current and previous container logs, and related deployment information. Do not modify resources. Clearly separate observed evidence from hypotheses. Return a concise incident report with recommended next checks.

## 9. Best Practices

- Keep the agent's goal narrow.
- Use least privilege and read-only tools first.
- Limit tool calls, retries, time, and tokens.
- Make outputs structured and evidence-backed.
- Treat logs, tickets, and retrieved documents as untrusted input.
- Require human approval for high-impact actions.
- Test against known incidents before deployment.

## 10. Further Reading

- OpenAI Agents documentation: https://platform.openai.com/docs
- Google Agent Development Kit: https://google.github.io/adk-docs/
- LangGraph: https://docs.langchain.com/oss/python/langgraph/overview
