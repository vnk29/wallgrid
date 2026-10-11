# WallGrid AI — Implementation Plan

**Document status:** Proposed roadmap for human review; documentation only  
**Project status:** No implementation, installation, capture, traffic generation, model training, or security testing is authorized by this plan  
**Source documents:** the four upstream design documents — `docs/PRD.md` (v0.1), `docs/SYSTEM_DESIGN.md`, `docs/ML_RESEARCH_PLAN.md`, and `docs/TECHNICAL_DESIGN.md` — plus `docs/M0_DECISION_REGISTER.md` and `docs/RECONCILIATION_REGISTER.md` as the governance/review baseline  
**Plan rule:** Existing source identifiers remain unchanged. This plan adds `IMP-*` task identifiers and links them to the source identifiers; it does not reclassify or silently resolve them.

## 1. Purpose, scope, and status conventions

This document converts the four upstream design documents, using the M0 Decision Register and Reconciliation Register as the governance/review baseline, into a dependency-ordered, reviewable roadmap for a Windows-first student project. It describes what may be done **after** the relevant approval gates pass, what evidence must be retained, and when work must stop. It is not an implementation specification, a source-code change, an authorization to install tools, or evidence that any capability exists.

The plan preserves the source documents' central boundaries:

- authorized laboratory observation only;
- bounded traffic and an operator emergency stop;
- least-privilege capture and separation from ordinary UI/API authority;
- explicit `unknown`, `invalid`, `degraded`, `stale`, and unavailable states;
- independent scenario truth and offline/live separation;
- no unrestricted payload or credential collection;
- no real containment, arbitrary command execution, or autonomous disruptive response;
- human approval for consequential simulation requests; and
- no claim that CNN, LSTM, CNN-LSTM, DIG, a dataset, a target, or a performance result is superior until the prescribed evidence exists.

### 1.1 Source authority and interpretation

1. `PRD.md` is authoritative for objectives, requirements, safety boundaries, acceptance obligations, decision IDs, target IDs, and the statement that the project is documentation-only until approved (`PRD §B`, §D, §E–§P).
2. `SYSTEM_DESIGN.md` is authoritative for logical module ownership, contracts, workflows, failure semantics, and component traceability. Its topology, storage, queue, formula, and precedence choices remain proposed or unresolved (`SYSTEM_DESIGN §3`, §6, §12–§14).
3. `ML_RESEARCH_PLAN.md` is authoritative for the proposed research protocol, split isolation, experiment matrix, DIG study, uncertainty, and result-recording rules. It contains no results and selects no dataset or model (`ML_RESEARCH_PLAN §1`, §3–§10, §13).
4. `TECHNICAL_DESIGN.md` is a provisional Windows/student-feasibility proposal. Its framework, backend, frontend, storage, capture tooling, and commands are alternatives or proposed defaults, not approved dependencies (`TECHNICAL_DESIGN §1–§3`, §8, §12).

### 1.2 Plan status values

| Status | Meaning in this plan |
|---|---|
| `Pending human review` | A governance or decision task can be reviewed, but no approval is implied. |
| `Not started` | The task is part of the roadmap and has no claimed execution evidence. |
| `Blocked: <gate>` | The task must not begin until the named approval, feasibility result, or evidence gate passes. |
| `Ready after <gate>` | The prerequisite is a future gate, not a current approval. |
| `Complete` | Reserved for a later progress report with retained evidence and reviewer disposition; no task in this initial plan is complete. |
| `Failed / N/A` | Used only after a task is evaluated and the reason and scope impact are recorded. |

Missing evidence is **blocked**, not passed. A functional screenshot, an empty capture, an aggregate accuracy value, or a successful-looking demo cannot substitute for the source-defined evidence.

## 2. Readiness assessment

### 2.1 Overall assessment

Planning can proceed because the four upstream design documents define a coherent research-and-demonstration objective and preserve stable requirement identifiers; the two registers provide the governance and reconciliation baseline. Implementation cannot responsibly begin yet. The project is at a **pre-M0 documentation gate**: requirements, owners, safety scope, environment, data, flow semantics, targets, and policy choices remain unapproved or unverified. The earliest implementation work must therefore be limited to work explicitly authorized after M0; the present task creates only this plan.

There are no established datasets, capture tools, model frameworks, target values, performance results, topology proofs, or completed tests. The current documentation baseline includes the four upstream design/planning documents plus the M0 Decision Register and Reconciliation Register; document presence is not approval or implementation evidence.

### 2.2 Contradictions, material tensions, and missing decisions

The table deliberately distinguishes a true conflict from a proposal that has not yet been selected. A proposal is not a resolution.

| ID | Source evidence and issue | Effect on implementation planning | Resolution / affected tasks |
|---|---|---|---|
| `READ-01` | `PRD` says documentation only and not authorization to implement or conduct security tests (`PRD lines 7–12`, §D/§G governance). `TECHNICAL_DESIGN` also says no implementation plan was created or authorized by that document (`TECHNICAL_DESIGN §15`). The user request authorizes creation of this plan, but not implementation. | This plan may exist, but every implementation, installation, capture, traffic, and training task remains gated. | Record the human approval record and scope boundary at M0. Affects all `IMP-*`; especially `IMP-001`–`IMP-006`. |
| `READ-02` | Requirement status is mixed: several functional mechanisms are proposed (`P`) or validation-dependent (`V`), while the surrounding objective is confirmed. The source documents say draft acceptance details become normative only after approval (`PRD §Document governance`, `SYSTEM_DESIGN §1`). | A roadmap must not present proposed mechanisms as settled requirements or mark them implemented. | Preserve each source status in the decision log and traceability register. Affects `IMP-001`–`IMP-006`, `IMP-027`, `IMP-071`. |
| `READ-03` | The PRD selects no topology; `SYSTEM_DESIGN` and `TECHNICAL_DESIGN` recommend A-to-B service traffic as a provisional Windows MVP but retain VM-mirror and physical mirror/TAP alternatives (`PRD §I.1`, `SYSTEM_DESIGN §5`, `TECHNICAL_DESIGN §8`). | The simplest topology is not the selected topology. A task depending on packet visibility cannot start from the recommendation alone. | Resolve `DEC-02`; prove visibility independently before model evaluation. Affects `IMP-008`–`IMP-012`, `IMP-040`, `IMP-044`, `IMP-067`. |
| `READ-04` | Npcap/libpcap-compatible capture, TShark, PyShark, Scapy, WinDivert, ETW/WFP, and other alternatives are listed without an approved choice (`TECHNICAL_DESIGN §3` and §8). | Installing or assuming one tool would create an unapproved dependency and could change visibility or privilege semantics. | Inventory first; choose only after the Windows feasibility slice and privilege review. Affects `IMP-007`–`IMP-012`, `IMP-014`, `IMP-040`. |
| `READ-05` | The technical proposal names Python/FastAPI/React/SQLite and compares PyTorch with TensorFlow/Keras, while the System Design intentionally avoids selecting a framework (`SYSTEM_DESIGN §1`, §12; `TECHNICAL_DESIGN §3`). | These are proposed options, not stack commitments. Repository and lockfile work cannot assume them before approval. | Resolve the minimum stack using the approved feasibility and reproducibility rule; keep alternatives visible. Affects `IMP-013`–`IMP-018`, `IMP-028`–`IMP-039`. |
| `READ-06` | No dataset, label mapping, rights decision, or independent source is selected (`PRD EVAL-001–004, EVAL-013`; `ML_RESEARCH_PLAN §3–§4`). | Training, final-test design, and multiclass claims are blocked. A convenient public dataset or guessed label map would violate the plan. | Complete candidate assessment, manifests, label mapping, and EVAL-013 applicability before training. Affects `IMP-019`–`IMP-027`, `IMP-028`–`IMP-039`, `IMP-068`. |
| `READ-07` | Flow key, direction, expiry, partial-flow policy, sequence axes, CNN layout, and LSTM/CNN-LSTM temporal semantics are proposed or unresolved (`PRD §I.3`, `DEC-04`; `SYSTEM_DESIGN §7`; `ML_RESEARCH_PLAN §5`; `TECHNICAL_DESIGN §7`). | A complete-flow offline model cannot be treated as valid for partial live windows. Dataset row order cannot be treated as time, and tabular columns cannot be treated as CNN spatial structure without evidence. | Freeze semantics from deterministic fixtures and live compatibility analysis. If sequence semantics are unsupported, record LSTM/CNN-LSTM as limited or not applicable rather than fabricating a sequence. Affects `IMP-022`–`IMP-024`, `IMP-028`–`IMP-045`. |
| `READ-08` | `TGT-01`–`TGT-07` are intentionally TBD, while performance and final acceptance depend on them (`PRD §B.1`, `§J.2`, `NFR-001–006`; `ML_RESEARCH_PLAN §6`). | No numeric quality, latency, capacity, resource, workload, retention, or study-adequacy acceptance can be claimed. “Near real time” and “reproducible” remain unmeasured. | Approve a justified target sheet before held-out test access and performance acceptance. Affects `IMP-025`, `IMP-033`, `IMP-038`, `IMP-065`, `IMP-070`–`IMP-075`. |
| `READ-09` | The PRD and System Design require four DIG check families and fail-closed response eligibility, but the observability, parameters, precedence, and risk formula remain proposed (`PRD §K.2–§K.4`, `DEC-06–07`; `SYSTEM_DESIGN §8`; `TECHNICAL_DESIGN §13`). | Functional DIG recording can be designed, but DIG effectiveness cannot be claimed and a formula/precedence cannot be silently adopted. | Approve observable checks, precedence, risk rubric, missing-context treatment, and fair B0–B3 comparison rules. Affects `IMP-048`, `IMP-052`–`IMP-057`, `IMP-074`. |
| `READ-10` | Confidence, risk, analyst disposition, and response permission are intentionally separate. A high score cannot override failed or unknown safety checks (`PRD FR-020–024`, `SEC-008`; `SYSTEM_DESIGN §8`). | A model score or risk ranking can never be used as an authorization shortcut. Any design that merges these fields is nonconforming. | Keep separate contracts and boundary fixtures. Affects `IMP-037`, `IMP-048`, `IMP-052`, `IMP-061`–`IMP-063`. |
| `READ-11` | Explanation method, reference data, ATT&CK mapping scope, curated asset fields, identity rules, retention, permitted fields, and non-loopback policy are unresolved (`DEC-08–09`, `PRD FR-013–019, FR-040`; `TECHNICAL_DESIGN §14`). | ORACLE, CENSUS, export, and privacy acceptance cannot assume SHAP, ATT&CK coverage, payload fields, or a retention period. | Approve scope and represent unavailable/unsupported outputs explicitly. Affects `IMP-047`–`IMP-050`, `IMP-064`–`IMP-066`. |
| `READ-12` | The experiment matrix requires a trivial reference, classical baseline, CNN, LSTM, and CNN-LSTM, but the documents also allow an infeasible or semantically invalid arm to be reported as failed/not applicable (`PRD EVAL-005`, `ML_RESEARCH_PLAN §5`, §10). | “Run every model” and “do not invent temporal semantics” are reconciled by an evidence gate, not by forcing an invalid run. A deep model is not promised to win. | Record every planned and failed/infeasible arm; seek a scope decision when sequence or compute feasibility fails. Affects `IMP-028`–`IMP-034`. |
| `READ-13` | EVAL-013 requires cross-dataset testing only when at least two suitable independent sources exist; the fallback is explicitly non-equivalent (`PRD EVAL-013`, `ML_RESEARCH_PLAN §4.2`). | The roadmap cannot promise cross-dataset generalization or substitute a same-source split without labeling the limitation. | Make an applicability decision before final-test freeze, then either run the untouched independent test or document the non-equivalent fallback. Affects `IMP-026`, `IMP-068`, `IMP-073`. |
| `READ-14` | The intended demonstration needs two physical systems or isolated VMs, independent scenario truth, designated reviewers, and capture permissions; all remain assumptions (`PRD §A, §I`; `SYSTEM_DESIGN §2, §5`; `TECHNICAL_DESIGN §8`). | A prerecorded screenshot or a single empty capture cannot satisfy the live demonstration. | Confirm resources, scope, reviewers, and clock method at M0/M1. Affects `IMP-003`, `IMP-008`–`IMP-012`, `IMP-044`, `IMP-067`. |
| `READ-15` | Logical modules are mandatory, but the deployment may be one process or a few bounded processes; ADR-02, ADR-05, and ADR-06 leave process, persistence, and event delivery unresolved (`SYSTEM_DESIGN §3`, §12; `TECHNICAL_DESIGN §9`). | Microservices, brokers, and a production database are not justified by the documents. Over-engineering could consume the student project's budget. | Choose the smallest architecture that passes contract, recovery, privilege, and measured-load tests. Affects `IMP-013`–`IMP-018`, `IMP-043`, `IMP-065`. |
| `READ-16` | SIBYL forecasting, anomaly detection, additional OS support, broader multiclass, ATT&CK mapping, and alternate explanations are optional or validation-dependent (`PRD §D.3`, FR-012, FR-019, FR-033). | Optional work must not displace must-have traceability, live compatibility, safety, or reproducibility. | Keep features disabled or explicitly out of scope until their separate gates pass. Affects `IMP-049`, `IMP-064`, `IMP-073`. |
| `READ-17` | Earlier drafts assigned different meanings to M3–M5. The PRD, ML Research Plan, System Design, Technical Design, and this plan now use the proposed crosswalk in `PRD §N`: M3 integrated observation/investigation, M4 DIG and simulation-only response, M5 historical/operational/policy study, and M6 final sealed evaluation/reproducibility/human acceptance. | The crosswalk is a proposed naming convention, not M0 approval. Evidence gates remain controlling and every source-specific scope change remains in its DEC/ADR/TGT record. | Adopt or revise the canonical crosswalk at M0; preserve the existing milestone identifiers and record any scope-changing deviation. Affects `IMP-005`, `IMP-027`, `IMP-054`–`IMP-070`, `IMP-075`. |
| `READ-18` | A historical `SYSTEM_DESIGN` drafting note said `docs/TECHNICAL_DESIGN.md` was absent, but the companion document exists and is explicitly marked proposed in the current System Design and this plan. | The existence contradiction is editorially corrected; existence is not approval. | Keep the current existence/status wording and record only substantive approval or supersession at M0. Affects `IMP-001`, `IMP-005`, `IMP-013`–`IMP-018`, `IMP-035`–`IMP-045`. |
| `READ-19` | `TECHNICAL_DESIGN` was originally written against the PRD and System Design, while the ML Research Plan governs experiment protocol, splits, common population, and DIG evaluation. | The five-document crosswalk and explicit source-authority statements now require consistency review before stack or packaging approval. Technical options remain proposed. | Complete the five-document consistency review at M0/M1; do not treat any Technical Design option as an approved implementation constraint. Affects `IMP-001`, `IMP-014`, `IMP-027`–`IMP-039`, `IMP-054`. |

### 2.3 Current blockers

The following blockers are material and must remain visible in every progress report:

- **`BLK-01 / M0 approval`:** DEC-01, project authority, requirement status, safety scope, role owners, and capacity decisions are not approved.
- **`BLK-02 / Windows visibility`:** topology, interface, driver/backend, privileges, two-system resources, clock method, and emergency stop are unverified.
- **`BLK-03 / Data and labels`:** no dataset, license/provenance record, label mapping, independent truth method, or EVAL-013 applicability decision exists.
- **`BLK-04 / Semantics`:** flow construction, feature units/order, partial windows, sequence representation, and live/training parity are not frozen.
- **`BLK-05 / Targets and evaluation`:** TGT-01–07, operating points, uncertainty method, and fair B0–B3 comparison rules are not approved.
- **`BLK-06 / Policy and privacy`:** risk rubric, DIG observability/precedence, response catalogue, explanation scope, asset/retention policy, and audit boundary are unresolved.
- **`BLK-07 / Capacity and tooling`:** Windows runtime, package availability, CPU/RAM/storage/GPU profile, and feasible experiment budget are unknown.
- **`BLK-08 / Document status`:** the Technical Design now exists as a proposed companion, but M0 must still record document precedence and any substantive approval/supersession decision.
- **`BLK-09 / Milestone adoption`:** the canonical M0–M6 crosswalk is documented in PRD §N and repeated by the companion plans, but its adoption remains a proposed M0 decision before progress claims.

Any task whose prerequisite includes one of these blockers is not implementation-ready. A fallback may reduce scope, but it must be recorded as a limitation and is not equivalent evidence.

## 3. Implementation principles

1. **Approval before action.** Keep documentation, implementation, installation, capture, traffic generation, training, and evaluation as separate authorization steps. M0 approval is a stop gate, not a formality.
2. **Small, verifiable increments.** Each task produces a bounded artifact and at least one positive, negative, or boundary check. Do not build the full pipeline before proving the earliest risky assumption: packet visibility and live/training feature compatibility.
3. **Evidence before conclusion.** Every requirement disposition links `objective/use case → PRD ID → design contract/decision → test or experiment → evidence artifact → acceptance decision` (`PRD §O.1`; `SYSTEM_DESIGN §13`; `TECHNICAL_DESIGN §13`).
4. **Version control and change control.** After approval, use a repository with reviewed changes, immutable task IDs, tagged protocol/configuration versions, and a decision log. Material changes after final-test access create a new development result and require a new independent test source where feasible (`PRD §J.3`; `ML_RESEARCH_PLAN §1`).
5. **Reproducible environments.** Record the Windows build, hardware, runtime, driver/backend, dependency lock/constraints, frontend lockfile if applicable, configuration, seeds, commands, and code revision. Do not install a package or driver merely to remove a setup gap; record the gap and obtain approval.
6. **Tests are deliverables.** Unit, contract, integration, security, model, failure-injection, performance, and authorized live-lab tests must retain fixture/manifests, expected and actual results, environment, timing boundaries, and evidence fingerprints. A missing result is blocked.
7. **Contracts over coupling.** Use versioned typed records and explicit rejection semantics. Logical modules may share a local process; microservices, cloud services, paid APIs, GPUs, brokers, or a separate database require evidence of need and approval (`SYSTEM_DESIGN §3`; `TECHNICAL_DESIGN §2–§3, §9`).
8. **Safe defaults.** Capture only approved metadata; payload collection is disabled. Unknown is not zero; invalid is not benign; unavailable is not success. Invalid features, incompatible packages, unknown safety prerequisites, stale approvals, audit-write failures, and unbounded queues fail closed where the source documents require it.
9. **Least privilege and human control.** Isolate capture privilege from dashboard/API privilege. Authenticate and authorize protected operations. Require human approval for simulation and never create real containment or arbitrary command paths.
10. **Offline/live separation.** Offline metrics do not establish live reliability. Independent scenario truth does not enter predictive features. Live predictions do not feed training automatically. Report packet/flow/prediction/incident/scenario/response denominators separately.
11. **Honest ML comparison.** Use the same approved split, label map, metric protocol, and bounded tuning budget for the trivial predictor, classical baseline, CNN, LSTM, and CNN-LSTM where feasible. Do not promise that a deep model or DIG policy wins; preserve negative and infeasible results.
12. **Final-test isolation.** Freeze data, labels, features, preprocessing, thresholds, calibration, model-selection rules, DIG parameters, and measurement boundaries before opening the untouched final source. Do not use final-test outcomes to tune the system.
13. **Student-feasible architecture.** Start with a local application or a few bounded processes and local durable state. Add complexity only after a measured contract, concurrency, durability, or recovery need is demonstrated.
14. **Review checkpoints.** Stop at M0, M1, M2, M3, M4, M5, and M6 until the required evidence is reviewed by the designated owner/supervisor. A successful partial demo does not waive a blocked gate.

## 4. Phase-by-phase roadmap

The phases below preserve the requested Phase 0–11 numbering; explicit task and milestone dependencies, not section order alone, control progression. No dates or effort estimates are invented. Contract work may run in parallel only where its stated prerequisites are satisfied. Every phase has an exit gate; passing a later functional demo does not retroactively pass an earlier blocked gate. In particular, HAWK's M3 work may precede or run alongside DIG contract definition, M4 DIG implementation waits for the required M3 conformance, and M5 measurement waits for M4. The milestone labels use the single proposed crosswalk in `PRD §N` and `ML_RESEARCH_PLAN §10`; no milestone name implies approval.

### Phase 0 — Document approval and decision resolution (M0)

**Purpose:** Establish authority, scope, owners, safety boundaries, and a controlled decision record before any implementation or environment change.

**Prerequisites:** The four upstream source documents, this plan, the M0 Decision Register, and the Reconciliation Register are available for human review. No repository, package, driver, capture, traffic, or training activity is required or permitted for this phase.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-001` | Review the four upstream source documents, this plan, and the two governance/review registers; confirm that the PRD remains authoritative and that proposed/validation-dependent items are not silently promoted. | P0 | None |
| `IMP-002` | Assign the project owner/supervisor, lab operator, research lead, technical lead, analyst/adjudicator, evaluator, and approval authority; resolve `DEC-01` and the capacity/optional-cut portion of `DEC-12`. | P0 | `IMP-001` |
| `IMP-003` | Approve the written authorized-lab scope: systems/interfaces, scenario categories, traffic limits, start/end conditions, emergency stop, permitted fields, and no-public/off-target testing (`SEC-001`, `SEC-006`). | P0 | `IMP-002` |
| `IMP-004` | Confirm must-have, should-have, optional, validation-dependent, and explicitly out-of-scope boundaries. Record that binary detection is the base task and that multiclass, anomaly detection, forecasting, ATT&CK, and extra platforms remain conditional. | P0 | `IMP-001`–`IMP-003` |
| `IMP-005` | Create the decision register for `DEC-01`–`DEC-14`, `TGT-01`–`TGT-07`, ADR-01–ADR-09, and the source contradictions/tensions. Assign each an owner, evidence required, and resolution gate without pretending to resolve M1 decisions at M0. | P0 | `IMP-002`–`IMP-004` |
| `IMP-006` | Approve this roadmap as the controlled next-step plan and explicitly authorize only the next approved activity. Record that implementation, installation, capture, traffic generation, training, and security testing remain separately gated. | P0 | `IMP-003`–`IMP-005` |

**Deliverables:**

- signed or otherwise attributable M0 approval record and scope statement;
- decision/assumption/blocker register preserving all original identifiers;
- role and review matrix, including who may approve simulations and who may adjudicate labels;
- initial scope-cut record for limited student capacity; and
- a dated-by-reviewer (not calendar-planned) authorization note for the next gate.

**Dependencies and start constraints:** No later implementation task may start on the strength of a verbal assumption. `IMP-007`–`IMP-012` require the scope and operator approval from `IMP-003`; repository work requires the stack/scope decision record even if capture feasibility proceeds in parallel.

**Acceptance criteria and evidence:** Every material issue in §2.2 has an owner and status; `DEC-01` and the M0 part of `DEC-12` are decided; the lab boundary and emergency stop are explicit; prohibited actions are written; source requirement statuses remain intact; and the approval record identifies what is and is not authorized. Evidence is the reviewed decision log and approval record, not a source-code commit.

**Exit gate — M0:** Supervisor/project owner approves requirements, safety boundaries, owners, scope, and the next permitted activity. If approval is absent or a required safety boundary is disputed, all implementation tasks remain `Blocked: M0`.

**Traceability:** `PRD §A–§D, §G, §N–§P`; `OBJ-01`–`OBJ-07`; `SEC-001`, `SEC-006`, `SEC-008`; `DEC-01`, `DEC-12`; `SYSTEM_DESIGN §1–§2, §9, §12`; `ML_RESEARCH_PLAN §1, §10, §13`; `TECHNICAL_DESIGN §1, §14`.

### Phase 1 — Windows environment and capture feasibility (M1 input)

**Purpose:** Establish whether the approved Windows host can observe the approved traffic through a known path, with correct privilege and health semantics. Packet visibility is the earliest deployment blocker and must be tested before detector evaluation.

**Prerequisites:** M0 exit; proposed capture-IPC/session/privacy contracts (`IMP-076`, `IMP-083`); approved lab scope, operator, independently operable emergency stop, two-system or isolated-VM resource plan, and permission to perform only the approved feasibility checks. No driver or tool is assumed installed.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-007` | Record the Windows edition/build, architecture, CPU/RAM/storage, VM availability, interfaces, administrator consent, existing capture backends/drivers, runtime availability, firewall/profile constraints, and clock sources. Use read-only inventory first. | P0 | M0 / `IMP-003` |
| `IMP-008` | Compare and approve one observation topology (A-to-B service traffic, isolated VM mirror, or physical mirror/TAP) and document coverage limits, interface direction, isolation, and why the relevant traffic reaches System B. | P0 | `IMP-007`, `DEC-02` |
| `IMP-009` | Assess candidate capture backends and optional diagnostics (including Npcap/libpcap-compatible paths or TShark) without assuming installation. Record “not installed” separately from “installed but unusable,” support, privilege, and process-boundary implications. | P0 | `IMP-007`, `IMP-008` |
| `IMP-010` | After explicit scope authorization and the capture/privacy contract review, run the bounded visibility proof with an independently recorded benign marker/count. Distinguish permission denied, wrong interface, mirror-disabled/no delivered traffic, driver/backend failure, genuinely idle traffic, and observable drops. | P0 | `IMP-008`, `IMP-009`, `IMP-076`, `IMP-083` |
| `IMP-011` | Establish timestamp and clock-offset measurement boundaries, and record offered/captured observations, resource use, queue/drop counters, and capture duration for the feasibility slice. | P1 | `IMP-010`, `IMP-082` |
| `IMP-012` | Verify the capture privilege boundary, independently operable stop procedure, authorized destination isolation, and that dashboard/API functions do not inherit capture elevation. Do not retain unrestricted payloads or credentials. | P0 | `IMP-003`, `IMP-010`, `IMP-076`, `IMP-083` |

**Deliverables:** Windows/environment inventory; approved topology record; capture-backend feasibility record; independent visibility evidence; interface/permission/loss/clock record; privilege-boundary review; and a capture fallback or stop record.

**Dependencies and start constraints:** `IMP-010` cannot proceed from an empty trace. A known marker must be independently generated and counted after scope approval. A topology recommendation in the source documents is not a passing result. Any required driver installation or privilege change requires a separate operator authorization and must be recorded; this plan does not perform it.

**Acceptance criteria and evidence:**

- the intended traffic is visible on B through the approved path, or a precise failure is recorded;
- wrong interface, denied permission, no delivered traffic, backend failure, and idle traffic are distinguishable;
- the empty-capture case is unhealthy/unknown, never benign;
- packet/capture privilege is narrower than ordinary UI/API authority;
- timestamps, clock uncertainty, drops, scope, start/end, independently operable emergency-stop behavior, and storage/retention limits are recorded; and
- no unrelated traffic, credentials, or unrestricted payloads are collected.

Evidence must include the topology/marker/count record, environment inventory, permissions, measurement boundaries, and reviewer disposition. The proposed probe must use the unexecuted `CF-01`–`CF-09` acceptance table in `docs/M0_DECISION_REGISTER.md §1.1`; those rows define pass, fail, and inconclusive outcomes but do not authorize execution. If visibility cannot be proven, the phase fails safely and later live tasks are blocked.

**Exit gate — M1 capture prerequisite:** `FR-001`, `SEC-001`–`SEC-002`, `NFR-007`, `NFR-013`, and `DEM-001`–`DEM-003` have evidence or an explicit blocked disposition. A failed capture path triggers the fallback in §8; it does not authorize a prerecorded substitute for the required live demonstration.

**Traceability:** `PRD §I.1–§I.2`, `FR-001`, `FR-006`, `SEC-001`–`SEC-002`, `SEC-006`, `DEM-001`–`DEM-003`, `NFR-007`, `NFR-013`; `SYSTEM_DESIGN §2, §5, §10–§11`; `ML_RESEARCH_PLAN §3.1, §8, §10`; `TECHNICAL_DESIGN §3` and §8.

### Phase 2 — Repository foundation, configuration, logging, health checks, and tests

**Purpose:** Establish the smallest reproducible local foundation and evidence harness. Logical modules and contracts are created only after M0 approval; the phase does not require a microservice platform.

**Prerequisites:** M0 exit, an approved initial stack boundary, repository authorization, and the environment facts available from `IMP-007`. Capture feasibility may continue in parallel, but no live detector is claimed.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-013` | Create the repository/branch policy, source-document references, change-review rules, immutable `IMP-*` task ledger, decision log, and evidence-directory convention. Do not rewrite governing documents during implementation without an approved documentation change. | P0 | M0 / `IMP-006` |
| `IMP-014` | Define the reproducible Windows environment profile, runtime version, dependency lock/constraints strategy, frontend lock strategy if applicable, and driver/backend inventory record. Do not install packages as part of this plan. | P0 | `IMP-007`, `IMP-013`, approved stack decision |
| `IMP-015` | Define versioned non-secret configuration schemas for topology, scope, feature contracts, health, thresholds, roles, retention, and policy references; keep secrets outside source, logs, errors, and exports. | P0 | `IMP-013`, `IMP-014`, `IMP-076`, `IMP-078`, `IMP-080`, `IMP-083`, `DEC-09` |
| `IMP-016` | Define structured logs and append-oriented audit records with actor/service, correlation/request ID, time, operation, outcome, reason, entity/version links, duration, redaction, integrity metadata, and audit-gap behavior. | P0 | `IMP-015`, `IMP-078`, `IMP-080`, `IMP-081`, `SEC-007`, `SEC-010` |
| `IMP-017` | Define liveness versus readiness and component health states for capture, flow/features, model, persistence, correlation, audit, and dashboard freshness, including no traffic, no attacks, unknown, stale, unavailable, and overloaded. | P0 | `IMP-015`, `IMP-016`, `IMP-079`, `IMP-082` |
| `IMP-018` | Establish the test harness and fixture conventions for positive, negative, boundary, malformed, duplicate, late, stale, replayed, unavailable, and unknown states. Link test IDs to requirements and evidence records. | P0 | `IMP-013`–`IMP-017` |

**Foundation contract lane:** `IMP-076`–`IMP-085` and `IMP-087` are definition tasks in this phase's contract lane. They must be specified before the consumer components listed below are implemented; they do not claim implementation or verification. `IMP-086` is a data/representation-dependent contract completed after Phase 3's candidate and feature decisions. `IMP-088` is the policy-rule contract at the start of Phase 8, after the upstream risk/context contracts and before any DIG arm is implemented or evaluated.

| Foundation contract | Downstream consumers that must not precede it |
|---|---|
| `IMP-076` authentication/session/RBAC/IPC | `IMP-015`, `IMP-040`, `IMP-060`, `IMP-062`, `IMP-080` |
| `IMP-077` durability/acknowledgement/recovery/replay/loss | `IMP-015`, `IMP-016`, `IMP-050`, `IMP-065`, `IMP-080` |
| `IMP-078` audit integrity/clock/tamper/retention failure | `IMP-015`, `IMP-016`, `IMP-062`, `IMP-065`, `IMP-080` |
| `IMP-079` health/reference lifecycle | `IMP-017`, `IMP-043`, `IMP-064` |
| `IMP-080` typed API/concurrency/idempotency | protected API/UI mutations, review, export, approval, simulation, reconnect |
| `IMP-081` common record envelope/identity/replay/unknown shapes | `IMP-022`, `IMP-041`, `IMP-046`, `IMP-050`, `IMP-080` |
| `IMP-082` timing/render instrumentation | `IMP-011`, `IMP-025`, `IMP-027`, `IMP-043`, `IMP-065` |
| `IMP-083` privacy/pseudonymization/retention/deletion | `IMP-015`, `IMP-066`, exports, storage and evidence records |
| `IMP-084` artifact trust/safe loading/promotion | `IMP-035`, `IMP-037`, `IMP-038` |
| `IMP-085` event-time context/revision/approval invalidation | `IMP-046`–`IMP-051`, `IMP-062` |
| `IMP-087` closed simulation catalogue/no-effect boundary | `IMP-061`–`IMP-063`, `IMP-088` shared safeguards |

**Deliverables:** approved repository conventions; environment/lockfile design; configuration schemas; structured logging/audit contract; health-state contract; test/evidence harness specification; and an initial traceability index.

**Dependencies and start constraints:** Exact frameworks and packages remain proposed until the decision register approves them. A local process or small bounded-process design is preferred; adding a broker, cloud service, or paid API requires a measured need and a new approval. Configuration, logs, and tests may be specified before all modules exist.

**Acceptance criteria and evidence:** configuration rejects invalid types/ranges/enums before state changes; logs and audit records are secret-free and correlation-linked; health distinguishes liveness/readiness and unknown from zero; versioned contract/error conventions are explicit; and the test harness can record expected/actual output and environment without modifying source documents. Evidence is schema/fixture review plus contract tests once implementation is authorized; no test result is claimed now.

**Exit gate:** Repository and environment conventions are approved, the initial stack decision is recorded, `IMP-076`–`IMP-085` and `IMP-087` have proposed contract records with owners and no consumer is allowed to claim conformance before its governing contract, and the test harness can support M1–M2 evidence. This phase may proceed in parallel with Phase 1 after M0, but model or live-pipeline acceptance still depends on M1.

**Traceability:** `PRD FR-036`–`FR-040`, `NFR-005`–`NFR-008`, `NFR-011`–`NFR-013`, `SEC-003`–`SEC-012`; `SYSTEM_DESIGN §3, §6, §9–§11`; `ML_RESEARCH_PLAN §9–§10`; `TECHNICAL_DESIGN §4–§6, §9–§12`.

### Phase 3 — Dataset assessment, manifests, feature definitions, leakage controls, and evaluation protocol (M1)

**Purpose:** Decide whether any data and feature representation can support an honest binary detector and controlled live evaluation. This phase is the gate before training, not a data-download or training phase by itself.

**Prerequisites:** M0 exit; `IMP-007`/`IMP-008` environment and topology facts; repository/evidence conventions from Phase 2 or an approved equivalent; research lead and supervisor available for data decisions.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-019` | Build the candidate dataset matrix covering rights, provenance, collection conditions, topology, raw packet/flow availability, labels, timestamps, support, missingness, duplicates, feature definitions, leakage risk, reproducibility, and live reconstructability. Retain rejected candidates and reasons. | P0 | M0, `IMP-007`, `IMP-008` |
| `IMP-020` | For each candidate under approved access, create an immutable manifest/fingerprint and quality profile: inclusion/exclusion counts, duplicates, missing values, class support/prevalence, unknown labels, and denominator impact. | P0 | `IMP-019`, data-access approval |
| `IMP-021` | Create the versioned label mapping and independent-truth/adjudication plan. Distinguish normal, supported attack, unsupported attack, unknown, invalid, and ambiguous; never map unknown to normal silently. | P0 | `IMP-019`, `IMP-020`, `DEC-03` |
| `IMP-022` | Create the field-level offline/live feature compatibility matrix: name, type, unit, direction, aggregation, availability time, missing policy, transformation, leakage risk, and disposition (`retain`, `reconstruct`, `remove from both`, or `scope decision`). | P0 | `IMP-010`, `IMP-019`–`IMP-021`, `IMP-081`, `IMP-083` |
| `IMP-023` | Resolve and record flow key, direction, idle/active expiry, partial-window/closed-flow policy, sequence grouping/order, padding/masking, overlap, warm-up, CNN layout rationale, and LSTM/CNN-LSTM applicability. | P0 | `IMP-010`, `IMP-022`, `DEC-04` |
| `IMP-024` | Construct the group/time/session/host/capture split hierarchy, duplicate and overlapping-window audit, common-population purge, and final-test isolation plan. Prohibit related flows, shared markers, and augmented derivatives from crossing partitions. | P0 | `IMP-020`–`IMP-023`, `IMP-081` |
| `IMP-025` | Approve the evaluation unit and metric/measurement protocol, including `P_common`, per-class support, macro-F1, false-positive definitions, coverage/abstention, response metrics, latency boundaries, throughput, resource sampling, uncertainty, and the actionable target sheet for `TGT-01`–`TGT-07`. Each target record must state metric/boundary, population/workload, selection/uncertainty method, owner/gate, and pass/fail/inconclusive evidence without inventing values. | P0 | `IMP-020`–`IMP-024`, `IMP-082`, `DEC-05`, `DEC-14` |
| `IMP-026` | Complete the EVAL-013 applicability assessment for at least two candidate independent sources when available using only Access Class A/B information unless the source is explicitly designated development data. Record the source-level access ledger, final-source designation, and the non-equivalent independent-capture/live fallback when applicable. | P0 | `IMP-019`–`IMP-025`, `IMP-081`, `IMP-083`, `DEC-13` |
| `IMP-027` | Freeze the research protocol, `P_common`/broader-coverage rules, experiment ledger fields, access classes/ledger, evidence locations, failed-run convention, final-test opening rule, and requirement-to-test/experiment traceability register. | P0 | `IMP-021`–`IMP-026`, `IMP-082` |

**Deliverables:** candidate comparison and rejected-source log; immutable manifest/fingerprint; label map and truth/adjudication protocol; feature compatibility matrix; approved flow/sequence representation decision; leakage/split report; target and metric sheet; EVAL-013 applicability record; frozen protocol; and traceability register.

**Dependencies and start constraints:** Training is blocked until the compatibility matrix, leakage-controlled split, `P_common` contract, and final-source access boundary are reviewed. A field that cannot be reconstructed live must be removed from both paths, reconstructed with validated semantics, or cause retraining/scope change; it must not be filled with a convenient constant. Cross-dataset testing is conditional and never inferred from two partitions of one capture. `IMP-086` defines the explanation decision before ORACLE implementation; it does not select a method or grant an exception.

**Acceptance criteria and evidence:**

- at least one lawful, provenance-documented candidate is selected or the lack of a suitable source is recorded;
- every selected feature has a live meaning, or its removal/reconstruction/scope decision is explicit;
- labels and unsupported/unknown cases have a versioned mapping and independent truth path;
- related records cannot cross the approved split without a documented exception;
- all preprocessing, calibration, thresholds, model-selection, `P_common`, and final-source access boundaries are defined;
- metrics use explicit units and denominators; and
- `TGT-01`–`TGT-07`, operating-point rules, and study-adequacy/uncertainty rules are approved before final-test access.

**Exit gate — M1:** Dataset/feature suitability, topology feasibility, split isolation, EVAL-013 applicability, compute/measurement budget, and target sheet are approved. If no honest live-compatible feature set or leakage-resistant split exists, Phase 4 is blocked and a scope/data decision is required.

**Traceability:** `PRD EVAL-001`–`EVAL-013`, `FR-002`–`FR-006`, `NFR-001`–`NFR-006`, `DEM-004`–`DEM-007`, `DEC-03`–`DEC-05`, `DEC-13`–`DEC-14`; `SYSTEM_DESIGN §4, §6–§7, §13`; `ML_RESEARCH_PLAN §3–§6, §8–§10, §12`; `TECHNICAL_DESIGN §6–§7, §13–§14`.

### Phase 4 — Baseline experiments and CNN/LSTM/CNN-LSTM evaluation (M2 input)

**Purpose:** Run the approved, leakage-controlled research matrix and preserve honest comparisons. This phase is not complete merely because a deep model trains or produces a high aggregate score.

**Prerequisites:** M1 exit; approved development dataset access, manifest, label mapping, feature/flow/sequence contract, `P_common` and broader-coverage rules, split, metrics, target sheet, tuning budget, environment, seeds, and final-test isolation. No final-test source may be opened for tuning or outcome-bearing inspection.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-028` | Validate the offline fixture and preprocessing pipeline, including training-only fitting, schema/order/types/units, missingness, label mapping, `P_common` eligibility, duplicate/overlap handling, sequence warm-up/masking, deterministic rerun behavior, and the CPU feasibility slice. | P0 | M1, `IMP-014`, `IMP-022`–`IMP-025`, `IMP-081` |
| `IMP-029` | Run and ledger the trivial reference predictor under the frozen development/evaluation protocol. | P0 | `IMP-028` |
| `IMP-030` | Run and ledger the approved classical ARGUS baseline with bounded tuning, seeds, cost, calibration/threshold handling, and failure reasons. | P0 | `IMP-028`, `IMP-029` |
| `IMP-031` | Run the CNN only with an approved feature-layout rationale and the same `P_common` split, metrics, tuning budget, and reproducibility controls; report broader operational coverage separately. | P0 | `IMP-028`, `IMP-030`, `IMP-023` |
| `IMP-032` | Run the LSTM only if timestamps and repeated observations support the approved `P_common` temporal sequence; otherwise record the limitation/not-applicable decision and its implication. | P0 | `IMP-028`, `IMP-023`, `IMP-030` |
| `IMP-033` | Run the CNN-LSTM under the same `P_common` sequence, warm-up, mask, budget, split, and metric rules when applicable; preserve out-of-memory, infeasible, or semantically invalid outcomes rather than hiding them. | P0 | `IMP-032` |
| `IMP-034` | Produce the development-only comparison, uncertainty/seed/scenario variability, resource/latency record, calibration/threshold/abstention analysis where supported, and validation-based model-selection record. | P0 | `IMP-029`–`IMP-033`, `IMP-025` |

**Deliverables:** run ledger; manifests and split/group evidence; preprocessing artifacts; baseline/deep-model configurations and failed-run records; per-class metrics/support/confusion matrices; calibration/coverage outputs where applicable; resource/latency records; and a validation-only selection decision.

**Dependencies and start constraints:** The final test remains untouched. LSTM/CNN-LSTM cannot start if sequence semantics are not supported. A CNN cannot be justified by reshaping tabular columns without a representation decision. Any scope change caused by compute, labels, leakage, or parity is recorded before further runs.

**Acceptance criteria and evidence:** The complete planned matrix is accounted for; each feasible arm uses the same approved `P_common` comparison protocol; sequence-ineligible, missing-feature, invalid, unsupported-label, timeout, and broader-coverage counts are retained; preprocessing never fits on validation/final data; every run has a manifest, seed, environment, configuration, status, and evidence path; per-class support and undefined metrics are reported; and the selected model is chosen by the approved development rule balancing quality, false alarms, cost, and live compatibility. No architecture winner or DIG benefit is assumed.

**Exit gate — development comparison, M2 input:** The baseline/deep-model comparison is reproducible within the approved tolerance or deviations are disclosed; leakage and label/feature checks pass; the selection rule is recorded; infeasible arms have reviewed reasons; and a supervisor approves either the selected model path or a scope revision. M2 does not close until Phase 5 supplies the compatible package. If sequence or compute feasibility fails, the project may continue only with an explicitly approved reduced claim.

**Traceability:** `PRD FR-007`–`FR-012`, `EVAL-004`–`EVAL-007`, `EVAL-009`, `EVAL-011`, `NFR-001`, `NFR-004`, `NFR-006`, `OBJ-02`; `SYSTEM_DESIGN §4A, §7, §13`; `ML_RESEARCH_PLAN §1.1, §5–§6, §8–§10`; `TECHNICAL_DESIGN §3.1, §7, §11–§13`.

### Phase 5 — Validated model packaging and inference contracts (M2 completion)

**Purpose:** Turn the validation-selected development artifact into a controlled, compatible, auditable inference package without silently accepting a model that live features cannot support.

**Prerequisites:** Phase 4 development-comparison review (`IMP-034`), not M2 exit; selected model decision; frozen feature/flow/sequence/`P_common` contract; training-only preprocessing artifact; approved label mapping, thresholds, calibration status, dependency/runtime profile, target protocol, and reviewed artifact-trust contract from `IMP-084`. `IMP-084` is a prerequisite for package construction and activation; it is not evidence that the loader is safe.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-035` | Define the package manifest binding model artifact, framework/runtime, preprocessing, feature schema/order/units, label mapping, threshold, calibration metadata, sequence policy, training-data manifest/fingerprint, dependencies, and integrity fingerprint under the artifact-trust contract. | P0 | `IMP-027`, `IMP-034`, `IMP-084` |
| `IMP-036` | Define and test the prediction contract: score meaning, class/unknown/abstain states, selected threshold, reliability state, model/preprocessor/data versions, timing, and original score/reason for abstention. | P0 | `IMP-035` |
| `IMP-037` | Implement the controlled activation decision, compatibility checks, fingerprint verification, readiness state, rollback to a previously compatible package, and safe failure on mismatch/load failure under the approved artifact-trust contract. | P0 | `IMP-035`, `IMP-036`, `IMP-084` |
| `IMP-038` | Verify CPU inference and deterministic fixture parity against the selected offline pipeline; record warm-up, preprocessing inclusion, batching, timeout, and resource boundaries without claiming end-to-end latency. | P0 | `IMP-036`, `IMP-037`, `TGT-02`, `TGT-04` |
| `IMP-039` | Freeze the deployable package and model-readiness evidence for live use; record unsupported labels/features and the conditions under which the package must abstain or become unavailable. | P0 | `IMP-037`, `IMP-038`, `DEC-03`, `DEC-04` |

**Deliverables:** versioned package manifest and fingerprint; prediction/inference contract; compatibility and activation tests; rollback/failure evidence; CPU fixture-parity record; and model-readiness/unsupported-condition record.

**Dependencies and start constraints:** An artifact is not deployable merely because it loads. A hash is integrity evidence, not proof of trustworthiness. Arbitrary executable deserialization is prohibited. Incompatible input or package failure must stop ordinary inference and expose degraded health rather than reuse an incompatible model silently.

**Acceptance criteria and evidence:** Package fields and versions are complete; incompatible schema/label/runtime/fingerprint fixtures are rejected before inference; activation is authorized and audited; rollback is deterministic; score/probability/calibration language is accurate; abstention preserves the original score and reason; and offline/live feature compatibility remains explicit.

**Exit gate — M2:** Phase 4 comparison and `IMP-035`–`IMP-039` package evidence are reviewed together. `AC-02` packaging and inference obligations are functionally evidenced, with target-dependent results still pending until the relevant acceptance studies. A compatible package may be offered to Phase 6; it is not a claim of live validity until Phase 6 parity passes.

**Traceability:** `PRD FR-003`, `FR-007`–`FR-011`, `FR-038`–`FR-039`, `SEC-005`, `SEC-011`, `NFR-006`, `DEM-005`, `AC-02`; `SYSTEM_DESIGN §4A, §7, §11`; `ML_RESEARCH_PLAN §5.3–§6, §8–§10`; `TECHNICAL_DESIGN §5–§7, §11–§13`.

### Phase 6 — Live capture, flow construction, and feature parity (M3 evidence)

**Purpose:** Connect the approved observation path to versioned flows/features and prove that the live semantic contract matches the training contract. The phase measures what B can observe; it never treats missing capture as benign traffic.

**Prerequisites:** M1 capture and data gates; Phase 2 contracts including `IMP-076`, `IMP-081`, and `IMP-082`; Phase 5 compatible package; approved scope and operator; frozen flow/feature/sequence/`P_common` rules; and authorization for the bounded live-lab scenarios. The packet-visibility proof from Phase 1 remains a prerequisite, not a substitute for this integrated parity test.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-040` | Integrate the approved capture adapter with readiness, selected-interface, permission, source identity, timestamp, loss, and privilege-boundary reporting. Keep capture authority separate from ordinary API/UI authority. | P0 | M1, `IMP-012`, `IMP-017`, `IMP-039`, `IMP-076`, `IMP-080` |
| `IMP-041` | Implement and verify the frozen flow key, direction, expiry, closure reasons, partial/unidirectional/truncated semantics, duplicate/late handling, and deterministic flow/event identity under the common record envelope. | P0 | `IMP-023`, `IMP-040`, `IMP-081` |
| `IMP-042` | Extract and validate live features using the same names/order/types/units/direction/missing rules/schema version as training. Quarantine invalid, non-finite, unsupported, or semantically incompatible vectors. | P0 | `IMP-022`, `IMP-041`, `IMP-039` |
| `IMP-043` | Measure offered/captured/processed observations and flows, queue age/depth, drops/rejections, feature failures, model readiness, timestamps, and bounded backpressure using the timing and health contracts. Unknown loss remains unknown rather than zero. | P0 | `IMP-040`–`IMP-042`, `IMP-017`, `IMP-079`, `IMP-082`, `TGT-02`–`TGT-04` |
| `IMP-044` | Execute the approved two-system scenario matrix after authorization: benign visibility first, then approved controlled conditions, plus uncertainty/integrity/denied-response fixtures as applicable. Retain independent scenario truth and timing; do not use scenario markers as predictive features. | P0 | `IMP-003`, `IMP-010`, `IMP-041`–`IMP-043`, `DEM-006` |
| `IMP-045` | Reconcile live/offline feature parity, flow membership, model eligibility, scenario-to-flow matching, drops, clock uncertainty, and misses. Record a live go/no-go or a bounded scope revision. | P0 | `IMP-042`–`IMP-044`, `IMP-025`, `IMP-027` |

**Deliverables:** live capture/readiness record; deterministic flow fixture results; feature parity matrix and mismatch log; live health/loss/queue metrics; authorized scenario truth and matching record; and a live eligibility/limitation decision.

**Dependencies and start constraints:** The live path must not be evaluated with a feature that was removed, changed in meaning, or filled with an unvalidated constant. A closed-flow model cannot be presented as equivalent to partial-window detection. Traffic must stay within the approved lab scope and bounded rate; no public, third-party, uncontrolled malware, or denial-of-service activity is in scope.

**Acceptance criteria and evidence:**

- expected traffic markers reach B through the approved path;
- feature names/order/types/units/direction/aggregation/missing semantics and schema versions reconcile with the training contract;
- invalid, incomplete, duplicate, late, or unsupported inputs have reason codes and do not become benign predictions;
- capture loss, queue state, processing failure, model readiness, timing, and drops are visible;
- scenario truth and matching windows are independent of prediction output; and
- the test retains all misses, false alarms, timeouts, and unknown outcomes.

**Exit gate:** capture/flow/feature parity evidence for the bounded activity is recorded, with `DEM-001`–`DEM-005` only partially applicable to this phase and `DEM-006`–`DEM-007` deferred to the integrated gate. No `AC-01` or live-model acceptance is claimed until the later integrated evidence bundle. Any parity mismatch blocks live model claims and returns to Phase 3 or 4.

**Traceability:** `PRD OBJ-01`, `OBJ-02`, `OBJ-06`, `FR-001`–`FR-011`, `NFR-002`–`NFR-005`, `NFR-007`, `NFR-011`, `NFR-013`, `DEM-001`–`DEM-007`, `AC-01`, `AC-02`, `AC-11`; `SYSTEM_DESIGN §3.2, §4B, §5–§7, §10–§11`; `ML_RESEARCH_PLAN §3.1, §5, §8, §10, §12`; `TECHNICAL_DESIGN §5–§8, §10–§11`.

### Phase 7 — Incident pipeline, correlation, asset context, risk, and evidence (M3 evidence)

**Purpose:** Turn eligible predictions and qualified failures into traceable incidents without inventing asset identity, evidence, causality, risk, or certainty.

**Prerequisites:** Phase 6 live/offline contract; Phase 2 persistence, audit, health, envelope, privacy, and timing contracts; `IMP-085` event-time/revision contract; `IMP-086` explanation validation contract; approved inventory, risk, privacy, evidence, and explanation decisions (`DEC-06`, `DEC-08`, `DEC-09`); and the controlled model/prediction contract.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-046` | Implement versioned correlation keys/windows, incident identity, duplicate prevention, late-arrival handling, grouping reasons, and preservation of every underlying event/prediction ID under the common envelope and event-time contract. | P0 | `IMP-041`, `IMP-042`, `IMP-017`, `IMP-081`, `IMP-085` |
| `IMP-047` | Implement curated CENSUS inventory, endpoint resolution, event-time asset snapshots, criticality provenance, and explicit known/unknown/ambiguous/NAT/reassigned states. | P0 | `IMP-046`, `IMP-085`, `DEC-09` |
| `IMP-048` | Implement separate confidence display and configurable versioned risk assessment using the approved severity/asset/evidence rubric, missing-context ranges/flags, boundary validation, and superseding history. Do not treat the proposed formula as an approved probability. | P0 | `IMP-047`, `IMP-085`, `DEC-06`, `FR-023`–`FR-024` |
| `IMP-049` | Implement ORACLE evidence references, fact/observation versus detector output versus analyst annotation versus hypothesis labels, bounded summaries, and the approved explanation method or explicit unavailable/scope-exception state. | P1 | `IMP-046`, `IMP-081`, `IMP-083`, `IMP-086`, `SEC-012` |
| `IMP-050` | Implement incident records and lifecycle (`new → acknowledged → investigating → closed`, with audited reopening), dispositions, evidence/asset/risk/explanation links, and versioned corrections without rewriting audit history. | P0 | `IMP-046`–`IMP-049`, `IMP-016`, `IMP-077`, `IMP-078`, `IMP-081`, `IMP-085` |
| `IMP-051` | Integrate prediction-to-incident lineage and test known, unknown, stale, conflicting, missing, duplicate, and delayed context. Verify that risk and evidence can prioritize review but cannot authorize response. | P0 | `IMP-048`–`IMP-050`, `IMP-045`, `IMP-085` |

**Deliverables:** correlation rule/version; incident records and lifecycle contract; inventory and asset-snapshot records; risk formula/rubric and boundary record; evidence/explanation records; and end-to-end lineage/reconciliation evidence.

**Dependencies and start constraints:** `CENSUS` may only assert an identity supported by the approved inventory and event-time evidence. `ORACLE` may not invent missing evidence or present an attribution as causal proof. If an explanation method is not validated, it is marked unavailable; this does not block basic evidence display unless the approved scope made it must-have without exception.

**Acceptance criteria and evidence:** underlying IDs and grouping reasons are preserved; duplicate and late arrivals do not create duplicate analyst incidents; asset changes do not rewrite historical snapshots; confidence, risk, disposition, and permission are separate fields; risk boundaries/unknowns/version changes are reproducible; every summary fact links to evidence or is marked unavailable/hypothesis; and the incident shows health and data limitations.

**Exit gate:** correlation, asset, evidence, and risk contracts have their own component evidence. `FR-020`–`FR-022` and response eligibility remain deferred to the DIG/WARDEN gates; `AC-03`, `AC-04`, and `AC-06` are not credited as complete until their full evidence bundles exist. No risk/confidence/evidence shortcut can reach response eligibility.

**Traceability:** `PRD OBJ-03`, `OBJ-04`, `FR-013`–`FR-024`, `FR-035`–`FR-036`, `NFR-009`, `SEC-005`, `SEC-007`, `SEC-012`, `AC-03`, `AC-04`, `AC-06`; `SYSTEM_DESIGN §3.1, §4C, §6, §8–§9`; `ML_RESEARCH_PLAN §6–§7, §12`; `TECHNICAL_DESIGN §5–§7, §10–§11`.

### Phase 8 — DIG policy conformance (M4 input) and deferred B0–B3 study (M5 evidence)

**Purpose:** Define and implement the four DIG check families, freeze the common B0–B3 study inputs, and preserve non-negotiable parser, authorization, audit, and simulation safeguards. The measured policy comparison is M5 work after M4 conformance; this phase does not presume a DIG improvement.

**Prerequisites:** M3 exit, including the Phase 9 HAWK work (`IMP-058`–`IMP-060`); Phase 7 incident/evidence/risk contracts; `IMP-087` simulation-boundary contract; and approved stage-specific `DEC-06`, `DEC-07`, `DEC-10`, and `DEC-14` inputs. First define `IMP-088`, then implement `IMP-052`–`IMP-054` for M4. The measured study `IMP-055`–`IMP-057` waits for M4 exit, including `IMP-063`, plus frozen study inputs, independent adjudication, approved applicable targets, and authorized failure fixtures. Policy-only fixtures may precede WARDEN, but they cannot establish approval, simulation, UI-failure, or effectiveness evidence.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-052` | Implement and record input/pipeline integrity, model reliability/operating conditions, evidence consistency, and response-eligibility checks with pass/fail/unknown/not-applicable, reason, evidence reference, policy version, and time under the approved `IMP-088` rule matrix. | P0 | `IMP-045`, `IMP-048`–`IMP-051`, `IMP-087`, `IMP-088`, `DEC-07` |
| `IMP-053` | Implement deterministic aggregate outcomes and approved precedence only after `IMP-088`; preserve the full vector and ensure required fail/unknown states block response eligibility regardless of confidence or risk. | P0 | `IMP-052`, `IMP-088`, `SEC-008`, `DEC-07` |
| `IMP-054` | Freeze common B0/B1/B2/B3 study inputs: detector, feature pipeline, `P_common`, split, scenario/candidate universe, correlation, action catalogue, shared parser/auth/simulation safeguards, tuning budgets, and operating-point or matched-coverage/review rules. | P0 | `IMP-025`, `IMP-026`, `IMP-045`, `IMP-087`, `IMP-088`, `DEC-10`, `DEC-14` |
| `IMP-055` | Execute and ledger the B0 fixed-threshold, B1 confidence-gated, B2 context-aware risk, and B3 proposed-DIG policies under the same controlled conditions. Report matched-population detection/coverage plus broader operational coverage, review, eligibility, response outcomes, latency, and resource overhead. | P0 | M4; `IMP-052`–`IMP-054`, `IMP-063`, `IMP-088` |
| `IMP-056` | Run applicable check-family ablations and authorized fault injections with clean controls: schema/unit errors, corrupt artifacts, packet loss/partial/late/duplicate flows, shifted permitted classes, ambiguous scores, conflicting evidence/stale assets, high-confidence safety failures, stale/replayed approvals, audit/storage/model/UI failures, and overload. | P0 | `IMP-055`, authorized safety fixtures |
| `IMP-057` | Apply the independently approved response rubric, retain blinded/adjudicated labels and disagreements, analyze unjustified eligibility/approval/simulation, justified coverage, review workload, latency/resource cost, uncertainty, and negative/inconclusive findings. | P0 | `IMP-055`, `IMP-056`, `DEC-10`, `EVAL-012` |

**Deliverables:** M4 DIG check contract, precedence decision, shared-versus-arm matrix, policy-only fixtures, and safety-boundary evidence; plus the deferred M5 study ledger, ablation/failure manifest, independent adjudication/rubric records, workload/overhead analysis, and evidence-bounded conclusion when those tasks execute.

**Dependencies and start constraints:** All arms retain real authorization, parser validation, audit, and simulation-only safeguards. An ablation may analyze hypothetical eligibility but must not remove the actual safety boundary. Invalid data that cannot safely enter a parser is recorded as a shared rejection; it is not made acceptable by weakening validation.

**Acceptance criteria and evidence:**

- every check family is separately observable and preserved;
- high-confidence plus failed/unknown integrity or safety never becomes eligible;
- stale, replayed, changed-target, unauthorized, duplicate, or audit-unavailable requests cannot simulate;
- the common detector/scenario inputs and fair development-only tuning rules are frozen for all arms;
- M4 policy-only fixtures and clean safety controls are accounted for; and
- later M5 results, when authorized, retain all arms, ablations, fault combinations, harms, abstentions, review burden, delay, resource cost, uncertainty, and the abstain-all/no-response reference.

**Exit gates:** `IMP-052`–`IMP-054` provide DIG conformance inputs to M4; Phase 9 WARDEN evidence is also required to close M4. Only afterward do `IMP-055`–`IMP-057` supply measured policy/ablation evidence to M5. Policy-only fixtures do not establish WARDEN/UI conformance or effectiveness. No improvement is required or presumed.

**Traceability:** `PRD OBJ-03`, `OBJ-05`, `OBJ-07`, `FR-020`–`FR-024`, `EVAL-010`, `EVAL-012`, `SEC-005`, `SEC-008`–`SEC-010`, `AC-05`, `AC-06`; `PRD §K.1–§K.6`; `SYSTEM_DESIGN §8–§9, §11`; `ML_RESEARCH_PLAN §7–§8, §10, §12`; `TECHNICAL_DESIGN §10–§11, §13`.

### Phase 9 — HAWK investigation (M3 evidence) and simulation-only WARDEN (M4 completion)

**Purpose:** Provide an analyst-facing view that exposes evidence, risk, health, uncertainty, and audit history, and a catalogue-based simulation workflow that never exercises real containment.

**Prerequisites:** HAWK `IMP-058`–`IMP-060` follows Phase 7 and the approved authentication/RBAC, API, audit, configuration, health, accessibility, and privacy contracts; it does not depend on Phase 8 or M3 exit, because it contributes to M3. DIG/response fields remain explicitly unavailable at that stage. WARDEN `IMP-061`–`IMP-063` additionally requires Phase 8's `IMP-052`–`IMP-054`, `IMP-087`, the approved response catalogue, human roles, and approval-expiry rules. WARDEN does not wait for the measured study `IMP-055`–`IMP-057`.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-058` | Build HAWK incident/history views with bounded pagination, combined filters, explicit denominators/time windows, evidence/asset/risk/DIG fields, model/preprocessing versions, and qualified/stale/unavailable states. | P0 | `IMP-050`, `IMP-051`, `IMP-017` |
| `IMP-059` | Add health/freshness/disconnection views and verify accessibility basics: labeled controls, keyboard-operable analyst flows, legible errors, consistent time basis/timezone, and no color-only risk/DIG semantics. | P0 | `IMP-017`, `IMP-058`, `NFR-010` |
| `IMP-060` | Implement authenticated analyst review actions, annotation, request-review, acknowledgement, disposition, close/reopen, actor/time/reason history, and explicit separation between a disposition and a future training label. | P0 | `IMP-050`, `IMP-058`, `SEC-003` |
| `IMP-061` | Define WARDEN's catalogue-only recommendation contract with target, effect, impact/risk, evidence, prerequisites, authorization, expiry, policy/version references, and conspicuous `SIMULATION ONLY` status under `IMP-087`. | P0 | `IMP-048`, `IMP-052`, `IMP-087`, `DEC-10` |
| `IMP-062` | Implement approval/rejection, exact target binding, revalidation, expiry, identity/permission checks, audit availability, idempotency, duplicate protection, and blocked outcomes. No endpoint/firewall/host modification or arbitrary command path may exist. | P0 | `IMP-061`, `IMP-076`, `IMP-078`, `IMP-080`, `IMP-085`, `IMP-087`, `SEC-008`–`SEC-010` |
| `IMP-063` | Conduct the human-review and simulation-only walkthrough using benign, attack/qualified, uncertain, invalid, denied, stale, replayed, duplicate, and approved-simulation fixtures; record reviewer task completion and usability issues without claiming a usability study. | P0 | `IMP-058`–`IMP-062`, `DEM-006`, `DEM-008` |

**Deliverables:** HAWK views and filter contract; health/freshness/accessibility record; review/disposition audit record; WARDEN catalogue and lifecycle; approval/revalidation/idempotency tests; simulation-only evidence; and human walkthrough record.

**Dependencies and start constraints:** The dashboard must show no traffic, no attacks, sensor failure, capture loss, model unavailable, pending explanation, stale UI, and audit gap as different states. The UI cannot imply that a disconnected or delayed view is current. WARDEN may simulate a declared effect only after authenticated human approval and current revalidation; there is no real-containment state.

**Acceptance criteria and evidence:** `FR-025`–`FR-031` require fixture evidence; filter counts must reconcile with denominators and windows; evidence and unavailable states must be visible; review history must be auditable; unauthorized/cross-role/stale/replayed/changed-target/duplicate requests must be blocked; audit failures must block sensitive actions; and independent boundary/code-path evidence must establish simulation-only behavior. `simulation_only: true` and `endpoint_changed: false` are recorded fields, not proof by themselves.

**Exit gates:** HAWK `IMP-058`–`IMP-060`, together with Phases 6–7, supplies M3 observation/investigation conformance; unavailable DIG fields do not claim M4. WARDEN `IMP-061`–`IMP-063`, together with Phase 8's `IMP-052`–`IMP-054`, supplies M4 safety, approval, revalidation, and independent no-effect evidence. The later M5 study cannot be a prerequisite for either exit. No live deployment claim follows from a UI walkthrough alone.

**Traceability:** `PRD OBJ-03`, `OBJ-04`, `OBJ-07`, `FR-025`–`FR-031`, `NFR-009`–`NFR-011`, `SEC-003`, `SEC-007`–`SEC-010`, `DEM-006`, `DEM-008`–`DEM-009`, `AC-07`, `AC-08`; `SYSTEM_DESIGN §4D–§5, §8–§11`; `ML_RESEARCH_PLAN §7, §12`; `TECHNICAL_DESIGN §5, §10–§11`.

### Phase 10 — Historical analytics, operational testing, and integrated two-system demonstration (M5 evidence)

**Purpose:** Validate descriptive history, operational health, recovery, capacity boundaries, reproducibility, and the complete two-system evidence chain under the approved lab scope.

**Prerequisites:** M3 observation/investigation conformance and M4 DIG/simulation-only conformance (`IMP-052`–`IMP-054`, `IMP-058`–`IMP-063`); approved TGT-01–07; `IMP-077`, `IMP-078`, `IMP-082`, and `IMP-083` contract records; approved retention/export/privacy rules; final-test isolation and access ledger; authorized scenario matrix and reviewers; independent truth; and a named Windows hardware/software profile. `IMP-055`–`IMP-057` are M5 study work, not prerequisites to M5.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-064` | Implement SIBYL descriptive trends by declared time window and denominator, separating predictions from confirmed dispositions and missing periods from zero. Keep forecasting disabled unless its separate gate passes. | P1 | `IMP-050`, `IMP-060`, `IMP-079`, `DEC-11` |
| `IMP-065` | Run idle, nominal, overload, restart, storage, audit, model, capture, queue, and UI-disconnect tests. Measure throughput, p50/p95/p99 latency boundaries, drops, backlog, CPU/RAM/storage and applicable GPU use, recovery, and affected-record accounting against approved TGTs and the timing/durability contracts. | P0 | `IMP-043`, `IMP-050`, `IMP-062`, `IMP-077`, `IMP-078`, `IMP-082`, `TGT-02`–`TGT-05` |
| `IMP-066` | Verify retention/deletion, minimized/redacted export, access control, audit of export/deletion, backup/retained-metadata disclosure, pseudonymization boundary, and storage growth against the approved operations/privacy policy. | P0 | `IMP-015`, `IMP-016`, `IMP-050`, `IMP-083`, `DEC-09`, `TGT-06` |
| `IMP-067` | Execute the approved integrated two-system demonstration: normal observation, supported controlled scenario, uncertainty, integrity failure, missed/timeout accounting, incident investigation, DIG, human review, denied/approved simulation, health failures, and recovery. Reconcile all IDs and timing/resource/drop evidence. | P0 | `IMP-045`, `IMP-051`, `IMP-057`, `IMP-063`, `IMP-065` |

**Deliverables:** descriptive history and drift/label-status report; performance/recovery/overload evidence; retention/export evidence; integrated two-system run record; final-evaluation preparation inputs; and a reconciled pre-final evidence index. Sealed final evaluation and exact reproducibility rerun are Phase 11 activities after the freeze gate.

**Dependencies and start constraints:** No final-test consultation may tune a model, threshold, preprocessing step, DIG parameter, or policy. A live demonstration must use authorized bounded traffic and independent truth. Performance claims must state measurement boundaries; model runtime is not end-to-end detection latency, and a measured UI update is not a hard real-time guarantee.

**Acceptance criteria and evidence:** `AC-09`–`AC-11` are addressed; historical denominators and missing periods are correct; failure/recovery accounting identifies persisted and lost events; target-dependent performance evidence uses the approved values and named profile; the full scenario-to-incident-to-review-to-simulation trace is inspectable; and cross-dataset/fallback limitations are explicit.

**Exit gate — M5 evidence gate:** All integrated must-have evidence is passed, failed with approved disposition, blocked with owner, or not applicable with evidence. No unresolved safety invariant, independent-truth break, final-test leak, or unknown measurement boundary may be presented as success.

**Traceability:** `PRD OBJ-01`–`OBJ-07`, `FR-027`, `FR-032`–`FR-040`, `NFR-002`–`NFR-006`, `NFR-009`–`NFR-014`, `DEM-006`–`DEM-009`, `EVAL-008`–`EVAL-010`, `EVAL-012`–`EVAL-013`, `AC-09`–`AC-11`; `SYSTEM_DESIGN §4F–§4G, §10–§13`; `ML_RESEARCH_PLAN §6–§10, §12`; `TECHNICAL_DESIGN §8–§12`.

### Phase 11 — Final evaluation, reproducibility audit, limitations, and presentation (M6)

**Purpose:** Produce a bounded final evidence package and presentation only after all applicable gates are reviewed. The final phase reports unmet requirements and negative findings rather than converting them into claims.

**Prerequisites:** M5/M6 input gate; complete evidence index; `P_common` and broader-coverage record; locked protocol and final-source access ledger; independent truth/adjudication; target sheet; reviewer availability; and no unresolved safety violation. `IMP-089` is the first final-source record-access gate and must be approved before `IMP-068` or `IMP-070` opens a final source.

**Planned tasks:**

| ID | Task | Priority | Dependencies |
|---|---|---:|---|
| `IMP-068` | Perform the locked EVAL-013 cross-dataset final evaluation if two suitable independent sources were approved; otherwise execute the strongest feasible independent capture-group/live fallback and label it non-equivalent. Start only after the pre-access freeze record; keep the final source sealed until the frozen run. | P0 | `IMP-026`, `IMP-034`, `IMP-039`, `IMP-067`, `IMP-089`, `EVAL-013` |
| `IMP-070` | Run the already-frozen final evaluation version using only the approved final metrics, scenarios, policy arms, resources, and live-demonstration outputs. Record any post-test change as development feedback, not independent confirmation. | P0 | `IMP-068`, `IMP-089`, final-test authorization |
| `IMP-069` | Re-run the sealed fixed evaluation after final execution and reconcile manifests, seeds, environment, package/policy versions, scenario truth, metrics, uncertainty, and deviations. Produce the reproducibility/evidence index; this is not independent confirmation. | P0 | `IMP-068`, `IMP-070`, `NFR-006` |
| `IMP-071` | Complete the requirement/acceptance traceability register for every `OBJ`, `UC`, `FR`, `NFR`, `SEC`, `DEM`, `EVAL`, `AC`, `DEC`, and applicable `TGT` identifier, with owner, evidence path/fingerprint, reviewer, and pass/fail/blocked/N/A reason. | P0 | `IMP-069`, `IMP-070` |
| `IMP-072` | Perform a reproducibility audit: clean environment/profile record, manifests/fingerprints, seeds, code/config/package/policy versions, commands, rerun tolerance, nondeterminism, deviations, and missing evidence. | P0 | `IMP-069`, `IMP-070` |
| `IMP-073` | Write the limitations and fallback report covering topology, population, labels, domain shift, feature/sequence scope, cross-dataset applicability, hardware, latency/capacity, optional features, privacy, and all unmet targets. | P0 | `IMP-068`–`IMP-072` |
| `IMP-074` | Assemble the presentation/evaluation pack: approved scope, architecture, live visibility proof, offline/live separation, model comparison, DIG results, analyst workflow, simulation-only response, evidence paths, and negative/inconclusive findings. | P0 | `IMP-071`–`IMP-073` |
| `IMP-075` | Hold human acceptance review; record final claims, accepted limitations, scope revisions, rejected claims, follow-up work, and the explicit decision to stop, continue under a new approval, or close the project. | P0 | `IMP-074`, `DEC-01` |

**Deliverables:** final evaluation report; complete traceability register; reproducibility audit; limitations/unmet-targets/fallback report; presentation pack; and signed human acceptance disposition.

**Dependencies and start constraints:** A presentation cannot hide a missing result, a blocked target, a failed safety fixture, an unproven live path, or a final-source access-ledger gap. No production-readiness, zero-day, universal-coverage, guaranteed-prevention, hard-real-time, or autonomous-response claim is permitted (`PRD §D.4`).

**Acceptance criteria and evidence:** every requirement is explicitly accounted for; the final claims match the approved population and evidence; re-runs and deviations are disclosed; optional/unsupported items are labeled; safety restrictions remain demonstrable; and a human reviewer accepts or rejects the scope. If a must-have remains unmet, the result is an unmet-requirement record and approved scope revision, not a silent pass.

**Exit gate — M6:** Supervisor/project owner accepts the final evaluation package, traceability, limitations, and presentation, or records that the project is incomplete/blocked. This plan stops at human review; it does not authorize additional implementation or external action.

**Traceability:** `PRD §B`, §D, §J–§P, `OBJ-01`–`OBJ-07`, `AC-01`–`AC-11`, `TGT-01`–`TGT-07`; `SYSTEM_DESIGN §13–§14`; `ML_RESEARCH_PLAN §10–§13`; `TECHNICAL_DESIGN §13–§15`.

## 5. Critical path and parallel work

### 5.1 Critical path

The minimum evidence path is:

`M0 approval (IMP-001–006)`  
→ `foundation contracts (IMP-076–085, IMP-087)` **and** `Windows visibility and privilege proof (IMP-007–012)` **and** `dataset/feature/split/target gate (IMP-019–027, IMP-086)`  
→ `reproducible baseline and representation decision (IMP-028–034)`  
→ `compatible package and inference contract (IMP-035–039)`  
→ `live flow/feature parity (IMP-040–045)`  
→ `incident/context/evidence/risk (IMP-046–051)`  
→ `functional DIG safety invariants and B0–B3 study (IMP-052–057)`  
→ `HAWK/analyst/WARDEN simulation controls (IMP-058–063)`  
→ `operational tests and integrated two-system evidence (IMP-064–067)`  
→ `pre-access final-protocol freeze (IMP-089)`  
→ `sealed final evaluation (IMP-068, IMP-070)`  
→ `exact reproducibility rerun (IMP-069)`  
→ `final audit and human acceptance (IMP-071–075)`.

The two earliest branches are intentionally coupled at their boundary: model training may not commit to a feature that cannot be reconstructed on the approved live path. Packet visibility and feature compatibility therefore receive priority over model architecture work. The first usable offline result is not a milestone if `IMP-010` or `IMP-022` is blocked.

### 5.2 Work that may run in parallel after its prerequisites

| Parallel stream | May proceed after | Must not cross |
|---|---|---|
| Windows inventory/visibility (`IMP-007`–`IMP-012`) | M0 and safety scope | Do not run traffic or capture outside the approved scope; no model evaluation before visibility proof. |
| Repository/contracts/test harness (`IMP-013`–`IMP-018`, `IMP-076`–`IMP-085`, `IMP-087`) | M0 and initial stack boundary | Do not assume an unapproved framework, package, driver, topology, identity, storage, or response path. |
| Dataset candidate assessment (`IMP-019`–`IMP-021`) | M0 and lawful data access | Do not train, open final data, or claim label quality before manifest/truth review. |
| Feature/flow/split/access protocol (`IMP-022`–`IMP-027`, `IMP-081`–`IMP-083`) | Early capture semantics and permitted candidate inspection | Do not freeze live semantics or open final records without visibility/compatibility evidence and the approved access class. |
| Fixture-backed HAWK contract/UI design (`IMP-058`–`IMP-060`) | Approved incident/health contracts | Do not claim live dashboard freshness or usability from fixtures alone. |
| CENSUS/ORACLE contract design (`IMP-047`–`IMP-049`) | Approved incident schema and privacy/asset/explanation decisions | Do not invent identity, evidence, causal explanations, or ATT&CK mappings. |

### 5.3 Tasks that cannot start until a prerequisite is verified

| Cannot start | Required verified prerequisite |
|---|---|
| Any package/driver installation, capture, traffic, training, or security test | M0 scope and explicit activity authorization (`IMP-003`, `IMP-006`). |
| `IMP-010` controlled visibility proof | Approved topology, bounded traffic plan, operator, emergency stop, and candidate backend review (`IMP-008`–`IMP-009`). |
| `IMP-022` live-compatible feature freeze | Independently evidenced observation path plus dataset field/provenance inspection (`IMP-010`, `IMP-019`–`IMP-021`). |
| `IMP-024` final split freeze | Immutable manifest, label mapping, flow/sequence semantics, and duplicate/group audit plan (`IMP-020`–`IMP-023`). |
| `IMP-028`–`IMP-034` model training | M1 data/feature/leakage/target gate and approved environment (`IMP-014`, `IMP-019`–`IMP-027`). |
| `IMP-035`–`IMP-039` deployment package | Validation-only selection, preprocessing, labels, thresholds, and calibration decision (`IMP-034`). |
| `IMP-044` approved live scenario evaluation | Live parity and safety fixtures, written scenario truth, isolation, and operator authorization (`IMP-003`, `IMP-040`–`IMP-043`). |
| `IMP-052`–`IMP-054` DIG conformance | M3 evidence; approved policy/response decisions, `IMP-087`, `IMP-088`, and safety invariants (`IMP-045`, `IMP-048`–`IMP-051`, `DEC-07`). |
| `IMP-055`–`IMP-057` measured B0–B3 study | M4 conformance including `IMP-063`; independent truth/adjudication, approved policy/response rubric, frozen base detector, applicable targets, and authorized fault fixtures (`IMP-025`, `IMP-026`, `DEC-10`, `DEC-14`). |
| `IMP-062` simulation | Exact approved catalogue entry, current evidence, authenticated authorized analyst, audit availability, freshness, and target binding (`IMP-061`). |
| `IMP-067` integrated demonstration | All component gates, approved TGTs, two-system visibility, independent truth, emergency stop, and human reviewers. |
| `IMP-068` final cross-dataset test | EVAL-013 applicability decision, `IMP-089` pre-access freeze, and frozen development choices; otherwise only the explicitly labeled non-equivalent fallback. |
| `IMP-070` final evaluation | `IMP-089` pre-access freeze, sealed final source, locked protocol, and human approval to open/run it. |

## 6. Testing strategy and evidence gates

Testing is a future implementation obligation. No test, capture, model run, attack scenario, or performance measurement was executed by creating this plan. Test identifiers should follow the conventions in `TECHNICAL_DESIGN §11` and `PRD §O.3`, for example `TEST-FR-003-01`, `TEST-SEC-009-01`, `EXP-EVAL-010-B3`, and `DEMO-DEM-006-01`.

Every test or experiment record must retain, as applicable: fixture or manifest and fingerprint; expected and actual output; source/configuration/policy/model versions; environment and driver profile; seed; start/end and timestamp basis; measurement boundary and units; affected IDs; logs/audit links; reviewer; and pass/fail/blocked/not-applicable disposition. Secrets, credentials, unrestricted payloads, and private notes do not belong in evidence.

| Test category | Minimum scope | Required evidence to pass the relevant gate |
|---|---|---|
| **Unit** | Flow keys/direction/expiry; timestamp and clock handling; feature type/unit/finite/range/missing checks; label mapping; thresholds and abstention; risk boundaries/monotonicity/rounding; DIG precedence; correlation windows; idempotency; lifecycle; redaction. | Deterministic fixtures with positive, negative, boundary, unknown, invalid, and failure cases; expected/actual results; coverage of the linked requirement; no unexamined exception path. |
| **Contract** | Versioned observation/flow/feature/prediction/incident/asset/risk/DIG/recommendation/audit/health/evaluation records; API schemas; error codes; pagination/filter denominators; package manifest compatibility. | Producer/consumer compatibility matrix; invalid-schema rejection without state change; version and migration disposition; request correlation and safe error evidence. |
| **Integration** | Capture adapter→flow/features; features→ARGUS; persistence/outbox/replay; CENSUS snapshots; ORACLE status; risk/DIG/WARDEN; HAWK publication; SIBYL aggregates; restart/storage/audit/model/UI failures. | Reconciled IDs and states across module boundaries; explicit degraded behavior; persistence/loss/replay accounting; no silent fallback or duplicate incident/action. |
| **Security and privacy** | Unauthenticated/cross-role denial; expiry/revocation; input size/type/range; unsafe deserialization; secret/log/export scanning; capture privilege separation; stale/replayed/changed approvals; audit-write failure; idempotency; inert rendering. | Denial and failure audit records; proof of no state change where required; no real-action invariant; redaction review; privilege/process evidence; security reviewer disposition. |
| **Model evaluation** | Manifest/rights/labels; group/time leakage audit; training-only preprocessing; trivial/classical/CNN/LSTM/CNN-LSTM matrix; calibration/threshold/abstention; per-class support; uncertainty; package parity; validation-only selection; cross-dataset applicability. | Immutable ledger, split/group manifest, failed-run records, metrics with denominators, seeds/environment, resource and latency boundaries, final-test lock record, and reviewer approval. |
| **Failure injection** | Malformed or reordered features; unit/schema mismatch; corrupt/incompatible model; packet loss/partial/reversed/duplicate/late flows; shifted/unseen permitted class; ambiguous score; stale/unknown asset; conflicting evidence; high-confidence safety failure; stale/replayed approval; audit/storage/model/UI outage; overload. | Independently known fault, clean control, expected/actual outcome, observability limitation, affected IDs, recovery result, safety invariant result, and honest “not observable” disposition where applicable. |
| **Performance and capacity** | Idle/nominal/overload and burst/sustained workloads; inference, feature-ready-to-visible, and observable-onset-to-visible latencies; throughput; drops/backlog; CPU/RAM/storage/GPU if applicable; recovery. | Named Windows profile; approved TGT values; p50/p95/p99/max and failure counts; offered/captured/processed rates; queue/drop/resource samples; timing boundary diagram; no hard-real-time claim without evidence. |
| **Authorized live-lab** | Two-system/isolated-VM topology; known benign visibility; approved controlled scenario; uncertainty/integrity/denied-response fixtures; scenario truth and clock offset; UI/review/simulation history; stop and recovery. | Written scope and stop record; independent markers/counts; scenario-to-flow/incident matching; all misses/false alarms/timeouts; resource/drop/health evidence; proof of no out-of-scope traffic and no endpoint modification. |

### 6.1 Test exit rules

- A requirement is not passed by a test that uses an unapproved scope, data source, target, or measurement boundary.
- A test with no result, missing denominator, missing independent truth, unknown loss, or unresolved clock boundary is blocked or limited, not passed.
- Negative tests are first-class evidence. A denied operation, quarantined input, stale approval, model-unavailable state, or blocked simulation is often the expected safe result.
- Performance tests use the target sheet approved at M1. Before that sheet exists, the test may characterize behavior but cannot pass performance acceptance.
- Live-lab tests are separately labeled from offline results. An offline metric cannot be copied into a live result, and a live walkthrough cannot replace the offline experiment matrix.

## 7. ML experiment gates

These gates prevent training or a successful run from being mistaken for an accepted detector. They apply to the baseline and every deep-learning arm.

| Gate | Required before proceeding | Blocking condition |
|---|---|---|
| **ML-G0 — authorization** | M0 scope, roles, lawful data access, compute/traffic boundaries, and safety approval. | Any unapproved installation, data use, traffic, or security test. |
| **ML-G1 — dataset suitability** | Rights/provenance, collection semantics, label authority, support, timestamps/groups, duplicates/missingness, feature definitions, and live compatibility assessment. | No lawful/suitable source, unknown provenance, or feature semantics that cannot be reconstructed or explicitly removed. |
| **ML-G2 — label and truth mapping** | Versioned normal/attack/supported/unsupported/unknown/invalid mapping; independent scenario truth and response adjudication plan. | Unknown labels silently mapped, predictions used as truth, or no adjudication path for response correctness. |
| **ML-G3 — split isolation** | Group/time/session/host/capture split; duplicate/window/marker/augmentation audit; untouched final source; preprocessing-fit boundaries. | Related records cross partitions, final source consulted, or leakage remains unresolved. |
| **ML-G4 — feature and representation semantics** | Feature names/order/types/units/direction/missing rules; flow/partial policy; CNN layout rationale; sequence ordering/padding/masking/warm-up if applicable. | Convenient constants, future/post-incident leakage, tabular-as-image assumption without rationale, or invented temporal order. |
| **ML-G5 — protocol and targets** | Fixed evaluation units, metrics/denominators, uncertainty method, tuning budgets, compute/resource measures, operating point, and approved `TGT-01`–`TGT-07`. | No target sheet, mixed denominators, unsupported metric claim, or unfair budget. |
| **ML-G6 — comparison ledger** | Trivial predictor, classical baseline, CNN, LSTM, CNN-LSTM where applicable; configurations, seeds, failures, costs, and reasons retained. | Missing planned arm without reason, hidden failed run, or architecture preference presented as evidence. |
| **ML-G7 — validation-only selection** | Model, preprocessing, feature set, threshold, calibration, abstention, DIG parameters, and package choice frozen from development/validation data. | Any final-test consultation used for tuning or selection. |
| **ML-G8 — independent/live evaluation** | Cross-dataset applicability decision; untouched independent source when applicable; separate live feature-parity and two-system evidence. | Benchmark metrics transferred to live claims, or non-equivalent fallback described as cross-dataset validation. |
| **ML-G9 — package acceptance** | Manifest binding artifact and semantics; fingerprint/compatibility/rollback tests; score meaning and calibration status; CPU/reproducibility evidence. | Incompatible package activation, arbitrary executable deserialization, or silent model fallback. |

If no valid sequence exists, LSTM and CNN-LSTM are recorded as unsupported/not applicable for the approved task and a scope/data decision is requested. This is an honest result, not a reason to fabricate sequences or to claim the classical baseline is equivalent evidence for temporal learning. No gate promises that CNN-LSTM will win.

## 8. Safety gates and non-negotiable restrictions

The following restrictions apply to every phase, experiment arm, ablation, demonstration, and fallback. A research comparison cannot disable them.

| Safety gate | Required control | Linked source requirements |
|---|---|---|
| **SG-01 Scope** | Written owner approval, systems/interfaces, scenario categories, traffic limits, start/end, isolation, operator emergency stop, and out-of-scope rejection before capture or traffic. | `SEC-001`, `DEM-008` |
| **SG-02 Least privilege** | Capture privileges are isolated to the necessary component/user; UI/API does not inherit elevation; actual Windows privilege boundary is verified. | `SEC-002`, `DEM-003` |
| **SG-03 Data minimization** | Approved metadata/features only; payload capture disabled by default; no credentials, unrestricted payload retention, unrelated switched traffic, or external evaluation leakage. | `SEC-006`, `FR-040`, `NFR-012` |
| **SG-04 Untrusted input** | Bound sizes/resources; parse packets, imports, API fields, model metadata, notes, and explanation inputs as untrusted; prohibit embedded commands and executable deserialization; render text inert. | `SEC-005`, `SEC-012` |
| **SG-05 Authorization** | Authenticate and authorize protected operations by role; test unauthenticated, cross-role, expired/revoked, and unauthorized approval attempts. | `SEC-003`–`SEC-004`, `FR-037` |
| **SG-06 Fail closed** | Integrity/safety fail or unknown blocks response eligibility; invalid features are quarantined; unavailable models and audit-write failures expose degraded state; stale/replayed/changed approvals block simulation. | `FR-020`–`FR-022`, `SEC-008`–`SEC-010`, `NFR-005` |
| **SG-07 Human approval** | Exact catalogue recommendation, target, evidence/policy versions, expiry, authenticated analyst approval, and simulation-time revalidation are required. | `FR-029`–`FR-031`, `SEC-009` |
| **SG-08 Simulation only** | No real containment, host isolation, process termination, firewall change, arbitrary command, or disruptive connector. Every consequential request is visibly and audibly `SIMULATION ONLY`; prove endpoint unchanged. | `OBJ-07`, `PRD §D.4`, `FR-031`, `DEM-008` |
| **SG-09 Auditability** | Record allowed, denied, failed, blocked, attempted, approval, rejection, simulation, and audit-gap outcomes; no silent rewrite of prior audit history. | `FR-036`, `SEC-007`, `SEC-010` |
| **SG-10 Independent truth** | Scenario labels, response adjudication, and analyst disposition are independently retained and not generated from model confidence, risk, DIG output, or predictions. | `EVAL-012`, `PRD §H`, `ML_RESEARCH_PLAN §7` |

This plan itself performs none of the restricted actions. Any later task that would create traffic, capture packets, test authorization, or inject a fault requires the relevant approval record and must stop if the emergency-stop or isolation condition is not verified.

## 9. Milestones and progress reporting

### 9.1 Gate milestones

| Milestone | Required evidence for review | Stop condition |
|---|---|---|
| **M0 — governance and documentation approval** | Approved scope, owners, safety, role separation, document precedence, canonical crosswalk, decision register, and next-step authorization. | No authority, disputed safety boundary, or unresolved scope that changes the product. |
| **M1 — feasibility, data, semantics, and evaluation protocol** | Windows visibility/privilege proof; topology; candidate rights/labels; feature/flow/sequence matrix; `P_common`; split/leakage audit; Access Class A/B ledger; EVAL-013 applicability; actionable targets and budgets. | No lawful/live-compatible feature set, no reliable visibility, unresolved leakage/common population/access boundary, or no approved target sheet. |
| **M2 — reproducible development experiments and package** | Trivial/classical/CNN/LSTM/CNN-LSTM ledger as applicable on matched `P_common`; failed-run reasons; validation-only selection; uncertainty/cost characterization; compatible package. | Sequence semantics unsupported without a scope decision, final-source consultation, leakage unresolved, or resources prevent the approved matrix without revision. |
| **M3 — integrated observation and investigation conformance** | Capture-to-flow/features/model parity; correlation; CENSUS; risk/confidence separation; ORACLE; HAWK functional evidence; lineage and measured boundaries. | Invalid inputs treated as benign, missing evidence invented, or confidence/risk conflated with permission. |
| **M4 — DIG and simulation-only response conformance** | Four DIG check families; approved decision tables; fail-closed fixtures; WARDEN catalogue; approval/revalidation/idempotency; simulation-only/no-effect evidence. | Any unsafe eligibility/simulation, real-action path, missing audit, or failed safety invariant. |
| **M5 — historical, operational, and policy study** | SIBYL descriptive trends; outages/recovery/overload; retention/export; B0–B3, ablations, injections, independent adjudication, workload/overhead, negative findings, integrated evidence. | Missing denominators, circular truth, unbounded loss, or unfair policy comparison. |
| **M6 — final sealed evaluation, reproducibility, and human acceptance** | `IMP-089` freeze; sealed final/cross-dataset source record where applicable; final metrics; reproducibility audit; full traceability; limitations/unmet targets; human acceptance. | Any claim exceeds evidence, final access precedes freeze, a must-have is silently omitted, or a safety/independent-truth gate remains unresolved. |

No milestone has been completed in this document. Dates, durations, and completion percentages are intentionally absent until the project owner approves scope and capacity.

### 9.2 Completion checklist

The following checklist is a future review instrument. All boxes are intentionally unchecked in this initial plan.

- [ ] M0 approval record and authorized-lab scope are reviewed (`IMP-001`–`IMP-006`).
- [ ] Windows profile, topology, packet visibility, clock, privilege, and emergency-stop evidence are reviewed (`IMP-007`–`IMP-012`).
- [ ] Repository, environment, authentication/session, API, envelope, persistence, audit, timing, privacy, artifact-trust, simulation-boundary, configuration, logs, health, and test conventions are approved (`IMP-013`–`IMP-018`, `IMP-076`–`IMP-085`, `IMP-087`).
- [ ] Dataset rights/provenance, manifests, labels, independent truth, feature matrix, flow/sequence semantics, `P_common`, splits, Access Class A/B ledger, targets, and EVAL-013 decision are frozen (`IMP-019`–`IMP-027`, `IMP-086`).
- [ ] All planned model arms and failed/infeasible runs are accounted for; final-test isolation and validation-only selection are evidenced (`IMP-028`–`IMP-034`).
- [ ] Model package compatibility, fingerprint, activation/rollback, score meaning, abstention, and CPU parity are evidenced (`IMP-035`–`IMP-039`).
- [ ] Live flow construction, feature parity, loss/queue health, scenario matching, and two-system evidence pass or have approved limitations (`IMP-040`–`IMP-045`).
- [ ] Correlation, assets, risk/confidence separation, evidence, explanations, and incident lifecycle pass or have approved scope dispositions (`IMP-046`–`IMP-051`).
- [ ] DIG invariants, B0–B3, ablations, injections, independent adjudication, workload, overhead, and negative results are recorded (`IMP-052`–`IMP-057`).
- [ ] HAWK review/accessibility/health and WARDEN approval/revalidation/simulation-only evidence pass (`IMP-058`–`IMP-063`).
- [ ] SIBYL, operational failures/recovery, retention/export, and integrated demo are recorded (`IMP-064`–`IMP-067`); the pre-access freeze, sealed final evaluation, and exact reproducibility rerun are recorded in order (`IMP-089`, `IMP-068`, `IMP-070`, `IMP-069`).
- [ ] Final traceability, limitations, evidence audit, presentation, and human acceptance disposition are complete (`IMP-070`–`IMP-075`).

### 9.3 Progress report template

Each progress report should be saved in the approved project evidence location after implementation is authorized. The template must not be used to imply work that was not run.

```text
WallGrid AI Implementation Progress Report

Report version / reviewer:
Reporting scope and approval record:
Current milestone and gate:
Report date/time basis:

1. Completed work since prior report
   - IMP task IDs and exact outputs:
   - Source decisions resolved or changed (preserve DEC/ADR/TGT IDs):
   - Scope changes and rationale:

2. Checks and experiments actually run
   - Test/experiment IDs:
   - Command/configuration/environment/profile:
   - Fixture/manifest/model/policy versions:
   - Expected result:
   - Actual result:
   - Metrics and denominators:
   - Measurement boundaries, timestamps, and resource evidence:
   - Pass/fail/blocked/not-applicable disposition:

3. Safety and authorization
   - Approved lab scope and run authorization:
   - Operator and reviewer:
   - Emergency-stop readiness:
   - Traffic/capture limits and privilege boundary:
   - Confirmation of no real containment/arbitrary command path:

4. Blockers and risks
   - BLK/READ IDs:
   - Affected IMP tasks and acceptance criteria:
   - Evidence missing or unknown:
   - Proposed fallback and why it is not equivalent:

5. Evidence paths and fingerprints
   - Decision/approval records:
   - Manifests and split/leakage records:
   - Test/experiment ledgers:
   - Logs/audit/health records:
   - Live scenario and resource records:
   - Reproducibility artifacts:

6. Next task
   - Exactly one next IMP task and prerequisite gate:
   - Owner/reviewer:
   - Required decision or evidence:
   - Stop condition:
```

## 10. Risks and fallback paths

Fallbacks reduce claims; they do not become equivalent evidence by being convenient. Every fallback must identify the original requirement, the evidence it cannot provide, the new scope boundary, and the approval needed to continue.

| Risk or trigger | Safe fallback path | Evidence and non-equivalence statement |
|---|---|---|
| **Packet capture or Windows visibility fails**: permission denied, wrong interface, mirror disabled, backend failure, or unexplained empty capture. | Stop live evaluation. Record the failure; request a topology, driver/backend, privilege, or scope decision. Continue only with offline fixtures/replay for work they actually support. | Offline/replay evidence does not prove two-system live visibility, capture loss behavior, or live feature parity. It cannot replace `DEM-001`–`DEM-003` or `DEM-006`. |
| **No two systems/VMs or no isolated authorized network** | Reduce to contract/offline work while seeking resources or revise the project scope with approval. Do not present a single-host screenshot as the two-system demonstration. | A single-host or prerecorded path is not equivalent to `OBJ-06`, `DEM-001`, `DEM-002`, or live operational evidence. |
| **Dataset lacks lawful access, reliable labels, timestamps, groups, or live-compatible fields** | Reject the candidate; retain the reason. Select another approved source, remove incompatible fields from both paths, reconstruct only with validated semantics, or request a binary/scope revision. | A convenient dataset or guessed label map is not evidence. Removing fields or restricting classes changes the population and must be reported. |
| **No suitable sequence semantics** | Keep the classical baseline and any semantically justified tabular model; mark LSTM/CNN-LSTM not applicable or blocked and request a scope/data decision. | A non-temporal sequence fabricated from row order is not evidence of temporal learning. The fallback is not equivalent to the required sequence comparison. |
| **Cross-dataset comparison is infeasible** | Complete and retain EVAL-013 applicability analysis; use independent capture-group holdouts and controlled live-lab evaluation as the strongest feasible alternative. | This fallback is explicitly **not equivalent** to cross-dataset validation. Report absent source diversity, domain shift limits, and unsupported classes. |
| **CPU/RAM/storage/toolchain limits prevent the approved matrix** | Use an approved bounded feasibility slice, reduce optional features/tuning budget, retain out-of-memory/failed runs, and seek scope revision. CPU is the baseline; GPU is optional and not a hidden prerequisite. | A reduced matrix does not establish the omitted arms or performance targets. Report hardware, budget, omitted experiments, and consequences. |
| **Live/training feature parity fails** | Revisit `IMP-022`–`IMP-024`: remove/reconstruct with evidence, retrain, or change scope. Do not fill a missing feature with a convenient constant. | Offline results from the incompatible feature set cannot support live claims. |
| **Targets or performance budgets are unmet or absent** | Record the measured result and boundary; do not label real-time or accepted performance. Seek a predeclared target/scope revision before further final evaluation. | An unmet target is not a pass, and changing it after seeing the held-out result requires a new evaluation version. |
| **Labels or independent response adjudication are ambiguous** | Report unknown/insufficient evidence; restrict supported claims; obtain supervisor adjudication or revise the response study. | Model output, confidence, risk, DIG, or analyst belief cannot define ground truth. |
| **Explanation/ATT&CK method is unsuitable or unavailable** | Show evidence and explicit explanation-unavailable status; request a scope exception. Keep attributions separate from causal claims and leave unsupported mappings unmapped. | A deterministic summary or feature attribution without the required evidence is not a causal explanation or ATT&CK validation. |
| **Risk/DIG checks are unobservable, circular, or add excessive review** | Preserve the four check contract, document unknowns, run ablations/matched comparisons where feasible, and report negative/inconclusive outcomes. Do not convert a circular check into independent evidence. | A policy may add delay, abstention, workload, or no benefit; no superiority claim is permitted. |
| **Storage, audit, model, UI, or capture outage** | Expose degraded/unknown/stale state; bound queues; block sensitive configuration/approval/simulation when required; recover with new provenance and reconcile lost events. | Recovery evidence must account for persisted and lost records. A system that silently continues or silently drops data fails the relevant gate. |
| **Optional history/forecasting/anomaly/multiclass feature is not validated** | Leave it disabled or mark unsupported and complete the must-have binary/descriptive/safety path. | Optional functionality is not accepted by a screenshot or an unvalidated prediction; it must not dilute the core evidence. |
| **Unauthorized traffic or scope escape is suspected** | Use the emergency stop, isolate the lab, preserve the audit record, notify the owner, and suspend the run pending review. | No result from a scope-violating run is valid project evidence; safety review takes precedence over schedule or completeness. |

## 11. Traceability map

### 11.1 Phase-to-source map

The task register in §12 supplies the row-level `IMP-*` links. This map supplies the higher-level relationship among phases, PRD requirements/acceptance, System Design components, and the relevant Technical Design/ML Research Plan sections.

| Phase | Main PRD objectives/requirements and acceptance | System Design components/sections | Technical Design sections | ML Research Plan sections |
|---|---|---|---|---|
| 0 | `OBJ-01`–`OBJ-07`; `DEC-01`, `DEC-12`; `SEC-001`, `SEC-008`; `AC-01`–`AC-11` as future obligations | §1–§2, §9, §12–§14 | §1, §14–§15 | §1, §10, §13 |
| 1 | `FR-001`, `FR-006`; `NFR-007`, `NFR-013`; `SEC-001`–`SEC-002`; `DEM-001`–`DEM-003`; `AC-01` | Capture/observation; §5, §10–§11 | §3, §8, §10–§11 | §3.1, §8, §10 |
| 2 | `FR-036`–`FR-040`; `NFR-005`–`NFR-008`, `NFR-011`–`NFR-013`; `SEC-003`–`SEC-012` | Persistence, audit, health, configuration, auth; §3, §6, §9–§11 | §4–§6, §9–§12 | §9–§10 |
| 3 | `EVAL-001`–`EVAL-004`, `EVAL-012`–`EVAL-013`; `FR-002`–`FR-006`; `NFR-001`, `NFR-006`; `DEM-004`–`DEM-005`; `DEC-03`–`DEC-05`, `DEC-13`–`DEC-14` | Offline path and model/feature lifecycle; §4A, §6–§7, §13 | §6–§7, §13–§14 | §3–§6, §8–§9, §12–§13 |
| 4 | `OBJ-02`; `FR-007`–`FR-012`; `EVAL-004`–`EVAL-007`, `EVAL-009`, `EVAL-011`; `NFR-001`, `NFR-004`, `NFR-006`; `AC-02` | ARGUS/offline lifecycle; §4A, §7, §13 | §3.1, §7, §11–§13 | §1, §5–§6, §8–§10, §12 |
| 5 | `FR-003`, `FR-007`–`FR-011`, `FR-038`–`FR-039`; `SEC-005`, `SEC-011`; `NFR-006`; `DEM-005`; `AC-02` | ARGUS/package lifecycle; §4A, §7, §11 | §5–§7, §10–§11 | §5.3–§6, §8–§10 |
| 6 | `OBJ-01`, `OBJ-02`, `OBJ-06`; `FR-001`–`FR-011`; `NFR-002`–`NFR-005`, `NFR-007`, `NFR-011`, `NFR-013`; `DEM-001`–`DEM-007`; `AC-01`, `AC-02`, `AC-11` | Capture, flow/features, ARGUS, health; §3.2, §4B, §5–§7, §10–§11 | §5–§8, §10–§11 | §3.1, §5, §8, §10, §12 |
| 7 | `OBJ-03`, `OBJ-04`; `FR-013`–`FR-024`, `FR-035`–`FR-036`; `SEC-005`, `SEC-007`, `SEC-012`; `AC-03`, `AC-04`, `AC-06` | Correlation, CENSUS, ORACLE, risk, persistence; §3.1, §4C, §6, §8–§9 | §5–§7, §10–§11 | §6–§7, §12 |
| 8 | `OBJ-03`, `OBJ-05`, `OBJ-07`; `FR-020`–`FR-024`; `EVAL-010`, `EVAL-012`; `SEC-005`, `SEC-008`–`SEC-010`; `AC-05`, `AC-06`; `H-DIG-01` | DIG, risk, WARDEN safety; §8–§9, §11 | §10–§11, §13 | §7–§8, §10, §12 |
| 9 | `OBJ-03`, `OBJ-04`, `OBJ-07`; `FR-025`–`FR-031`; `NFR-009`–`NFR-011`; `SEC-003`, `SEC-007`–`SEC-010`; `DEM-006`, `DEM-008`–`DEM-009`; `AC-07`, `AC-08` | HAWK, WARDEN, auth, audit, health; §4D–§5, §8–§11 | §5, §10–§11 | §7, §12 |
| 10 | `OBJ-01`–`OBJ-07`; `FR-027`, `FR-032`–`FR-040`; `NFR-002`–`NFR-006`, `NFR-009`–`NFR-014`; `DEM-006`–`DEM-009`; `EVAL-008`–`EVAL-010`, `EVAL-012`–`EVAL-013`; `AC-09`–`AC-11` | SIBYL/export, health, persistence, evaluation; §4F–§4G, §10–§13 | §8–§12 | §6–§10, §12 |
| 11 | `OBJ-01`–`OBJ-07`; all applicable `AC`, `TGT`, `EVAL`, `DEM`, `SEC`, `NFR`, and `FR` rows; `PRD §O–§P` | Traceability and design review; §13–§14 | §13–§15 | §10–§13 |

### 11.2 Traceability rules

- The phase map is an index, not a blanket pass. Every individual `FR-*`, `NFR-*`, `SEC-*`, `DEM-*`, and `EVAL-*` identifier needs an evidence row before release acceptance (`PRD §O.2`).
- Acceptance IDs `AC-01`–`AC-11` are evidence bundles, not implementation tasks. Each bundle must include positive, negative, boundary, and failure-path fixtures where applicable.
- `DEC-*`, `ADR-*`, and `TGT-*` entries retain their source names and statuses. A plan task may resolve one only at its approved gate and with recorded rationale, impact, owner, and reviewer.
- A `not applicable` result is valid only when the source-defined applicability condition is assessed and the reason is retained. It is not a way to avoid an inconvenient experiment.
- The final register must distinguish `passed`, `failed`, `blocked`, and `not applicable`; unknown or missing evidence never becomes a pass.

## 12. Task register

Sections 12.1–12.5 contain exactly one canonical definition for each `IMP-001`–`IMP-089`. Phase rows are scheduling summaries of the same tasks, not additional definitions; their prerequisites must agree with this register. `IMP-086` is defined only in §12.4 and follows M1 before ORACLE work. All statuses below are initial plan statuses. None asserts completed implementation or testing.

Task prerequisites are directed dependencies. PRD requirement IDs are governing constraints, not tasks that must already have passed. DEC/ADR references require approved stage-specific inputs before implementation; a contract-definition task may prepare the design evidence for that decision rather than require its own approval output as an input. Contract review/approval is required before any consuming implementation, and later verification cannot be a prerequisite for drafting that contract. A milestone names reviewed completion of its contributing tasks, never an input to those same tasks. Status labels such as “M3 decisions” identify pending decisions for that stage, not M3 exit; explicit milestone prerequisites control.

### 12.1 Governance, feasibility, foundation, and M1 planning tasks

| ID | Task | Priority | Prerequisites | Outputs | Test evidence | Linked PRD requirement IDs | Status |
|---|---|---:|---|---|---|---|---|
| `IMP-001` | Review source documents and preserve status | P0 | Four upstream source docs, this plan, and two governance/review registers | Review record; source/status checklist | Attributable review and contradiction log | `OBJ-01`–`OBJ-07`; `DEC-01`–`DEC-14`; `TGT-01`–`TGT-07`; `PRD §Document governance` | Pending human review |
| `IMP-002` | Assign authority, owners, roles, capacity | P0 | `IMP-001` | Approval/RACI record; `DEC-01`, `DEC-12` entries | Owner acceptance and role-separation record | `DEC-01`, `DEC-12`; user roles in `PRD §C` | Pending human review |
| `IMP-003` | Approve lab scope and emergency stop | P0 | `IMP-002` | Scope, isolation, limits, stop procedure | Scope approval; safety checklist | `SEC-001`, `SEC-006`, `SEC-008`; `DEM-008` | Pending human review |
| `IMP-004` | Confirm release boundaries and scope cuts | P0 | `IMP-001`–`IMP-003` | Must/should/optional/out-of-scope record | Reviewed scope matrix | `OBJ-01`–`OBJ-07`; `FR-012`, `FR-019`, `FR-033`; `PRD §D` | Pending human review |
| `IMP-005` | Create decision/assumption/blocker register | P0 | `IMP-002`–`IMP-004` | DEC/ADR/TGT register with owners/gates | Register review; no silently resolved item | `DEC-01`–`DEC-14`; `TGT-01`–`TGT-07`; `PRD §P` | Pending human review |
| `IMP-006` | Approve roadmap and next activity only | P0 | `IMP-003`–`IMP-005` | Explicit authorization note | Human approval; implementation still unstarted | All applicable; `PRD §A`, §P | Pending human review |
| `IMP-007` | Inventory Windows profile and permissions | P0 | M0; `IMP-003` | OS/hardware/interface/backend/clock inventory | Read-only inventory review | `DEC-02`; `DEM-003`; `NFR-007`, `NFR-013` | Blocked: M0 |
| `IMP-008` | Select and document observation topology | P0 | `IMP-007`; `DEC-02` | Topology/coverage/isolation decision | Independent topology review | `DEM-001`–`DEM-003`; `FR-001` | Blocked: M0 |
| `IMP-009` | Assess capture backend/tool candidates | P0 | `IMP-007`, `IMP-008` | Availability/privilege/support matrix | Backend feasibility record; no assumption of install | `FR-001`; `SEC-002`; `DEM-003` | Blocked: M0 |
| `IMP-010` | Prove bounded packet visibility | P0 | `IMP-008`, `IMP-009`, `IMP-076`, `IMP-083`; scope auth | Marker/count/permission/interface evidence | Positive marker plus wrong-interface/denied/empty controls | `FR-001`, `FR-006`; `DEM-002`–`DEM-003`; `SEC-001` | Blocked: M0 |
| `IMP-011` | Record clock, resource, loss boundaries | P1 | `IMP-010`, `IMP-082` | Clock/resource/loss measurement record | Timestamp and boundary review | `FR-006`, `FR-039`; `NFR-002`, `NFR-004`, `NFR-013` | Blocked: M0 |
| `IMP-012` | Verify capture privilege and stop boundary | P0 | `IMP-003`, `IMP-010`, `IMP-076`, `IMP-083` | Privilege/stop/isolation review | Denial, separation, independent-stop, and payload-boundary tests | `SEC-001`, `SEC-002`, `SEC-006`, `SEC-010`; `DEM-008` | Blocked: M0 |
| `IMP-013` | Establish repository and change control | P0 | M0; `IMP-006` | Repository policy; task/evidence conventions | Review/change-control check | `NFR-006`, `NFR-008`; `PRD §O` | Blocked: M0 |
| `IMP-014` | Define reproducible Windows environment | P0 | `IMP-007`, `IMP-013`; stack decision | Runtime/lock/driver profile | Environment reproducibility record | `NFR-006`, `NFR-007`; `DEC-02`, `DEC-12` | Blocked: M0 |
| `IMP-015` | Define versioned configuration and secret boundary | P0 | `IMP-013`, `IMP-014`, `IMP-076`, `IMP-078`, `IMP-080`, `IMP-083`; `DEC-09` | Config schemas; secret-reference policy | Invalid config/no-state-change and secret scan | `FR-037`, `FR-038`, `FR-040`; `SEC-004`; `NFR-008` | Blocked: M0 |
| `IMP-016` | Define structured logs and audit records | P0 | `IMP-015`, `IMP-078`, `IMP-080`, `IMP-081`; `SEC-007`, `SEC-010` | Log/audit contract and audit-gap state | Redaction, denied-op, audit-failure, integrity, and clock fixtures | `FR-036`, `FR-037`, `FR-040`; `SEC-007`, `SEC-010` | Blocked: M0 |
| `IMP-017` | Define liveness/readiness/health states | P0 | `IMP-015`, `IMP-016`, `IMP-079`, `IMP-082` | Health contract and transitions | No-traffic/failure/stale/model-unavailable fixtures | `FR-006`, `FR-027`, `FR-034`; `NFR-005`, `NFR-011` | Blocked: M0 |
| `IMP-018` | Establish test and evidence harness | P0 | `IMP-013`–`IMP-017` | Fixture/test ID/evidence conventions | Positive/negative/boundary fixture review | `NFR-008`; `SEC-005`, `SEC-008`; `PRD §O.3` | Blocked: M0 |
| `IMP-019` | Assess dataset candidates and rights | P0 | M0; lawful Access Class A/B review; `IMP-007`, `IMP-008` | Candidate matrix; rejected-source log; source designation/access ledger | Provenance/license/declared-semantic review without final record/outcome access | `EVAL-001`, `EVAL-013`; `DEC-03`, `DEC-13` | Blocked: M0 + data authorization |
| `IMP-020` | Create manifests and quality profiles | P0 | `IMP-019`; approved development-data access or permitted source-level checks | Immutable manifest/fingerprint; exclusions; access-class record | Duplicate/missing/support/denominator audit only for an approved development source; final-source record access is prohibited | `EVAL-002`; `SEC-011` | Blocked: M0 + data authorization |
| `IMP-021` | Freeze label map and truth/adjudication plan | P0 | `IMP-019`, `IMP-020`; `DEC-03` | Mapping version; independent truth protocol | Unknown/unsupported/ambiguous label fixtures | `EVAL-001`, `EVAL-007`, `EVAL-012`; `FR-008` | Blocked: M0 + data authorization |
| `IMP-022` | Build live/offline feature compatibility matrix | P0 | `IMP-010`, `IMP-019`–`IMP-021`, `IMP-081`, `IMP-083` | Field-level matrix and dispositions | Feature parity/schema/unit/missing/privacy tests | `FR-003`–`FR-005`; `DEM-005`; `EVAL-001`, `EVAL-004`, `EVAL-008` | Blocked: M0 + visibility/data |
| `IMP-023` | Resolve flow/sequence/CNN semantics | P0 | `IMP-010`, `IMP-022`; `DEC-04` | Flow/timeout/sequence/layout decision | Deterministic flow and representation review | `FR-002`, `FR-003`; `DEM-004`, `DEM-005`; `EVAL-005` | Blocked: M0 + visibility/data |
| `IMP-024` | Freeze leakage-controlled splits | P0 | `IMP-020`–`IMP-023`, `IMP-081` | Group/time manifest; leakage report | Duplicate/window/marker boundary tests | `EVAL-003`, `EVAL-004`; `NFR-001`, `NFR-006` | Blocked: M1 inputs |
| `IMP-025` | Approve metrics, boundaries, targets, budgets | P0 | `IMP-020`–`IMP-024`, `IMP-082`; `DEC-05`, `DEC-14` | Metric/measurement protocol; `P_common` contract; actionable `TGT-01`–`TGT-07` sheet | Each target has metric/boundary, population/workload, selection/uncertainty, owner/gate, and pass/fail/inconclusive review; no numeric value is invented | `EVAL-007`, `EVAL-009`, `EVAL-010`; `NFR-001`–`NFR-004`; `TGT-01`–`TGT-07` | Blocked: M0 + M1 |
| `IMP-026` | Decide EVAL-013 applicability | P0 | `IMP-019`–`IMP-025`, `IMP-081`, `IMP-083`; `DEC-13` | Independent-source decision, access ledger, or fallback record | Provenance/compatibility review using permitted access classes; untouched-source plan | `EVAL-013`; `EVAL-008`; `DEC-13` | Blocked: M1 data |
| `IMP-027` | Freeze protocol and traceability ledger | P0 | `IMP-021`–`IMP-026`, `IMP-082` | Protocol version; `P_common`/coverage rules; access/experiment/evidence register | Final-test lock and ledger schema review | `EVAL-004`–`EVAL-006`; `NFR-006`; `PRD §O` | Blocked: M1 |

### 12.2 Model, packaging, live path, and incident tasks

| ID | Task | Priority | Prerequisites | Outputs | Test evidence | Linked PRD requirement IDs | Status |
|---|---|---:|---|---|---|---|---|
| `IMP-028` | Validate offline fixtures, preprocessing isolation, parity, CPU slice | P0 | M1; `IMP-014`, `IMP-022`–`IMP-025`, `IMP-081` | Fixture/parity record; preprocessing and `P_common` artifacts | Fit-isolation, schema, duplicate/overlap, sequence, and rerun tests | `FR-003`, `FR-010`; `EVAL-003`–`EVAL-005`; `NFR-006` | Blocked: M1 |
| `IMP-029` | Run trivial reference predictor | P0 | `IMP-028` | Reference run ledger and metrics | Fixed split/denominator record | `EVAL-005`, `EVAL-007`; `NFR-001` | Blocked: M1 |
| `IMP-030` | Run classical ARGUS baseline | P0 | `IMP-028`, `IMP-029` | Baseline config, metrics, cost, failures | Repeated seed and validation-only tuning evidence | `FR-007`; `EVAL-005`, `EVAL-006`, `EVAL-011` | Blocked: M1 |
| `IMP-031` | Run justified CNN arm | P0 | `IMP-028`, `IMP-030`, `IMP-023` | CNN config/results/representation rationale | Same-protocol metrics and failed-run ledger | `FR-007`; `EVAL-005`–`EVAL-007` | Blocked: M1 + representation |
| `IMP-032` | Run or disposition LSTM arm | P0 | `IMP-028`, `IMP-023`, `IMP-030` | LSTM results or sequence limitation | Sequence order/mask/warm-up tests | `FR-007`; `EVAL-005`; `DEM-004`, `DEM-005` | Blocked: M1 + sequence decision |
| `IMP-033` | Run or disposition CNN-LSTM arm | P0 | `IMP-032` | CNN-LSTM results or infeasibility record | Same budget/sequence/resource evidence | `FR-007`; `EVAL-005`, `EVAL-006` | Blocked: M1 + sequence decision |
| `IMP-034` | Compare, quantify uncertainty/cost, select by validation | P0 | `IMP-029`–`IMP-033`, `IMP-025` | Comparison ledger; selection decision | Metrics/support/uncertainty/resource/final-test lock review | `FR-007`–`FR-011`; `EVAL-005`–`EVAL-007`, `EVAL-009`, `EVAL-011`; `NFR-001`, `NFR-004`, `NFR-006` | Blocked: M1 |
| `IMP-035` | Define deployable package manifest | P0 | `IMP-027`, `IMP-034`, `IMP-084` | Bound artifact/schema/label/dependency manifest | Manifest completeness/fingerprint/trust review | `FR-003`, `FR-010`, `FR-038`; `SEC-011` | Blocked: Phase 4 review |
| `IMP-036` | Define prediction/score/threshold/abstention contract | P0 | `IMP-035` | Prediction contract and score semantics | Unknown/abstain/calibration fixtures | `FR-008`–`FR-011`; `NFR-008` | Blocked: package manifest |
| `IMP-037` | Verify activation, compatibility, fingerprint, rollback | P0 | `IMP-035`, `IMP-036`, `IMP-084` | Activation/rollback behavior and audit | Incompatible/corrupt/unsafe/load-failure tests | `FR-010`, `FR-038`; `SEC-005`, `SEC-011`; `NFR-005` | Blocked: package contracts |
| `IMP-038` | Verify CPU inference and fixture parity | P0 | `IMP-036`, `IMP-037`; TGT-02/TGT-04 protocol and safety limits for characterization; approved values for acceptance | CPU readiness and timing/resource record | Determinism, timeout, warm-up/boundary tests | `FR-009`, `FR-039`; `NFR-002`, `NFR-004`, `NFR-006` | Blocked: package and scoped authorization |
| `IMP-039` | Freeze live-eligible package and unsupported states | P0 | `IMP-037`, `IMP-038`; `DEC-03`, `DEC-04` | Package release record; readiness/limitations | Activation/readiness/unsupported-feature fixtures | `FR-003`, `FR-008`–`FR-011`, `FR-038`; `DEM-005` | Blocked: package readiness |
| `IMP-040` | Integrate capture readiness and privilege boundary | P0 | M1; `IMP-012`, `IMP-017`, `IMP-039`, `IMP-076`, `IMP-080` | Capture adapter/readiness records | Permission/interface/health/privilege/IPC tests | `FR-001`, `FR-006`; `SEC-002`; `DEM-003` | Blocked: M1 + M2 |
| `IMP-041` | Implement deterministic flows and closure/dedup policy | P0 | `IMP-023`, `IMP-040`, `IMP-081` | Flow records, IDs, closure reasons | Packet/flow fixtures for direction/expiry/late/duplicate/envelope | `FR-002`, `FR-004`, `FR-005`; `DEM-004` | Blocked: M1 + M2 |
| `IMP-042` | Extract/validate live features against training contract | P0 | `IMP-022`, `IMP-041`, `IMP-039` | Live feature records/quarantine reasons | Parity/schema/unit/missing/invalid tests | `FR-003`–`FR-005`; `DEM-005`; `NFR-008` | Blocked: M1 + M2 |
| `IMP-043` | Measure live loss, queues, health, timing | P0 | `IMP-040`–`IMP-042`, `IMP-017`, `IMP-079`, `IMP-082`; targets | Loss/queue/resource/health record | Overload/drop/backlog and unknown-loss tests | `FR-006`, `FR-039`; `NFR-002`–`NFR-005`, `NFR-011` | Blocked: M1 + targets |
| `IMP-044` | Run authorized two-system scenarios and independent truth | P0 | `IMP-003`, `IMP-010`, `IMP-041`–`IMP-043` | Scenario truth/matching/live run record | Benign visibility plus approved controlled/failure fixtures | `OBJ-01`, `OBJ-06`; `DEM-001`–`DEM-007`; `EVAL-008`, `EVAL-012` | Blocked: M1 + scope auth |
| `IMP-045` | Reconcile parity and issue live go/no-go | P0 | `IMP-042`–`IMP-044`, `IMP-025`, `IMP-027` | Parity report; live limitation/go-no-go | Scenario-flow/prediction/drop/clock reconciliation | `FR-001`–`FR-011`; `NFR-002`–`NFR-005`, `NFR-007`, `NFR-013`; `AC-01`, `AC-02` | Blocked: M1 + M2 |
| `IMP-046` | Implement versioned correlation and incident identity | P0 | `IMP-041`, `IMP-042`, `IMP-017`, `IMP-081`, `IMP-085` | Correlation rule; incident IDs/grouping reasons | Duplicate, unrelated, boundary, late-arrival, event-time tests | `FR-035`, `FR-036`; `AC-07`, `AC-10` | Blocked: M2 + live contract |
| `IMP-047` | Implement CENSUS snapshots and unknown identity | P0 | `IMP-046`, `IMP-085`; `DEC-09` | Inventory/resolution/snapshot records | Known/unknown/ambiguous/NAT/reassignment/event-time tests | `FR-013`–`FR-015`; `SEC-003`; `AC-03` | Blocked: M3 decisions |
| `IMP-048` | Implement separated confidence and versioned risk | P0 | `IMP-047`, `IMP-085`; `DEC-06` | Risk rubric/formula/version/range records | Boundary/monotonicity/unknown/re-score tests | `FR-009`, `FR-015`, `FR-021`, `FR-023`, `FR-024`; `AC-06` | Blocked: M3 decisions |
| `IMP-049` | Implement evidence, summaries, explanation status | P1 | `IMP-046`, `IMP-081`, `IMP-083`, `IMP-086`; `DEC-08`, `SEC-012` | Evidence/explanation/fact-hypothesis records | Provenance/missing/failure/inert-render tests | `FR-016`–`FR-019`; `NFR-009`, `NFR-014`; `AC-04` | Blocked: M3 decisions |
| `IMP-050` | Implement incident lifecycle and append/supersede history | P0 | `IMP-046`–`IMP-049`, `IMP-016`, `IMP-077`, `IMP-078`, `IMP-081`, `IMP-085` | Incident/review/disposition records | Lifecycle/reopen/audit/version/recovery tests | `FR-025`, `FR-028`, `FR-035`, `FR-036`; `AC-03`, `AC-07`, `AC-10` | Blocked: M3 |
| `IMP-051` | Integrate lineage and qualified context states | P0 | `IMP-048`–`IMP-050`, `IMP-045`, `IMP-085` | End-to-end context/provenance reconciliation | Missing/stale/conflict/duplicate/delay fixtures | `FR-013`–`FR-024`, `FR-035`–`FR-036`; `AC-03`, `AC-04`, `AC-06` | Blocked: M3 |

### 12.3 DIG, analyst workflow, operations, and final tasks

| ID | Task | Priority | Prerequisites | Outputs | Test evidence | Linked PRD requirement IDs | Status |
|---|---|---:|---|---|---|---|---|
| `IMP-052` | Implement four DIG check families | P0 | `IMP-045`, `IMP-048`–`IMP-051`, `IMP-087`, `IMP-088`; `DEC-07` | Check vector with state/reason/evidence/version/time | Integrity/reliability/evidence/eligibility fixtures | `FR-020`–`FR-022`; `SEC-008`; `AC-05` | Blocked: M3 + policy decisions |
| `IMP-053` | Implement approved deterministic DIG precedence | P0 | `IMP-052`, `IMP-088`; `SEC-008`, `DEC-07` | Aggregate outcome and precedence record | High-confidence failed/unknown safety boundary tests | `FR-020`–`FR-022`; `SEC-008`–`SEC-010`; `AC-05` | Blocked: M3 + policy decisions |
| `IMP-054` | Freeze common B0–B3 study inputs and rules | P0 | `IMP-025`, `IMP-026`, `IMP-045`, `IMP-087`, `IMP-088`; `DEC-10`, `DEC-14` | Frozen detector/`P_common`/scenario/policy/tuning record | Fairness, arm-input, final-test-lock review | `EVAL-010`, `EVAL-012`; `FR-020`–`FR-024`; `AC-05` | Blocked: M4 decisions |
| `IMP-055` | Execute and ledger B0/B1/B2/B3 | P0 | M4; `IMP-052`–`IMP-054`, `IMP-063`, `IMP-088` | Policy outputs, matched/broader metrics, workload/overhead | Same-scenario/denominator/uncertainty analysis | `OBJ-05`; `EVAL-010`; `FR-020`–`FR-024` | Blocked: M4 exit |
| `IMP-056` | Run DIG ablations and fault injections | P0 | `IMP-055`; authorized fixtures | Fault manifest and expected/actual safety results | Clean controls, combinations, observability limits | `EVAL-010`; `SEC-005`, `SEC-008`–`SEC-010`; `AC-05` | Blocked: M4 + safety auth |
| `IMP-057` | Adjudicate response outcomes and report harms | P0 | `IMP-055`, `IMP-056`; `DEC-10`, `EVAL-012` | Blinded/adjudicated labels; B0–B3 analysis | Independent rubric, agreement, coverage/workload/overhead | `EVAL-010`, `EVAL-012`; `OBJ-03`, `OBJ-05`, `OBJ-07`; `AC-05` | Blocked: M4 |
| `IMP-058` | Build HAWK incident/history views and filters | P0 | `IMP-050`, `IMP-051`, `IMP-017` | Bounded views, filters, denominators, provenance | Filter/count/boundary/pagination fixtures | `FR-025`–`FR-027`; `NFR-009`, `NFR-011`; `AC-07` | Blocked: M3 |
| `IMP-059` | Add health/freshness and accessibility | P0 | `IMP-017`, `IMP-058`; `NFR-010` | Health/disconnect UI; accessibility checklist | Stale/disconnected/no-traffic/keyboard checks | `FR-025`, `FR-027`; `NFR-009`–`NFR-011` | Blocked: M3 |
| `IMP-060` | Implement authenticated review/disposition actions | P0 | `IMP-050`, `IMP-058`; `SEC-003` | Review lifecycle, actor/time/reason history | RBAC/lifecycle/audit/no-training-label tests | `FR-028`, `FR-036`, `FR-037`; `SEC-003`; `AC-07` | Blocked: M3 |
| `IMP-061` | Define catalogue-only WARDEN recommendations | P0 | `IMP-048`, `IMP-052`, `IMP-087`; `DEC-10` | Recommendation catalogue/eligibility contract | Missing-prerequisite and simulation-label fixtures | `FR-029`; `SEC-008`; `AC-08` | Blocked: M4 decisions |
| `IMP-062` | Implement approval, revalidation, idempotency, simulation boundary | P0 | `IMP-061`, `IMP-076`, `IMP-078`, `IMP-080`, `IMP-085`, `IMP-087`; `SEC-008`–`SEC-010` | Approval/simulation/audit records; no-real-action path | Stale/replay/changed/duplicate/audit-failure/no-action tests | `FR-030`, `FR-031`, `FR-036`, `FR-037`; `SEC-003`, `SEC-008`–`SEC-010`; `AC-08` | Blocked: M4 |
| `IMP-063` | Conduct human review/simulation-only walkthrough | P0 | `IMP-058`–`IMP-062`; `DEM-006`, `DEM-008` | Walkthrough and usability/issues record | Authorized fixture walkthrough; action-history reconciliation | `FR-025`–`FR-031`; `NFR-009`, `NFR-010`; `DEM-006`, `DEM-008`, `AC-07`, `AC-08` | Blocked: M4 |
| `IMP-064` | Implement descriptive SIBYL history | P1 | `IMP-050`, `IMP-060`, `IMP-079`; `DEC-11` | Time-window trend/denominator report; forecast status | Missing-period/confirmed-vs-predicted fixtures | `FR-032`–`FR-034`; `AC-09` | Blocked: M4 + history decision |
| `IMP-065` | Run operational, overload, outage, recovery, performance tests | P0 | `IMP-043`, `IMP-050`, `IMP-062`, `IMP-077`, `IMP-078`, `IMP-082`; approved applicable targets | Resource/latency/throughput/recovery report | Idle/nominal/overload/failure/recovery evidence | `FR-006`, `FR-027`, `FR-039`; `NFR-002`–`NFR-006`, `NFR-011`–`NFR-013`; `AC-10` | Blocked: M4 + targets |
| `IMP-066` | Verify retention, deletion, export, minimization | P0 | `IMP-015`, `IMP-016`, `IMP-050`, `IMP-083`; `DEC-09`, `TGT-06` | Retention/export/deletion/audit report | Redaction/access/deletion/retained-metadata tests | `FR-036`, `FR-040`; `NFR-012`; `SEC-004`, `SEC-006`, `SEC-007` | Blocked: M4 + policy/targets |
| `IMP-067` | Run integrated two-system demonstration | P0 | `IMP-045`, `IMP-051`, `IMP-057`, `IMP-063`, `IMP-065` | Scenario-to-simulation evidence pack | Full ID/timing/resource/drop/health reconciliation | `OBJ-01`–`OBJ-07`; `DEM-001`–`DEM-009`; `AC-11` | Blocked: M5 inputs |
| `IMP-068` | Run the locked EVAL-013 cross-dataset evaluation or the explicitly non-equivalent fallback after the pre-access freeze; keep the final source sealed until the frozen run. | P0 | `IMP-026`, `IMP-034`, `IMP-039`, `IMP-067`, `IMP-089`; `EVAL-013` | Untouched-source result or fallback limitation | Provenance/lock/degradation/shift report | `EVAL-008`, `EVAL-013`; `NFR-001`–`NFR-004`, `NFR-006` | Blocked: freeze and EVAL-013 decision |
| `IMP-070` | Run the already-frozen final evaluation version under the approved final metrics, scenarios, policy arms, resources, and live-demonstration outputs. | P0 | `IMP-068`, `IMP-089`; final-test authorization | Locked final metrics/scenario/policy record | Untouched-final-source and no-tuning audit | `EVAL-006`, `EVAL-008`–`EVAL-013`; `NFR-001`–`NFR-004` | Blocked: freeze and final-test approval |
| `IMP-069` | Re-run the sealed fixed evaluation after final execution and reconcile evidence; this verifies reproducibility and is not independent confirmation. | P0 | `IMP-068`, `IMP-070`; `NFR-006` | Reproducibility/evidence index | Manifest/seed/environment/tolerance/deviation audit | `EVAL-002`–`EVAL-004`, `EVAL-009`; `NFR-006`; `PRD §O` | Blocked: sealed final evaluation |
| `IMP-071` | Complete full requirement/acceptance traceability | P0 | `IMP-069`, `IMP-070` | FR/NFR/SEC/DEM/EVAL/AC/DEC/TGT register | Every identifier has evidence/disposition/reviewer | All applicable; `PRD §O.1`–`§O.3` | Blocked: final evaluation |
| `IMP-072` | Perform reproducibility audit | P0 | `IMP-069`, `IMP-070` | Clean rerun/environment/fingerprint audit | Rerun tolerance and nondeterminism record | `NFR-006`; `EVAL-002`–`EVAL-004`, `EVAL-009` | Blocked: final evaluation |
| `IMP-073` | Write limitations, fallbacks, unmet-target report | P0 | `IMP-068`–`IMP-072` | Evidence-bounded limitations/scope report | Reviewer check against claims and targets | `OBJ-01`–`OBJ-07`; `TGT-01`–`TGT-07`; `PRD §D.4`, §M | Blocked: final evaluation |
| `IMP-074` | Assemble final presentation/evidence pack | P0 | `IMP-071`–`IMP-073` | Presentation, evidence index, negative findings | Presentation walkthrough cannot omit blocked items | `OBJ-01`–`OBJ-07`; `AC-01`–`AC-11`; `DEM-001`–`DEM-009` | Blocked: final review |
| `IMP-075` | Hold human acceptance and disposition | P0 | `IMP-074`; `DEC-01` | Accepted/rejected/incomplete decision; next scope | Supervisor review and signed disposition | All applicable; `DEC-01`; `PRD §P` | Blocked: human acceptance |

### 12.4 Explicit foundational gap tasks

The following stable task IDs replace previously descriptive placeholders in the reconciliation register. They are proposed planning tasks, not completed work. Each remains blocked by its stated gate and decisions; no acceptance criterion below has execution evidence. `IMP-076`–`IMP-085` and `IMP-087` are foundation contracts, `IMP-086` is a data/representation contract, and `IMP-088` is a Phase 8 policy contract that must precede its DIG consumers.

| ID | Task and proposed owner | Dependencies | Explicit deliverables | Objective acceptance criteria | Status |
|---|---|---|---|---|---|
| `IMP-076` | Define authenticated operation, session, RBAC, and capture-IPC contract. **Proposed owner:** security/technical lead. | M0; `IMP-002`, `IMP-003`; `DEC-09`; `SEC-002`–`SEC-004` | Operation-to-role matrix; subject/session lifecycle with expiry, revocation, logout, and failure states; protected transport/browser requirements; capture-to-ordinary-process data-flow and IPC ACL record. | Every protected operation has an actor, permission, denial reason, session state, and audit requirement; server-side checks precede state change; capture authority is denied dashboard administration, secrets, model promotion, audit deletion, and response controls; transport exceptions are explicit and owner-approved. | Blocked: M0 and security decisions |
| `IMP-077` | Define durable persistence, acknowledgement, recovery, replay, deduplication, and loss-accounting contract. **Proposed owner:** technical lead. | M0; `IMP-002`, `IMP-003`, `IMP-013`; `ADR-05`, `ADR-06`; `NFR-005` | Boundary-by-boundary durability matrix; schema/migration and checkpoint policy; outbox/replay/idempotency rules; database, disk-full, queue, restart, corruption, and stop/recovery matrix. | Each handoff states whether persistence precedes acknowledgement/publication; persisted, lost, rejected, replayed, duplicated, and unavailable outcomes are distinguishable; no exactly-once claim is made without evidence; silent loss is explicitly disallowed. | Blocked: M0 and persistence decisions |
| `IMP-078` | Define audit integrity, clock, tamper-evidence, retention, and audit-failure contract. **Proposed owner:** security/technical/privacy leads. | `IMP-076`, `IMP-077`, `IMP-013`; `SEC-007`, `SEC-010`; `ADR-05`; `NFR-013`, `DEC-09` | Audit envelope and ordering fields; tamper-evidence mechanism and trust boundary; wall/monotonic/event-time rules; retention/deletion impact record; audit-write failure policy. | The contract specifies expected handling for altered, deleted, reordered, truncated, and clock-anomalous records; sensitive changes block or become explicitly unavailable when required audit evidence cannot be committed; fully privileged-host limits are stated; mechanism remains proposed until reviewed. | Blocked: M0 and audit/privacy decisions |
| `IMP-079` | Define FR-034 traffic/feature/model-health reference lifecycle. **Proposed owner:** analytics/operations lead. | M0; `IMP-013`; `DEC-03`, `DEC-05`; `FR-034` | Named monitored signals; reference population/window and update policy; delayed-label availability states; missing-period/zero/drift/degradation state model; reference-version and review record. | A fixture specification distinguishes no data, missing period, zero activity, unlabeled drift indicator, and verified degradation; verified performance degradation requires delayed/verified labels; reference changes have an owner, version, and effective time. | Blocked: M0 and data/target decisions |
| `IMP-080` | Define typed API operation, permission, error, pagination, concurrency, idempotency, and reconnect contract. **Proposed owner:** technical/security lead. | `IMP-076`, `IMP-077`, `IMP-078`, `IMP-081`, `IMP-013`; `DEC-09`, `DEC-10` | Request/response schemas; operation-permission matrix; bounded filter/pagination ordering; error/status map; optimistic-concurrency and idempotency rules; update/reconnect reconciliation record. | Every read, export, mutation, review, approval, and simulation operation has a permission, bounded input/output, deterministic error behavior, concurrency rule, and retry rule; denial-audit writes are distinguished from denied business-state changes; invalid requests do not change business state. | Blocked: M0 and contract decisions |
| `IMP-081` | Define the common record envelope, identity namespaces, replay/reprocessing semantics, unknown-state encoding, `P_common` shape references, and feature/sequence constraints. **Proposed owner:** contract/technical lead. | M0; `IMP-013`, `IMP-077`, `IMP-078`; `FR-004`, `FR-005`, `NFR-008` | Versioned envelope; source/restart/event/processing IDs; replay and reprocessing rules; explicit missing/unknown/invalid/not-applicable representation; feature-array, sequence-mask, and anchor-reference constraints. | A producer/consumer review validates required fields and cross-field shapes; original event identity is distinct from a processing attempt; unknown, invalid, unavailable, and sequence-ineligible values cannot silently become zero, benign, or an attack label. | Blocked: M0 and contract decisions |
| `IMP-082` | Define monotonic timing and evaluation-render instrumentation. **Proposed owner:** evaluation/technical lead. | M0; `IMP-013`, `IMP-076`, `IMP-077`; `FR-027`, `FR-039`; `NFR-002`, `NFR-013` | Stage-span specification for capture, feature-ready, publication, render, first alert, update, timeout, and miss; clock-domain/uncertainty record; denominator and timeout rules. | Each latency metric has explicit start/end events, clock basis, denominator, and missing-render treatment; wall-clock changes cannot create negative durations; first alerts and later incident updates are not conflated. | Blocked: M0 and evaluation decisions |
| `IMP-083` | Define privacy, pseudonymization, transient-processing, retention, export, deletion, and backup contract. **Proposed owner:** privacy/technical lead. | M0; `IMP-002`, `IMP-003`, `IMP-013`; `DEC-09`; `SEC-006` | Field collection matrix; transient versus persisted payload rules; stable mapping namespace and access boundary; export redaction; retention, deletion, backup, and audit-metadata rules. | Every collected field is classified; payload-disabled, transient, diagnostic, persisted, and exported states are distinct; approved linkage remains possible without exposing the mapping; deletion and backup behavior are explicit; no privacy approval is claimed. | Blocked: M0 and privacy decisions |
| `IMP-084` | Define artifact trust, promotion, safe loading, dependency, and rollback contract. **Proposed owner:** security/research/technical leads. | M0; `IMP-002`, `IMP-013`, `IMP-076`, `IMP-077`, `IMP-078`; `DEC-03`, `DEC-09`; `SEC-005`, `SEC-011` | Trusted producer and promotion roles; signed-manifest/trust-root proposal; quarantine and dependency/SBOM record; restricted loader and immutable rollback policy. | Unsigned, tampered, corrupt, incompatible, unsafe, or unauthorized artifacts have a defined rejection path; a matching hash is not treated as trust; executable deserialization is prohibited; activation and rollback are auditable. | Blocked: M0 and artifact-trust decisions |
| `IMP-085` | Define event-time asset context, incident revision, and approval-invalidation contract. **Proposed owner:** technical/analyst lead. | M0; `IMP-013`, `IMP-076`, `IMP-077`, `IMP-078`, `IMP-081`; `DEC-09`; `FR-014`, `FR-035` | Inventory validity intervals; per-event asset context; incident revision/supersession rules; late-arrival and approval-invalidation matrix. | A late event resolves against event-time context; changing identity, evidence, policy, or target can invalidate a stale approval; multiple underlying endpoints remain traceable; unknown or ambiguous identity is preserved. | Blocked: M0 and identity/incident decisions |
| `IMP-086` | Define explanation-method validation and scope-exception contract. **Proposed owner:** research/analyst lead. | M1; `IMP-019`, `IMP-022`, `IMP-081`; `DEC-08`; `FR-017`–`FR-019`; `AC-04` | Method-suitability criteria; reference-data provenance; bounded execution and failure states; distinction between per-request unavailable, release-level exception, attribution, and causal claims. | Runtime explanation failure is not counted as release fulfillment; explanations are labelled as model-behavior evidence rather than causality; a validated path or explicit supervisor exception is required before a must-have explanation claim. | Blocked: M1 and explanation decision |
| `IMP-087` | Define independent simulation-only boundary and no-effect evidence contract. **Proposed owner:** security/lab lead. | M0; `IMP-002`, `IMP-003`, `IMP-076`, `IMP-077`, `IMP-078`, `IMP-080`; `DEC-10`; `SEC-008`–`SEC-010`; `FR-030`, `FR-031` | Closed recommendation catalogue; approval binding fields; action/process graph; stale/replay/duplicate/changed-target rules; independent endpoint/network/process observation plan. | Arbitrary command-like inputs are rejected; stale, replayed, duplicate, unauthorized, or changed-target requests are blocked; the simulator has no shell, firewall, host-control, or arbitrary network-control path; `simulation_only` and `endpoint_changed` fields are not sole proof. | Blocked: M0 and safety decisions |
| `IMP-088` | Define executable DIG/risk decision tables and shared-versus-arm rule matrix. **Proposed owner:** safety/research/analyst leads. | M3; `IMP-025`, `IMP-026`, `IMP-027`, `IMP-048`–`IMP-051`, `IMP-087`; `DEC-06`, `DEC-07`, `DEC-14`; `EVAL-010` | Check catalogue with observable inputs, freshness, thresholds, unknown/not-applicable behavior, precedence, risk mapping, shared safeguards, arm-specific differences, `P_common` rules, and policy-only fixtures. | Every check has observable inputs and expected boundary states; failed/unknown integrity or safety cannot be compensated by confidence or risk; raw output, arm proposal, common eligibility, approval, and simulation remain separate; effectiveness remains H-DIG-01. | Blocked: M3/M4 and DIG/risk decisions |

These tasks are planning records only. Their objective criteria describe future evidence to collect; they do not assert that implementation, testing, or verification has occurred.

### 12.5 Explicit pre-access final-freeze task

The final-test freeze is a distinct gate, not an action performed after final data access. It is proposed and unexecuted.

| ID | Task and proposed owner | Dependencies | Explicit deliverables | Objective acceptance criteria | Status |
|---|---|---|---|---|---|
| `IMP-089` | Create the pre-access final-protocol freeze record. **Proposed owner:** research lead/supervisor. | M5; `IMP-026`, `IMP-034`, `IMP-039`, `IMP-054`, `IMP-065`, `IMP-067`; `DEC-05`, `DEC-13`, `DEC-14` | Immutable freeze record covering model, preprocessing, calibrator, thresholds, `P_common` construction rules and broader coverage, sequence policy, risk/DIG/correlation policy, metrics, candidate population rules, scenario universe, Access Class D identity, final-source access ledger, and compromised-partition/change procedure. | All consequential choices and the final access ledger are approved and recorded before any Access Class D record, label, feature, row-derived statistic, score, or result is opened; final anchor membership is produced only by the frozen construction rules during the authorized run; no final source was consulted for tuning; any post-freeze change creates a development result and requires a recorded disposition/new independent source where feasible; reproducibility reruns are not treated as independent evidence. | Blocked: M5 and final-protocol approval |

## 13. Reconciled planning baseline (proposed, pending approval)

This section supersedes contradictory dependency wording above for future planning; it does not claim any task complete. The single proposed crosswalk is: **M0 governance/document approval → M1 feasibility, data, semantics, common population, access boundary, and target protocol → M2 development characterization, model comparison, and package → M3 integrated observation/investigation conformance → M4 DIG and simulation-only response conformance → M5 historical/operational/policy study → IMP-089 final-protocol freeze → sealed final data/labels/scores access → exact reproducibility rerun → human acceptance**. No phase may claim evidence from a later component.

| Proposed stage | Permitted evidence boundary | Explicit dependency rule |
|---|---|---|
| M0 governance/document approval | Authority, owners, scope, privacy/safety limits, decision register, canonical crosswalk, and exactly one proposed next activity | NOT APPROVED; no implementation, installation, training, or traffic is authorized here |
| M1 feasibility/data/semantics/protocol | Inventory/visibility, candidate rights/labels, feature/flow/sequence, `P_common`, leakage, Access Class A/B ledger, targets, and EVAL-013 applicability | Precedes model commitment; no Access Class D record/outcome access |
| M2 development experiments/package | Training/development-only baselines and model arms, calibration/threshold development, feasibility characterization, validation-only selection, compatible package | No final data; failed/infeasible arms retained; targets may be characterized but not silently accepted |
| M3 integrated observation/investigation | Live/offline parity, correlation, CENSUS/ORACLE, HAWK, risk/confidence separation, and lineage | Future components must exist and pass their own gates before integrated claims |
| M4 DIG/simulation-only response | Contracts, auth/RBAC, persistence/audit, health, policy-only DIG fixtures, WARDEN catalogue, approval/revalidation, and no-effect boundary | Policy-only DIG fixtures are not effectiveness evidence; any real-action path blocks |
| M5 historical/operational/policy study | Descriptive history, recovery/performance, retention/export, B0–B3, ablations, injections, adjudication, and integrated rehearsal | Unknown boundaries, silent loss, circular truth, or unfair comparison blocks |
| IMP-089 final-protocol freeze | Immutable model/preprocess/calibrator/threshold/`P_common`/sequence/risk/DIG/correlation/policy/metric/population/scenario selections and Access Class D ledger | Freeze precedes first final record, label, feature, row-derived statistic, score, or cross-dataset target access; no retuning afterward |
| Sealed final evaluation | Locked final metrics and approved scenarios on sealed sources; compromised partitions get recorded disposition | Development experiments and reruns are not independent final evidence |
| Exact reproducibility rerun | Same frozen inputs/configuration, manifest, seeds, environment, and tolerance | Verifies reproducibility only; it is not a new independent confirmation |

### 13.1 Corrected task-order constraints

`IMP-089` is the proposed **pre-access freeze gate**. `IMP-068` and `IMP-070` may access sealed final data only after `IMP-089` and the required final-test authorization. `IMP-069` is the exact reproducibility rerun after final execution; it cannot authorize or substitute for an independent final evaluation. `IMP-044` is the smallest later integrated live activity; it does not require DIG, WARDEN, or UI components to pass. `IMP-052`–`IMP-054` may produce policy-only DIG fixtures before WARDEN exists; measured `IMP-055`–`IMP-057` wait for M4 conformance and the WARDEN walkthrough, while WARDEN/UI acceptance remains in `IMP-061`–`IMP-063`.

### 13.2 Security and evidence gaps carried forward

The following remain required/proposed and unimplemented with owners to be assigned in M0/M1: capture privilege/IPC isolation; lab egress and independent stop; identity/session/RBAC; audit integrity/time; persistence/outbox/recovery/loss accounting; privacy/pseudonymization; artifact trust; approval binding/atomic revalidation/idempotency; immutable serving versions; event-time context and incident revisions; calibrated/degraded modes; DIG independence; and independent proof of simulation-only/no endpoint change. Self-reported fields are never the sole proof. See `docs/RECONCILIATION_REGISTER.md` rows WG-AUD-007–027 and `docs/M0_DECISION_REGISTER.md`.
