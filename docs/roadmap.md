# Roadmap

## Phase 1: Build the evidence path

Deliver a local Kubernetes demo containing three small services, correlated structured logs, OpenTelemetry instrumentation, and two deterministic MCP tools: `search_logs` and `get_trace`.

Definition of done:

- A synthetic transaction can be followed across all services.
- Tool inputs are schema-validated and bounded.
- Tool responses include stable evidence IDs.
- Sensitive fixtures are redacted in automated tests.
- No model is required to reconstruct the timeline.

## Phase 2: Add bounded AI reasoning

Pass a minimized evidence bundle to a model and require a validated investigation-report schema.

Definition of done:

- Every hypothesis cites evidence IDs.
- Missing evidence produces an explicit uncertainty outcome.
- Invalid or uncited outputs fail closed.
- Model calls and tool calls are traced.
- At least ten synthetic incident cases run automatically.

## Phase 3: Improve retrieval and evaluation

Add metrics, Kubernetes events, relevant runbooks, evidence ranking, adversarial cases, and regression gates.

Definition of done:

- At least 25 labelled incident cases.
- Published baseline measurements.
- Prompt-injection cases cannot expand permissions or override policy.
- A model or prompt change cannot merge when agreed quality thresholds regress.

## Phase 4: Demonstrate production engineering

Add authentication, authorization, audit views, provider configuration, deployment documentation, a small dashboard, and a recorded end-to-end demo.

Definition of done:

- One-command local environment.
- Documented SLOs and failure handling.
- Threat model reviewed against the implementation.
- Public demo uses synthetic data only.
- Versioned `v0.1.0` release.

## First ten implementation issues

1. Record ADR: monorepo modules and build tooling.
2. Define evidence and investigation-report JSON schemas.
3. Build synthetic payment API, processor, and provider adapter.
4. Add correlation IDs and OpenTelemetry instrumentation.
5. Create local Kind cluster and observability stack.
6. Implement the read-only MCP server skeleton.
7. Implement bounded `search_logs`.
8. Implement `get_trace` and evidence normalization.
9. Build deterministic timeline reconstruction.
10. Create the first five labelled incident scenarios.
