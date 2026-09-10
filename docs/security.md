# Security and threat model

## Security posture

PayLens is read-only by default. The model never receives unrestricted shell, SQL, Kubernetes, provider, or payment-system access. Every source is exposed through a purpose-built, policy-enforced tool.

## Protected assets

- Transaction and customer identifiers.
- PANs, CVVs, payment tokens, and authentication material.
- Personally identifiable information.
- Provider requests, responses, and credentials.
- Internal service topology and operational metadata.
- Secrets, certificates, API keys, and access tokens.
- Settlement and reconciliation records.
- Evidence bundles, investigation reports, and audit records.

## Trust boundaries

1. **User to Investigation API:** authenticated, authorized, and audited.
2. **Investigation API to orchestrator:** original user scope remains attached to the investigation.
3. **Orchestrator to MCP tools:** each call is independently authorized and bounded.
4. **MCP tools to sources:** source-specific, read-only credentials and allowlists.
5. **Evidence processor to model:** only redacted, minimized evidence crosses the boundary.
6. **Report to user:** output is filtered against the original authorization and validated citations.

## Primary threats and controls

### Excessive access

A user, prompt, or model could request data outside the approved environment, namespace, service, merchant, provider, or time range.

Controls: server-side authorization, source and workload allowlists, bounded time windows, result limits, deny-by-default policy, and immutable audit records.

### Sensitive-data disclosure

Payment records, logs, traces, or provider payloads may contain regulated or confidential values.

Controls: structured logging standards, deterministic layered redaction, deny patterns, evidence minimization, automated leakage tests, and model invocation only after redaction.

### Prompt injection in evidence

Logs, provider payloads, runbooks, and external text are untrusted data and may contain instructions intended to manipulate the model.

Controls: isolate evidence from system policy, prohibit evidence from changing permissions, validate every tool argument outside the model, restrict tool schemas, and test adversarial cases.

### Unsupported conclusions

The model could invent a cause, financial outcome, blast radius, or remediation.

Controls: stable evidence IDs, structured outputs, claim-to-citation validation, deterministic state comparison, explicit uncertainty outcomes, and human approval before any operational action.

### Tool misuse

Generic access could expose secrets or mutate production systems.

Controls: no generic shell, SQL, `kubectl`, or provider client; prohibit secret retrieval and write verbs; expose only purpose-built read operations.

### Denial of service and cost abuse

Large windows, broad correlation searches, or repeated investigations could overload source systems or model budgets.

Controls: quotas, pagination, caching, concurrency and row limits, evidence-size limits, model budgets, rate limiting, and cancellation deadlines.

### Cross-transaction data leakage

Related-transaction searches could expose unrelated merchants or customers.

Controls: carry the initiating authorization scope into correlation queries, aggregate where possible, minimize returned fields, and validate report output against the same scope.

### Integrity and provenance failure

Stale, duplicated, or tampered evidence could produce a misleading investigation.

Controls: source identity, normalized timestamps, deduplication, integrity hashes where applicable, collection timestamps, and audit linkage between evidence and report.

## Prohibited initial actions

Initial PayLens versions must not:

- restart pods or edit deployments;
- execute arbitrary SQL or shell commands;
- retrieve Kubernetes Secrets;
- trigger refunds or reversals;
- alter transaction or provider state;
- modify settlement or reconciliation records.

Future production-changing actions require a separate authorization boundary, explicit human approval, idempotency, complete auditability, and independent safety review.

## Production-readiness gate

No real environment should be connected until the project has:

- automated authorization and RBAC tests;
- redaction and sensitive-data leakage tests;
- prompt-injection and malicious-evidence tests;
- immutable audit logging;
- enforced query, time, correlation, and result-size limits;
- documented retention and deletion policies;
- approved model-provider data controls;
- a reviewed threat model and incident response plan.

Public demos use synthetic or explicitly public data only.
