# WallGrid AI — Product Requirements Document

**Project title:** Deep Learning Based AI Intrusion Detection and Cyber Defense Platform  
**Project name:** WallGrid AI  
**Context:** Final-year B.Tech Computer Science engineering project  
**Version:** 0.1 — draft for review  
**Date:** 2026-10-11  
**Status:** Documentation only; not authorization to implement or conduct security tests

## Document governance and requirement language

This PRD defines the intended product, evidence needed to evaluate it, and boundaries for subsequent design. No dataset, model, software stack, deployment topology, performance result, or novelty claim has been established. This document is a draft; the other design and planning documents are also proposals pending attributable human review. Implementation, installation, capture, traffic generation, training, and security testing require separate authorization. See `docs/M0_DECISION_REGISTER.md` and `docs/RECONCILIATION_REGISTER.md`.

Requirement status and priority are independent:

- **C — Confirmed:** Explicitly required by the project brief. Acceptance details in this draft remain subject to PRD approval.
- **P — Proposed:** A recommended behavior or design decision that requires approval; it must not be represented as already agreed or implemented.
- **H — Research hypothesis:** An experimentally assessed proposition, not a promised improvement or established contribution.
- **V — Validation-dependent:** A capability conditional on evidence, feasibility, permissions, or suitable data. It cannot be advertised as supported before validation.
- **M / S / O:** Must-have / should-have / optional priorities. A validation-dependent item can be a must-have investigation without a guaranteed positive outcome.

“Shall” denotes a testable obligation if this draft is approved. “Proposed” denotes a default awaiting approval. “TBD” is an unresolved decision, not a waived requirement. Future changes must preserve requirement identifiers and record rationale, impact, and approval. Material scope or target changes after evaluation starts require a new evaluation version, not retroactive acceptance.

**Status audit note:** The status labels below were reviewed against the original project brief. Detailed engineering mechanisms, measurement procedures, interface behaviors, and safeguards that were inferred while elaborating the brief are marked **P** until the project owner or supervisor approves them. Reclassifying a requirement from C to P changes its approval status, not its intended safety strength: proposed safety controls remain mandatory for any implementation unless explicitly revised and approved. No row is evidence that the requirement has been implemented or tested.

## A. Executive summary and problem statement

WallGrid AI is a modular research and demonstration platform for network intrusion detection and analyst-assisted cyber defense. It will observe authorized laboratory traffic, construct network flows, produce compatible features, apply a trained detector, connect detections to asset context, explain available evidence, prioritize incident risk, and recommend auditable responses.

The engineering problem is broader than offline classification. A useful demonstration must establish that the monitoring host can actually see the relevant packets; that live and training features have compatible meanings; that uncertainty and missing evidence are exposed; and that response eligibility is not inferred from model confidence alone. Offline benchmark scores do not establish live-lab reliability or operational security effectiveness.

The proposed Decision Integrity Gate (DIG) investigates whether separately recorded integrity, reliability, evidence-consistency, and response-eligibility checks improve decision quality under declared conditions. It may instead add delay, increase review burden, or fail to improve outcomes. All such results must be reported.

The intended final demonstration uses two physical systems or appropriately isolated VMs. System A produces normal and explicitly authorized test traffic. System B runs WallGrid and receives traffic through a verified observation path. A full incident walkthrough includes detection, evidence, asset-aware risk, investigation, DIG outcomes, recommendation, human review, and simulated response history. Initial development is on Windows; cross-platform support is not assumed.

## B. Objectives and measurable success criteria

| ID | Objective | Success evidence and acceptance boundary |
|---|---|---|
| OBJ-01 | Demonstrate a complete observable pipeline | For each approved demo scenario, retain a trace from scenario label and capture observations through feature/model versions to the resulting detection or explicit failure outcome. No unexplained missing events in the bounded fixture set. |
| OBJ-02 | Evaluate deep-learning detection honestly | Run the approved baseline and CNN, LSTM, and CNN-LSTM experiment matrix on the same fixed split and `P_common` prediction-anchor population for direct comparison; report per-class metrics, variability, compute cost, broader operational coverage, and unsupported tasks. The final model is selected by a recorded rule, not architecture preference. |
| OBJ-03 | Separate confidence, risk, and permission | Acceptance fixtures show that each quantity has a separate field and meaning; high confidence cannot override a failed input-integrity or response-safety check. |
| OBJ-04 | Support analyst investigation | Every displayed incident has evidence references, model/preprocessing versions, risk rationale, check outcomes, and an auditable review path, including explicit unavailable states. |
| OBJ-05 | Evaluate DIG as a hypothesis | Complete the four-policy comparison, applicable ablations, and failure injections in Section K; report benefits, harms, abstentions, response errors, and review workload regardless of outcome. |
| OBJ-06 | Establish deployment feasibility | Demonstrate two-system traffic visibility and required capture privileges; measure throughput, drops, latency, and resources at the approved workload. No hard real-time guarantee is implied. |
| OBJ-07 | Keep defense actions safe | Zero actual containment, arbitrary command execution, or autonomous disruptive actions in initial-release acceptance tests. Every simulated consequential action requires recorded approval. |

### B.1 Target-selection contract

The following numeric targets are intentionally **TBD**. Invented numbers would not constitute realistic acceptance criteria without hardware, data, scenario prevalence, and flow semantics. Before final model selection and before opening the held-out test set, the student team and supervisor shall approve a target sheet as a future evaluation deliverable. Targets must be justified by preliminary training/validation measurements and the intended lab workload, not the held-out outcome.

| Target ID | Decision to freeze | Required definition |
|---|---|---|
| TGT-01 | Detection quality floors | Binary and supported-class precision/recall floors, macro-F1 floor, maximum false-positive rate, minimum evaluable support per class, and treatment of rare/absent classes. |
| TGT-02 | Latency budgets | p50/p95/p99 inference, eligible-feature-to-visible-incident, and observable-onset-to-visible-incident budgets; timeout handling and sample size. |
| TGT-03 | Capacity envelope | Offered packets/s, flows/s, concurrent flows, event rate, payload/packet mix, test duration, burst profile, and acceptable capture/drop/backlog limits. |
| TGT-04 | Resource envelope | CPU, RAM, persistent-storage growth, and GPU memory/utilization limits if used, on a named Windows hardware/software profile. |
| TGT-05 | Analyst workload and abstention | Acceptable reviews per hour or per 1,000 flows, coverage range, abstention limits, and supported reviewer capacity; do not prescribe a DIG improvement. |
| TGT-06 | Operations and privacy | History retention, evidence retention, log retention, recovery and dashboard freshness budgets, and allowed collection fields. |
| TGT-07 | Study adequacy | Minimum independent scenarios/capture groups, repeat runs/seeds, failure-injection counts, and uncertainty-interval method. |

Final acceptance requires either evidence meeting approved targets or an explicit unmet-requirement record and supervisor-approved scope revision. Missing targets or measurements prevent performance acceptance; a functional demo is not a substitute.

### B.2 Target execution contract — proposed, not approved

The target identifiers below define how a future target sheet shall be made actionable without inventing a value now. `TBD` means that the numeric threshold, tolerance, or minimum has not been approved. The target owner shall record the proposed value, rationale, population, workload, uncertainty method, approval record, and expiry/change impact before final-test freeze. A development characterization may inform a target but may not be relabeled as final acceptance evidence.

| Target | Exact metric and boundary | Population / workload | Proposed selection and uncertainty method | Owner and approval gate | Evidence for pass, fail, or inconclusive |
|---|---|---|---|---|---|
| TGT-01 Detection quality floors | On the locked common prediction population `P_common`: binary and supported-class precision, recall, F1, macro-F1, binary false-positive rate, and per-class support. Flow/window, incident, and scenario denominators remain separate. | Untouched final `P_common` for direct model acceptance; supported classes only. Live-lab scenarios are reported separately and never substituted for the offline population. | Select threshold and floors from development/validation characterization under the approved operating-point rule. Use cluster-level intervals over independent scenario/capture groups; confidence level, interval algorithm, and rare/absent-class rule are approved at M1 and frozen before final access. | Research lead and supervisor; DEC-05 / M1, before IMP-089 and final-source access. | Frozen target sheet, population manifest, confusion matrices, class support, threshold/version record, uncertainty intervals, and a reviewer disposition. Pass requires all approved floors and support conditions; fail records unmet floors; inconclusive records insufficient support, unknown labels, or unresolved denominator/leakage boundaries. |
| TGT-02 Latency budgets | Monotonic inference, feature-ready-to-visible, and observable-onset-to-visible elapsed times; report p50/p95/p99, maximum, timeouts, first alert versus update, and stage boundaries. Negative or cross-clock durations are invalid. | Named Windows profile and approved idle/nominal/overload workloads; model latency uses `P_common`-eligible predictions, while end-to-end latency uses the declared live scenario population. | Characterize development-only distributions, then propose budgets using the approved workload and safety limits. Use repeated runs and scenario/run-level uncertainty; percentile sample size, clock method, and interval rule are frozen at M1. | Evaluation/technical lead and supervisor; DEC-05 / M1, before performance acceptance and IMP-089. | Stage-span specification, clock-offset record, sample counts, p50/p95/p99/max, timeout/miss accounting, workload manifest, and reviewer disposition. Pass requires approved budgets and complete boundaries; fail records an exceeded budget; inconclusive records missing render/clock/sample evidence. |
| TGT-03 Capacity envelope | Offered/captured/processed packets/s and flows/s, eligible predictions/s, incidents/s, concurrent flows, queue depth/age, drops, rejects, and backlog under burst and sustained periods. | Approved lab traffic only, with named packet/flow/event mix, duration, burst profile, and isolation limits. Payload collection remains disabled unless separately approved. | Characterize idle, nominal, burst, and overload behavior before proposing envelope values. Use run-level and scenario-level variability; exact offered rates, duration, drop/backlog tolerances, and uncertainty rule remain TBD until M1. | Lab operator and technical lead; DEC-02/DEC-05 / M1; safety limits are required before any authorized run. | Workload and scope authorization, offered/captured/processed counters, queue/drop records, resource samples, duration, and recovery disposition. Pass requires sustained operation within approved limits; fail records overload or loss beyond the target; inconclusive records unavailable counter scope or unmeasured traffic. |
| TGT-04 Resource envelope | CPU method/aggregation, RAM, persistent-storage growth/I/O, and applicable GPU memory/utilization, with idle/steady/peak values and measurement interval. | Named Windows hardware/software/driver profile across idle, nominal, overload, restart, and retention workloads. | Characterize resource distributions and storage growth before proposing limits. Use repeated-run summaries and run-level uncertainty; no GPU requirement is implied and exact limits remain TBD. | Technical lead and lab operator; DEC-02/DEC-05/DEC-12 / M1, before performance acceptance. | Hardware/profile manifest, sampling method, resource time series or summaries, storage/recovery records, competing-workload declaration, and reviewer disposition. Pass requires approved limits and bounded growth; fail records an exceeded resource or unsafe storage condition; inconclusive records missing profile, sampling, or attribution. |
| TGT-05 Analyst workload and abstention | Valid coverage, valid uncertainty abstention, invalid-input rejection, review events/incidents per hour and per 1,000 eligible inputs, queue age/backlog, repeat reviews, and reviewer minutes when feasible. | `P_common` for direct model coverage/error comparisons; broader operational population for workload and live capacity. DIG improvement is not a target. | Select operating points on development data using an approved coverage/review-budget rule. Use scenario/capture-group variability and disclose reviewer-minute sampling limits; exact workload/coverage limits remain TBD. | Research lead, analyst, and supervisor; DEC-05/DEC-14 / M1, before DIG study freeze. | Frozen operating-point record, coverage/error curves, abstention reasons, invalid counts, workload denominators, queue measurements, reviewer sample protocol, and disposition. Pass/fail concerns approved workload and coverage bounds; inconclusive records insufficient reviewer or scenario evidence. |
| TGT-06 Operations and privacy | Retention periods and storage growth, evidence/log/history deletion behavior, recovery and dashboard-freshness spans, allowed collection fields, payload state, export redaction, backup handling, and retained audit metadata. | All persisted record classes, approved exports, backups, and deletion-eligible evidence under the authorized lab scope. | Policy values are selected from privacy/lab review, not tuned against final outcomes. Exact retention/freshness/recovery values and field allowlist remain TBD; uncertainty is replaced by explicit policy coverage and deletion/recovery evidence. | Privacy lead, lab owner, and technical lead; DEC-09 / M0 for safety boundaries and M1 for the target sheet, before collection/export. | Field classification, retention/deletion/backup policy, access ledger, redaction review, storage-growth and recovery evidence, audit records, and reviewer disposition. Pass requires policy/evidence alignment; fail records prohibited collection, deletion/recovery breach, or unbounded storage; inconclusive records unknown backup or deletion scope. |
| TGT-07 Study adequacy | Minimum independent scenarios/capture groups, repeat runs/seeds, applicable failure-injection counts, independent adjudication coverage, aggregation unit, and uncertainty-interval method. | The approved model-comparison population, live scenario matrix, DIG candidate universe, and independent capture/scenario groups; correlated flows are not independent samples. | Propose adequacy from study design and development feasibility, not from favorable final results. Predeclare resampling/interval method, confidence level, multiplicity/exploratory treatment, and missing/ambiguous handling at M1. | Research lead and supervisor; DEC-05/DEC-14 / M1, before final-protocol freeze. | Study plan, group manifest, run/seed ledger, fault manifest, adjudication coverage, interval method, exclusions, and reviewer disposition. Pass requires the approved study design to be completed; fail records missing required independent units; inconclusive records unresolved truth, group identity, or interval assumptions. |

### B.3 Immediate safety constraints versus empirical targets

The following controls apply immediately to any future authorized activity and are not empirical targets: no public or third-party traffic; written scope and allowlist; bounded rate, duration, storage, and retention; a configured storage high-water/stop condition (numeric value TBD) with explicit degraded/loss accounting; an independently operable stop; no credentials or unrestricted payload retention; capture privilege isolation; authenticated operation-level authorization; fail-closed response eligibility; audit-gap blocking for sensitive changes; and simulation-only WARDEN behavior. A violation is a safety stop or failed disposition even if an empirical target would otherwise be met.

TGT-01–TGT-05 and TGT-07 remain empirical/protocol decisions. TGT-06 combines immediate privacy policy constraints with later operational budgets. None of these definitions approves a number, dataset, workload, or experiment.

## C. Target users and primary use cases

| User | Needs | Primary use cases |
|---|---|---|
| Student researcher/model evaluator | Repeatable experiments and defensible conclusions | Select data, reproduce preprocessing, compare models/policies, inspect failures, export measurements. |
| Lab security analyst | Understand and prioritize detections | Review evidence and assets, filter history, distinguish low confidence from high risk, annotate dispositions, approve simulations. |
| Lab administrator/operator | Safe, healthy deployment | Configure capture and approved inventory, manage access and policies, inspect pipeline health, stop traffic observation, diagnose visibility gaps. |
| Faculty evaluator | Traceable assessment | Reproduce the two-system demonstration, inspect result provenance, review limitations and requirement-test coverage. |

Proposed logical permissions are **viewer**, **analyst**, **administrator**, and **evaluator** (read/export approved evaluation material). One person may hold several roles in the lab, but the acting identity and authorization shall be recorded. A deployment shall not silently equate network access with permission to administer or approve actions.

Primary use cases: UC-01 normal-traffic observation; UC-02 supported attack detection and incident investigation; UC-03 uncertain/out-of-distribution review; UC-04 invalid-input quarantine; UC-05 recommended response and approved simulation; UC-06 experiment/policy comparison; UC-07 historical and operational-health review. Detailed workflows appear in Section H.

## D. Scope and release boundaries

### D.1 Must-have

- Verified two-system authorized-lab observation, Windows development feasibility, flow construction, and versioned live-compatible features.
- ARGUS baseline comparisons and CNN/LSTM/CNN-LSTM experiments; a selected, validated binary detector. Multiclass output only for supported labels/data.
- CENSUS curated asset inventory, roles, criticality, and incident endpoint association; unknown assets remain visible.
- ORACLE evidence and investigation summaries, plus at least one suitable, validated explanation path or an explicitly approved scope exception.
- Risk/confidence separation, documented configurable risk calculation, explicit review states.
- A bounded experimental DIG policy preserving all four check families, with uncertainty and safety limitations.
- HAWK incident history, filters, review, statistics, operational health, and measured near-real-time updates within approved budgets.
- WARDEN recommendations, policy decisions, human approval, simulated action history, and no real disruptive action connectors.
- SIBYL descriptive historical trends; cross-cutting correlation, access control, input validation, audit, versioning, and measurements.
- Reproducible offline evaluation and separately labeled live-lab evaluation; requirement traceability and acceptance evidence.

### D.2 Should-have

- Validated confidence calibration, distribution-shift indicators with documented limitations, and a robust recovery/resume path.
- Comparison of more than one explanation method where useful; evidence-supported MITRE ATT&CK mapping for eligible incidents.
- Analyst-oriented export with privacy controls; reproducible scenario replay in addition to live capture.
- Measured explanation stability and bounded asynchronous explanation computation.

### D.3 Optional / validation-dependent

- Multiclass detection beyond the binary requirement, optional anomaly detection, and additional deep-learning variants.
- Passive inventory enrichment; active discovery only under separate explicit authorization.
- SIBYL forecasting only after historical-data sufficiency and time-aware validation gates are met.
- Additional OS support, alternative capture topologies, and broader encrypted-traffic metadata analysis.

### D.4 Explicitly out of scope for the initial implementation

- Autonomous blocking, host isolation, process termination, firewall changes, or other disruptive defense; production response connectors.
- Offensive automation against public, third-party, or non-consenting systems; uncontrolled malware or denial-of-service demonstrations.
- Claims of zero-day detection, production SOC readiness, guaranteed prevention, hard real-time execution, or universal attack coverage.
- Automatic visibility into unrelated switched traffic, TLS decryption, collection of credentials, or unrestricted packet-payload retention.
- Unvalidated forecasting, automatic ground-truth generation from predictions, or unsupported ATT&CK assertions.
- Enterprise multi-tenancy, high-availability deployment, compliance certification, and production-scale availability guarantees.

## E. Functional requirements

Status labels below capture origin; proposed acceptance details become normative only after approval. Each requirement shall have at least one linked future test and evidence record (Section O).

### E.1 Observation and feature pipeline

| ID | Status / priority | Requirement and verification condition |
|---|---|---|
| FR-001 | C / M | The platform shall observe traffic only on configured, authorized lab interfaces and expose capture readiness, selected interface, and permission errors. A denied-permission fixture shall report unhealthy capture rather than healthy zero-attack traffic. |
| FR-002 | P / M | The platform shall construct flows using a documented, versioned key, direction convention, idle/active expiry rules, and timestamp units. A deterministic packet fixture shall yield the expected flow membership, feature values, and closure reasons. |
| FR-003 | C / M | Training and inference shall enforce the same feature names/order, types, units, direction semantics, missing-value rules, transformations, and schema version. An incompatible fixture shall be rejected before inference and shall not be treated as normal. |
| FR-004 | P / M | Every flow/event shall carry a unique identifier, observation-source identifier, event/processing timestamps, schema/extractor version, and completeness/quality state. Missing required lineage shall make it invalid for ordinary detection. |
| FR-005 | P / M | Malformed, unsupported, duplicate, late, and partially observed inputs shall follow documented policies with reason codes and counters. Duplicate ingestion shall not create duplicate analyst-visible incidents for the same event identity. |
| FR-006 | P / M | The platform shall measure capture loss where observable, extraction failures, queue age/depth, processing failures, and model readiness. Unknown loss shall display as unknown, not zero. |

### E.2 ARGUS — AI Detection Engine

| ID | Status / priority | Requirement and verification condition |
|---|---|---|
| FR-007 | C / M | ARGUS shall support reproducible comparison of simple/non-deep-learning baselines with CNN, LSTM, and CNN-LSTM experiments under the same approved split, `P_common` prediction-anchor population, and metric protocol. Broader arm-specific operational coverage is reported separately. Failed/infeasible experiments shall retain reasons rather than disappear from results. |
| FR-008 | C / M | ARGUS shall output normal/attack predictions for a validated binary task and, only for a validated label mapping, supported attack classes. Unsupported/unknown states shall not be silently mapped to normal or a known attack. |
| FR-009 | P / M | Every prediction shall expose the decision score, score meaning, selected threshold, model version, preprocessing version, prediction time, and reliability/abstention state. A score shall not be called a probability unless its interpretation and calibration have been evaluated. |
| FR-010 | P / M | Training preprocessing shall be fitted only on the training partition. The deployable model package shall bind preprocessing, feature schema, label mapping, thresholds, dependency metadata, and training-data version; incompatible packages shall not activate. |
| FR-011 | P / M | ARGUS shall support an explicit abstain/review outcome under configured uncertainty or unsupported operating conditions. The original score/output and reason shall remain available; abstention shall not delete the event. |
| FR-012 | V / O | Optional anomaly detection shall have its own validation, score meaning, thresholds, and evaluation against labeled/controlled cases. An anomaly score alone shall not establish an attack label. |

### E.3 CENSUS — Asset Intelligence

| ID | Status / priority | Requirement and verification condition |
|---|---|---|
| FR-013 | P / M | CENSUS shall maintain stable inventory identifiers, observed network identifiers, role, owner/context if supplied, and criticality with provenance and last-update time. Curated inventory is sufficient initially. |
| FR-014 | P / M | Incidents shall associate source and destination with inventory records and retain an event-time asset-context snapshot. Unknown, ambiguous, NAT-obscured, or reassigned endpoints shall be identified explicitly rather than asserted as a device identity. |
| FR-015 | P / M | Criticality levels and their numeric conversion shall be documented and configurable by authorized users. Changes shall be audited, versioned, and tested at score boundaries; unknown criticality shall use an explicit policy, not an implicit zero. |

### E.4 ORACLE — Investigation and Explainability

| ID | Status / priority | Requirement and verification condition |
|---|---|---|
| FR-016 | C / M | ORACLE shall display detection evidence, relevant flow properties, asset context, and observation limitations with provenance. Missing evidence shall appear as unavailable and shall not be invented by a summary. |
| FR-017 | P / M | The platform shall provide an explanation method appropriate to the selected model/features (for example SHAP), recording method, model, reference/background data version, and completion/failure status. Attributions shall be presented as model-behavior explanations, not causal proof of an attack. |
| FR-018 | P / M | Investigation summaries shall distinguish measured facts, detector outputs, analyst annotations, and hypotheses; each factual assertion shall reference retained evidence. Deterministic summaries are sufficient; a generative model is not required. |
| FR-019 | V / S | An ATT&CK mapping shall include technique ID/name, framework version, observed evidence, applicability rationale, and uncertainty. Where evidence does not meet the relevant definition, display unmapped rather than infer a technique from an attack label alone. |

### E.5 DIG and Risk/Confidence Engine

| ID | Status / priority | Requirement and verification condition |
|---|---|---|
| FR-020 | P / M | The software shall implement and record separate DIG checks for input integrity, model reliability/operating conditions, evidence consistency, and proposed-response eligibility. Each check shall retain pass/fail/unknown/not-applicable, reason, evidence reference, policy version, and time. This functional requirement does not assert that DIG improves security outcomes; that question is evaluated under H-DIG-01 in Section K. |
| FR-021 | C / M | A failed integrity or response-safety check shall prohibit response eligibility regardless of score or risk. Unknown safety prerequisites shall also prohibit eligibility; detection/evidence may remain visible with qualification. Test high-confidence predictions with failed/unknown checks. |
| FR-022 | P / M | DIG shall produce explicit accept-for-triage, needs-review, invalid-input, or response-ineligible outcomes while preserving individual check results and the ungated prediction when valid. Derived outcome precedence shall be deterministic and tested. |
| FR-023 | C / M | Incident risk shall be distinct from confidence and response permission, incorporating declared severity, asset criticality, and corroborating evidence using a documented, versioned formula. Show factors, missing-context treatment, and limitations for each score. |
| FR-024 | C / M | Authorized users shall configure risk/score thresholds and review rules with range/consistency validation and audit. Re-scoring historical incidents shall create a new assessment version, preserving the original. |

### E.6 HAWK — Security Dashboard

| ID | Status / priority | Requirement and verification condition |
|---|---|---|
| FR-025 | C / M | HAWK shall display incident ID, classification, event/display timestamps, relevant endpoints, confidence meaning, risk, asset context, evidence, and DIG state. Incomplete/stale results shall be visibly marked. |
| FR-026 | C / M | HAWK shall provide paginated incident history and combined filters for time, class, risk band, endpoint/asset, and review state, with statistics whose denominators and time window are shown. Boundary fixtures shall verify filter and count correctness. |
| FR-027 | C / M | New/updated incidents shall reach the dashboard within approved freshness budgets at the declared load when measured. Real-time or near-real-time behavior is a deployment objective to be measured, not an unverified guarantee. Disconnection, delayed processing, and stale display shall be distinguishable from an absence of incidents. |
| FR-028 | C / M | Authorized analysts shall acknowledge incidents, annotate evidence, request review, record dispositions, and close/reopen incidents with reason and actor/time history. A disposition shall not silently become a training label. |

### E.7 WARDEN — Response Management

| ID | Status / priority | Requirement and verification condition |
|---|---|---|
| FR-029 | C / M | WARDEN shall propose only catalogued responses with target, supporting evidence, intended effect, impact/risk, prerequisites, authorization requirements, and simulation status. Unmet prerequisites shall produce an ineligible recommendation with reasons. |
| FR-030 | C / M | Consequential response simulations shall require authenticated human approval bound to the exact incident, target, recommendation, and policy/evidence version. Rejection, expiry, or changed prerequisites shall prevent the simulation until new approval. |
| FR-031 | C / M | The initial release shall execute only simulated containment; it shall not invoke real containment or accept arbitrary commands. Repeated action requests shall not create repeated effects, and every request, approval, rejection, simulation result, and failure shall be audited. |

### E.8 SIBYL and cross-cutting behavior

| ID | Status / priority | Requirement and verification condition |
|---|---|---|
| FR-032 | C / M | SIBYL shall show historical alert/attack-label trends by stated time window and normalized denominators where available, separating predictions from confirmed analyst dispositions and missing periods from zero counts. |
| FR-033 | V / O | Forecasting shall remain disabled until approved data sufficiency, chronological backtesting, naive-baseline comparison, and error/interval criteria are met. Forecasts shall show horizon, uncertainty, provenance, and limitations. |
| FR-034 | C / M | The platform shall monitor traffic/feature changes and model-health indicators. Performance degradation claims shall require delayed/verified labels; unlabeled drift alone shall be reported as an indicator, not proven accuracy loss. |
| FR-035 | C / M | Versioned correlation rules shall group related events by approved keys and time windows, preserve underlying event IDs, and expose why they were grouped. Tests shall cover duplicates, unrelated simultaneous events, window boundaries, and late arrivals. |
| FR-036 | C / M | Structured logs and audit records shall link capture, flow, prediction, incident, review, and response identities where applicable and include actor/service, time, outcome, reason, and relevant versions without secrets. |
| FR-037 | C / M | All exposed APIs shall authenticate/authorize protected operations, validate input size/type/range, and reject invalid requests without state changes. Configuration, inventory, model activation, policy changes, and response approvals require explicit permissions. |
| FR-038 | P / M | Model activation shall be an audited controlled operation with compatibility/readiness checks and rollback to a previously compatible version. Load failure shall stop ordinary inference and expose degraded health, not reuse a model silently. |
| FR-039 | C / M | The platform shall record inference and end-to-end timing, throughput, resources, errors, and drops with units and measurement scope sufficient for the evaluation protocol. End-to-end timing shall include, where applicable, traffic observation, flow construction, feature extraction, inference, persistence/processing, and alert publication; boundaries shall be reported rather than implied. |
| FR-040 | P / M | Authorized evaluation export shall preserve scenario labels, predictions, policy outcomes, and versions while applying approved minimization/redaction. Export and retention/deletion operations shall be audited and shall not expose secrets or unrestricted payloads. |

## F. Non-functional requirements

| ID | Status / priority | Requirement and acceptance method |
|---|---|---|
| NFR-001 | P / M | Detection quality shall be measured on an untouched, leakage-controlled test partition using Section J metrics and TGT-01. Publish class support and errors; aggregate accuracy alone is insufficient. |
| NFR-002 | P / M | Measure and report inference and end-to-end latency distributions and timeout rates against TGT-02 under TGT-03. End-to-end measurement shall include traffic observation, flow construction, feature extraction, inference, queueing/persistence/transport, and alert publication where applicable. Report measurement boundaries, p50/p95/p99, throughput, and dropped or unprocessed traffic where measurable; do not label model runtime as total detection latency or claim real-time behavior without evidence. |
| NFR-003 | P / M | Sustain the approved capacity envelope while meeting the specified drop/backlog limits. Overload tests shall expose degradation, bounded storage/queues, and explicit dropped/rejected counts; silently dropping events fails acceptance. |
| NFR-004 | P / M | Measure CPU, RAM, storage growth, and applicable GPU resources during idle, nominal, and overload runs on the named machine. TGT-04 determines resource acceptance. |
| NFR-005 | P / M | Following capture/model/storage failure, the platform shall expose degraded status, prevent unsafe recommendations, and recover within the approved budget or record an unresolved gap. Recovery tests shall account for every persisted and lost event. |
| NFR-006 | P / M | Re-running a fixed evaluation with the recorded dataset manifest, seeds, preprocessing, environment, and configuration shall reproduce results within a predeclared numerical tolerance; nondeterministic operations and deviations shall be reported. |
| NFR-007 | P / M | Initial capture, feature extraction, model execution, and UI operation shall be demonstrated on the approved Windows profile. Dependency and driver constraints shall be recorded; other platforms require separate validation. |
| NFR-008 | P / M | Module boundaries shall expose versioned data contracts and explicit failure semantics. Contract tests shall detect incompatible producers/consumers without requiring a specific transport, framework, or database. |
| NFR-009 | P / M | An evaluator using a supplied incident fixture shall locate class, endpoints, evidence, risk rationale, check failures, and action history and distinguish simulated from real actions. Record task completion and usability issues; do not claim a usability study from one walkthrough. |
| NFR-010 | P / M | Risk/DIG status shall not rely on color alone; essential analyst flows shall be keyboard-operable, with labeled controls, legible errors, and consistent timestamp/timezone formatting. Verify a manual accessibility checklist. |
| NFR-011 | P / M | Monitoring shall distinguish no traffic, no attacks, sensor failure, capture loss, model unavailable, pending explanation, and stale UI using separate states and tested health transitions. |
| NFR-012 | P / M | Retention and storage limits shall follow TGT-06. Deletion tests shall verify removal of eligible evidence and disclose retained audit metadata, backups, and any limitations rather than promise universal erasure. |
| NFR-013 | P / M | Event timestamps shall be stored in an unambiguous time basis with display timezone; cross-host clock offset and its uncertainty shall be recorded for latency studies. Clock anomalies shall not yield silently accepted negative latency. |
| NFR-014 | P / S | Expensive explanation/history tasks should not block primary detection; measure the detection impact of enabling them and expose pending/failed explanations. Any claimed near-real-time benefit remains subject to measured latency and throughput evidence. |

## G. Security, privacy, safety, and audit requirements

The threat model includes malformed capture data, untrusted uploaded data/model artifacts, spoofed endpoints, unauthorized API use, compromised analyst credentials, adversarial or shifted traffic, stale context, policy misconfiguration, and audit/evidence tampering. This is a bounded lab project, not a claim of comprehensive defense against these threats.

| ID | Status / priority | Requirement and verification |
|---|---|---|
| SEC-001 | C / M | Capture and test execution shall require a recorded lab scope: owner approval, systems/interfaces, scenario categories, traffic limits, start/end conditions, and emergency stop. Out-of-scope destinations or scenarios shall not be accepted into the approved run plan. |
| SEC-002 | C / M | Packet capture privileges shall be restricted to the component/user requiring them. The dashboard and ordinary API shall not require elevated OS privileges solely because capture does. Verify actual privilege boundaries on Windows. |
| SEC-003 | C / M | Protected data and operations shall enforce authenticated identity and operation-level authorization. Tests shall cover unauthenticated access, cross-role denial, session expiry/revocation, and attempts to approve actions without analyst permission. |
| SEC-004 | P / M | Non-loopback UI/API access shall use authenticated encrypted transport, unless a documented lab-only exception is approved with exposure restrictions. Secrets shall not be committed, displayed in errors, or included in logs/exports. |
| SEC-005 | C / M | Parse all packets, imports, API fields, and model metadata as untrusted input with size/resource limits. Do not execute embedded commands or deserialize executable artifacts from untrusted sources; malformed fixtures shall fail safely. |
| SEC-006 | C / M | Collect only approved metadata/features by default; payload capture is disabled unless separately justified and authorized. IP/device identifiers shall be treated as potentially sensitive; external evaluation exports shall use approved pseudonymization where needed. |
| SEC-007 | C / M | Evidence, policies, approvals, and model provenance shall be access-controlled. Audit records shall be append-only to ordinary users, with a documented tamper-detection method and its trust boundary; do not claim resistance to a fully privileged host attacker. |
| SEC-008 | C / M | A failed or unknown safety prerequisite shall fail closed for response eligibility. Overrides may annotate or request review but shall not bypass simulation-only limits or erase a check result. |
| SEC-009 | P / M | Approval shall be revalidated at simulation time against current target, authorization, evidence freshness, and policy version; stale/replayed approvals shall fail. A time-of-check/time-of-use fixture shall demonstrate this behavior. |
| SEC-010 | C / M | Audit shall include denied operations and attempted responses as well as successful actions. Audit-write failure shall prevent approval/simulation and sensitive configuration changes; detection may continue only with an explicit degraded/audit-gap indicator. |
| SEC-011 | P / M | Model packages and dataset manifests shall include integrity fingerprints and trusted-source provenance. A hash mismatch shall prevent activation; hashes alone shall not be represented as proof of trustworthiness. |
| SEC-012 | C / M | Incident text, network-derived fields, and analyst notes shall be rendered as untrusted content, not executable markup or instructions. Any later external explanation service requires separate privacy approval; none is assumed here. |

All response controls apply even in simulation, so the demonstration exercises the intended governance without affecting real endpoints. Data-retention periods, permitted fields, role assignments, and network exposure remain decisions in Section P.

### G.1 Proposed cross-cutting control contracts — not implemented or verified

The following contracts make the requirements testable while preserving their proposed/pending status. They are design obligations for later tasks, not evidence that a control exists.

| Contract | Proposed minimum | Required negative/boundary evidence | Traceability |
|---|---|---|---|
| Authentication, session, and authorization | Every protected request carries an authenticated subject, session identifier/state, operation, target, and authorization decision. Sessions have explicit issuance, expiry, revocation, logout, and failure states. Operation-level permissions are checked server-side before state change; role names alone do not grant capture, model promotion, audit deletion, approval, or simulation. | Unauthenticated, expired, revoked, cross-role, wrong-target, and unauthorized approval requests are rejected without business-state change; denials and audit-write failures are recorded. | SEC-003–004, FR-037, IMP-076, IMP-080 |
| Capture isolation and emergency stop | Capture runs under the narrowest required OS identity/process boundary. IPC has an allowlist, authenticated peer/process identity, bounded messages, and no dashboard/API authority. The lab stop can be operated independently of the capture/dashboard process and blocks new traffic/capture on activation. | Capture identity cannot administer the dashboard, access secrets, promote models, delete audit records, or approve/simulate responses. Stop during idle, capture, queue, and restart states leaves a visible audit/health record and no continued authorized activity. | SEC-001–002, DEM-008, IMP-012, IMP-076 |
| Durable acknowledgement and recovery | Every handoff declares whether persistence precedes acknowledgement/publication, its checkpoint, retry, deduplication key, replay policy, and failure state. Persisted, lost, rejected, replayed, duplicated, and unavailable records are distinguishable; no exactly-once claim is made without evidence. | Database/disk-full, queue overflow, process restart, partial write, corruption, and replay fixtures reconcile input, persisted, lost, and reprocessed identities. Silent loss blocks acceptance. | FR-005–006, NFR-005, FR-036, IMP-077, IMP-081 |
| Audit integrity and trust boundary | Audit records are append-oriented, access-controlled, clocked with event and monotonic timing where applicable, linked to actor/service and entity/version IDs, and protected by a proposed tamper-evidence mechanism whose trust boundary is stated. The design does not claim protection from a fully privileged host attacker. | Altered, deleted, reordered, truncated, clock-anomalous, denied, failed, and audit-unavailable cases are detected or explicitly marked unknown; sensitive changes fail closed when the required audit record cannot be committed. | SEC-007/010, FR-036, NFR-013, IMP-078, IMP-082 |
| Collection, pseudonymization, retention, deletion, and backup | Every field is classified as prohibited, transient, diagnostic, persisted, or exportable. Payload is disabled by default. Pseudonymization uses a documented stable namespace and access boundary; mapping material is separate from exports. Retention, deletion, backup, and retained-audit-metadata behavior are explicit and disclose limits. | A field-level review rejects unapproved payload/credential fields; exports cannot reverse the approved boundary; deletion tests distinguish eligible evidence from retained audit metadata/backups; unknown deletion scope is inconclusive, not erased. | SEC-006, FR-040, NFR-012, TGT-06, IMP-083 |
| API errors, concurrency, and idempotency | Every operation has a versioned request/response schema, permission, bounded inputs/outputs, safe error code/detail, correlation ID, ownership rule, concurrency/version rule, and retry/idempotency behavior. Error responses contain no secrets or executable content. | Invalid type/range/size/enum/timestamp, stale version, duplicate key, retry, reconnect, and unauthorized mutation requests produce deterministic outcomes and no unintended state change. | FR-037, NFR-008, IMP-080, IMP-081 |
| Simulation-only response | WARDEN accepts only a closed catalogue entry with an exact incident/target/policy/evidence binding. Approval is authenticated, expiring, single-use or idempotently consumed, and revalidated at simulation time. The simulator has no shell, firewall, host-control, endpoint-changing, or arbitrary network-control path. | Arbitrary command-like input, stale/replayed/duplicate/changed-target/unauthorized approval, audit outage, and simulator failure are blocked. Independent process/network/endpoint observations establish no real effect; `simulation_only` and `endpoint_changed` fields are not sole proof. | FR-029–031, SEC-008–010, IMP-087, IMP-061–063 |

All contract rows remain proposed until their owner, decision ID, evidence, and approval record are completed. A future implementation may use one process or several bounded processes, but it shall preserve these boundaries.

## H. End-to-end workflows and behavioral contracts

### H.1 Shared records and state semantics — proposed logical design

These are implementation-independent contracts, not a prescribed microservice architecture. One process or several components may satisfy them.

- **Observation/flow:** source/interface, flow ID, endpoints/protocol, first/last packet times, completeness, closure rule, counters, extractor/schema version, permitted evidence references.
- **Prediction:** flow ID, score(s), selected class, threshold, confidence/calibration meaning, uncertainty/abstention state, model/preprocessor/data versions, inference timing.
- **Incident:** unique ID, underlying events, grouping reason/version, time bounds, classification and qualification, event-time asset context, risk assessments, evidence, explanation state, analyst state.
- **DIG assessment:** assessment ID, associated prediction/incident/recommendation, individual check outcomes/reasons/evidence, policy version, aggregate outcome, evaluation time.
- **Recommendation/action:** approved catalogue entry, target, effect, impact, prerequisites, policy/evidence versions, expiry, approval/rejection identity, simulation result, audit links.

Proposed incident lifecycle: **new → acknowledged → investigating → closed**, with reopening allowed and audited. **Needs review**, **invalid input**, **explanation pending**, and **response ineligible** are independent qualifiers, not replacement attack labels. Closure requires a disposition (confirmed authorized test/attack, benign, false positive, inconclusive) and reason. Dispositions express reviewer evidence, not unquestionable truth.

Proposed recommendation lifecycle: **proposed → ineligible OR awaiting approval → rejected/expired OR approved → simulated/failed**. Any changed prerequisite invalidates previous approval. There is no `executed-real-containment` state in the initial release.

### H.2 Normal traffic — UC-01

1. Operator verifies authorization, healthy capture, compatible active model, clock status, and chosen interface.
2. System A generates a known benign scenario and independently logs its scenario ID and start/end times.
3. System B observes packets, constructs eligible flows, validates features, and performs inference.
4. Valid normal outcomes update traffic statistics and retained evaluation records according to retention policy. The dashboard need not create one incident per benign flow.
5. Failed/unknown integrity or reliability checks prevent a reassuring normal conclusion: create a health/review event instead.
6. Acceptance: expected visibility is verified, benign decisions have lineage, normal counts reconcile with recorded outcomes, and any false-positive incidents remain measurable rather than suppressed from evaluation.

### H.3 Detected attack — UC-02

1. The authorized scenario supplies independent ground truth; WallGrid does not consume scenario labels as predictive features.
2. Capture, feature validation, and inference produce a supported attack output with score meaning and versions.
3. Correlation creates/updates an incident without losing original event IDs; CENSUS resolves both endpoints or labels them unknown.
4. Risk assessment shows severity, asset criticality, and corroboration. ORACLE attaches evidence, a bounded summary, and available explanation; pending explanation is explicit.
5. DIG records every applicable check; the dashboard shows qualified findings even if response eligibility fails.
6. Analyst investigates and records disposition. WARDEN may propose an eligible simulation but shall not run it automatically.
7. Acceptance: scenario-to-incident linkage and timing are inspectable. A missed scenario is recorded as a false negative under the declared matching window, not removed from the demo report.

### H.4 Uncertain prediction / unsupported conditions — UC-03

1. Valid inputs produce ambiguous scores, unreliable calibration conditions, insufficient sequence context, or a validated shift indicator.
2. Preserve scores and detected conditions; mark the prediction abstained or qualified according to policy.
3. Create/route a review event using declared thresholds and workload controls. Risk remains a separate assessment; high criticality can increase review priority without inventing attack certainty.
4. Unknown/failed response prerequisites make recommendations ineligible. An analyst may obtain additional evidence and create a new assessment; the old assessment remains.
5. Acceptance: uncertainty is not converted to normal, no event vanishes from coverage denominators, and review volume/latency are measured.

### H.5 Input-integrity failure — UC-04

1. A schema mismatch, invalid value, corrupt artifact, missing feature, or untrusted pipeline condition is detected.
2. Quarantine/reject the affected item, record sanitized diagnostic evidence and a reason code, and mark integrity failed. Do not invoke ordinary model inference on invalid features.
3. If integrity failure is discovered after inference, preserve but invalidate/qualify that prediction and prevent its use for response eligibility.
4. Raise operational-health/review information; continue unaffected traffic only when isolation is verified. Halt the affected pipeline if integrity cannot be bounded.
5. After repair, reprocess only under the approved replay/deduplication policy with new provenance; never rewrite prior audit entries.
6. Acceptance: invalid data is neither benign evidence nor an unexplained gap; failures are counted, displayed, and block unsafe response decisions.

### H.6 Response recommendation — UC-05

1. WARDEN identifies a catalogue recommendation for a qualified incident and records proposed target, rationale, impact, and prerequisites.
2. DIG evaluates evidence, safety, and authorization independently of confidence/risk. A failure/unknown blocks eligibility with a visible reason.
3. For eligible recommendations, an authorized analyst sees the exact target and conspicuous **SIMULATION ONLY** label, then approves or rejects with reason.
4. Before simulation, recheck identity, approval freshness, target binding, policy/evidence versions, and audit availability.
5. Simulate the declared effect without issuing containment commands; record completion/failure and no actual endpoint modification.
6. Acceptance: unauthorized, stale, replayed, duplicate, or changed-target requests fail safely. An eligible recommendation is permission to request approval, not proof that an action is warranted.

## I. Two-system deployment and demonstration requirements

### I.1 Proposed topology alternatives — choose and validate one

| Option | Visibility mechanism | Constraints |
|---|---|---|
| A targets a lab service on B | B captures its own relevant inbound/outbound interface traffic | Simplest proposed MVP; service and capture compete for resources. Coverage is traffic to/from B, not all of A's activity. |
| A and B as isolated VMs with supported virtual mirroring | A uses an isolated virtual network; traffic is explicitly mirrored to B | Hypervisor mirror/promiscuous settings and interface support must be verified; promiscuous mode alone does not guarantee delivery. |
| Physical mirror/TAP observation | A's relevant link is mirrored or tapped to B | Requires available compatible hardware, correct mirror direction, bandwidth budget, and permission. |

Traffic replay can supplement deterministic evaluation, but it shall not replace the required live two-system walkthrough. A separate destination may be needed for mirror/TAP scenarios; its role must be declared rather than implying two hosts expose arbitrary transit traffic. No topology is selected by this PRD.

### I.2 Deployment and demonstration requirements

| ID | Status / priority | Requirement / pass evidence |
|---|---|---|
| DEM-001 | C / M | Use two distinct physical systems or isolated VMs with documented identities, interfaces, IP ranges, isolation, and observation path. Show why the selected traffic reaches B. |
| DEM-002 | C / M | Verify visibility with known traffic and independently observed counts/capture markers before model evaluation. Detect wrong-interface and mirror-disabled fixtures; an empty capture shall not be accepted as a successful benign run. |
| DEM-003 | C / M | On Windows, validate capture driver/backend compatibility, permissions, firewall exposure, timestamps, and interface selection. Any required VM or non-Windows capture alternative requires explicit approval and shall not be reported as Windows-native validation. |
| DEM-004 | C / M | Freeze flow closure/inference eligibility rules before the demo. Record effects of active/idle timeouts, unidirectional observations, retransmissions, fragments, truncated packets, encrypted payloads, and unsupported protocols. |
| DEM-005 | C / M | Maintain an offline/live feature compatibility matrix. Any dataset feature unavailable or semantically different live shall be removed from both pipelines, consistently reconstructed, or trigger model retraining/scope change; it shall not be filled with a convenient constant without validation. |
| DEM-006 | C / M | Execute at least one approved normal and one supported controlled-attack scenario plus uncertainty, integrity-failure, and denied-response fixtures. The walkthrough shall show evidence, assets/risk, investigation, DIG decisions, and response simulation/rejection. |
| DEM-007 | C / M | Record scenario truth independently, scenario-to-flow/incident matching rules, timing uncertainty, configuration/model versions, capture drops, resources, and all misses/false alarms. Clear scenario markers must not leak into predictive features. |
| DEM-008 | C / M | Isolate the lab from unintended targets; use bounded traffic rates and an operator stop procedure. Confirm no real containment action and no traffic outside approved scope during acceptance. |
| DEM-009 | P / M | Rehearse capture failure, model unavailable, UI disconnect, and process restart. The presentation shall disclose degraded state and recovery behavior rather than substituting an unlabelled prerecorded result. |

### I.3 Flow and sequence decisions that must precede implementation

Proposed default for approval: bidirectional transport flows keyed by protocol and normalized endpoint pair, preserving originator/responder meaning when observable; explicit fallback for unknown direction. Select idle/active timeouts and define whether inference occurs on closed flows or independently validated partial windows. A model trained on complete flows cannot automatically classify partial flows with equivalent validity. Long-lived flows require a validated active-window policy or a disclosed detection delay.

CNN feature layouts and LSTM sequence axes require justification. Tabular column order is not automatically spatial structure, and arbitrary dataset row order is not time. Sequence grouping, ordering, padding/masking, window overlap, warm-up, and boundary handling shall match training and live inference. Future packets or post-incident information must not leak into an earlier detection claim.

## J. Dataset selection and model evaluation

### J.1 Dataset and experiment requirements

| ID | Status / priority | Requirement / evidence |
|---|---|---|
| EVAL-001 | C / M | Select dataset(s) only after comparing license/access rights, provenance, collection conditions, label quality, class support, timestamps, raw-packet/flow availability, feature definitions, known leakage risks, and suitability for live extraction. Record rejected candidates and reasons. No named dataset is assumed selected. |
| EVAL-002 | C / M | Maintain immutable dataset manifests/fingerprints and documented cleaning/label mappings. Quantify duplicates, missing values, class imbalance, unknown labels, and excluded records, including how each exclusion affects reported denominators. |
| EVAL-003 | C / M | Define train/validation/test partitions using temporal and/or host/session/capture-group separation appropriate to the data. Related flows, overlapping windows, duplicates, and augmented derivatives shall not straddle partitions. Publish a leakage assessment; random row splitting alone requires specific justification. |
| EVAL-004 | C / M | Fit scaling, imputation, encoding, resampling, feature selection, calibration, and thresholds without test-set information. Calibration uses a designated training/validation subset; oversampling occurs only within training. Freeze the final configuration before test access. |
| EVAL-005 | C / M | Compare at least a trivial reference predictor and an appropriate classical supervised baseline with CNN, LSTM, and CNN-LSTM under documented tuning budgets and the same approved `P_common` prediction-anchor population for direct comparison. Record architecture/input rationale, sequence warm-up/padding/masking, dropped or ineligible examples, denominators, seeds, stopping rules, hyperparameters, training time, and failed runs. If temporal data is unsuitable, report the sequence-model limitation and obtain a scope/data decision instead of claiming meaningful temporal learning; broader arm-specific operational coverage is reported separately. |
| EVAL-006 | C / M | Choose the deployed model with a predeclared development/validation-based rule balancing quality, false alarms, inference cost, and live compatibility. Architecture, preprocessing, feature selection, thresholds, calibration, model-selection decisions, DIG parameters, and other tunable decisions shall be finalized using development/validation data only. Report all planned model results; a simpler winner is acceptable. |
| EVAL-007 | C / M | Evaluate binary classification and supported multiclass classification separately, reporting confusion matrices, per-class support, precision/recall/F1, macro F1, and false-positive rate. Absent-support classes shall be marked not estimable, not assigned a misleading perfect score. |
| EVAL-008 | C / M | Evaluate offline held-out data and live-lab scenarios separately. Report domain mismatch and actual live feature parity; do not transfer benchmark metrics to live traffic without measurement. |
| EVAL-009 | C / M | Repeat stochastic training and controlled scenarios under TGT-07. Report variability and uncertainty intervals at the independent capture/scenario level where feasible, rather than treating correlated flows as independent evidence. |
| EVAL-010 | C / M | Report all four DIG policies, applicable ablations, injected failures, review/response outcomes, and latency/resource overhead using the same frozen base detector and controlled scenario inputs. |
| EVAL-011 | V / S | Evaluate score reliability using reliability plots and calibration metrics (for example Brier score and defined-bin ECE) when probability interpretation is supportable. State class imbalance and binning limitations. |
| EVAL-012 | C / M | Use independently recorded scenario labels and documented adjudication for incident/response correctness. Prediction, risk, DIG output, and analyst belief alone shall not define ground truth. Ambiguous cases remain unknown and are reported separately. |
| EVAL-013 | P / M | Before deciding whether cross-dataset evaluation is applicable, complete and record a compatibility and provenance assessment. When at least two suitable, sufficiently compatible independent datasets are available, perform cross-dataset generalization testing. An independent dataset shall be separately collected or originate from a separate collection source and shall not be merely a duplicate, transformed copy, or partition of the same underlying capture data. Document provenance, collection characteristics, labels, feature definitions, limitations, and compatibility, including missing or incompatible feature handling. Train/develop on one dataset and keep the final independent test dataset untouched until final evaluation; do not fit preprocessing, feature selection, calibration, thresholds, model selection, DIG parameters, or other tunable decisions on it. Evaluate per-class precision and recall, macro F1, false-positive rate, class support, coverage/abstention, and relevant latency/resource measures; analyze degradation and distribution shift; explicitly report unsupported classes and label-mapping limitations. If two suitable datasets cannot be compared, document the reason and use the strongest feasible alternative, such as independent capture-group holdouts and controlled live-lab evaluation, while stating that the fallback is not equivalent to cross-dataset validation. |

### J.2 Metric definitions and measurement boundaries

The evaluation unit must be explicit: packet, flow/window, prediction, correlated incident, scenario, or response proposal. Flow-level and incident-level results shall not be combined in one denominator.

| Measure | Definition / reporting rule |
|---|---|
| Precision, recall, F1 | For each supported class, precision = TP/(TP+FP), recall = TP/(TP+FN), F1 = 2PR/(P+R). State handling of undefined denominators. Macro F1 is the unweighted mean over the predeclared evaluable classes; weighted F1 may supplement it. |
| Binary false-positive rate | Benign flows classified as attack / all labeled benign flows, under the declared threshold. Separately report benign flows routed to review; abstentions are not true negatives. Also report benign scenarios producing at least one false incident. |
| Operational false alerts | False-positive incidents per observation hour and per 1,000 benign flows, with offered traffic prevalence and correlation rule. Show alert counts before/after correlation. |
| Abstention / coverage | Coverage = non-abstained valid predictions / all eligible valid inputs. Report abstention by true class, invalid-input rejection separately, and accuracy/error on the covered subset. Show coverage-versus-error curves where possible. |
| End-to-end attack handling | For labeled attacks, report detected, sent to review, rejected as invalid, missed, and timed out separately. Report automatic detection recall and detection-or-review coverage without equating review with correct classification. |
| Inference latency | Monotonic elapsed time around the actual prediction computation, documenting inclusion/exclusion of preprocessing, batching, warm-up, and accelerator synchronization. Report sample count, p50/p95/p99, maximum, and failures. |
| Feature-ready-to-visible latency | Time from feature eligibility/readiness to first correctly rendered incident or review state; includes queueing, inference, policy, persistence, transport, and dashboard update. Report explanation-ready delay separately. |
| Observable-onset-to-visible latency | Time from first relevant malicious packet visible at B to its matched visible incident, including flow/window waiting. Scenario-start-at-A latency may also be reported only with clock-offset bounds. Missed/timed-out scenarios remain in completion/timeout rates, not silently omitted from latency analysis. |
| Throughput | Offered/captured/processed packets and flows per second, eligible predictions/s, incidents/s, dropped/rejected items, queue backlog, and sustained versus burst duration. Report input and completed output rates separately. |
| Resource use | CPU sampling method and aggregation, peak/steady RAM, disk growth and I/O where relevant, optional GPU utilization/memory, hardware profile, run length, and competing workload. |
| Review workload | Review events and distinct correlated incidents per hour/per 1,000 inputs, queue backlog/age, and repeated reviews. Measure reviewer minutes on a stated task sample if feasible; counts alone are workload proxies, not measured human time. |
| Response decision quality | Eligible but unjustified proposals, unjustified approvals/simulations, justified eligible proposals, and justified proposals blocked/deferred. Use the independent response rubric in Section K and report unknown-label counts. |

Threshold sweeps and PR curves may be added without replacing fixed operating-point results. Class prevalence, split construction, and scenario mix shall accompany metric tables. A lab dataset with deliberately balanced attacks does not estimate real deployment precision.

### J.2.1 Common model-comparison population — proposed contract

For direct comparison, define one dataset-independent contract before training and instantiate it only after a candidate dataset, label map, flow/window semantics, and sequence feasibility are approved. Let **`P_common`** be the set of prediction anchors that every feasible arm is permitted to score under the same approved split, label map, feature contract, and evaluation window. The trivial predictor, classical ARGUS baseline, CNN, LSTM, and CNN-LSTM shall report direct-comparison metrics on the same `P_common`; an arm that cannot score an anchor is not silently given a different denominator.

The proposed contract is:

1. **Prediction unit:** one eligible flow or fixed event-time window is one prediction anchor. Packet, flow/window, correlated incident, scenario, and response-proposal results remain separate. The exact flow/window definition is still DEC-04 and must be frozen before model training.
2. **Eligibility:** the anchor has a versioned independent label or an explicitly accounted-for unknown/ambiguous label; required features are available or reconstructed with approved live-compatible semantics; the anchor is not invalid, quarantined, duplicated, outside the approved group/time split, or contaminated by future/post-incident information.
3. **Sequence representation:** an LSTM or CNN-LSTM may represent the same anchor using ordered observations from its approved flow/session/endpoint context. Required warm-up observations precede the scored anchor and are not separate scored examples. If the required context is unavailable, the anchor is `sequence_ineligible`, excluded from `P_common`, and counted separately; it is not fabricated with row order or future observations.
4. **Padding and masking:** padding, masking, overlap, and boundary behavior are fixed in the representation contract. Padding cannot create label or future information; mask values and the reason for any padded/incomplete sequence are retained. Whether padding is permitted for the selected data remains a data-dependent decision.
5. **Missing features:** a missing required feature makes the anchor invalid/quarantined unless the frozen schema explicitly permits an `unknown` state. Any imputation, scaling, encoding, feature selection, or resampling is fitted on training data only and applied unchanged to validation, final test, and live data. Missingness is never converted to benign evidence.
6. **Label assignment:** every arm receives the same anchor-level label from the versioned mapping and independent truth/adjudication record. Unknown, unsupported, ambiguous, and invalid labels remain in accounting but are excluded from the supported-class denominator under the approved rule.
7. **Overlap purging:** related flows/windows, duplicate records, augmented derivatives, shared scenario markers, shared capture artifacts, and any group defined as dependent by the leakage audit are purged before partitioning and before constructing `P_common`. No related unit may cross train, validation, final-test, or independent-source boundaries.
8. **Denominators:** quality, coverage, and abstention on `P_common` use the same denominator across arms. Invalid-input rejection, sequence ineligibility, timeout, and unsupported-label exclusion are reported as separate counts. Each arm may report a broader operational population, but those results are explicitly labelled non-matched coverage and are not used as a direct architecture comparison.
9. **Evidence and change control:** the `P_common` manifest records anchor IDs or privacy-preserving stable references, split/group membership, label status, feature/sequence eligibility, exclusions and reasons, and denominator effects. The population, warm-up rule, overlap purge, and exclusion policy are frozen before the final protocol and cannot be changed after final-source access without treating the result as development feedback.

This is a proposed, dataset-agnostic contract. It does not select a dataset, decide whether sequence arms are applicable, or claim that any architecture will outperform another. The data-specific anchor definition, common-population size/support, and sequence policy remain DEC-03/DEC-04/DEC-14 decisions.

### J.3 Model-selection and final-test isolation

All architecture choices, feature/preprocessing choices, thresholds, calibration procedures, model-selection rules, DIG policy parameters, and other tunable decisions shall be finalized using development/validation data and approved operating-point rules. The independent cross-dataset test, when required by `EVAL-013`, shall remain untouched until final evaluation and shall not be repeatedly consulted to select a model or tune a policy. The same isolation applies to any designated final independent capture-group or live-lab test source.

If final testing reveals weaknesses and the design is changed, the affected result shall be treated as development feedback, the change shall be documented, and the revised design shall be evaluated on a new independent test source where feasible. If no new independent source is feasible, report the limitation and do not present the revised result as an independent confirmation.

### J.3.1 Final-source access classes and ledger — proposed contract

The word **access** is intentionally divided into classes so that a public compatibility review cannot be confused with consultation of final-test outcomes. The exact dataset and source direction remain DEC-03/DEC-13 decisions.

| Access class | Permitted before final-protocol freeze | Prohibited before final-protocol freeze | Required record |
|---|---|---|---|
| **A — Public metadata/provenance** | Official license and access terms, source owner, collection description, published schema/feature definitions, declared label taxonomy, topology, dates, and public documentation. | No private records, labels, features, scores, samples, or derived outcome statistics. | URI or source identifier, retrieval time, version, content fingerprint, actor, purpose, and rights decision. |
| **B — Source-level integrity/independence** | Whole-source checksums, archive/file names and sizes, published manifests, source-level partition identifiers, collection lineage, and overlap/duplicate checks that use only source metadata or cryptographic source fingerprints. | Row/window/flow labels or features, row-level duplicate/overlap counts, class distributions, model outputs, or any derived statistic that could influence selection. | Access-ledger entry, operation type, input/output artifact fingerprints, assertion that no record-level outcome was opened, and reviewer. |
| **C — Approved development data** | Records, labels, features, timestamps, groups, and derived statistics from a source explicitly designated as development/training/validation after M0/data authorization. | Treating the source as an untouched final source after record-level inspection; using it as independent confirmation. | Dataset designation, manifest/fingerprint, purpose, actor, approval, fields accessed, transformations, derived outputs, and exclusion/retention record. |
| **D — Sealed final-test source** | No record-level access, label/feature inspection, scoring, derived statistic, preprocessing fit, or model/policy consultation before `IMP-089` is approved and final-test authorization is recorded. | Any raw record, label, feature, timestamp, group ID, row/window duplicate check, score, class distribution, threshold/calibration result, DIG result, or other outcome-bearing derivative before the freeze and authorization. | Immutable ledger entry created before opening, authorization/version, source fingerprint, sealed-source identity, actor/tool, one-way opening event, locked configuration fingerprint, and post-run reconciliation. |

Public documentation about a source may support the M1 applicability assessment. If record-level data from a candidate source is opened before `IMP-089`, that source is development data for this evaluation version and cannot be presented as an untouched final independent source. A final source may be opened only once the final protocol, `P_common`, labels, preprocessing, model-selection rule, thresholds, DIG parameters, measurement boundaries, and access ledger have been approved. Permitted post-freeze integrity checks are limited to the locked run and must not tune any choice.

The ledger shall capture every attempted and successful access, including denied access: source designation, access class, actor/service, purpose, authorization/decision ID, timestamp basis, fields or operations requested, artifact fingerprints, outputs or derived-statistic fingerprints, and reviewer disposition. A missing ledger entry blocks final-test acceptance. No dataset-specific source is selected by this protocol.

## K. DIG research hypothesis and evaluation protocol

### K.1 Hypothesis and questions

**H-DIG-01:** Under declared traffic, labeling, and operating conditions, a policy that independently evaluates input integrity, model reliability, evidence consistency, and response eligibility may reduce unjustified response decisions compared with simpler policies, at a measurable cost in abstentions, review burden, and processing delay.

This is not a novelty claim. A future literature review must establish relationships to selective prediction, out-of-distribution detection, policy-based response, safety gating, and evidence-aware triage. Negative or inconclusive results are valid project findings. The software requirement is to implement and record the four check families; the experiments determine whether the resulting policy changes detection quality, review workload, latency, resource use, or unjustified response decisions; only collected, independently evaluated results may support conclusions about effectiveness, generalization, or contribution.

Research questions:

1. At comparable detector quality/coverage or review budgets, does DIG change the number/rate of unjustified response-eligibility decisions?
2. How many correct detections or justified responses are delayed, deferred, or lost because of gating?
3. Which check families contribute measurable value, and which duplicate existing confidence/risk policies?
4. Do checks detect injected integrity/reliability/context failures, and how often do they incorrectly reject clean inputs?
5. How much additional review workload, latency, and resource consumption is introduced?
6. Do conclusions persist across held-out scenarios/classes/asset contexts and repeated runs, or only in the constructed test mix?

### K.2 Proposed policy semantics

| Check family | Example observable inputs | Proposed result semantics / limitation |
|---|---|---|
| Input/pipeline integrity | Schema compatibility, finite/range checks, completeness, extractor/model fingerprints, capture-health state | Fail for a demonstrably invalid prerequisite; unknown when provenance/health cannot be established. Valid ranges cannot prove absence of adversarial manipulation. |
| Model reliability | Validated operating envelope, score calibration, uncertainty criteria, validated shift indicator, sequence warm-up | A large score is not sufficient. Indicators require their own thresholds/validation and do not guarantee adversarial robustness. |
| Evidence consistency | Independently observed protocol/counters, timing, rule-supported traffic evidence, contradictory observations | Check explicit class-specific expectations only when justified. Missing/insufficient evidence is unknown; the model's own attribution is not independent corroboration. |
| Response eligibility | Approved target, evidence sufficiency, action scope/impact, identity/permissions, policy freshness, simulation-only constraint | Fail/unknown blocks eligibility. A pass does not authorize execution or establish attack truth; human approval remains separate. |

Proposed precedence: integrity fail → invalid-input; integrity unknown → needs-review with response-ineligible; reliability/evidence fail or unknown → needs-review and response-ineligible; response check fail/unknown → response-ineligible; otherwise accept-for-triage and, if a recommendation exists, eligible-to-request-approval. Not-applicable requires a reason and shall never bypass a required safety prerequisite. Normal valid flows with no recommendation may mark response eligibility not-applicable.

DIG shall preserve the vector of check outcomes. It shall not reduce safety to a weighted average where high confidence can cancel failed integrity. Thresholds, evidence rules, missing-data handling, and precedence require approval and versioning.

### K.3 Proposed initial risk calculation — not a probability

To make the risk requirement concrete without asserting empirical validity, the proposed configurable formula is:

**Risk = round(100 × (wS × S + wA × A + wE × E))**, where S, A, E are in [0,1], weights are non-negative and sum to 1.

- **S:** documented severity of the supported attack hypothesis; unknown classifications require an explicitly configured provisional severity and review flag.
- **A:** criticality of the asset plausibly impacted by that hypothesis. Source and destination are both retained; if affected direction is ambiguous, use the higher criticality provisionally and label the ambiguity. This default requires approval.
- **E:** strength of independently corroborating evidence under a documented rubric; confidence and model attributions shall not be counted again as independent evidence.
- Weights, criticality/severity mapping, risk-band thresholds, evidence rubric, and unknown-factor defaults are **TBD** and no final values have been selected. Their approval is a prerequisite to treating the risk score as an accepted project policy. Show provisional status and a min/max score range for unknown factors, rather than pretending the default is observed evidence.

Confidence is displayed separately and is not an implicit authorization multiplier. Risk ranks review; it does not prove attack probability, monetary loss, or response eligibility. Tests shall verify boundaries, weight validation, monotonicity for each known factor, deterministic rounding, missing-context flags, and versioned recalculation. Alternative formulas require documented review, not an undocumented implementation choice.

### K.4 Mandatory comparison arms

Use a frozen detector, feature pipeline, split/scenarios, correlation rules, action catalogue, and independent labels. Declare operating-point selection rules before final evaluation. Tune each policy only on its permitted development partition with documented comparable search budgets; no arm may consult final test data. All arms retain the same non-negotiable authorization, parser validation, and simulation-only safeguards.

| Arm | Policy being compared | Required outputs |
|---|---|---|
| B0 — fixed threshold | Detector predicts attack above a validation-selected fixed score threshold; fixed class-to-recommendation rules | Detection/alert output and would-propose/would-mark-eligible decisions under shared safety constraints. |
| B1 — confidence-gated | B0 plus a distinct validated reliability/confidence rule that can defer to review | Coverage, selective errors, review decisions, and recommendation eligibility. It must not be merely an unexplained second threshold identical to B0. |
| B2 — context-aware risk prioritization | Detector plus versioned severity/asset/evidence risk scoring and risk-based triage/recommendation thresholds | Ranked queue, review decisions, risk factors, and eligibility decisions. No full four-family integrity policy. |
| B3 — proposed DIG | Same detector/context inputs plus explicitly separated four-family checks and precedence | Individual reasons, aggregate states, withheld/deferred recommendations, and overhead. |

Record shared versus arm-specific rules so the comparison is interpretable. B3 may use B2's risk calculation for prioritization, but its safety checks remain non-compensatory. Thresholds, evidence rules, missing-data handling, precedence, and operating-point selection require approval and versioning. Report natural selected operating points and, where feasible, matched alert-review-budget or matched-coverage comparisons; do not cherry-pick a favorable threshold. Matched comparisons shall use equivalent scenarios, labels, evaluation data, and fair tuning budgets. Invalid data that cannot be passed safely to a baseline parser is recorded as a shared rejection; assess additional policy behavior using valid but semantically inconsistent inputs or offline decision fixtures, never by removing essential safeguards.

### K.5 Ablations and failure injection

Required applicable ablations: B3 without each check family in turn; without asset context; without independent corroboration; and alternative uncertainty/shift checks if claimed. Keep the detector and other parameters fixed or explicitly report retuning. Ablations of response-safety logic evaluate hypothetical eligibility decisions only; the real simulator authorization boundary remains enforced.

| Injection / condition | Expected safety behavior | Measurements |
|---|---|---|
| Reordered/missing features, unit mismatch, non-finite values, wrong schema | Reject/quarantine; no normal inference on invalid data | Check sensitivity, false rejection on clean inputs, reason correctness. |
| Corrupt model/preprocessor, incompatible artifact, extractor-version mismatch | Prevent activation/inference; show degraded health | Detection time, affected event count, recovery, missing audit evidence. |
| Packet loss, partial/reversed flows, delayed expiry, duplicates/late arrivals | Qualify/reject according to declared policy; preserve accounting | Coverage, errors, delay, false incidents, correlation correctness. |
| Valid but shifted traffic, unseen permitted class, ambiguous score, insufficient sequence context | Review/qualify if validated indicator detects it; no promise of universal detection | Shift sensitivity/false alarms, selective error, class-wise abstention. |
| Conflicting evidence or stale/unknown asset identity | Show conflict/unknown; no invented certainty | Risk changes, review load, incorrect consistency failures/passes. |
| High confidence plus failed integrity/safety | Preserve check failure; block response eligibility | Number of unjustified eligible decisions; fixture invariant violations must be zero. |
| Unauthorized/stale/replayed approval, changed target, audit-write failure | No simulation; preserve denial where possible and expose audit failure | Bypass attempts blocked, decision reasons, any unauthorized state change. |
| Overload, storage outage, model outage, UI disconnect | Explicit degraded status and bounded failure handling | Drops, queue age, recovery gaps, latency, resource limits. |

For each injection, record preconditions, independently known fault, affected records, expected outcome, actual outcome, and whether the check can realistically observe that fault. Include clean controls and combinations of faults; do not count an unobservable fault as a guaranteed detectable condition.

### K.6 Adjudication and response-error measurement

Before evaluating policies, approve a response rubric covering target identity, supported evidence, legitimate/benign activity, proportionality, action impact, authorization, and freshness. A lab scenario may legitimately warrant **no response** even if correctly classified as an authorized attack test.

- Label each common candidate recommendation **justified**, **unjustified**, or **insufficient evidence**, independently of the evaluated policy. Prefer adjudication blinded to policy arm; record reviewer agreement/disagreements and supervisor resolution where feasible.
- Unjustified-eligibility rate = eligible proposals labeled unjustified / all eligible proposals with determinate labels. Also report unjustified eligible proposals / all unjustified candidates, absolute counts, and insufficient-evidence eligibility separately.
- Unjustified-approval/simulation rate uses actually approved/simulated proposals with determinate labels as its denominator. Report these separately from policy eligibility: human review can conceal policy errors.
- Justified-response coverage = eligible justified proposals / all justified candidates. Report deferrals, review queue age, and simulated time-to-decision.
- Use the same candidate universe across arms or report both proposal-generation and eligibility differences. Show the trivial abstain-all/no-response reference: zero unjustified actions alone is not useful performance.

Report detection quality, response precision/coverage, review workload, and overhead together. Improvements require uncertainty-aware evidence; failure to meet a hypothesis shall not be relabeled as proof of safety or novelty.

## L. Major-module acceptance criteria

All criteria below are draft acceptance obligations, not completed tests. Each module needs positive, negative, boundary, and failure-path fixtures where applicable. Performance acceptance additionally depends on approved TGT values.

| Acceptance ID / module | Required demonstration and evidence | Requirement links |
|---|---|---|
| AC-01 — Capture/features | Known live traffic is visible on B; deterministic packet fixtures yield expected versioned flows/features; incompatible/malformed inputs are rejected; missing visibility and drops are exposed. Offline/live compatibility matrix has no unexplained feature substitution. | FR-001–006; NFR-003, 007, 013; DEM-001–005 |
| AC-02 — ARGUS | Reproduce baseline/CNN/LSTM/CNN-LSTM experiment records and selected model; show binary inference with versions, declared thresholds, and abstention. Multiclass is accepted only with supported labels and metrics. Model architecture, preprocessing, thresholds, calibration, and selection shall be frozen from development/validation data before final testing. Where applicable, complete the untouched cross-dataset test and report degradation, unsupported classes, coverage, latency, and resource measures. Meet approved quality/latency budgets or record a failed criterion. | FR-007–012, 038–039; EVAL-001–009, 011–013; NFR-001–004, 006 |
| AC-03 — CENSUS | Correctly associate known endpoints, mark unknown/ambiguous identities, retain original asset context after inventory changes, and reproduce criticality mapping/boundary scores. | FR-013–015, 024; SEC-003 |
| AC-04 — ORACLE | An evaluator traces each summary fact to evidence, sees an appropriate versioned explanation or explicit failure, and distinguishes attribution from cause. Unsupported ATT&CK mapping is absent; supported mapping includes definition-based rationale. | FR-016–019; NFR-009, 014; SEC-012 |
| AC-05 — DIG | The software implements all four check families and retains outcomes/reasons; high-confidence unsafe/invalid fixtures cannot pass; unknown prerequisites route correctly. Separately complete the four-arm comparisons, declared operating-point rules, matched-budget/coverage comparisons where feasible, applicable ablations, and failure-injection results without requiring improvement. Conclusions about effectiveness remain conditional on collected evaluation results. | FR-020–022; SEC-008–010; EVAL-010, 012; Section K |
| AC-06 — Risk/confidence | Risk formula and factors reproduce fixture scores, handle unknown context visibly, and pass monotonicity/boundary tests. Changing confidence alone cannot bypass a failed safety check. Threshold changes and reassessments retain versions/history. | FR-009, 015, 021, 023–024; Section K.3 |
| AC-07 — HAWK | Show incident fields, combined filters/counts, review/closure/reopening, and freshness/disconnection states; verify essential keyboard flows. Statistics reconcile with fixtures and measured end-to-end update latency—including applicable observation, flow, feature, inference, and publication stages—meets the approved budget or records the unmet result. | FR-025–028, 035; NFR-002, 009–011 |
| AC-08 — WARDEN | Show eligible and denied recommendations, approval/rejection, stale-approval rejection, duplicate-request safety, and audited simulation. Verify no endpoint/firewall/host modification and no executable real-response path. | FR-029–031; SEC-008–010; DEM-008 |
| AC-09 — SIBYL | Known historical fixtures yield expected time-bucket totals with missing periods and predicted/confirmed labels separated. Drift indicators are labeled correctly; forecasting remains disabled unless its separate data/backtesting gate passes. | FR-032–034; NFR-012–013 |
| AC-10 — Cross-cutting security/operations | Pass authorization/input-validation/version/correlation contracts; demonstrate outages, bounded overload, recovery, audit gaps, retention, and redacted export. Reconcile event IDs and health states across modules. | FR-035–040; NFR-003–008, 011–013; SEC-001–012 |
| AC-11 — Integrated demonstration | Execute approved two-system scenario matrix including benign, detected/missed attack accounting, uncertainty, input failure, and recommended/denied simulation; retain independent truth, timings, resource report, measured end-to-end latency boundaries, throughput, dropped/unprocessed traffic where measurable, and limitations. Complete EVAL-013 when its dataset-availability condition is met, or document the non-equivalent fallback and justification. | OBJ-01–07; DEM-001–009; EVAL-013; Section H |

An explanation method, optional class, or forecast that fails validation shall be marked unsupported. Removing a must-have acceptance criterion requires explicit scope approval, not silent substitution with a screenshot.

## M. Risks, dependencies, limitations, and mitigations

| Risk / dependency | Impact | Mitigation and decision owner |
|---|---|---|
| Training/live feature mismatch | Strong offline results fail on live traffic | Research lead completes compatibility matrix before training commitment; obtain compatible raw data or reduce/retrain feature set. |
| No usable network visibility or capture privileges | No valid two-system demonstration | Lab operator validates topology/driver early; prefer approved A-to-B service topology if mirroring is unavailable. |
| Leakage, duplicates, synthetic artifacts, unreliable labels | Inflated or misleading results | Research lead uses group/time splits, label audits, independent scenario truth, and documented exclusions. |
| Rare classes or dataset imbalance | Unstable recall/precision and poor generalization | Report support/intervals; use training-only imbalance treatment; restrict class claims rather than fabricate coverage. |
| Deep architectures unsuitable for feature/sequence structure | Complexity without defensible benefit | Justify representations, retain classical baselines, and select by validation rule; escalate data/scope mismatch. |
| Windows driver/tool/accelerator incompatibility | Blocked capture or training | Operator verifies a minimal end-to-end feasibility slice early; document CPU fallback and any approved environment change. |
| CPU/RAM/GPU/time constraints | Limited experiment matrix, delayed demo | Freeze bounded tuning budgets, smaller representative experiments, and optional-feature cuts; supervisor approves scope changes. |
| Flow timeout/queueing dominates latency | “Live” UI is delayed despite fast model | Measure separate latency stages; validate partial windows or disclose closed-flow delay; never claim hard real-time. |
| Encrypted/partial traffic and endpoint ambiguity | Missing evidence, incorrect attribution | Use available metadata, show unknowns, limit supported scenarios; do not promise payload-based visibility. |
| DIG checks restate model confidence or consume circular evidence | Apparent benefit lacks validity | Separate independent evidence, report feature overlap, run ablations and matched-budget comparisons. |
| Excessive abstention/false positives | Review overload masks misses | Measure queue burden and attack coverage jointly; disclose operational tradeoffs and abstain-all reference. |
| Invalid high-confidence prediction or stale approval | Unjustified response | Non-compensatory integrity/safety checks, revalidation, human approval, simulation-only release. |
| Audit/storage failure and sensitive logs | Lost accountability/privacy exposure | Bounded retention, minimization, restricted access, tamper-evidence limits, and fail-closed action auditing. |
| Too little history for forecasting or no delayed labels | Unsupported predictions/performance drift claims | Keep forecasting disabled; use descriptive trends/unlabeled shift indicators with explicit limitations. |
| Finite lab scenarios, cross-dataset unavailability, and domain shift | Results do not generalize to production or independent collection sources | Require EVAL-013 when suitable datasets exist; otherwise document why comparison is infeasible and use independent capture-group holdouts and controlled live evaluation as a non-equivalent fallback. Report population/topology/protocol boundaries and separate benchmark from live results. |
| Unauthorized testing or escaped traffic | Harm beyond project scope | Written scope, isolated network, traffic limits, stop procedure, and operator verification before every run. |

Dependencies requiring confirmation: dataset permissions and provenance, usable feature extractor/capture backend, administrator consent for capture setup, two isolated hosts/VM resources, model-training compute, local storage, approved test services/traffic generators, independent scenario labels, and supervisor availability for scope/target approval. No cloud service, paid API, GPU, or external explanation service is mandatory by default.

## N. Milestones and high-level deliverables

Calendar dates, team assignments, and effort estimates are TBD. The following is the **single proposed milestone crosswalk** for all five design/planning documents. It is a naming and evidence convention, not approval. A source-specific decision may narrow, defer, or remove a deliverable only through its existing DEC/ADR/TGT record; no milestone row silently approves scope. The listed future deliverables are **not authorized for creation by this task**.

| Milestone | Common meaning across documents | Prerequisites | Concrete deliverables | Acceptance / stop condition |
|---|---|---|---|---|
| **M0 — Governance and documentation approval** | Establish authority, roles, scope, safety boundaries, document precedence, decision owners, and the next permitted activity. No implementation or experiment authorization is implied by the milestone name. | Seven documents available for attributable human review; no installation, capture, traffic, dataset download, training, or security test is required or permitted. | M0 approval record; role/RACI and reviewer matrix; scope/stop/privacy statement; decision and blocker register; adopted proposed crosswalk; next-activity authorization note. | Owner/supervisor approves or rejects requirements, roles, scope, safety, and the next activity. Missing authority or disputed safety keeps all action tasks blocked. |
| **M1 — Feasibility, data, semantics, and evaluation protocol** | Establish whether the authorized Windows/lab path, data, labels, live feature semantics, leakage controls, common comparison population, access boundary, and measurement protocol are feasible. | M0 exit; read-only environment inventory; separate authorization for any bounded benign feasibility activity; lawful candidate metadata/data access; no final-source record access. | Topology/visibility and privilege record; candidate/rejected-source matrix; manifest/label/truth records; feature/flow/sequence contract; `P_common` proposal; split/leakage report; final-source access ledger protocol; actionable TGT-01–07 target sheet; EVAL-013 applicability record. | Evidence supports an approved scope or records a safe blocker/scope revision. No final labels, records, scores, derived outcome statistics, or results are opened before the later freeze. |
| **M2 — Reproducible development experiments and package** | Run development-only comparison and select/package a model by the frozen validation rule without consulting final-test outcomes. | M1 exit; approved development source/manifest; `P_common` and arm-specific coverage rules; training-only preprocessing; seeds/budgets; final source sealed. | Trivial/classical/CNN/LSTM/CNN-LSTM ledger; failed/infeasible-arm records; metrics/support/uncertainty/resource characterization; validation-only selection; compatible model package and inference contract. | Comparison is accounted for, leakage checks pass, and a supervisor accepts the selected model/scope or records an infeasible/reduced claim. Any final-source consultation blocks the gate. |
| **M3 — Integrated observation and investigation conformance** | Prove the live/offline semantic path from authorized observation through flow/features, inference, correlation, assets, evidence, risk, and analyst-facing incident views. | M1 and M2 exits; compatible package; foundation/auth/persistence/audit/privacy contracts; approved live scope; no unresolved feature-parity blocker. | Capture/flow/feature parity evidence; incident/correlation lineage; CENSUS snapshots; ORACLE evidence/explanation status; risk/confidence separation; HAWK health/review fixtures; live limitations. | AC-01–AC-04, AC-06–AC-07 functional evidence is complete or explicitly blocked/limited. Invalid input, missing evidence, or confidence/risk/permission conflation stops the milestone. |
| **M4 — DIG and simulation-only response conformance** | Establish functional DIG decision tables, fail-closed eligibility, WARDEN's closed simulation catalogue, human approval, revalidation, and no-real-action boundary. This milestone does not claim DIG effectiveness. | M3 conformance; approved risk/DIG/response decisions; shared-versus-arm rule matrix; authentication/session/API/audit contracts; independent simulation-boundary plan. | Four-check vector and policy version; policy-only DIG fixtures; WARDEN catalogue; immutable approval/revalidation/idempotency records; independent no-effect evidence plan; human review/simulation walkthrough. | AC-05 and AC-08 safety invariants pass or have explicit blocked dispositions. Any real-action path, unsafe eligibility, missing audit, or unbounded simulation stops the milestone. |
| **M5 — Historical, operational, and policy study** | Measure descriptive history, performance/capacity/resource/recovery behavior, and the B0–B3/ablation/failure-injection study using independent truth and frozen inputs. | M3 and M4 exits; approved TGT-01–07; retention/privacy rules; named hardware profile; authorized scenario/fault matrix; independent adjudication. | SIBYL trends; operational/overload/recovery evidence; retention/export evidence; B0–B3 ledger; ablation/fault records; workload/overhead/uncertainty analysis; integrated two-system evidence. | AC-09–AC-11 inputs are complete, denominators and boundaries are known, and negative/inconclusive results are retained. Safety, independent-truth, or loss-accounting gaps block the milestone. |
| **M6 — Final sealed evaluation, reproducibility, and human acceptance** | Freeze the final protocol before opening final-source records, run the sealed final evaluation, perform the exact reproducibility rerun, complete traceability/limitations, and obtain human disposition. | M5 exit; final-test authorization; `IMP-089` pre-access freeze; sealed source; locked `P_common`, model, preprocessing, labels, thresholds, policies, metrics, scenarios, and access ledger. | Immutable freeze/access record; final offline/cross-dataset/live results or non-equivalent fallback; exact rerun and deviations; complete traceability; limitations/unmet-target report; presentation and signed acceptance disposition. | Final claims match the locked population and evidence. A post-freeze change becomes development feedback and requires a new independent source where feasible; unmet requirements remain unmet or explicitly descoped. |

This crosswalk is the canonical meaning of M0–M6. The ML Research Plan, System Design, Technical Design, and Implementation Plan shall use these names and evidence boundaries rather than introducing a second M3/M4/M5 interpretation.

## O. Traceability and future verification approach

### O.1 Stable linkage

Use this chain: **objective/use case → requirement → design decision/data contract → test or experiment → evidence artifact → acceptance decision**. Design/test artifacts will be created only after this PRD is approved. Keep identifiers immutable; superseded requirements retain their disposition.

The future traceability register shall contain: requirement ID and version, status/priority, objective/use case, owning module, proposed design reference, test/experiment IDs, fixture/data/model/policy versions, expected result or target, actual result, evidence location/fingerprint, reviewer, date, and pass/fail/blocked/not-applicable with reason. Missing evidence is blocked, not passed. Validation-dependent items require a documented applicability decision.

### O.2 Initial coverage map

| Area | Requirement families | Future verification |
|---|---|---|
| Visibility and feature integrity | FR-001–006; DEM-001–005; NFR-007, 013 | Capture integration fixtures, deterministic flow tests, schema contracts, two-host visibility and clock tests. |
| Model quality and provenance | FR-007–012, 038–039; EVAL-001–009, 011–013; NFR-001–004, 006 | Split/leakage audit, baseline/model experiments, parity fixtures, artifact activation/rollback, untouched independent cross-dataset test when applicable, fallback justification when not, degradation/shift analysis, and timing/load runs. |
| Asset-aware investigation | FR-013–019, 023–024 | Identity ambiguity fixtures, criticality/risk boundaries, evidence/summary provenance review, explanation and mapping validation. |
| Decision integrity | FR-020–022; EVAL-010; SEC-008–010 | Separate functional DIG check-recording tests from research-policy comparisons; predeclared operating-point tests, fixed-policy comparisons, matched-budget/coverage analysis where feasible, check-family ablations, injected-failure and stale-approval tests. |
| Analyst workflow and history | FR-025–028, 032–035; NFR-009–014 | UI task tests, statistics/filter fixtures, correlation boundaries, lifecycle/audit and historical backtesting gates. |
| Safe response and platform security | FR-029–031, 036–037, 040; SEC-001–012; NFR-005, 008, 012 | Role-denial and hostile-input tests, replay/idempotency fixtures, no-real-action verification, audit failure/recovery, retention/export checks. |
| Integrated outcomes | OBJ-01–07; DEM-006–009; EVAL-013; AC-01–11 | Authorized scenario matrix, untouched final-test isolation evidence, cross-dataset evaluation or documented non-equivalent fallback, signed acceptance review, complete metrics and unresolved-gap report. |

This family map is a starting index, not a substitute for individual requirement coverage. Every FR, NFR, SEC, DEM, and EVAL identifier requires an explicit row before release acceptance. Every must-have must have at least one positive and applicable negative test; research obligations need experiment evidence rather than fabricated pass/fail improvement claims.

### O.3 Proposed test naming and evidence rules

- Use identifiers such as `TEST-FR-003-01` (contract), `TEST-SEC-009-01` (negative authorization), `EXP-EVAL-010-B3` (policy study), and `DEMO-DEM-006-01` (live walkthrough).
- Retain fixture manifest, expected/actual output, command/configuration used, environment, seed, measurement boundaries, and timestamps. Redact private data without destroying necessary linkage.
- A failed regression blocks the linked acceptance criterion until fixed/retested or explicitly descoped. No screenshot alone proves classifier quality, event completeness, or response safety.
- The final coverage report shall enumerate passed, failed, blocked, and not-applicable requirements and disclose unmet numeric targets. There are **no executed tests or measured project results at PRD stage**.

## P. Unresolved decisions, assumptions, and approval gate

| Decision ID | Open question / proposed assumption | Owner and latest resolution gate |
|---|---|---|
| DEC-01 | Who approves requirements, lab scope, targets, and final acceptance? Proposed: supervisor/project owner, with student team preparing evidence. | Project owner; M0 |
| DEC-02 | What Windows version/hardware, VM platform, capture backend/driver, and topology are available? Proposed simplest demo is A-to-B service traffic. | Lab operator; M1 |
| DEC-03 | Which dataset(s), binary/multiclass label mapping, and authorized attack scenarios have compatible live features and sufficient labels? | Research lead/supervisor; M1 |
| DEC-04 | What flow keys, timeouts, partial-window policy, sequence semantics, and feature schema will be used? | Technical/research lead; M1 before model commitment |
| DEC-05 | What values and justifications fill TGT-01–07, including missing-class handling and acceptable measurement uncertainty? | Team/supervisor; M1, before held-out testing |
| DEC-13 | After completing the required provenance and compatibility assessment using the permitted Access Class A/B checks, are at least two suitable, sufficiently compatible independent datasets available under EVAL-013, and if not, what documented justification and strongest non-equivalent fallback will be used? Which source/direction is Access Class D and how will its immutable ledger be sealed before record-level access? | Research lead/supervisor; M1 applicability, `IMP-089` before final record access |
| DEC-14 | Which `P_common` prediction-anchor population, sequence warm-up/padding/masking, overlap purge, exclusion, denominator, operating-point, matched alert-review budget or matched-coverage comparison, uncertainty, and fair tuning rules apply to the model and B0–B3 studies? | Research lead/supervisor; M1 protocol, before final evaluation |
| DEC-06 | Which severity/criticality/evidence rubric, weights, unknown defaults, review rules, and risk bands are approved? | Analyst role/supervisor; before M3 implementation |
| DEC-07 | Which DIG checks are observable and independently testable; what score calibration/shift indicators and policy precedence are feasible? | Research lead; protocol frozen before M4 study |
| DEC-08 | Which explanation method and reference data are suitable, and which ATT&CK mappings, if any, have sufficient evidence? | Research lead/analyst; M3 |
| DEC-09 | Which curated asset fields, identity-resolution rules, role assignments, retention periods, permitted capture fields, and non-loopback access policy apply? | Lab owner/operator; M1, before data collection |
| DEC-10 | Which simulation-only recommendation catalogue, approval expiry, and independent response-adjudication rubric apply? | Lab owner/analyst/supervisor; before M4 |
| DEC-11 | What history supports SIBYL, and is forecasting worth validating within the project timeline? Default: descriptive trends only. | Research lead/supervisor; M5 |
| DEC-12 | What calendar, team capacity, training budget, and optional-feature cuts are approved? | Team/supervisor; M0/M1 |

Assumptions requiring confirmation: the team can obtain two isolated execution environments and capture permissions; a usable supervised dataset and bounded authorized scenarios exist; curated inventory is acceptable; local-only operation without external APIs is acceptable; analyst approval and label adjudication can be performed by designated people. If any assumption fails, revise scope and acceptance criteria before implementation, rather than hiding the limitation.

### Approval checklist

- [ ] Review confirmed requirements, proposed behaviors, and material status reclassifications; approve/revise them.
- [ ] Confirm must-have versus validation-dependent/optional boundaries.
- [ ] Assign owners and resolution gates for DEC-01–14 and TGT-01–07.
- [ ] Approve authorized-lab and simulation-only safety constraints.
- [ ] Approve the evaluation/traceability approach, including negative outcomes.
- [ ] Explicitly authorize the next documentation step; implementation remains unstarted.

**End of PRD. Stop after this document and wait for review and approval.**
