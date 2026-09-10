# Security and threat model

## Protected assets

- Customer and transaction identifiers.
- Payment-card data and authentication tokens.
- Personally identifiable information.
- Internal service topology and operational metadata.
- Credentials, secrets, certificates, and API keys.
- Incident reports and audit records.

## Primary threats

### Excessive access

A user or model could request logs outside the approved environment, namespace, workload, or time range.

Controls: server-side allowlists, RBAC, authorization checks on every tool call, bounded queries, and audit records.

### Sensitive-data disclosure

Logs may contain PANs, tokens, emails, phone numbers, credentials, or payloads that must not reach a model or user.

Controls: structured logging standards, layered deterministic redaction, deny patterns, sampling tests, and model invocation only after redaction.

### Prompt injection in logs

An attacker may place instructions in request fields or log messages in an attempt to influence the model.

Controls: treat all retrieved content as untrusted evidence, isolate it from system instructions, prohibit evidence from changing tool policy, validate tool arguments independently, and test adversarial cases.

### Unsupported conclusions

The model may invent a cause or remediation that is not supported by evidence.

Controls: evidence IDs, schema validation, claim-to-citation verification, explicit uncertainty outcomes, and human approval before operational action.

### Tool misuse

A generic shell or Kubernetes tool would allow arbitrary commands or resource access.

Controls: expose purpose-built read operations only; prohibit shell execution, secret retrieval, write verbs, and unbounded selectors.

### Denial of service and cost abuse

Large time windows or repeated investigations could overload the cluster, telemetry backend, or model budget.

Controls: quotas, pagination, concurrency limits, caching, maximum evidence size, model budget limits, and rate limiting.

## Initial trust boundaries

1. Engineer to Investigation API: authenticated, authorized, and audited.
2. Investigation API to MCP server: service identity and policy-bound tool calls.
3. MCP server to operational sources: read-only credentials and source-specific permissions.
4. Evidence processor to model: redacted and minimized evidence only.
5. Report to engineer: filtered according to the engineer's original authorization.

## Production-readiness gate

No real environment should be connected until the project has automated RBAC tests, redaction tests, audit logging, query limits, documented retention, model-provider data controls, and an approved security review.
