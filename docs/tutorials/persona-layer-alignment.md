# OSSF Toolbelt Personas ↔ Gemara Layer Alignment

This document maps [OSSF Toolbelt personas](https://github.com/ossf/toolbelt/tree/main/personas) to the [Gemara](https://gemara.openssf.org/) seven-layer model. **The primary step-by-step content lives in the tutorials** ([Guidance Catalog Guide](guidance/guidance-tutorial.md), [Threat Assessment](controls/threat-assessment.md) and [Control Catalog Guide](controls/control-catalog-guide.md), [Organization Risk & Policy](policy/organizational-policy.md)); this narrative explains who typically writes or ingests each layer's artifacts and how the personas align with those flows, so the tutorials and the narrative stay consistent.

---

## Tutorial user journeys (writing vs. ingesting)

For each definition layer, **who writes** the Gemara artifact, **who ingests** it, and **why** are made explicit below. Detail by persona type is in [Persona ↔ Layer Alignment](#persona--layer-alignment) and [Mapping by Persona Type](#mapping-by-persona-type-detail).

### Layer 1: Vectors and Guidance

**Personas writing Layer 1 artifacts:** Regulators, governing bodies, or authors of organization-specific guidance for internal reuse (e.g. translating existing controls from a spreadsheet into Gemara Layer 1 for widespread usage and compatibility—see use-cases such as CNCF CNSCC).

| Persona | Why / how |
|---------|-----------|
| **Guinevere** (GRC Security Governance Lead) — regulator/governing-body | Bridges policy and engineering. She has a unique internal use-case: developing guidance for use across **globally distributed teams**. She can author or adopt guidance catalogs that feed into policy and Layer 2. |
| **Grear** (GRC Data Governance Analyst) — regulator/governing-body | Works in a **government agency**; focuses on data/ML against **privacy regulations** (GDPR, HIPAA), traceability, and approvals. Use when guidance is **regulatory or governing-body driven** (e.g. data protection, public sector). |
| **Sachiko** (Solution Architect) — authoring or adopting guidance catalogs | “The person people go to when something needs to scale securely.” She shares **reusable secure-by-design blueprints**, aligns architecture with **SLSA and SAMM**, and can create guidance for **secure ML architecture** that Layer 2 can reference (e.g. guideline-mappings) and that maps to external frameworks. |

**Personas ingesting Layer 1 artifacts:** **Guinevere** (as GRC lead) when she pulls Layer 1 into Layer 2—control catalogs reference guidance via **guideline-mappings**. She uses Layer 1 to ensure policies and controls align to standards.

**Outputs:** Guidance catalogs (guidelines, families, mapping-references to OWASP, NIST, etc.). Tutorial: [Guidance Catalog Guide](guidance/guidance-tutorial.md).

### Layer 2: Threats and Controls

**Personas writing Layer 2 artifacts:** **Pang** (Product Security Engineer).

| Persona | Why / how |
|---------|-----------|
| **Pang** | Expert security engineer who collaborates with product engineers to perform a **secure architecture review** and **threat assessment**: identify **capabilities** and their **associated threats**. He cares how a system can be broken and what **technical controls** can prevent that. He outlines controls that mitigate the threats from the threat assessment and uses the **[FINOS CCC](https://www.finos.org/common-cloud-controls-project) Core** reusable catalog whenever possible to import capabilities, threats, or controls instead of redefining them. |

**Personas ingesting Layer 2 artifacts:**

| Persona | Why |
|---------|-----|
| **Guinevere** | Uses Pang’s **threat-informed controls** to support **risk mitigation**. She needs to know that a “Data Breach” has $10M impact and that Pang’s controls reduce likelihood. Threats are mitigated by controls, which support mitigation of **associated risks** in Layer 3. |
| **Carl** (the Consumer) — vendor/manufacturer ([Toolbelt personas](https://github.com/ossf/toolbelt/tree/main/personas)) | **Ingests** threat and control catalogs (e.g. OSPS Baseline, CCC) to **understand the security posture** of the open source software his project consumes. He wants the software he's building to **comply with regulatory requirements** and **manage risks associated with consumption**. Layer 2 control catalogs and evaluation outputs (e.g. Layer 5 Pass/Fail) give him a clear view of what to satisfy. |

**Outputs:** **Threat catalog** (specific ways capabilities can be misused or exploited); **Control catalog** (defense mechanisms based on what could go wrong). Tutorials: [Threat Assessment Guide](controls/threat-assessment.md), [Control Catalog Guide](controls/control-catalog-guide.md).

### Layer 3: Risk and Policy

**Personas writing Layer 3 artifacts:** **Guinevere** (GRC Security Governance Lead). A **Compliance** persona may assist with **assessment plan** and **scope**.

| Persona | Why / how |
|---------|-----------|
| **Guinevere** | She is the **risk manager** who maps **threats to business impact**. She contextualizes Pang’s Layer 2 outputs into a **risk register**: she ingests the threat catalog, assigns **business impact** and **likelihood** to turn technical threats into **informed risks**. By mapping risks to the organization’s **risk appetite**, she defines **policy**—which risks are **accepted** vs **mitigated** (triggering enforcement of Pang’s controls)—and establishes **assessment requirements** for mitigations. Pang’s automation can use this policy to pull in and validate the right security controls in the scoped environment. |

**Personas ingesting Layer 3 artifacts:**

| Persona | Why |
|---------|-----|
| **Governance lead** (with expertise in scope / assessment plan) | Uses **Layer 1** vectors and guidance, is aware of **technology scope** and **evaluation methods**, and ensures **adherence** (how often assessments and evaluations run). |
| **Grear** (Data Governance Analyst) | Her organization must meet **GDPR and HIPAA**. She uses the policy’s **adherence** and **assessment plan** to define how often assessments and evaluations occur and to enable compliance. |

**Outputs:** Policy document (scope, imports, risks, adherence, implementation-plan). Tutorial: [Organization Risk & Policy](policy/organizational-policy.md).

---

## Schema and validation

- **Schemas:** Tutorials conform to **layer-1.cue** (Guidance Catalog), **layer-2.cue** (Threat/Control Catalog), **layer-3.cue** (Policy). Validation uses the corresponding CUE definition (e.g. `cue vet -c -d '#GuidanceCatalog' …`).
- **Personas:** Names and roles are from the [OSSF Toolbelt personas](https://github.com/ossf/toolbelt/tree/main/personas); primary tutorial personas are Sachiko (L1), Pang (L2), Guinevere (L3).

---

## Tutorial primary personas (for coherence)

| Layer | Primary persona | Role |
|-------|------------------|------|
| **Layer 1** | **Sachiko** | Solution Architect; guidance catalogs for org/domain reuse. For regulator context: **Grear** or **Regulated Ragnar**. |
| **Layer 2** | **Pang** | Product Security Engineer; threat and control catalogs (assessment requirements). |
| **Layer 3** | **Guinevere** | GRC / Security Governance Lead; policy (scope, imports, adherence, risks). |

Who writes vs. ingests each layer: see [Tutorial user journeys](#tutorial-user-journeys-writing-vs-ingesting) above.

## Gemara Layers (Summary)

| Layer | Name | Primary Artifacts | Who typically authors/consumes | Tutorial persona |
|-------|------|-------------------|--------------------------------|------------------|
| **1** | Guidance | Vectors, Guidance Catalogs (guidelines, families, mapping-references to OWASP, NIST, HIPAA, etc.) | Standards bodies, industry groups, internal teams | **Sachiko** (org-specific); **Grear** / **Regulated Ragnar** (regulator/governing-body) |
| **2** | Controls | Threat Catalogs (capabilities, threats), Control Catalogs (controls, assessment-requirements, guideline-mappings) | Security/GRC, platform/infra, project baselines | **Pang** |
| **3** | Policy | Risk Catalogs, Policy Documents (scope, imports, adherence, optional implementation-plan, risks) | GRC, CISO, compliance, legal | **Guinevere** |
| **4** | Sensitive Activities | (Bridge: where policy meets implementation) | Implementers (dev, infra, data) |
| **5** | Evaluation | Evaluation Logs (intent + behavioral evaluations, assessment-logs, control-id, result) | Evaluators, QA, security, tooling (e.g. Privateer) |
| **6** | Enforcement | Preventive (gates) and Remediative (corrective) actions | Platform/infra, security, CI/CD |
| **7** | Audit & Monitoring | Audit logs, continuous compliance monitoring | Auditors, CISO, compliance |

For primary/supporting personas by type, see [Persona ↔ Layer Alignment](#persona--layer-alignment) and [Mapping by Persona Type](#mapping-by-persona-type-detail) below.

**Projects and tooling using Gemara (Layer 2):** [FINOS CCC](https://www.finos.org/common-cloud-controls-project) (CCC), [OSPS Baseline](https://baseline.openssf.org/), and [Privateer](https://github.com/privateerproj/privateer) (Layer 5). See [References](#references) for schema and tutorial links.

---

## Persona ↔ Layer Alignment

### Strongest fits (primary layer engagement)

| Persona | Best-fit Gemara layer(s) | Rationale |
|---------|-------------------------|-----------|
| **Security Engineer** (Pang, Guinevere, Finn) | **Layer 2, Layer 3, Layer 5** | Defines and consumes **controls** (threat-informed, assessment requirements); aligns policy with standards (Layer 3); runs or interprets **evaluations** (config scans, pentests). Test-data: `good-ccc.yaml`, `good-osps.yml`, `pvtr-baseline-scan.yaml`. |
| **Open Source Professional** (Siddharth, Carl, OSPO, Regulated Ragnar) | **Layer 2, Layer 3** | **Consumers/suppliers** (e.g. **Carl the Consumer** — vendor/manufacturer from [Toolbelt](https://github.com/ossf/toolbelt/tree/main/personas)) need to assess OSS posture (Scorecard, SLSA, S2C2F) and meet regulatory/compliance requirements. They consume **control catalogs** (e.g. OSPS Baseline, CCC) and **policies** that reference them (Layer 2: `layer-2.cue`; Layer 3: `layer-3.cue`). |
| **Infrastructure/Platform Engineer** (Chinmay, Ophelia) | **Layer 2, Layer 5, Layer 6** | Build and operate systems that **implement controls** (e.g. image integrity, supply chain); run **evaluations** (config scans, pipeline checks) and **enforcement** (gates, remediation). Tutorials: threat assessment → control catalog → validation. |
| **Data Operations Practitioner** (Daniel, Dibby, Grear GRC) | **Layer 1, Layer 2, Layer 3** | **Data governance** aligns with **guidance** (privacy/ML standards), **controls** (data protection, lineage), and **policy** (GDPR, HIPAA). Personas explicitly want schemas, policy-as-code, and auditability—all Gemara concerns. |

### Secondary / cross-layer fits

| Persona | Relevant layers | Rationale |
|---------|------------------|-----------|
| **Software Developer** (Debbie, Ursula, Danika, Sachiko, Allison, Timmy) | **Layer 1, Layer 4, Layer 5** | **Consume guidance** (best practices, standards); perform **sensitive activities** (design, code, deploy); are **evaluated** (e.g. OSPS checks via Privateer). Tutorials: guidance catalog (Layer 1), control catalog used in evaluation (Layer 5). |
| **Executive / C-Suite** | **Layer 3, Layer 7** | Own **risk appetite** and **policy**; consume **audit** and high-level compliance posture. |
| **Product Marketing & Community** | **Layer 1 (messaging)** | Communicate **guidance** and baselines (e.g. “we follow OSPS/OWASP”) rather than authoring catalogs. |

---

## Mapping by Persona Type (Detail)

### 1. Security Engineer → Layers 2, 3, 5

- **Pang (Product Security Engineer)**  
  - Bakes security into backlogs, uses standards (e.g. SLSA, SAMM), integrates tooling with CI/CD.  
  - **Layer 2**: Authors or adopts **control catalogs** with assessment requirements; **Layer 5**: Uses evaluation tooling (e.g. Privateer) that produces logs keyed to `control-id` and assessment requirements (see `pvtr-baseline-scan.yaml`).

- **Guinevere (GRC / Security Governance Lead)**  
  - Defines “secure enough,” policy-as-code, baselines.  
  - **Layer 3**: **Policy documents** that reference controls and risks; **Layer 2**: Reusable control catalogs and threat mappings.

- **Finn (Finder)**  
  - Focus on vulnerabilities and disclosure; less direct authorship of Gemara artifacts but feeds into **threat** and **control** refinement (Layer 2) and evidence for **evaluation/audit** (Layers 5–7).

### 2. Open Source Professional (OSPO, Supplier, Consumer) → Layers 2, 3

- **Siddharth (Supplier)**  
  - Needs to understand upstream OSS and comply with regulations; uses Scorecard, SLSA, S2C2F, OSV, OpenVEX.  
  - **Layer 2**: Consumes **threat/control catalogs** (e.g. OSPS, CCC) to assess and communicate posture; **Layer 3**: Organizational **policies** that reference those controls.

- **Carl (Consumer)** — vendor/manufacturer ([Toolbelt](https://github.com/ossf/toolbelt/tree/main/personas))  
  - Must **understand the security posture** of the open source software his project consumes; wants the software he's building to **comply with regulatory requirements** and **manage risks associated with consumption**.  
  - **Layer 2**: Consumes **control catalogs** and threat catalogs (e.g. OSPS, CCC) to assess posture; **Layer 5**: Evaluation results (e.g. Pass/Fail per control) as in `pvtr-baseline-scan.yaml`.

- **Regulated Ragnar**  
  - Strong **Layer 3** (policy, risk) and **Layer 2** (controls that satisfy regulators).

### 3. Infrastructure/Platform Engineer → Layers 2, 5, 6

- **Chinmay (Cloud Platform Admin)**  
  - Secures infra, integrates supply chain checks into build/deploy, needs “secure enough” OSS.  
  - **Layer 2**: Implements **controls** (e.g. from CCC, OSPS); **Layer 5**: Config/behavior **evaluations**; **Layer 6**: **Enforcement** (gates, remediation).

- **Ophelia (IT Infrastructure Engineer)**  
  - CI/CD, Terraform, Argo; wants reusable, secure patterns.  
  - Same layer pattern: **controls** in catalogs, **evaluation** in pipelines, **enforcement** on non-compliance.

### 4. Data Operations Practitioner → Layers 1, 2, 3

- **Daniel (Data Scientist)**  
  - Reproducibility, model provenance, alignment with regulations.  
  - **Layer 1**: **Guidance** (e.g. secure ML, model cards); **Layer 2**: Controls for data/model integrity; **Layer 3**: Data protection **policy**.

- **Dibby (Data Engineer)**  
  - Pipeline security, provenance, compliance.  
  - **Layer 2**: **Controls** for pipelines (e.g. signing, validation); **Layer 3**: **Policy** for data traceability and compliance.

- **Grear (Data Governance Analyst)**  
  - Privacy, classification, auditability.  
  - **Layer 3**: **Policy** (GDPR, HIPAA); **Layer 2**: Controls and schemas for classification/lineage; **Layer 7**: **Audit** evidence.

### 5. Software Developer → Layers 1, 4, 5

- **Debbie, Ursula, Danika**  
  - Consume **guidance** (Layer 1) and work inside **sensitive activities** (Layer 4); their work is **evaluated** (Layer 5) via control checks (e.g. OSPS in Privateer).

- **Sachiko (Solution Architect), Allison (AI/ML Engineer), Timmy (Test Engineer)**  
  - Align architecture and pipelines with **guidance** and **controls** (Layers 1–2); tests and pipelines contribute to **evaluation** (Layer 5).

### 6. Executive / C-Suite → Layers 3, 7

- Own **risk appetite**, **policy** (Layer 3), and consume **audit** and **continuous monitoring** (Layer 7). They approve scope and compliance posture rather than authoring catalogs.

### 7. Product Marketing & Community → Layer 1 (messaging)

- Communicate adoption of **guidance** and baselines (e.g. “aligned with OSPS/OWASP”) without necessarily authoring Gemara artifacts.

---

## References

- **Gemara model**: [docs/model](https://github.com/gemaraproj/gemara/tree/main/docs/model) (Layers 1–7). Layer 1 narrative (Vectors, Guidance, “without foreknowledge of implementation details,” authors: internal / industry / government / standards bodies): [05.1-Layer-1.md](../model/05.1-Layer-1.md).
- **Layer 1 schema**: [layer-1.cue](../../layer-1.cue) (guidance catalogs, guidelines, catalog type, vector-mappings).
- **Layer 2 schema**: [layer-2.cue](../../layer-2.cue) (threat catalogs, control catalogs). See also **projects using Gemara at Layer 2**: [FINOS CCC](https://www.finos.org/common-cloud-controls-project), [OSPS Baseline](https://baseline.openssf.org/).
- **Layer 3 schema**: [layer-3.cue](../../layer-3.cue) (policy documents: scope, imports, adherence, etc.).
- **Tutorials**: [Guidance Catalog Guide](guidance/guidance-tutorial.md) (Layer 1), [Threat Assessment](controls/threat-assessment.md) and [Control Catalog Guide](controls/control-catalog-guide.md) (Layer 2).
- **Test-data**: `good-ccc.yaml`, `good-osps.yml` (Layer 2), `pvtr-baseline-scan.yaml` (Layer 5). For Layer 3, use the [layer-3.cue](https://github.com/gemaraproj/gemara/blob/main/layer-3.cue) schema as the source of truth (test-data policy examples may not be up-to-date).
- **OSSF Toolbelt personas**: [personas](https://github.com/ossf/toolbelt/tree/main/personas) (README + softwaredeveloper, securityengineer, opensourcepro, dataoperationspractitioner, infrastructureandplatformengineer).
- **Tutorial primary personas (canonical descriptions)**:
  - **Sachiko** (Layer 1): [Software Developer personas → Sachiko the Solution Architect](https://github.com/ossf/toolbelt/blob/main/personas/softwaredeveloper.md#name-sachiko-the-solution-architect).
  - **Grear** (Layer 1, regulator/governing-body lens): [Data Operations Practitioner → Grear GRC (Data Governance Analyst)](https://github.com/ossf/toolbelt/blob/main/personas/dataoperationspractitioner.md#name-noura-the-data-governance-analyst).
  - **Pang** (Layer 2): [Security Engineer personas → Pang the Product Security Engineer](https://github.com/ossf/toolbelt/blob/main/personas/securityengineer.md#name-pang-the-product-security-engineer).
  - **Guinevere** (Layer 3): [Security Engineer personas → Guinevere GRC (Security Governance Lead)](https://github.com/ossf/toolbelt/blob/main/personas/securityengineer.md#name-guinevere-grc-security-governance-lead).
