# WallGrid AI — Documentation Reconciliation Register

**Status:** Editorial reconciliation for M0 preparation; proposed baseline only. **M0 is NOT APPROVED.** No row is evidence of implementation, feasibility, experiment, security-control verification, supervisor approval, or a positive result. `Corrected` is reserved for unambiguous editorial/reference corrections.

## 1. Source and review basis

Reviewed in full: `PRD.md`, `SYSTEM_DESIGN.md`, `ML_RESEARCH_PLAN.md`, `TECHNICAL_DESIGN.md`, `IMPLEMENTATION_PLAN.md`, and `M0_DECISION_REGISTER.md`; this register records the reconciliation baseline. Review pointers are the final reports in workspace `ws_27a5193adac5`: Agent 5 `ses_ed862cf33ffewpgMzc5OOJwH15` / `msg_127acd0a0001qneu7HVWCB6q5b`; Agent 6 `ses_ed8558659ffejOnxyuY8Rb0XUE` / `msg_127b80c90001yTh60ICV8DuIco`; Agent 8 `ses_ed8547cc0ffeg0t139F2zvcMoa` / `msg_127b4d6e2001O8DyxclSdKiKcw`; Agent 9 `ses_ed8541f00ffeBr05oFBaud6qrc` / `msg_127b4bc2a001a4IYfQlGrUHItx`; Agent 10 `ses_ed853cdeeffe6hSxyuyBHvA1XO` / `msg_127b7c0270010MIsWSdHOg5cUl` (WG-AUD-001–029). Reports are concise review evidence, not approvals or test results.

## 2. Reconciliation rows

| ID | Source / reviewer pointer | Problem | Resolution / editorial correction | Owner | Primary status | Unresolved decision (if any) | Approval? |
|---|---|---|---|---|---|---|---|
| WG-AUD-001 | PRD governance; SYSTEM §1/§14; TECH §1; Agent 10 | Draft wording could be mistaken for approval; no attributable M0 record | Clarify drafts/proposals and require M0 register/version record | Owner/supervisor | proposed | Which authority/version record and M0 scope will be approved | Yes |
| WG-AUD-002 | IMP Phases 6–9; Agent 10 | Circular phase evidence and M3/M4/M5 inconsistency | Use the canonical PRD §N / ML §10 crosswalk; place policy-only DIG before WARDEN/UI; no later evidence in early exits | Technical/research leads | proposed | M0 adoption of the crosswalk; any scope-changing milestone deviation remains a DEC/ADR/TGT decision | Yes |
| WG-AUD-003 | IMP-068–070; Agent 10 | Final access/rerun precede freeze | Use Access Classes A–D, immutable access ledger, `IMP-089` pre-access freeze, and rerun-only reproducibility semantics | Research lead/supervisor | proposed | Dataset-specific Access Class D designation and approval of the final-freeze/access protocol | Yes |
| WG-AUD-004 | PRD FR-002/003, DEM-004/005; ML §3–5; Agent 10 | Data-to-live and sequence contract undefined | Require field-level parity and representative evidence before model commitment | Research/technical | proposed | The approved data-to-live, flow, and sequence contract | Yes |
| WG-AUD-005 | PRD DEM-001–003; TECH §8; Agents 8/10 | Topology/backend/driver/account/interface/visibility unproven | Inventory and separately authorize bounded probe; recommendations remain conditional | Lab owner/operator | proposed | The topology, backend, privilege model, and probe authorization | Yes |
| WG-AUD-006 | PRD TGT-01–07; IMP-025; Agent 10 | Targets depend on measurements that targets block | Use PRD §B.2: define metric/boundary, population/workload, selection, uncertainty, owner/gate, and pass/fail/inconclusive evidence; separate immediate safety limits from empirical target values | Supervisor/research/lab | proposed | Numeric values, confidence/interval choices, and target-sheet approval remain pending; no target is invented | Yes |
| WG-AUD-007 | PRD §O; IMP-015–018; Agent 10 | Range mapping hides missing auth, persistence, tamper, FR-034 tasks | Add individual 88-row traceability map and explicit task/oracle gaps | Technical lead | proposed | Technical-lead acceptance of the added task IDs and future oracles | Yes |
| WG-AUD-008 | TECH §5.1; Agent 10 | API list is not a contract | Define `IMP-080` typed operation, permission, error, pagination, concurrency, idempotency, and reconnect contract before protected consumers | Technical/security | proposed | Approval of the concrete operation matrix and transport/session choices | Yes |
| WG-AUD-009 | SYSTEM §6; TECH §6; Agent 10 | Envelope, IDs, replay, unknowns, shapes, lineage unspecified | Define `IMP-081` common envelope, namespaces, processing/replay identities, unknown states, and shape constraints before record producers/consumers | Contract owners | proposed | Approval of the formal envelope/migration contract; implementation and compatibility evidence remain future | Yes |
| WG-AUD-010 | PRD FR-008–011/020–023; TECH §6; Agent 10 | Raw output, abstention, qualification, risk, eligibility conflated | Preserve raw prediction separately; independent qualifiers and approval | Research/technical/safety | proposed | The approved projection between raw prediction, abstention, qualification, risk, eligibility, and approval | Yes |
| WG-AUD-011 | PRD FR-027/039, NFR-002/013; Agent 10 | Timing clocks and render boundaries incomplete | Define `IMP-082` monotonic stage spans, clock uncertainty, first/update/miss accounting, and missing-render rules before performance tests | Evaluation/technical | proposed | Approval of instrumentation and measurement boundary; no measured result exists | Yes |
| WG-AUD-012 | PRD EVAL-004; ML §5; Agent 10 | Transform fitting language inconsistent | Training-only transform fitting; separate calibration/threshold boundaries | Research lead | corrected | — | No substantive |
| WG-AUD-013 | ML EXP matrix; Agent 10 | Sequence subset differs from baseline population | Use the proposed `P_common` contract: same anchors/labels/splits/denominators, explicit warm-up/padding/missing/overlap exclusions, and separate broader coverage | Research/supervisor | proposed | Dataset-specific anchor, support, and sequence applicability approval | Yes |
| WG-AUD-014 | PRD EVAL-013; ML §4; Agent 10 | Inspection and A→B/B→A “untouched” boundary unclear | Use Access Classes A–D, source-level A/B checks, an immutable ledger, and sealed directional target(s) after `IMP-089` | Research/supervisor | proposed | Dataset-specific source designation and permitted directional protocol | Yes |
| WG-AUD-015 | ML §7; IMP-052–057; Agent 10 | Common safeguards and arm policies overlap | Use `IMP-087` simulation boundary and `IMP-088` shared-versus-arm matrix separating raw output, proposal, common eligibility, approval, and simulation | Research/safety | proposed | Approval of shared safeguards and arm-specific policy parameters | Yes |
| WG-AUD-016 | PRD §K; Agent 10 | DIG/risk checks lack executable inputs/freshness/unknown rules | Define `IMP-088` observable inputs, freshness, thresholds, unknown/not-applicable rules, precedence, risk mapping, and policy-only fixtures; no effectiveness claim | Analyst/research/supervisor | proposed | Approval of executable policy tables and independence analysis | Yes |
| WG-AUD-017 | PRD SEC-004; TECH §10; Agent 10 | IPC/auth design missing; “should” weakens transport | `IMP-076`/`IMP-080` restore mandatory encrypted non-loopback transport with approved exception only, explicit session/RBAC/IPC contracts | Security/lab | proposed | Approval of any SEC-004 exception and concrete auth/session/IPC design | Yes |
| WG-AUD-018 | PRD FR-030; TECH §6.4; Agent 10 | Approval schema, atomicity, retry conflicts missing | `IMP-080`/`IMP-085`/`IMP-062` define immutable approval binding, atomic revalidation/consumption, invalidation, and retry conflicts | Technical/security | proposed | Approval of schema/atomicity/consumption rules; no implementation evidence | Yes |
| WG-AUD-019 | PRD FR-038; TECH §7; Agent 10 | Serving versions and threshold activation lifecycle missing | `IMP-084` precedes package/activation work and defines trusted promotion, safe loading, immutable serving versions, and rollback | Research/technical/security | proposed | Trust-root/promotion choice and activation lifecycle approval | Yes |
| WG-AUD-020 | PRD NFR-005/012; Agent 9/10 | Durable boundaries/outbox/audit trust/retention undefined | `IMP-077`/`IMP-078` define persistence-before-ack, recovery/replay/loss accounting, tamper trust, and retention/deletion failure behavior | Technical/security/privacy | proposed | Approval of durable boundary, audit trust, recovery, and retention design | Yes |
| WG-AUD-021 | PRD SEC-006/FR-040; Agent 10 | Pseudonymization may break links; transient/stored payload unclear | `IMP-083` defines field classification, stable pseudonym namespace, transient/persisted/export boundary, retention, deletion, and backup disclosure | Privacy/technical | proposed | Approval of collection/pseudonymization/retention boundary; no privacy verification | Yes |
| WG-AUD-022 | PRD FR-014/035; Agent 10 | Event-time context and incident revisions incomplete | `IMP-085` defines per-event validity, inventory intervals, revisions, late arrivals, and approval invalidation before incident consumers | Technical/analyst | proposed | Approval of event-time/revision/invalidation rules | Yes |
| WG-AUD-023 | PRD FR-017/AC-04; Agent 10 | Unavailable XAI could be treated as fulfillment | `IMP-086` distinguishes per-request failure, release-level exception, model-behavior attribution, and causal claims; require validated path or explicit supervisor exception | Supervisor/research | proposed | Whether a validated explanation path or an explicit scope exception will be approved | Yes |
| WG-AUD-024 | PRD FR-031; TECH §6.4; Agent 9/10 | `simulation_only`/`endpoint_changed` are self-assertions | `IMP-087` requires a closed catalogue, no-action graph, independent endpoint/network/process observation, and negative evidence; fields alone are insufficient | Security/lab | proposed | The independent simulation-boundary evidence required for acceptance | Yes |
| WG-AUD-025 | PRD FR-006/NFR-011; Agent 10 | Idle, missing marker, liveness, loss semantics blur | Separate expected-marker, idle, readiness, verified visibility, aggregate/attributable loss, storage/queue limits, and independently operable stop behavior in `IMP-079`, `IMP-082`, and the CF table | Technical/lab | proposed | The approved liveness, idle, visibility, loss, storage, and stop interpretation rules | Yes |
| WG-AUD-026 | PRD DEC-02; TECH §2–3; Agent 10 | Windows 11 and dual-framework scope unapproved | Mark Windows proposed; limit framework comparison to approved slice | Technical/research | corrected | Whether Windows 11 and any dual-framework comparison will be approved scope | Yes |
| WG-AUD-027 | ML hypotheses; Agent 10 | Contrasts/primary metrics/uncertainty/conclusion rules incomplete | Map each retained hypothesis to `P_common`/broader coverage, target/uncertainty contract, matched policy rules, and negative/inconclusive conclusions; no positive DIG gate | Research/supervisor | proposed | Hypothesis-specific contrast, metric, uncertainty, and conclusion approval | Yes |
| WG-AUD-028 | IMP READ-01/17; SYSTEM §6/end; Agent 10 | Broken references/stale existence wording/AC typo | Corrected references, “Missing,” and Technical Design existence wording | Editor | corrected | — | No substantive |
| WG-AUD-029 | IMP SG-08; Agent 10 | Audible SIMULATION ONLY lacks upstream authority | Mark audio proposed/out of current approved scope pending owner decision | Owner | unresolved | Whether audio labeling is intended scope and, if so, its acceptance rule | Yes |

`Primary status` has exactly one value: `unresolved`, `proposed`, or `corrected`. A `corrected` row records only an applied unambiguous editorial correction; any remaining substantive choice is recorded separately in `Unresolved decision (if any)` and is not implied to be approved.

Agent 5 planning recommendations are incorporated in the phase crosswalk and task-order rows; Agent 6 findings preserve prior art, genuine temporal order, distinct leakage/shift/labels, non-causal XAI, and non-commitment to candidate RQs; Agent 8 findings constrain the next probe; Agent 9 security findings remain proposed/unimplemented, not vulnerabilities confirmed by tests.

## 3. Canonical requirement traceability planning (88 rows)

Every row is future planning only. `P` means pending evidence; `NA?` means applicability decision is missing. Role is the proposed responsible role; objective oracle is the future positive/negative check or named decision gap.

| ID | Proposed task(s) | Role | Future oracle / gap | Applicability / evidence |
|---|---|---|---|---|
| FR-001–FR-006 | `IMP-040`–`IMP-043`, `IMP-076`, `IMP-081` | Technical/capture | Positive marker; denied/wrong/idle/loss distinctions; envelope and IPC boundaries | C/P; P |
| FR-007–FR-012 | IMP-028–034 | Research | Common matrix; unsupported/abstain preservation | C/P/V; P |
| FR-013–FR-015 | IMP-047–048 | Asset/analyst | Known/unknown/NAT/re-score boundaries | P; P |
| FR-016–FR-019 | IMP-049 | Research/analyst | Provenance, unavailable, non-causal XAI; AC-04 exception | C/P/V; P |
| FR-020–FR-024 | IMP-052–057 | Safety/research | Four-state checks, high-score fail-closed, policy-only fixtures | C/P; P |
| FR-025–FR-028 | IMP-058–060 | UI/analyst | Filter denominators, stale UI, lifecycle/RBAC | C; P |
| FR-029–FR-031 | IMP-061–063 | Safety/technical | Approval binding, replay/idempotency, independent no-effect proof | C; P |
| FR-032–FR-034 | `IMP-064`, `IMP-079` | Analytics/technical | Missing-period vs zero; delayed labels; reference drift fixture | C/V; P |
| FR-035–FR-040 | `IMP-046`, `IMP-050`, `IMP-065`, `IMP-066`, `IMP-077`, `IMP-078`, `IMP-080`, `IMP-082`, `IMP-083`, `IMP-085` | Technical/security | Correlation, audit, auth, timing, export, persistence, and event-time boundaries | C/P; P |
| NFR-001–NFR-004 | IMP-025,034,065,070 | Evaluation | Approved target sheet, denominators, boundaries | P; P |
| NFR-005–NFR-008 | `IMP-016`–`IMP-018`, `IMP-037`, `IMP-065`, `IMP-077`, `IMP-078`, `IMP-080`, `IMP-081` | Technical/security | Recovery/loss, rerun, contract compatibility | P; P |
| NFR-009–NFR-014 | `IMP-058`–`IMP-060`, `IMP-064`–`IMP-066`, `IMP-078`, `IMP-082`, `IMP-083`, `IMP-086` | UI/ops/privacy | Human walkthrough, accessibility, state, retention, clocks, explanation limits | P; P |
| SEC-001–SEC-004 | `IMP-003`, `IMP-012`, `IMP-060`, `IMP-062`, `IMP-076` | Lab/security | Scope/stop, privilege/IPC, RBAC/session, encrypted transport | C/P; P |
| SEC-005–SEC-008 | `IMP-018`, `IMP-035`, `IMP-049`, `IMP-052`, `IMP-056`, `IMP-078`, `IMP-084`, `IMP-088` | Security | Bounds, safe loader, minimization, tamper/access, decision tables | C; P |
| SEC-009–SEC-012 | `IMP-016`, `IMP-035`, `IMP-049`, `IMP-062`, `IMP-076`, `IMP-078`, `IMP-080`, `IMP-084`, `IMP-087` | Security/technical | TOCTOU, denied audit, provenance, inert rendering, simulation boundary | P/C; P |
| DEM-001–DEM-003 | IMP-007–012 | Lab operator | Topology, markers, Windows backend/privilege | C; P |
| DEM-004–DEM-005 | IMP-022–024,041–045 | Research/technical | Flow/feature parity and causal availability | C; P |
| DEM-006–DEM-009 | `IMP-044`, `IMP-063`, `IMP-065`, `IMP-067` | Owner/analyst | Full scenario, rehearsal, failure/recovery, simulation | C/P; P |
| EVAL-001–EVAL-004 | IMP-019–027 | Research/privacy | Rights, manifest, labels, split, train-only transforms | C; P |
| EVAL-005–EVAL-009 | `IMP-028`–`IMP-034`, `IMP-068`–`IMP-070`, `IMP-089` | Research | Matrix, metrics, live separation, uncertainty, pre-access freeze | C; P |
| EVAL-010–EVAL-013 | `IMP-054`–`IMP-057`, `IMP-068`–`IMP-070`, `IMP-089` | Research/safety | Fair B0–B3, truth, cross-dataset applicability, sealed final | C/P; P |

The grouped rows above cover exactly **40 FR + 14 NFR + 12 SEC + 9 DEM + 13 EVAL = 88 canonical requirements**; each range expands to the individual identifiers in the PRD and remains pending. Missing foundational tasks and acceptance oracles are recorded rather than fabricated as complete.

### 3.1 Individual expansion (canonical IDs)

| ID | Task / owner / future objective oracle | Status |
|---|---|---|
| FR-001 | IMP-040 / capture / marker + denied permission | P |
| FR-002 | IMP-041 / flow / deterministic key-expiry fixture | P |
| FR-003 | IMP-042 / features / schema-unit mismatch rejection | P |
| FR-004 | IMP-041 / technical / lineage envelope check | P |
| FR-005 | IMP-041 / technical / duplicate-late reason counters | P |
| FR-006 | IMP-043 / ops / observable vs unknown loss | P |
| FR-007 | IMP-029–034 / research / common experiment ledger | P |
| FR-008 | IMP-030–034 / research / unknown label not remapped | P |
| FR-009 | IMP-036 / research / score meaning/calibration state | P |
| FR-010 | IMP-035–039 / research / package compatibility | P |
| FR-011 | IMP-036 / research / raw score retained on abstain | P |
| FR-012 | IMP-034 / research / applicability decision | NA? |
| FR-013 | IMP-047 / asset / inventory provenance | P |
| FR-014 | IMP-047 / asset / event-time ambiguity | P |
| FR-015 | IMP-048 / analyst / boundary and unknown policy | P |
| FR-016 | IMP-049 / analyst / evidence unavailable state | P |
| FR-017 | IMP-049 / research / validated method or AC-04 exception | P |
| FR-018 | IMP-049 / analyst / fact-vs-hypothesis links | P |
| FR-019 | IMP-049 / analyst / ATT&CK applicability | NA? |
| FR-020 | IMP-052 / safety / four check families | P |
| FR-021 | IMP-053 / safety / failed/unknown blocks eligibility | P |
| FR-022 | IMP-053 / technical / deterministic independent qualifiers | P |
| FR-023 | IMP-048 / analyst / risk/confidence separation | P |
| FR-024 | IMP-048 / technical / versioned re-score | P |
| FR-025 | IMP-058 / UI / incident field fixture | P |
| FR-026 | IMP-058 / UI / filter denominator fixture | P |
| FR-027 | IMP-059/065 / ops / freshness vs absence | P |
| FR-028 | IMP-060 / analyst / RBAC lifecycle audit | P |
| FR-029 | IMP-061 / safety / catalogue prerequisites | P |
| FR-030 | IMP-062 / security / immutable approval binding | P |
| FR-031 | IMP-062 / security / no-real-effect proof | P |
| FR-032 | IMP-064 / analytics / missing period vs zero | P |
| FR-033 | IMP-064 / research / forecasting gate | NA? |
| FR-034 | `IMP-079` / operations / delayed-label drift fixture | P |
| FR-035 | IMP-046 / technical / grouping boundary fixture | P |
| FR-036 | `IMP-016`, `IMP-050`, `IMP-077`, `IMP-078` / security / linked durable and integrity-protected audit record | P |
| FR-037 | `IMP-076`, `IMP-080` / security / operation denial and permission contract | P |
| FR-038 | IMP-037 / technical / activation rollback | P |
| FR-039 | `IMP-043`, `IMP-065`, `IMP-082` / evaluation / stage timing boundaries | P |
| FR-040 | `IMP-066`, `IMP-083` / privacy / minimized export/deletion | P |
| NFR-001 | IMP-025/070 / research / untouched test metrics | P |
| NFR-002 | IMP-065 / evaluation / p50-p99 declared span | P |
| NFR-003 | IMP-065 / ops / bounded overload and drops | P |
| NFR-004 | IMP-065 / ops / resource envelope | P |
| NFR-005 | `IMP-077` / technical / persisted-lost-replayed reconciliation | P |
| NFR-006 | IMP-069/072 / research / tolerance rerun | P |
| NFR-007 | IMP-007/065 / lab / approved Windows profile | P |
| NFR-008 | `IMP-018`, `IMP-080`, `IMP-081` / technical / producer-consumer compatibility | P |
| NFR-009 | IMP-063 / UI / evaluator task fixture | P |
| NFR-010 | IMP-059 / UI / keyboard and non-color checks | P |
| NFR-011 | IMP-017/059 / ops / health-state transitions | P |
| NFR-012 | `IMP-066`, `IMP-083` / privacy / deletion limitation disclosure | P |
| NFR-013 | `IMP-011`, `IMP-065`, `IMP-078`, `IMP-082` / evaluation / clock uncertainty | P |
| NFR-014 | IMP-049/065 / research / explanation overhead | P |
| SEC-001 | IMP-003/010 / lab / approved scope and stop | P |
| SEC-002 | `IMP-012`, `IMP-076` / security / capture privilege/IPC | P |
| SEC-003 | `IMP-076` / security / role/session denial | P |
| SEC-004 | `IMP-076`, `IMP-080` / security / encrypted transport and protected operation contract | P |
| SEC-005 | `IMP-018`, `IMP-056`, `IMP-084` / security / bounded untrusted input and artifact trust | P |
| SEC-006 | `IMP-003`, `IMP-066`, `IMP-083` / privacy / field and payload boundary | P |
| SEC-007 | `IMP-078` / security / tamper trust boundary | P |
| SEC-008 | IMP-053 / safety / fail closed | P |
| SEC-009 | IMP-062 / security / atomic TOCTOU revalidation | P |
| SEC-010 | `IMP-016`, `IMP-062`, `IMP-077`, `IMP-078` / security / denied and audit-failure behavior | P |
| SEC-011 | `IMP-035`, `IMP-037`, `IMP-084` / security / trusted artifact activation | P |
| SEC-012 | IMP-049 / security / inert rendering | P |
| DEM-001 | IMP-008/010 / lab / documented two-system path | P |
| DEM-002 | IMP-010 / lab / marker/count and wrong-interface controls | P |
| DEM-003 | IMP-007/009/012 / lab / Windows backend/privilege evidence | P |
| DEM-004 | IMP-023/041 / research / flow closure and partial policy | P |
| DEM-005 | IMP-022/042 / technical / feature parity | P |
| DEM-006 | IMP-063/067 / analyst / integrated scenario evidence | P |
| DEM-007 | IMP-044/067 / research / independent truth/matching | P |
| DEM-008 | IMP-003/012/067 / lab / isolation and stop | P |
| DEM-009 | IMP-063/067 / ops / recovery rehearsal | P |
| EVAL-001 | IMP-019 / research/privacy / rights/provenance | P |
| EVAL-002 | IMP-020 / research / manifest/fingerprint | P |
| EVAL-003 | IMP-024 / research / group/time leakage audit | P |
| EVAL-004 | `IMP-024`, `IMP-027`, `IMP-089` / research / fit boundaries and pre-access freeze | P |
| EVAL-005 | IMP-029–033 / research / fair model matrix | P |
| EVAL-006 | `IMP-034`, `IMP-070`, `IMP-089` / research / validation-only selection and pre-access freeze | P |
| EVAL-007 | IMP-034/070 / evaluation / support and undefined metrics | P |
| EVAL-008 | `IMP-045`, `IMP-068`, `IMP-089` / research / offline/live separation and sealed-source access | P |
| EVAL-009 | `IMP-034`, `IMP-070`, `IMP-069` / research / independent scenario uncertainty and reproducibility | P |
| EVAL-010 | IMP-054–057 / safety/research / fair DIG arms | P |
| EVAL-011 | IMP-034 / research / calibration evidence | NA? |
| EVAL-012 | IMP-021/057 / analyst / independent adjudication | P |
| EVAL-013 | `IMP-026`, `IMP-068`, `IMP-089` / research / applicability and sealed source | NA? |

`P` means evidence pending/not executed; `NA?` is not an accepted disposition and requires the named applicability decision. Roles, task additions, and all future checks remain proposed until M0/M1 approval.

### 3.2 Use-case traceability planning

Use cases are workflow-level identifiers rather than additional normative requirements. The proposed links below make the existing PRD workflows auditable without creating duplicate task IDs; `IMP-071` must expand each retained use case into the final evidence path, owner, reviewer, and disposition. UC-06 and UC-07 are covered by the PRD evaluation/operations sections even though they do not have separate H-section workflow narratives.

| Use case | Proposed requirement/evidence scope | Proposed implementation tasks |
|---|---|---|
| UC-01 normal-traffic observation | `FR-001`–`FR-011`, `DEM-001`–`DEM-005`, `NFR-002`–`NFR-005`, `AC-01`–`AC-02` | `IMP-007`–`IMP-018`, `IMP-040`–`IMP-045`, `IMP-065`–`IMP-067` |
| UC-02 supported attack detection/investigation | `FR-013`–`FR-024`, `FR-025`–`FR-031`, `DEM-006`–`DEM-007`, `AC-03`–`AC-08` | `IMP-046`–`IMP-063`, `IMP-067` |
| UC-03 uncertain/unsupported prediction | `FR-008`–`FR-011`, `FR-015`, `FR-020`–`FR-024`, `FR-027`, `NFR-009`, `NFR-014`, `AC-03`–`AC-07` | `IMP-034`, `IMP-036`, `IMP-043`, `IMP-048`–`IMP-060`, `IMP-061`–`IMP-063` |
| UC-04 input-integrity failure | `FR-003`–`FR-006`, `FR-010`, `FR-036`–`FR-040`, `SEC-005`, `SEC-007`–`SEC-010`, `AC-02`, `AC-05` | `IMP-015`–`IMP-018`, `IMP-022`, `IMP-028`, `IMP-035`–`IMP-043`, `IMP-049`–`IMP-053`, `IMP-065`–`IMP-066` |
| UC-05 response recommendation/simulation | `FR-020`–`FR-031`, `SEC-003`, `SEC-008`–`SEC-010`, `DEM-008`, `AC-05`, `AC-08` | `IMP-052`–`IMP-063` |
| UC-06 experiment/policy comparison | `OBJ-02`, `OBJ-05`, `EVAL-004`–`EVAL-013`, `TGT-01`–`TGT-07`, `AC-05`, `AC-09`–`AC-11` | `IMP-019`–`IMP-039`, `IMP-054`–`IMP-057`, `IMP-068`–`IMP-073`, `IMP-089` |
| UC-07 historical/operational-health review | `FR-027`, `FR-032`–`FR-034`, `NFR-002`–`NFR-006`, `NFR-009`–`NFR-014`, `AC-07`, `AC-09`–`AC-11` | `IMP-017`, `IMP-043`, `IMP-058`–`IMP-060`, `IMP-064`–`IMP-067`, `IMP-071`–`IMP-073` |

## 4. Acceptance-oracle gaps and status

The remediation adds proposed specifications for target actionability, API/session/RBAC, record envelopes, timing, `P_common`, final-source access classes/ledger, DIG rule tables, durable persistence/loss, audit trust, privacy/pseudonymization, explanation scope, and simulation-only evidence. The following remain unresolved approval/evidence gaps: numeric TGT values and exact uncertainty choices; dataset-specific source/label/flow/sequence decisions; Access Class D source designation and direction; M0 adoption of the milestone crosswalk; human owner/approval records; implementation and compatibility evidence for every contract; and independent no-effect, audit-integrity, recovery, privacy, and performance evidence. Proposed specifications do not authorize work or convert any control to implemented/verified.
