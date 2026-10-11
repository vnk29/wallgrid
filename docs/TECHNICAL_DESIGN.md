# WallGrid AI — Technical Design

**Status:** Proposed technical design for human review; not implementation authorization  
**Consistency basis:** `docs/PRD.md` v0.1, `docs/SYSTEM_DESIGN.md`, `docs/ML_RESEARCH_PLAN.md`, `docs/IMPLEMENTATION_PLAN.md`, `docs/M0_DECISION_REGISTER.md`, and `docs/RECONCILIATION_REGISTER.md`  
**Scope:** Documentation only. No implementation, installation, capture, model training, or traffic testing is authorized by this document.

## 1. Reading guide and decision status

This document translates the PRD and System Design into a student-feasible technical proposal and is checked for consistency with the ML Research Plan and Implementation Plan. It does not replace them. The PRD remains authoritative for requirements, safety boundaries, acceptance obligations, unresolved decisions, and numeric targets. The System Design remains authoritative for logical module ownership, contracts, and failure semantics; the ML Research Plan governs the proposed research protocol; the Implementation Plan governs dependency ordering and evidence gates.

The following labels are used throughout:

- **Source-stated fact:** explicitly stated in the source documents; this is not human approval and is not evidence that it has been implemented or validated.
- **Proposed design:** a candidate technical choice awaiting review and approval.
- **Alternative:** a realistic option retained for comparison.
- **Unresolved:** must not be silently selected; requires a decision owner, feasibility evidence, or hardware/data verification.

No dataset, final model, target value, topology, risk weight, DIG effectiveness, or performance result is selected or claimed here.

## 2. Design objectives and constraints

The proposed system must support the PRD's complete trace from authorized observation through versioned features, detection, asset context, evidence, risk, DIG, analyst review, and simulation-only response. It must distinguish confidence, risk, analyst disposition, and permission; show unknown or unavailable states; preserve lineage; and fail closed for consequential operations.

The initial environment is a Windows 11 student development machine, but the exact Windows build, CPU/RAM/storage, VM support, administrator access, interface topology, capture driver, and optional GPU are **unresolved** (DEC-02). CPU-only execution is the baseline feasibility assumption; GPU use is optional and requires measurement and approval.

The design intentionally favors one local application or a small number of bounded processes over microservices. Logical module boundaries are mandatory; network-distributed services are not.

## 3. Proposed technology choices and alternatives

The choices below are provisional defaults, not approved stack decisions.

| Concern | Provisional recommendation | Realistic alternatives | Rationale and unresolved issue |
|---|---|---|---|
| Backend API | Python 3.12 with FastAPI, Pydantic v2, and Uvicorn | Django/DRF, Flask, ASP.NET Core, Node.js/TypeScript | Python aligns with ML and packet-analysis libraries and gives typed request validation. FastAPI's async features should not imply an async capture/model design. ASP.NET Core may be preferable if Windows operations and identity become dominant. Approval and version pinning are pending. |
| Frontend | TypeScript with React and Vite; accessible component library only after review | Vue, Svelte, server-rendered templates, React with Next.js | React has broad student documentation and supports HAWK's filters/state views. A static/local SPA avoids unnecessary server infrastructure. SSR is unnecessary unless a later requirement establishes it. |
| ML framework | Evaluate both PyTorch and TensorFlow/Keras under the same experiment protocol; provisional default is PyTorch only if it is easier to reproduce the approved matrix on CPU | TensorFlow/Keras, scikit-learn for classical baselines, ONNX Runtime for deployment | PyTorch is readable and common for CNN/LSTM research; TensorFlow/Keras offers mature high-level sequence APIs and deployment tooling. Neither is silently selected. The final choice must be based on CPU feasibility, reproducibility, model packaging, Windows support, and the approved experiment matrix (DEC-03/04/12). |
| Classical baseline | scikit-learn or a documented equivalent | statsmodels, XGBoost if separately approved | Required baseline comparison, not a claim that deep learning wins. The exact baseline is unresolved until data semantics are known. |
| Storage | SQLite for local metadata and durable records, with SQLAlchemy/SQLModel or direct parameterized SQL; append-oriented audit tables and file-based artifact storage | PostgreSQL, DuckDB for analytical evaluation, JSONL/Parquet, a single document store | SQLite is student-feasible, local, transactional, and avoids infrastructure. DuckDB/Parquet may be added for offline evaluation only. PostgreSQL is an alternative if concurrency or scale evidence requires it. Retention and audit tamper-evidence remain DEC-09/ADR-05 unresolved. |
| Event delivery | In-process bounded queues or a bounded local queue abstraction with explicit backpressure/drop counters | Direct calls, SQLite-backed outbox, Redis, RabbitMQ/Kafka | Start with bounded local handoffs; use an outbox when durability is needed. A broker is disproportionate unless load tests demonstrate need. Exact choice is ADR-06 unresolved. |
| Visualization | React/TypeScript dashboard using tables, accessible charts, and explicit health/unknown states | Plotly, ECharts, Vega-Lite, server-rendered HTML | Visual encodings must not rely on color alone. Chart library choice is secondary to denominator, time-window, provenance, and accessibility contracts. |
| Packet capture | A capture adapter around Npcap/libpcap-compatible capture or an approved alternative; TShark is optional, never assumed | Wireshark/TShark subprocess, PyShark, Scapy, WinDivert, ETW/WFP-based collection | Native capture should be isolated from API privileges. TShark can aid feasibility/replay but adds process/version/parse risks. Windows packet visibility must be proven, not assumed. |
| Testing | pytest, Hypothesis where useful, FastAPI contract tests, Playwright for browser workflows, controlled packet/flow fixtures, and two-system lab tests | unittest, pytest-httpx, Cypress, .NET test stack | Tests must cover contracts, failure states, model parity, security, and the full two-system flow. The exact browser tool and fixture format remain provisional. |
| Configuration | Versioned non-secret YAML/TOML or JSON plus environment-provided secret references; schema-validated at startup | `.env` for local development, Windows Credential Manager, OS environment variables | `.env` may be local-only and ignored, never committed. Configuration changes require authorization, audit, and versioning. |

### 3.1 PyTorch versus TensorFlow/Keras evaluation

Neither framework is approved. The technical evaluation should use the same manifests, splits, preprocessing contract, seeds, tuning budget, CPU hardware, batch/sequence definitions, metrics, and failure-recording rules for both where practical.

**PyTorch advantages:** direct and inspectable tensor/control-flow code; broad research examples; straightforward custom sequence/window handling; simple CPU fallback. **Risks:** packaging and serialization choices need discipline; Windows wheels and optional acceleration must be verified; custom nondeterminism can undermine reproduction.

**TensorFlow/Keras advantages:** concise model definitions; established recurrent/CNN APIs; SavedModel/Keras packaging and TensorFlow tooling. **Risks:** Windows-native GPU support and version combinations can be restrictive; graph/eager differences and preprocessing layers need careful parity checks; the stack may be heavier for a CPU-only student project.

**Provisional selection rule:** choose the framework that passes the CPU feasibility slice, deterministic fixture parity, package load/activation, model-matrix coverage, and reproducible rerun checks with acceptable student complexity. If neither passes, preserve the classical baseline and request a scope decision; do not claim deep-learning completion. ONNX export/runtime is an alternative deployment experiment, not a required escape hatch.

## 4. Illustrative repository layout (not created on disk)

The following is a proposal only. This document creates no implementation files, tests, or configuration. `docs/IMPLEMENTATION_PLAN.md` is a separate proposed companion roadmap; its existence is not authorization or evidence.

```text
wallgrid-ai/
  docs/PRD.md
  docs/SYSTEM_DESIGN.md
  docs/TECHNICAL_DESIGN.md
  src/wallgrid/
    api/                 # authenticated HTTP endpoints and DTOs
    capture/              # privilege-isolated adapter and readiness
    observation/          # observation lineage and quality state
    flows/                # flow key, direction, expiry, deduplication
    features/             # versioned extractor and validators
    argus/                # inference, thresholds, abstention, package checks
    census/               # inventory resolution and event-time snapshots
    oracle/               # evidence, bounded summaries, explanations
    risk/                 # confidence display and risk assessments
    dig/                  # four check families and precedence
    correlation/          # grouping rules and incident identity
    hawk/                 # query/read models, review lifecycle, health
    warden/               # catalogue, approvals, simulation-only actions
    sibyl/                # historical aggregates and export
    persistence/          # repositories, transactions, retention
    auth/ audit/ health/ config/
  frontend/               # proposed React application
  models/                  # approved package artifacts, never arbitrary executables
  evaluation/              # manifests and generated evidence, policy-controlled
  tests/                   # unit, contract, integration, model, security, e2e
```

## 5. Module boundaries and API contracts

Modules communicate through versioned typed records, not shared mutable objects. An implementation may place several modules in one process. Every boundary must define accepted input, output, identity, version, error/rejection semantics, and whether the operation is idempotent.

| Module | Consumes | Produces | Boundary rule |
|---|---|---|---|
| Capture/observation | Authorized interface and scope | Observation records, readiness, loss/health | Must not declare attacks; permission failure is unhealthy, not zero traffic. |
| Flow/features | Observations and frozen config | Flows, features, quarantine records | Owns flow semantics and eligibility; invalid schema/units/non-finite values never become benign input. |
| ARGUS | Eligible feature vectors and compatible package | Predictions, model health, timing | Does not own risk or permission; unavailable package stops ordinary inference. |
| Correlation/incidents | Predictions/events | Incidents and grouping rationale | Must preserve source IDs and deduplicate deterministically. |
| CENSUS/risk | Endpoints, inventory, evidence factors | Asset snapshots and versioned risk | Unknown/ambiguous identity remains explicit; risk is not probability or authorization. |
| ORACLE | Incident and evidence references | Evidence records, explanation status, summaries | Missing evidence is unavailable; attribution is not causal proof. |
| DIG | Prediction, health, evidence, policy, recommendation | Four check outcomes and aggregate state | Non-compensatory; required fail/unknown blocks response eligibility. |
| HAWK | Incidents, health, history | Views, filters, analyst actions | Must show stale/disconnected state and audit actor/time. |
| WARDEN | Qualified incident, catalogue, DIG, approval | Recommendation and simulation records | Catalogue-only, exact-target approval, revalidation, idempotency, no real action. |
| SIBYL/export | Historical records and labels | Descriptive aggregates and redacted exports | Predicted and confirmed labels, missing periods, and denominators stay separate. |

### 5.1 Proposed HTTP API surface

These are contracts to review, not implemented endpoints. All protected operations require an authenticated identity and operation-level RBAC.

```text
GET  /health/live                 process liveness only
GET  /health/ready                dependency/model/capture readiness with explicit states
GET  /api/v1/incidents            paginated, bounded filters; no implicit “all” export
GET  /api/v1/incidents/{id}       incident plus provenance and qualified states
POST /api/v1/incidents/{id}/review-actions  acknowledge/annotate/dispose/close/reopen
GET  /api/v1/health                component states, queue/drop/error counters
POST /api/v1/model-activations     authorized, compatibility-checked, audited activation
POST /api/v1/recommendations       catalogue recommendation proposal
POST /api/v1/recommendations/{id}/approve
POST /api/v1/recommendations/{id}/simulate  exact binding, revalidation, simulation only
GET  /api/v1/evaluations/{id}      versioned result and measurement boundaries
POST /api/v1/exports               authorized, minimized, audited export request
```

Every response should include a contract/schema version, correlation/request ID, explicit error code when unsuccessful, and safe human-readable detail. Mutation requests require idempotency keys where repetition could duplicate a state transition. Input size, range, enum, timestamp, and ownership checks occur before state changes. Error bodies never disclose secrets or executable content.

## 6. Versioned record schemas

The following are logical schemas, not database DDL. Each record has `schema_version`, stable ID, event/processing/publication times as applicable, and provenance links. Version changes are additive only when compatibility is demonstrated; breaking changes create a new major version and a migration/consumer decision. `unknown` is distinct from `null`/missing, and `invalid` is not eligible for ordinary inference.

### 6.1 Observation and flow (`observation.v1`, `flow.v1`)

```yaml
observation.v1:
  observation_id: string
  source_id: string
  interface_id: string
  packet_sequence: string|null
  event_time_utc: timestamp
  processing_time_utc: timestamp
  endpoint_src: pseudonymous identifier
  endpoint_dst: pseudonymous identifier
  protocol: enum/unknown
  direction: forward|reverse|unknown
  byte_count: integer|null
  capture_quality: complete|truncated|dropped|unknown|invalid
  parent_reference: string|null
  extractor_version: string
  reason_codes: [string]

flow.v1:
  flow_id: string
  observation_ids: [string]
  normalized_key: string
  direction_semantics: string
  first_event_time_utc: timestamp
  last_event_time_utc: timestamp
  active/idle_policy_version: string
  closure_reason: idle|active|end_marker|shutdown|partial|unknown
  packet_count: integer
  byte_count: integer
  completeness: complete|partial|unidirectional|truncated|unknown
  feature_schema_version: string
  quality_state: eligible|qualified|invalid|quarantined
```

### 6.2 Features and prediction (`features.v1`, `prediction.v1`)

```yaml
features.v1:
  feature_id: string
  input_flow_or_window_id: string
  ordered_names: [string]
  values: [number|string|boolean|null]
  types: [string]
  units: [string]
  missing_states: [present|unknown|not_applicable|invalid]
  sequence_id: string|null
  timestep_index: integer|null
  padding_mask: [boolean]|null
  direction/ordering_policy_version: string
  extractor_version: string
  schema_version: string
  eligibility: eligible|invalid|quarantined
  rejection_reasons: [string]

prediction.v1:
  prediction_id: string
  input_id: string
  output_class: normal|attack|supported_class|unknown|abstain
  score: number|null
  score_meaning: declared score description
  threshold: number|null
  calibration_state: evaluated|not_evaluated|not_probability|unknown
  model_version: string
  preprocessor_version: string
  training_data_manifest: string
  inference_started_utc: timestamp
  inference_finished_utc: timestamp
  reliability_state: reliable|qualified|unknown|unavailable
  abstention_reason: string|null
```

### 6.3 Incident, asset, risk, and DIG (`incident.v1`, `asset_context.v1`, `risk.v1`, `dig.v1`)

```yaml
incident.v1:
  incident_id: string
  source_event_ids: [string]
  prediction_ids: [string]
  correlation_rule_version: string
  grouping_reason: string
  first/last_event_time_utc: timestamp
  classification: normal|attack|supported_class|unknown|qualified
  qualifiers: [needs_review, invalid_input, explanation_pending, response_ineligible]
  source_asset_snapshot_id: string
  destination_asset_snapshot_id: string
  risk_assessment_ids: [string]
  evidence_ids: [string]
  review_state: new|acknowledged|investigating|closed
  disposition: authorized_test|attack|benign|false_positive|inconclusive|unknown
  disposition_actor_time_reason: object|null

asset_context.v1:
  snapshot_id: string
  inventory_id: string|null
  observed_identifiers: [string]
  role: string|null
  owner_context: string|null
  criticality: string|unknown
  criticality_provenance: string
  resolution_state: known|unknown|ambiguous|nat_obscured|reassigned
  last_update_utc: timestamp|null
  event_time_snapshot_version: string

risk.v1:
  assessment_id: string
  incident_id: string
  formula_version: string
  severity_factor: number|unknown
  asset_factor: number|unknown
  evidence_factor: number|unknown
  weights/bands: object
  missing_factor_flags: [string]
  score: number|unknown
  score_range: object|null
  confidence_reference: string|null
  supersedes: string|null
  assessed_by_and_time: object

dig.v1:
  assessment_id: string
  incident_id: string
  prediction_id: string|null
  recommendation_id: string|null
  checks:
    input_integrity: {state: pass|fail|unknown|not_applicable, reason, evidence_ids}
    model_reliability: {state, reason, evidence_ids}
    evidence_consistency: {state, reason, evidence_ids}
    response_eligibility: {state, reason, evidence_ids}
  policy_version: string
  aggregate_outcome: accept_for_triage|needs_review|invalid_input|response_ineligible
  evaluated_utc: timestamp
```

### 6.4 Recommendation, simulation, and audit (`recommendation.v1`, `simulation.v1`, `audit.v1`)

```yaml
recommendation.v1:
  recommendation_id: string
  catalogue_entry: string
  incident_id: string
  target: string
  intended_effect: string
  impact/risk: string
  prerequisites: [string]
  evidence_version: string
  policy_version: string
  expires_utc: timestamp
  state: proposed|ineligible|awaiting_approval|rejected|expired|approved|simulated|failed
  approval_identity_time_reason: object|null
  idempotency_key: string

simulation.v1:
  simulation_id: string
  recommendation_id: string
  exact_target_binding: string
  approval_id: string
  revalidation: {identity, permission, freshness, versions, audit_available, result}
  result: completed|failed|blocked|duplicate
  simulation_only: true
  endpoint_changed: false
  completed_utc: timestamp|null
  audit_ids: [string]

audit.v1:
  audit_id: string
  actor_or_service: string
  authenticated_operation: string
  target_entity_ids: [string]
  event_time_utc: timestamp
  outcome: allowed|denied|failed|blocked
  reason: string
  relevant_versions: object
  integrity_metadata: object|null
  secret_free_details: object
```

Asset, risk, DIG, recommendation, simulation, and audit records are append/supersede oriented. Historical records are not rewritten to “correct” prior decisions.

## 7. Training/inference parity and model packaging

The training and live paths shall share one semantic contract before model commitment:

1. identical feature names, order, types, units, direction, missing states, transformations, flow/window definition, schema, and extractor version;
2. preprocessing fitted only on permitted training/development data, never the held-out test source;
3. explicit sequence grouping, time ordering, padding/masking, overlap, warm-up, partial-flow behavior, and active-window policy;
4. no scenario marker, future packet, post-incident evidence, or independent label may leak into predictive features;
5. incompatible inputs are rejected or quarantined, not filled with an unvalidated constant.

The deployable package should contain a manifest binding model artifact, framework/runtime version, preprocessor, feature schema, label mapping, threshold, calibration metadata if supported, sequence policy, training-data manifest/fingerprint, dependency lock reference, and package integrity fingerprint. Activation must validate schema and runtime compatibility, verify the fingerprint, record readiness, and support rollback to a previously compatible package. Failure must leave inference unavailable/degraded rather than silently selecting an incompatible artifact. Arbitrary executable deserialization is prohibited.

Reproducibility requires immutable dataset manifests, split/group definitions, configuration, code revision when code exists, seeds, CPU/GPU profile, dependency lock, training command approved later, run status, and all failed/infeasible experiments. Report nondeterministic operations and numerical tolerance. Never claim reproducibility until rerun evidence exists.

The experiment matrix remains required: trivial reference, classical baseline, CNN, LSTM, and CNN-LSTM under common approved splits, metrics, and proposed `P_common` prediction-anchor population. PyTorch and TensorFlow/Keras are alternatives to evaluate, not a hidden decision. If the data cannot justify spatial or temporal structure, document the limitation and request a scope decision.

### 7.1 Common population and final-source access

`P_common` is one matched set of eligible flow/window anchors used for direct comparison by every feasible arm. The anchor has a common label mapping, split/group, live-compatible feature eligibility, and no duplicate/overlap/future leakage. LSTM/CNN-LSTM context represents the same anchor; warm-up is not scored, insufficient context is excluded as `sequence_ineligible`, and padding/masking is fixed before the final protocol. Missing required features are invalid unless the approved schema allows `unknown`. Invalid, unsupported, sequence-ineligible, timeout, and arm-specific broader-coverage counts are reported separately. This contract is proposed and remains dataset/flow/sequence dependent.

Final-source access follows PRD J.3.1: public metadata/provenance and source-level integrity/independence checks are distinct from development record access. A source whose labels, features, records, row-derived statistics, scores, or results are opened is development data for that evaluation version. Access Class D final records remain sealed until `IMP-089` and final-test authorization; the immutable access ledger records attempted/successful access, actor, purpose, authorization, operations, and fingerprints.

## 8. Windows packet-capture feasibility and dependencies

Packet visibility is a prerequisite to be tested. It is not guaranteed by Windows, promiscuous mode, a library, or an empty capture.

### 8.1 Candidate dependencies

- **Npcap** is a candidate Windows capture driver/library. Official documentation: <https://npcap.com/guide/>. Its installation mode, compatible interfaces, service state, and permissions require verification by the lab operator.
- **Wireshark/TShark** is an optional diagnostic/replay/parser dependency. Official documentation: <https://www.wireshark.org/docs/> and TShark manual: <https://www.wireshark.org/docs/man-pages/tshark.html>. It must not be assumed installed, and a subprocess is not the same as a supported production capture boundary.
- **libpcap/WinPcap compatibility** may be exposed by a chosen Python binding, but the binding's supported Windows backend and version compatibility must be checked from its official documentation before approval.
- **Microsoft packet/network APIs** may be alternatives for later investigation, but WFP/ETW choices can change visibility semantics and privilege requirements; they are not silently interchangeable with packet capture.

### 8.2 Availability and permission checks (to be approved later)

The feasibility check should be a read-only inventory first, then a controlled capture test:

1. Record Windows edition/build, architecture, interface names/identifiers, driver/backend versions, Python/runtime versions, and whether the operator can install or enable the required driver.
2. Check whether Npcap and/or TShark are available using their documented installation/service locations and a version query; do not infer availability from a Python import alone. The check must report “not installed” distinctly from “installed but unusable.”
3. Enumerate interfaces and explicitly select one. Verify adapter state, IP configuration, loopback limitations, and whether the chosen path is the intended authorized path.
4. Test capture under the narrowest privilege. If elevation or an administrator-installed driver is required, isolate it to the capture component. The dashboard/API must not inherit capture elevation.
5. Generate only approved, bounded traffic between System A and the chosen System B service. Confirm independent markers/counts at A and B. Record timestamps and clock offset uncertainty.
6. Intentionally distinguish permission denied, wrong interface, no delivered traffic/mirror disabled, driver/backend failure, and genuinely idle traffic. An empty trace is unknown/unhealthy, never benign.

### 8.3 Feasibility exit evidence

Before detector evaluation, retain a written topology, interface, driver/backend, permission, scope, traffic, start/end, stop-procedure, capture-marker, loss, timestamp, and resource record. The initial `CF-01`–`CF-09` probe is benign-only; any approved controlled scenario is a later, separately authorized activity and is not part of that probe. It must not capture unrelated switched traffic, collect credentials, retain unrestricted payloads, or conduct public/off-target testing. If Windows capture cannot be made reliable, stop and request a topology/tool/scope decision; do not substitute prerecorded results for the required live demonstration.

## 9. Local storage, delivery, and operational behavior

The provisional local architecture is a single backend process plus frontend, with a capture adapter isolated by process or OS boundary if required. SQLite stores durable metadata, incidents, versions, review state, audit records, health transitions, and evaluation references. Large model/data/evidence artifacts are content-addressed files with a manifest and access controls; payload retention is disabled by default. An optional DuckDB/Parquet evaluation store is separate from operational state.

An in-process bounded queue is the starting event handoff. If a restart or audit requirement needs durability, add a SQLite outbox and replay/deduplication policy. Redis, Kafka, RabbitMQ, and a separate database are alternatives only after measured concurrency, backlog, or durability requirements justify their complexity. Unbounded queues are prohibited. Every drop, rejection, queue age, and affected scope is visible.

The design must support retention/deletion rules once DEC-09/TGT-06 are approved. Deletion must disclose retained audit metadata, backups, and limitations. SQLite backup/recovery, file permissions, atomic writes, and audit append behavior require tests; no availability or tamper-resistance claim is implied. Every handoff shall state persistence-before-acknowledgement/publication, checkpoint, retry, deduplication, replay, and loss accounting. Persisted, lost, rejected, replayed, duplicated, and unavailable states remain distinct; exactly-once delivery is not assumed.

## 10. Authentication, RBAC, configuration, logging, health, and safe failure

Roles remain viewer, analyst, administrator, and evaluator. Role membership does not automatically grant capture, configuration, approval, or simulation authority. Protected API/UI access requires authentication; non-loopback access shall use authenticated encrypted transport unless a documented lab-only exception is approved under SEC-004. A proposed session contract records subject, session state, issue/expiry/revocation/logout, operation, target, and server-side permission decision; expired, revoked, cross-role, wrong-target, and unauthorized requests fail before business-state change. This document does not select the exception.

Configuration is versioned and non-secret settings are schema-validated. Secrets, if later needed, come from environment/Windows credential facilities or another approved local secret store; they are not committed, logged, returned in errors, or included in exports. Model activation, inventory, thresholds, policy, topology, capture, exports, approvals, and retention changes require explicit permissions and audit records.

Structured logs include request/correlation ID, actor/service, component, timestamp, outcome, reason code, record IDs, versions, and duration. Network fields and notes are untrusted inert text. Logs are bounded and redacted. Audit includes denied attempts and failed writes, event/monotonic timing where applicable, append ordering, and a proposed tamper-evidence mechanism/trust boundary. Altered, deleted, reordered, truncated, and clock-anomalous records are detected or marked unknown. If audit storage fails, approval, simulation, and sensitive configuration changes fail closed; detection may continue only with an explicit audit-gap state.

Health endpoints distinguish process liveness from readiness and separately report capture visibility, flow/feature processing, model readiness, persistence, correlation, audit, and dashboard freshness. States include healthy, degraded, failed, unknown, stale, no traffic, no attacks, model unavailable, pending explanation, and overloaded. Unknown is not zero.

Safe-failure rules include: no ordinary inference on invalid features; no silent compatible-package fallback; no response eligibility after required fail/unknown; no simulation after stale/replayed/changed approval; no duplicate effects; bounded queues; explicit loss; no autonomous or real containment; and no rewriting prior audit history. Capture is isolated from dashboard/API authority and the emergency stop remains independently operable. WARDEN has a closed catalogue with no shell, firewall, host-control, endpoint-changing, or arbitrary network-control path; independent no-effect evidence is required because status fields alone are not proof.

Field collection is classified as prohibited, transient, diagnostic, persisted, or exportable. Payload capture is disabled by default. Pseudonymization uses a documented stable namespace with mapping material separated from exports. Retention, deletion, backup, and retained-audit-metadata behavior are explicit proposed policies, not universal-erasure claims. Every API operation has a versioned schema, permission, bounded inputs/outputs, safe error code/detail, correlation ID, concurrency rule, and idempotency/retry behavior.

## 11. Test strategy

Tests are evidence obligations, not completed results. Naming should follow the PRD examples such as `TEST-FR-003-01`, `TEST-SEC-009-01`, `EXP-EVAL-010-B3`, and `DEMO-DEM-006-01`.

1. **Unit tests:** flow keys, direction/expiry, timestamp handling, feature validation, missing/unknown semantics, thresholds, risk boundaries/monotonicity/rounding, DIG precedence, correlation windows, idempotency, redaction, and state transitions.
2. **Contract tests:** versioned record producers/consumers, API schemas, error codes, pagination/filter denominators, model-package manifest compatibility, and invalid-schema rejection without state changes.
3. **Integration tests:** capture adapter to flow/features; features to ARGUS; persistence/outbox replay; CENSUS snapshots; ORACLE status; DIG/WARDEN approval; HAWK publication; SIBYL aggregates. Include restart, storage, audit, model, and UI disconnect failures.
4. **Model tests:** training/live fixture parity; preprocessing fit isolation; label mapping; sequence order/padding/masking; package fingerprint/activation/rollback; CPU inference; score meaning; abstention; reproducible rerun tolerance; leakage and duplicate audits. Evaluate PyTorch and TensorFlow/Keras alternatives under the same protocol if both remain candidates.
5. **Security tests:** unauthenticated and cross-role denial, expiry/revocation, input size/type/range, unsafe deserialization, secret/log/export scanning, capture privilege separation, stale/replayed/changed approval, audit-write failure, idempotency, and no-real-action invariants.
6. **Two-system end-to-end tests:** independently recorded System A scenarios and System B capture; known benign visibility; wrong interface; permission denied; mirror-disabled/no visibility; supported controlled scenario; uncertainty/partial/duplicate/late input; model unavailable; UI disconnect; analyst review; denied and approved simulation. Reconcile scenario, packet/flow, prediction, incident, risk, DIG, review, simulation, timing, resource, loss, and audit IDs. The live test is separate from offline evaluation and must use only authorized lab traffic.

Each test retains fixture/manifest, expected and actual output, environment, configuration, seed, measurement boundary, timestamps, and evidence fingerprint. A missing result is blocked, not passed. Numeric targets TGT-01–07 must be approved before performance acceptance.

## 12. Dependency pinning and Windows setup proposal

No packages are installed by this task. After approval, the team should use a committed lockfile/constraints file, a documented Python version, a separately pinned frontend lockfile, and a captured OS/driver/backend inventory. Exact versions are intentionally not invented because compatibility must be tested on the available Windows profile.

Proposed commands for later approval (not executed):

```text
py -3.12 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt -c constraints.txt
npm ci
python -m pytest
npm run test
npm run build
```

These commands are illustrative and are not an implementation plan or authorization to create the referenced files. They must be revised after stack approval. Known Windows risks include Python/ML wheel availability, TensorFlow GPU limitations, Npcap driver installation and service permissions, interface naming, firewall/profile behavior, long path/encoding issues, clock skew between hosts, filesystem locking, subprocess quoting, CPU memory pressure, VM adapter/mirror semantics, and optional TShark not being installed. GPU availability is unknown and must not be a prerequisite.

## 13. Provisional architecture decision records and traceability

| Technical decision | Source ADR/DEC | Provisional position | Required evidence/approval |
|---|---|---|---|
| A-to-B service traffic on Windows first | ADR-01, DEC-02, DEM-001–003 | Proposed MVP visibility option, not selected | Windows driver/permission and independent visibility proof |
| Few bounded processes, no microservice platform | ADR-02, NFR-008, DEC-12 | Proposed | Failure injection, queue/storage measurements, privilege boundary review |
| Bidirectional normalized flow | ADR-03, DEC-04, DEM-004 | Proposed | Deterministic fixtures and live parity; timeout/partial policy approval |
| Package binds model/preprocessor/schema/labels | ADR-04, DEC-03/04, FR-010/038 | Proposed | Incompatibility, fingerprint, activation, rollback tests |
| SQLite/local artifact storage | ADR-05, DEC-09, TGT-06 | Proposed default | Recovery, retention, concurrency, audit-gap tests; storage approval |
| Bounded local handoff/outbox | ADR-06, FR-005/006 | Unresolved mechanism | Duplicate/late/restart/load tests |
| Asynchronous explanation | ADR-07, DEC-08 | Proposed | Queue isolation, latency/failure, provenance, method approval |
| Versioned local configuration | ADR-08, DEC-09 | Proposed | RBAC, audit, rollback, secret handling tests |
| Non-compensatory DIG and configurable risk | ADR-09, DEC-06/07 | Proposed semantics; formula unresolved | Boundary fixtures, four-arm study, rubric/precedence approval |
| PyTorch versus TensorFlow/Keras | DEC-03/04/12; FR-007; EVAL-005 | Unresolved comparison; CPU-first selection rule | Same-protocol feasibility and reproducibility evidence |

Traceability follows `objective/use case → PRD requirement → module/contract/ADR → test/experiment → evidence → acceptance`. Major coverage is:

- Observation/features: OBJ-01/06, FR-001–006, NFR-002–005/007/011/013, DEM-001–005, AC-01.
- ARGUS/model lifecycle: OBJ-02, FR-007–012/038–039, EVAL-001–013, NFR-001–006, AC-02.
- Asset/investigation: OBJ-04, FR-013–019/023–024, AC-03/04/06.
- DIG/risk/safety: OBJ-03/05/07, FR-020–024/029–031, SEC-008–010, EVAL-010/012, AC-05/06/08.
- Dashboard/history/correlation: FR-025–028/032–035, NFR-009–014, AC-07/09.
- Cross-cutting security/operations: FR-036–040, NFR-003–008/011–013, SEC-001–012, AC-10.
- Integrated demonstration: OBJ-01–07, DEM-001–009, EVAL-013, AC-11.

The technical design does not claim any item passes. Every individual identifier still needs a future evidence row as required by PRD Section O.

### 13.1 Proposed milestone crosswalk

The common names and meanings are defined in PRD §N. This technical proposal uses the same crosswalk and does not introduce alternative M3/M4/M5 meanings:

| Milestone | Technical boundary | Prerequisite and deliverable | Acceptance/stop |
|---|---|---|---|
| M0 — Governance and documentation approval | Scope, authority, roles, safety, document precedence | Human review → approval/decision/blocker record | No implementation or experiment authorization without approval. |
| M1 — Feasibility, data, semantics, and evaluation protocol | Windows visibility, data/labels, live semantics, `P_common`, leakage, access ledger, targets | M0 → feasibility and protocol artifacts | No final record/outcome access or model commitment with unresolved boundaries. |
| M2 — Reproducible development experiments and package | Matched model matrix and validation-only package | M1 → run ledger, selected package, inference contract | Final source sealed; infeasible arms remain explicit. |
| M3 — Integrated observation and investigation conformance | Capture through incident/evidence/dashboard path | M2 plus foundation contracts → parity and lineage evidence | Invalid/missing evidence or authority conflation stops. |
| M4 — DIG and simulation-only response conformance | Four checks, WARDEN catalogue, approval/revalidation/no-effect boundary | M3 plus policy contracts → safety evidence | No real action or DIG effectiveness claim. |
| M5 — Historical, operational, and policy study | History, performance/recovery, B0–B3, injections, integrated evidence | M3/M4 plus approved targets/privacy → operational evidence | Unknown boundaries or silent loss blocks. |
| M6 — Final sealed evaluation, reproducibility, and human acceptance | Pre-access freeze, sealed run, exact rerun, limitations, human disposition | M5 → `IMP-089` and final evidence | Final access follows freeze; unmet claims remain disclosed. |

## 14. Pre-implementation dependency and feasibility checklist

Implementation should not begin until the following are reviewed and recorded:

- [ ] DEC-01 owner, review authority, and approval path confirmed.
- [ ] Windows 11 build, CPU/RAM/storage, VM option, network interfaces, administrator consent, and emergency stop confirmed (DEC-02).
- [ ] Npcap/TShark availability checks defined; no installation assumed; selected capture backend and privilege boundary approved.
- [ ] Two-system topology and authorized scope approved; independent traffic truth and clock method available.
- [ ] Visibility proof plan uses the unexecuted `CF-01`–`CF-09` acceptance table in `docs/M0_DECISION_REGISTER.md §1.1` and distinguishes no traffic, wrong interface, mirror disabled, permission failure, drops, timeout, and sensor failure.
- [ ] Candidate datasets assessed for license, provenance, labels, feature definitions, live reconstruction, leakage, and EVAL-013 applicability (DEC-03/13).
- [ ] Access Classes A–D, immutable final-source ledger, and `IMP-089` pre-access freeze are approved; public metadata is not confused with final record/outcome access.
- [ ] Flow key, direction, expiry, partial-window, sequence, CNN layout, and LSTM ordering rules approved (DEC-04).
- [ ] `P_common` prediction-anchor, warm-up, padding/masking, missing-feature, overlap-purge, exclusion, and broader-coverage rules are approved before the model matrix.
- [ ] TGT-01–07 and operating-point/fair-comparison rules approved (DEC-05/14).
- [ ] PyTorch versus TensorFlow/Keras CPU feasibility and packaging comparison completed; no GPU dependency introduced.
- [ ] Risk formula/rubric and DIG observability/precedence approved (DEC-06/07).
- [ ] Explanation method, reference data, ATT&CK boundary, curated asset fields, retention, export minimization, and non-loopback policy approved (DEC-08/09).
- [ ] Simulation catalogue, approval expiry, response rubric, and human adjudicators approved (DEC-10).
- [ ] History sufficiency and forecasting gate decided; default remains descriptive trends only (DEC-11).
- [ ] Storage, queue/outbox, audit tamper-evidence boundary, recovery, and retention strategy approved (ADR-05/06).
- [ ] `IMP-076`–`IMP-088` contract dependencies are placed before their consumers; none is implemented or verified merely by this proposal.
- [ ] Dependency lock strategy and reproducible environment profile approved; actual install checks run only after authorization.
- [ ] Test fixtures cover positive, negative, boundary, overload, malformed, stale, replayed, unavailable, and unknown states.
- [ ] Security review confirms no payload/credential collection, no public/off-target traffic, no executable model deserialization, and simulation-only response.
- [ ] Human review confirms this document remains proposed and does not authorize implementation.

## 15. Contradiction check and unresolved choices

This design was checked against the PRD, System Design, ML Research Plan, Implementation Plan, M0 Decision Register, and Reconciliation Register. It preserves the following non-negotiable constraints: documentation-only status; authorized lab traffic; no real containment or arbitrary commands; no invented targets, dataset, model winner, performance, or DIG benefit; independent truth; offline/live separation; explicit unknown/invalid/degraded states; feature parity and `P_common` comparison boundaries; capture privilege separation; audit fail-closed behavior; two-system visibility proof; final-source access isolation; and human approval before simulation. It does not present the System Design's proposed topology, flow semantics, risk formula, DIG precedence, storage, queue, framework, or capture tooling as approved facts.

Unresolved choices requiring human review include Windows hardware/build and privileges; topology and capture backend/driver; Npcap/TShark availability; dataset and labels; Access Class D source designation and direction; flow/sequence semantics and `P_common` instantiation; PyTorch versus TensorFlow/Keras; all TGT values; cross-dataset applicability; risk weights/rubric; observable DIG checks; explanation method; asset/retention policy; queue/storage/audit mechanism; simulation catalogue; history sufficiency; and project capacity/optional-feature cuts.

**Human-review stop:** no implementation files, package installs, packet capture, model training, traffic generation, or operational changes were created or authorized by this document. `docs/IMPLEMENTATION_PLAN.md` is a proposed roadmap and remains separately gated.
