# WallGrid AI — ML Research Plan

**Status:** Proposed research protocol for human review; documentation only  
**Requirements source:** `docs/PRD.md` (version 0.1, 2026-10-11)  
**Scope:** ARGUS detection research, live/offline evaluation, and the DIG policy study  

> **Important status statement.** No training, calibration, dataset evaluation, live capture, failure injection, or performance measurement has been performed. This document contains no results. Dataset names, labels, targets, architectures, hyperparameters, operating points, DIG parameters, and deployment choices remain unresolved unless explicitly approved in the PRD or by the project owner. This plan does not authorize implementation, security testing, traffic generation, or creation of datasets.

## 1. Purpose, boundaries, and decision rules

This plan turns the PRD into a reproducible research protocol without silently converting proposed behavior into an approved design. It covers binary detection first, supported multiclass detection only if the label and feature evidence support it, and the four-policy Decision Integrity Gate (DIG) comparison. It does not claim production readiness, universal attack coverage, zero-day detection, prevention, hard real-time operation, or novelty.

The governing rule is **freeze before final test**: dataset manifests, label mapping, feature schema, split groups, preprocessing, common comparison population, model-selection rule, calibration, thresholds, DIG parameters, measurement boundaries, and the final-source access ledger must be approved and versioned before any outcome-bearing final-test record is opened. Public metadata and source-level integrity/independence checks may be inspected under the access classes in PRD J.3.1; labels, features, rows/windows, row-derived statistics, scores, and results from a source designated as final are prohibited until `IMP-089` and final-test authorization are recorded. Any change after final-test access creates a new development result and requires a new independent test source where feasible (PRD EVAL-004, EVAL-006, J.3, J.3.1).

### 1.1 Research objectives

| Objective | Research question | Evidence required |
|---|---|---|
| ROBJ-1 | Can a live-compatible feature contract support honest offline and controlled live evaluation? | Dataset/feature compatibility matrix, schema tests, provenance, capture visibility evidence, and explicit unsupported fields. |
| ROBJ-2 | How do a trivial predictor, ARGUS classical baseline, CNN, LSTM, and CNN-LSTM compare under leakage-resistant splits? | Complete experiment records, per-class metrics, uncertainty, latency, throughput, resources, and failed-run reasons. |
| ROBJ-3 | Does a selected model generalize across independent collection sources when EVAL-013 is applicable? | Pre-test compatibility decision and untouched cross-dataset final evaluation, or a documented non-equivalent fallback. |
| ROBJ-4 | Does DIG change response-eligibility quality under controlled conditions? | B0/B1/B2/B3 comparison, independent adjudication, ablations, fault injections, workload, and overhead. |
| ROBJ-5 | Can conclusions be reproduced and bounded? | Immutable manifests, seeds, environment records, experiment ledger, uncertainty analysis, and limitations report. |

### 1.2 Hypotheses (not claims)

- **H-ML-01:** A model representation that matches the actual temporal or tabular semantics may outperform a mismatched representation under the same development budget. This is an empirical hypothesis, not a promise that a deep model wins.
- **H-ML-02:** Leakage-resistant group/time splits will produce more conservative and more decision-relevant estimates than random row splitting when related flows or capture artifacts exist.
- **H-ML-03:** A validated abstention/review mechanism can trade coverage for selective error and review workload; its operating point must be selected on development data.
- **H-DIG-01 (from PRD):** Independent integrity, reliability, evidence-consistency, and response-eligibility checks may reduce unjustified response decisions at a measurable cost in abstention, review, latency, or resources. Negative or inconclusive findings are valid.

No hypothesis is a novelty claim. Each is falsifiable, and every comparison must report harms as well as benefits.

## 2. Literature review and evidence ledger

The review will be maintained as a citation ledger with retrieval date, version, exact title/authors, venue or issuing body, DOI/official URL, claims used, and relevance to a WallGrid decision. A source is not evidence that WallGrid works. The following recent sources were checked against their official publisher or issuing-body pages and are appropriate starting points; the investigator must extend the search before protocol freeze, particularly for current network-intrusion datasets and CNN/LSTM comparisons.

### 2.1 Verifiable 2023–2026 sources

1. **NIST, _Artificial Intelligence Risk Management Framework (AI RMF 1.0)_, NIST AI 100-1 (2023).** Official publication page: <https://www.nist.gov/itl/ai-risk-management-framework>. Relevance: risk management, validity/reliability, transparency, and monitoring; informs separation of model score, risk, and permission and the uncertainty/limitations sections.
2. **NIST, _Adversarial Machine Learning: A Taxonomy and Terminology of Attacks and Mitigations_, NIST AI 100-2 (2023).** DOI: <https://doi.org/10.6028/NIST.AI.100-2>. Relevance: threat/shift/failure vocabulary and the requirement not to infer robustness from ordinary benchmark scores. It does not establish that any proposed DIG check detects adversarial manipulation.
3. **NIST, _Artificial Intelligence Risk Management Framework: Generative Artificial Intelligence Profile_, NIST AI 600-1 (2024).** Official publication page: <https://www.nist.gov/itl/ai-risk-management-framework/ai-rmf-generative-ai-profile>. Relevance: supplemental governance language for uncertainty, documentation, human oversight, and incident handling; most WallGrid results still require task-specific evidence.
4. **NIST, _Computer Security Incident Handling Guide_, SP 800-61 Revision 3 (2025).** DOI: <https://doi.org/10.6028/NIST.SP.800-61r3>. Relevance: incident lifecycle and human-led response context; it is not a detector benchmark and is not used to set ML targets.
5. **Angelopoulos, A. N., and Bates, S., “Conformal Prediction: A Gentle Introduction,” _Foundations and Trends in Machine Learning_, 2023.** Official author/publisher landing page: <https://people.eecs.berkeley.edu/~angelopoulos/publications/>. Relevance: motivates coverage/abstention and distribution-free uncertainty reporting; any conformal method, exchangeability assumption, and finite-sample guarantee must be stated and tested rather than assumed for shifted network data.
6. **Gawlikowski et al., “A Survey of Uncertainty in Deep Learning for Neural Networks,” _Artificial Intelligence Review_ (2023).** DOI landing page: <https://doi.org/10.1007/s10462-023-10503-1>. Relevance: distinguishes uncertainty sources and motivates reporting calibration/abstention limitations. Verify the final bibliographic record and use only claims supported by the paper.

The review must separately search IEEE Xplore, ACM Digital Library, SpringerLink, USENIX, arXiv, and official dataset repositories for 2023–2026 work on network intrusion detection, flow feature semantics, cross-dataset evaluation, calibration/selective prediction, temporal deep learning, and operational false-alert measurement. Search strings and exclusions will be recorded. Older seminal sources may be cited for methods, but they do not satisfy the recent-source requirement by themselves. No citation above is evidence of a particular dataset property, model superiority, or WallGrid target.

### 2.2 Established literature versus proposed methodology

**Established literature/practice:** calibration is distinct from ranking/classification; selective prediction reports coverage with conditional error; group/time separation is necessary when observations are dependent; precision depends on prevalence; resource and latency boundaries affect operational interpretation; independent adjudication is needed for response correctness; NIST AI RMF and SP 800-61r3 provide governance/incident-handling context.

**Proposed WallGrid methodology:** the candidate comparison matrix; the exact split hierarchy; feature-compatibility scoring; the representation decision tree; the training-only preprocessing rules; the B0–B3 policy experiment; the fault-injection catalogue; the report template; and any operating point or target sheet. These require approval and may be rejected after feasibility review.

**Assumptions to verify:** usable labels exist; independent scenario truth can be recorded; flow/window semantics can be reconstructed live; Windows capture is feasible; at least one suitable supervised source exists; two sufficiently compatible independent datasets may or may not exist; and compute/time permit the approved matrix. None is currently established.

## 3. Dataset-selection protocol (no dataset selected)

Dataset selection is a decision, not a convenience. The research lead will create one immutable candidate record per source and retain rejected candidates with reasons. No source is selected in this document.

### 3.1 Selection criteria and gates

1. **Rights and provenance:** license, access restrictions, collection owner, collection date/conditions, permitted processing, and independent origin.
2. **Observation semantics:** raw packets, bidirectional/unidirectional flows, windowing, timestamps, capture topology, protocols, encryption, drops, and whether live extraction can reproduce the fields.
3. **Labels:** label authority, scenario/event granularity, adjudication, ambiguous/unknown labels, attack taxonomy, benign definition, and label leakage risks.
4. **Support and diversity:** class counts, hosts, sessions, capture groups, temporal span, topology, protocol and application diversity, missingness, duplicates, and prevalence. No numeric minimum is invented; TGT-01/TGT-07 remain TBD.
5. **Feature compatibility:** name, type, unit, direction, aggregation, required context, availability during inference, and schema version for every candidate feature.
6. **Leakage risk:** scenario markers, attack names, post-event fields, duplicated windows, host identifiers, collection-specific artifacts, augmentation, and shared capture groups.
7. **Reproducibility:** stable download/version, checksums, documentation, scripts or unambiguous reconstruction, and retention of exclusions.

### 3.2 Candidate comparison matrix

| Criterion | Candidate A | Candidate B | Candidate C | Decision evidence / unresolved question |
|---|---|---|---|---|
| License/access and provenance | TBD | TBD | TBD | Obtain written/official terms and source lineage. |
| Raw packet / flow availability | TBD | TBD | TBD | Can the live extractor reproduce the same semantic unit? |
| Collection topology and visibility | TBD | TBD | TBD | What did the sensor observe, and what was invisible? |
| Labels and adjudication | TBD | TBD | TBD | Are labels independently grounded, ambiguous, or inferred? |
| Timestamps and group keys | TBD | TBD | TBD | Are session/capture/host/time boundaries available? |
| Feature definitions and units | TBD | TBD | TBD | Build field-level compatibility matrix; no silent constants. |
| Class support and prevalence | TBD | TBD | TBD | Quantify after manifest inspection; do not promise multiclass. |
| Missingness, duplicates, and artifacts | TBD | TBD | TBD | Measure and report denominator impact. |
| Leakage exposure | TBD | TBD | TBD | Document markers, overlapping windows, and related captures. |
| Independent-source suitability | TBD | TBD | TBD | Required for EVAL-013 applicability decision. |
| Reproducibility and compute cost | TBD | TBD | TBD | Record download/version and feasible processing budget. |
| Provisional disposition | Not selected | Not selected | Not selected | Supervisor/research-lead approval required at M1. |

### 3.3 Label mapping and feature compatibility

Create a versioned mapping table before training:

| Source label/value | WallGrid binary label | WallGrid supported class | Evidence/definition | Ambiguity/action |
|---|---|---|---|---|
| TBD | normal / attack / unknown | TBD | TBD | Never map unknown to normal silently. |

Mapping rules must distinguish normal, supported attack, unsupported attack, unknown, invalid input, and ambiguous/adjudication-required. Binary and multiclass tasks are evaluated separately. A label may be excluded from a supported-class claim while remaining in accounting. Report all exclusions and denominators.

The compatibility matrix will have one row per field: source name, live name, type, unit, direction, timing/aggregation, missing-value policy, availability point, leakage risk, transformation, and disposition (`retain`, `reconstruct`, `remove from both`, or `scope decision`). Training a field that live inference cannot produce is prohibited. Dataset row order is not a temporal axis; tabular column order is not CNN spatial structure.

## 4. Leakage-resistant evaluation design

### 4.1 Split hierarchy

The preferred hierarchy is: **final independent source/capture group → development source(s) → training and validation groups**. Groups are chosen from the data-generating process: capture/session, host or endpoint where appropriate, scenario/run, and chronological period. The precise hierarchy remains data-dependent and must be frozen after inspection.

- No related flows, overlapping windows, duplicates, augmented derivatives, scenario markers, or shared capture artifacts may cross partitions.
- Temporal order is preserved for time-dependent claims; future packets/post-incident fields cannot enter an earlier decision.
- Validation is used for model selection, representation, threshold, calibration, DIG tuning, and compute trade-offs. Training-fitted transforms remain training-only; any separately approved development refit boundary must be recorded. Repeated validation use is logged.
- The final test source is untouched until the final locked run. If it reveals a design change, the result becomes development feedback and a new independent source is required where feasible.
- Random row splitting is not a default and requires a documented dependence audit and approval.

### 4.2 EVAL-013 cross-dataset generalization

Before deciding applicability, record provenance, collection conditions, declared labels/features, topology, time, supported classes, rights, and semantic compatibility for at least two candidate independent sources using public metadata or the permitted source-level integrity/independence checks in §4.3. “Independent” means separately collected or a separate collection source, not a duplicate, transformed copy, or partition of the same capture. If record-level labels/features or row-derived statistics are needed to decide applicability, the source cannot simultaneously be treated as an untouched final source in that evaluation version; record the designation change in the access ledger and use a new sealed source where feasible.

If suitable sources exist, train/develop on source A, complete `IMP-089`, freeze all choices and the access ledger, and test once on untouched source B; reverse direction only as a separately predeclared study with a separately sealed target. Report per-class precision/recall, macro-F1, false-positive rate, support, coverage/abstention, latency/resources, unsupported classes, label mapping, degradation and shift. If not comparable, document why and use independent capture-group holdouts plus controlled live-lab evaluation as the strongest feasible fallback, explicitly stating it is **not equivalent** to cross-dataset validation.

### 4.3 Final-source access classes and ledger

The research ledger shall use the following proposed access classes, aligned with PRD J.3.1:

- **A — public metadata/provenance:** official license, source lineage, declared schema/labels/features, collection conditions, and published documentation. No records or outcome-bearing derivatives.
- **B — source-level integrity/independence:** whole-source checksums, archive/file metadata, published manifests, source-level partition identifiers, and cryptographic/source-lineage overlap checks. No row/window/flow values, labels, class distributions, row-level duplicate counts, scores, or results.
- **C — approved development data:** record-level fields and derived statistics from a source explicitly designated for training/development/validation after M0/data authorization. It cannot later be presented as an untouched final source for the same evaluation version.
- **D — sealed final source:** no record-level or outcome-bearing access until `IMP-089` is approved and final-test authorization is recorded. The one-way opening event, locked configuration fingerprint, tools, actor, requested fields, and all derived artifacts are written to the immutable access ledger before opening.

The access ledger records attempted and successful access, source designation, class, purpose, actor/service, authorization/decision ID, time basis, fields/operations, input/output fingerprints, and reviewer. A missing or ambiguous ledger entry blocks final-test acceptance. Exact source identity, direction, label compatibility, and whether a second source is suitable remain unresolved until candidate review; this protocol does not select a dataset.

## 5. Representations and model families

### 5.1 Required comparison

All feasible arms use the same approved task, split, label mapping, metric protocol, **`P_common` prediction-anchor population**, and bounded tuning budget. The matrix includes: trivial reference predictor; appropriate classical supervised baseline (ARGUS baseline); CNN; LSTM; CNN-LSTM. A failed or infeasible arm remains in the ledger with cause, resources, and implication.

### 5.1.1 Common comparison population and arm coverage

`P_common` is the same set of eligible flow/window prediction anchors for every direct model comparison. It is not selected by choosing the easiest sequence subset after seeing results. The dataset-specific instantiation remains pending, but the protocol shall retain these fields:

| Population rule | Required protocol |
|---|---|
| Prediction unit | One approved flow or event-time window anchor; packet, flow/window, incident, scenario, and response denominators remain separate. |
| Label assignment | One versioned label map and independent truth/adjudication result per anchor; normal, supported attack, unsupported, unknown, invalid, and ambiguous states remain distinguishable. |
| Feature eligibility | Required live-compatible fields present or validated as reconstructable; invalid, quarantined, semantically incompatible, or missing-required anchors are excluded with reason. Training-fitted transforms are never refit on validation/final data. |
| Group and overlap purge | Remove duplicates, overlapping windows, augmented derivatives, shared markers, related flows, shared capture artifacts, and any dependent group before partitioning and before `P_common` construction. |
| Temporal sequence | LSTM/CNN-LSTM use the same anchor with ordered event-time context. Warm-up observations are not scored; insufficient context is `sequence_ineligible`, excluded from `P_common`, and counted separately. Future observations are prohibited. |
| Padding/masking | Padding and masks are fixed before the final protocol; masks and incomplete-boundary reasons are recorded. No padding value may carry labels or future information. Applicability remains data-dependent. |
| Denominators | Direct quality/coverage/abstention metrics use `P_common` for every arm. Invalid-input rejection, sequence ineligibility, timeout, unsupported labels, and missing required features are separate counts. |
| Operational coverage | Each arm may report its broader valid population and live/operational coverage separately. These results are not direct architecture comparisons and cannot replace matched-population metrics. |

The `P_common` manifest contains stable or pseudonymous anchor references, split/group membership, label status, feature/sequence eligibility, warm-up and mask status, exclusion reason, and denominator effect. A change after final-source access is development feedback, not an updated final comparison. If no valid temporal sequence exists, LSTM/CNN-LSTM are reported as unsupported/not applicable rather than evaluated on fabricated sequences.

### 5.2 Representation decision

1. **Flow-tabular representation:** one `P_common`-eligible flow/window per example with an explicit ordered feature schema. Use for the classical baseline and, if approved, a 1-D CNN over feature channels. A CNN is not justified merely by reshaping columns into an image.
2. **Temporal sequence representation:** ordered observations from the same flow/session/endpoint under a declared event-time rule, with padding/masking, warm-up, overlap, and boundary behavior. Use for LSTM and CNN-LSTM only when timestamps and repeated observations support a temporal claim.
3. **Packet/short-window representation:** only if raw/metadata observations and live extraction are both authorized and reproducible; define direction, truncation, loss, and variable length. No payload or credential collection is assumed.
4. **No-valid-sequence outcome:** if data cannot support sequence semantics, report the LSTM/CNN-LSTM limitation and seek a scope/data decision rather than fabricate temporal structure.

Input shape, normalization, padding/masking, ordering, and partial-flow inference are recorded as hypotheses. Long-lived flows may require active windows or an explicit detection-delay limitation. All representations must preserve live/offline semantic compatibility.

### 5.3 Training-only preprocessing and imbalance

Fit imputation, scaling, encoding, feature selection, resampling, augmentation, and any learned representation on training data only. Apply frozen transforms to validation, final test, and live data. Calibration and threshold selection are also training/development operations, never final-test operations.

Candidate imbalance methods are class-weighted loss, training-only oversampling, focal or other approved loss, and no correction as a reference. The method, random seed, sampling unit, and effect on effective prevalence are recorded. Validation/test are never oversampled. Report per-class support and sensitivity to rare/absent classes; do not report a misleading perfect score for absent support.

### 5.4 Thresholds and calibration

Select thresholds on a designated development/validation subset using a predeclared rule balancing quality, false alarms, coverage, review burden, and live compatibility. Keep fixed operating-point results plus optional threshold/PR/coverage curves. A score is not called a probability until calibration is evaluated.

Where probability interpretation is supportable, use a designated calibration subset or cross-fitting within development data; report reliability plots, Brier score, defined-bin ECE, bin counts, and class-imbalance limitations. Calibration, thresholds, and DIG operating points are frozen before final testing. Report abstention reason and original score; invalid-input rejection is separate from valid uncertainty abstention.

## 6. Metrics and measurement boundaries

The unit is declared for every table: packet, flow/window, prediction, correlated incident, scenario, or response proposal. Denominators are never mixed.

- Per supported class: support, precision, recall, F1; confusion matrix; macro-F1 as the unweighted mean over predeclared evaluable classes; undefined values are marked not estimable.
- Binary false-positive rate: benign flows classified as attack / labeled benign flows; report benign review routing separately and benign scenarios with at least one false incident.
- Operational false alerts: false incidents per observation hour and per 1,000 benign flows, before/after correlation, with prevalence and correlation rule.
- Coverage/abstention: non-abstained valid predictions / all eligible valid inputs; show coverage-versus-error, class-wise abstention, invalid rejection, and covered-subset error.
- Attack handling: detected, reviewed, invalid, missed, and timed out; do not equate review with correct classification.
- Response quality: unjustified eligible proposals, unjustified approvals/simulations, justified eligible proposals, justified proposals blocked/deferred, and insufficient-evidence counts using independently adjudicated labels.
- Latency: inference p50/p95/p99 and maximum; feature-ready-to-visible and observable-onset-to-visible with queueing, persistence, transport, and dashboard boundaries; clock offsets and uncertainty.
- Throughput/capacity: offered, captured, processed packets/flows per second, eligible predictions/s, incidents/s, drops, rejections, backlog, and burst/sustained duration.
- Resources: CPU method/aggregation, RAM, storage growth/I/O, GPU utilization/memory if applicable, hardware/software profile, run length, and competing workload.
- Review workload: events/incidents per hour/1,000 inputs, queue age/backlog, repeat reviews, and reviewer minutes if feasible; counts are workload proxies.

Targets, minimum support, latency budgets, capacity, resource limits, coverage/review limits, retention, and study adequacy are **TGT-01–07 TBD**. The actionable metric/boundary, population/workload, selection, uncertainty, owner/gate, and pass/fail/inconclusive evidence contract is defined in PRD §B.2; no number is invented here. Safety limits apply before empirical target approval.

## 7. DIG study: B0/B1/B2/B3

Use one frozen detector, feature pipeline, split, correlation rule, action catalogue, and independently adjudicated candidate universe. Shared parser validation, authorization, and simulation-only safeguards remain active in every arm; ablations of safety logic are hypothetical eligibility analyses only.

| Arm | Policy | Required comparison outputs |
|---|---|---|
| B0 | Validation-selected fixed detector threshold plus fixed class-to-recommendation rules | Detection/alert output, would-propose and would-mark-eligible decisions. |
| B1 | B0 plus a distinct validated reliability/confidence defer-to-review rule | Coverage, selective errors, review, eligibility, threshold distinction. |
| B2 | Detector plus versioned severity/asset/evidence risk prioritization | Ranked queue, risk factors, review, eligibility; no full four-family DIG. |
| B3 | Same detector/context plus separated integrity, reliability, evidence-consistency, and response-eligibility checks with non-compensatory precedence | Individual outcomes/reasons, aggregate state, withheld/deferred recommendations, overhead. |

Tune each arm only on permitted development data with fair, recorded search budgets. Report natural operating points and, where feasible, matched alert-review-budget or matched-coverage comparisons. Include abstain-all/no-response as a reference; zero unjustified actions alone is not useful performance.

Required applicable ablations: B3 without each check family; without asset context; without independent corroboration; and any claimed uncertainty/shift check. Required injections include schema/feature/unit errors, corrupt artifacts, packet loss/partial/reversed flows, duplicates/late arrivals, shifted or unseen permitted classes, ambiguous scores, conflicting evidence/stale assets, high-confidence safety failures, stale/replayed approvals, audit-write failure, overload, storage/model/UI outages. For each, record fault precondition, independently known fault, expected behavior, actual behavior, observability limitation, clean control, and combinations.

Independent ground truth is scenario recording plus documented adjudication, blinded to arm where feasible. Labels are justified/unjustified/insufficient evidence; disagreements and supervisor resolution are retained. Prediction, confidence, risk, DIG output, or analyst belief alone cannot define truth.

## 8. Proposed experiment matrix

| ID | Question/arm | Data/split | Main factors | Outputs | PRD links |
|---|---|---|---|---|---|
| EXP-BASE-01 | Trivial predictor | Development `P_common`; final `P_common` only after `IMP-089`/authorization; broader reference coverage separate | Prevalence/reference rule | Per-class metrics, FPR, coverage, exclusion counts | EVAL-005, EVAL-007 |
| EXP-BASE-02 | Classical ARGUS baseline | Same locked `P_common`; broader operational coverage separate | Bounded tuning, seed set | Quality, calibration, latency/resources, exclusions | EVAL-005–006 |
| EXP-DL-01 | CNN | Same locked `P_common`; approved flow layout only | Representation rationale and same budget | Matched quality/coverage plus operational coverage | EVAL-005–007 |
| EXP-DL-02 | LSTM | Same locked `P_common` anchors when sequence context is valid; sequence-ineligible anchors reported separately | Ordering/window/mask/warm-up rules | Matched metrics, sequence feasibility, exclusion counts | EVAL-005, DEM-004–005 |
| EXP-DL-03 | CNN-LSTM | Same locked `P_common` anchors and sequence eligibility rule as EXP-DL-02 | Same budget as EXP-DL-02 | Matched metrics; failed-run reason if infeasible | EVAL-005–006 |
| EXP-GEN-01 | Cross-dataset direction A→B | Untouched independent source | Frozen model/preprocessing | Degradation, shift, unsupported classes | EVAL-013 |
| EXP-GEN-02 | Cross-dataset direction B→A, if predeclared | Untouched independent source | Same protocol | Same; otherwise applicability record | EVAL-013 |
| EXP-CAL-01 | Threshold/calibration | Development only then locked final | Calibration method/binning | Brier/ECE/reliability, threshold | EVAL-004, EVAL-011 |
| EXP-DIG-01 | B0/B1/B2/B3 | Same frozen detector/scenarios | Policy parameters | Detection, response, coverage, workload, overhead | EVAL-010, K.4 |
| EXP-DIG-02 | DIG ablations | Same | One factor removed at a time | Incremental value/harm | EVAL-010, K.5 |
| EXP-FAULT-01 | Failure injections | Controlled valid/invalid fixtures | Fault type × clean control | Safety invariant, detectability, recovery | EVAL-010, SEC-008–010 |
| EXP-LIVE-01 | Offline/live parity | Authorized two-system scenarios | Feature/flow/topology | Visibility, parity, live metrics | EVAL-008, DEM-001–007 |
| EXP-PERF-01 | Capacity/latency/resources | Named approved Windows profile | Idle/nominal/overload, burst/sustain | p50/p95/p99, throughput, drops, CPU/RAM/storage/GPU | NFR-002–004, FR-039 |
| EXP-REPRO-01 | Re-run reproducibility | Manifest/frozen config | Seeds/environment | Tolerance, deviations, fingerprints | EVAL-002–004, NFR-006 |

The matrix is proposed, not evidence that every arm is feasible. The final report marks each row complete, failed, blocked, or not applicable with reason.

## 9. Reproducibility, uncertainty, compute, and tracking

Each run receives an immutable ID and records: dataset manifest/fingerprint, inclusion/exclusion counts, split/group manifest, label/schema versions, preprocessing artifact, model architecture/configuration, seeds, code/environment/dependency/driver versions, hardware, command/config, start/end times, checkpoints, stopping rule, metrics, latency boundaries, resource samples, failures, and reviewer. The ledger must not contain secrets or unrestricted payloads.

Uncertainty is estimated at the independent capture/scenario/group level where feasible, not by pretending correlated flows are independent. Predeclare bootstrap or other interval method, randomization, confidence level, aggregation unit, missing/ambiguous handling, and multiplicity/exploratory status. Report seed variability and scenario variability separately when possible. Do not use final-test uncertainty to tune the system.

Compute constraints remain TBD. Before M1, record Windows hardware/OS, CPU/RAM/GPU availability, storage, driver/capture constraints, run-time budget, maximum tuning runs, batch/sequence limits, and optional-feature cuts. Use bounded budgets shared fairly across model families; report wall-clock training time, inference cost, memory, and failed/out-of-memory runs. No cloud service, paid API, GPU, or external explanation service is assumed mandatory.

## 10. Risks, milestones, and gates

| Gate | Common milestone meaning and required decision/evidence | Stop condition |
|---|---|---|
| M0 — Governance and documentation approval | Attributable authority, roles, scope, safety boundaries, document precedence, decision register, and exactly one permitted next activity. No implementation or experiment authorization is implied. | No approved scope/authority, disputed safety boundary, or unresolved role ownership. |
| M1 — Feasibility, data, semantics, and evaluation protocol | Windows/lab feasibility; candidate rights/provenance; labels/truth; live feature and flow/sequence semantics; `P_common`; leakage/split plan; source-access classes/ledger; EVAL-013 applicability; actionable TGT-01–07 definitions and approvals. | No lawful/live-compatible source, reliable visibility, leakage-resistant split, common population, or approved measurement plan. |
| M2 — Reproducible development experiments and package | Development-only trivial/classical/CNN/LSTM/CNN-LSTM ledger, failed-run reasons, training-only preprocessing, validation-only selection, uncertainty/cost characterization, and compatible package/inference contract. | Final-source consultation, leakage unresolved, unsupported temporal semantics without disposition, or resources require scope revision. |
| M3 — Integrated observation and investigation conformance | Capture/flow/features/live parity, correlation, CENSUS, ORACLE, risk/confidence separation, HAWK health/review, and lineage evidence. | Invalid input treated as benign, missing evidence invented, or live/offline contract not reconciled. |
| M4 — DIG and simulation-only response conformance | DIG decision tables and policy-only fixtures; B0–B3 inputs frozen; WARDEN closed catalogue; authentication, approval, revalidation, idempotency, audit, and independent no-effect boundary. No DIG improvement is promised. | Unsafe eligibility/simulation, missing audit, real-action path, or policy layers not separable. |
| M5 — Historical, operational, and policy study | Descriptive history; performance/capacity/resource/recovery; retention/export; B0–B3, ablations, injections, independent adjudication, workload/overhead, and integrated two-system evidence. | Missing denominators/boundaries, circular truth, unbounded loss, or unfair comparison. |
| M6 — Final sealed evaluation, reproducibility, and human acceptance | `IMP-089` pre-access freeze; sealed final/cross-dataset/live evaluation; exact rerun; traceability; limitations/unmet targets; and human disposition. | Any final access before freeze, claim beyond evidence, or unresolved safety/independent-truth boundary. |

Primary risks are dataset/license failure, label ambiguity, leakage, live mismatch, topology/capture failure, sequence unsuitability, rare classes, compute/toolchain limits, domain shift, calibration failure, circular DIG evidence, excessive abstention/review burden, and unauthorized traffic. Mitigations follow PRD Sections M and P; no risk is resolved by inventing a dataset, target, or result.

## 11. Results-report template

```text
WallGrid AI ML Evaluation Report
Protocol/version and approval record:
Executed? (yes/no):
Statement of scope and safety authorization:

1. Executive summary (results only; no unsupported claims)
2. Requirements and traceability coverage
3. Dataset manifests, provenance, license, exclusions, duplicates, missingness
4. Label mapping, ambiguity, independent ground truth, adjudication
5. Feature/live compatibility and flow/sequence semantics
6. Split construction, leakage audit, and untouched-final-test evidence
7. EVAL-013 applicability decision and cross-dataset results/fallback limitation
8. Preprocessing, imbalance, model configurations, seeds, tuning budgets
9. Baseline/CNN/LSTM/CNN-LSTM results and failed runs
10. Thresholds, calibration, abstention, reliability plots and limitations
11. Metrics: support, per-class P/R/F1, macro-F1, FPR, coverage, errors
12. Latency, throughput, drops, backlog, CPU/RAM/storage/GPU, boundaries
13. DIG B0/B1/B2/B3, matched comparisons, ablations, injections
14. Independent response adjudication and review workload
15. Uncertainty intervals, seed/scenario variability, sensitivity analyses
16. Reproducibility ledger, environment, fingerprints, deviations
17. Risks, unsupported tasks, unmet TGTs, negative/inconclusive findings
18. Conclusions restricted to the approved population and evidence
19. Human review decisions and next actions

Appendices: run ledger, manifests, confusion matrices, calibration tables,
fault records, raw metric definitions, resource samples, and traceability register.
```

## 12. Traceability table

Every major experiment must have a future evidence artifact and a disposition; this table is a planning map, not a completed verification record.

| Experiment / activity | Major requirement IDs | Evidence artifact to retain | Acceptance boundary |
|---|---|---|---|
| Dataset candidate assessment and manifest | EVAL-001–002, EVAL-013, SEC-011 | Candidate matrix, license/provenance record, manifest/fingerprint, rejected-source log | No dataset selected without review; independent-source decision recorded. |
| Label and feature compatibility | FR-003–005, DEM-004–005, EVAL-001–004, EVAL-012 | Field matrix, mapping version, exclusions/denominators, parity fixtures | Unsupported/unknown labels and incompatible fields explicit; no silent substitution. |
| Split/leakage audit | EVAL-003–004, NFR-001, NFR-006 | Group/time manifest, duplicate/window audit, leakage report | Related records do not cross partitions; final source untouched. |
| Baseline/CNN/LSTM/CNN-LSTM | FR-007–012, EVAL-005–007, NFR-001, NFR-004, NFR-006 | Run ledger, configs, seeds, metrics, failed runs, model artifacts | Fair matrix; selected model uses validation-only rule; no architecture assumed winner. |
| Calibration/threshold/abstention | FR-009–011, EVAL-004, EVAL-006–007, EVAL-011, NFR-001 | Calibration data split, reliability plots, Brier/ECE, threshold record, coverage curves | No probability claim without calibration evidence; final test not used for tuning. |
| Cross-dataset generalization | EVAL-008, EVAL-013, NFR-001–004, NFR-006 | Compatibility decision, untouched test log, degradation/shift report | Test only if two suitable independent sources exist; otherwise non-equivalent fallback disclosed. |
| Live/offline parity and two-system scenarios | OBJ-01–02, OBJ-06, DEM-001–007, EVAL-008, FR-001–006 | Capture readiness, scenario truth, timing, feature parity, drops/visibility report | Empty/denied capture is unhealthy, not benign; live metrics separate from offline. |
| B0/B1/B2/B3 DIG comparison | OBJ-05, FR-020–024, EVAL-010, K.4, NFR-002–004 | Frozen detector/policy configs, arm outputs, matched-budget analysis | Four arms reported, including harms and abstain-all reference. |
| DIG ablations/failure injections | FR-020–022, SEC-005, SEC-008–010, EVAL-010, K.5 | Fault manifest, independent fault truth, expected/actual, clean controls | Failed/unknown safety prerequisites cannot yield eligible response. |
| Response adjudication | FR-029–031, EVAL-012, K.6, SEC-008–010 | Blinded rubric, reviewer decisions, agreement, approvals/simulations | Prediction/risk/DIG cannot define ground truth; no real action. |
| Performance and resource study | FR-027, FR-039, NFR-002–004, TGT-02–05 | Hardware profile, boundaries, p50/p95/p99, throughput, drops, resources | No model-runtime-as-end-to-end or hard-real-time claim. |
| Reproducibility and uncertainty | EVAL-002–004, EVAL-009, NFR-006, O.1–O.3 | Ledger, seeds, environment, fingerprints, intervals, deviations | Re-run within approved tolerance or disclose deviation; correlated units handled honestly. |

## 13. Unresolved decisions for human review

At minimum, human approval is required for DEC-01–DEC-14 and TGT-01–TGT-07 in the PRD, including dataset(s), binary/multiclass scope, label mapping, topology, feature/flow/sequence semantics, Windows hardware and compute budget, split groups, EVAL-013 applicability, operating-point and matched-budget rules, calibration method, DIG observability/precedence/parameters, risk and response rubrics, explanation scope, retention/privacy, and final acceptance targets.

This plan intentionally does not resolve any of those decisions, select a dataset, prescribe numeric targets, claim literature novelty, report results, create code, or create datasets. `SYSTEM_DESIGN.md` and `TECHNICAL_DESIGN.md` are existing proposed companion documents; their existence is not approval, and this plan does not authorize changing their status. Stop for human review before implementation or training.
