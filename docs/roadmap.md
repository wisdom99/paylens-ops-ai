# Roadmap

## Phase 1 — Payment evidence path

Build synthetic payment API, payment processor, provider adapter, transaction datastore, correlated logs, and OpenTelemetry traces.

Implement:

- `get_transaction`
- `get_payment_events`
- `search_logs`
- `get_trace`

Definition of done:

- One synthetic transaction can be reconstructed end to end without an LLM.
- Tool inputs are schema-validated, authorized, and bounded.
- Tool outputs use the shared evidence contract and stable evidence IDs.
- Technical and provider states are represented separately.
- Sensitive fixtures are redacted by automated tests.

## Phase 2 — AI-assisted investigation

Add a provider-neutral model integration and structured investigation-report schema.

Definition of done:

- Every hypothesis cites evidence IDs.
- Missing evidence produces an explicit uncertainty outcome.
- Invalid, uncited, or policy-violating outputs fail closed.
- Model calls and tool calls are traced.
- At least ten labelled synthetic cases run automatically.

## Phase 3 — Related failure detection

Add failure-pattern extraction, bounded transaction correlation, provider/service health comparison, metrics, and deployment-change context.

Implement:

- `get_service_metrics`
- `get_kubernetes_events`
- `get_provider_context`
- `find_related_transactions`

Definition of done:

- PayLens can determine whether an incident is isolated or systemic.
- Related results are constrained by approved dimensions and time windows.
- A representative transaction timeline explains the common pattern.
- Blast-radius claims are reproducible from cited evidence.

## Phase 4 — Reconciliation intelligence

Add provider-state comparison, retry and reversal scenarios, settlement records, and deterministic reconciliation rules.

Implement:

- `get_reconciliation_context`
- provider-status adapters for synthetic services;
- retry, duplicate, missing-reversal, and settlement-mismatch scenarios.

Definition of done:

- PayLens distinguishes technical failure from financial outcome.
- Reports represent platform, provider, reversal, and settlement states independently.
- Ambiguous transactions produce `reconciliation_required` when appropriate.

## Phase 5 — Evaluation and security

Add labelled incident datasets, adversarial log cases, prompt-injection tests, redaction tests, RBAC tests, and quality regression gates.

Measure:

- root-cause accuracy;
- evidence precision and recall;
- timeline and payment-state accuracy;
- related-transaction and reconciliation accuracy;
- citation validity and unsupported-claim rate;
- latency and model cost.

Definition of done:

- At least 25 labelled scenarios cover failure, ambiguity, and missing evidence.
- Prompt injection cannot expand permissions or override policy.
- Model or prompt changes cannot merge when agreed quality thresholds regress.
- Published baseline results include known limitations.

## Phase 6 — Portfolio release

Add authentication, authorization, immutable audit history, a minimal investigation dashboard, deployment documentation, and an end-to-end recorded demo.

Definition of done:

- One-command local environment.
- Documented SLOs, failure handling, and retention policies.
- Threat model reviewed against the implementation.
- Public demo and datasets use synthetic data only.
- Versioned `v0.1.0` release.

## First implementation issues

1. Record ADR for monorepo modules and build tooling.
2. Define evidence and investigation-report JSON schemas.
3. Build the synthetic payment API, processor, provider adapter, and transaction store.
4. Add transaction, correlation, and trace IDs plus OpenTelemetry instrumentation.
5. Create the local Kind cluster and observability stack.
6. Implement the read-only MCP server skeleton and common policy envelope.
7. Implement bounded `get_transaction` and `get_payment_events`.
8. Implement bounded `search_logs` and `get_trace`.
9. Build deterministic timeline reconstruction and payment-state comparison.
10. Create the first five labelled payment investigation scenarios.

## Reference demo

A synthetic provider becomes slow after a deployment and 27 payments time out. PayLens correlates failed records, traces, provider latency, and deployment context; identifies ambiguous transactions; and recommends provider-status, duplicate, and reversal checks. The demo proves payment investigation and impact analysis, not merely log summarization.
