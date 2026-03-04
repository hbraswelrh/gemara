---
layout: page
title: Gemara Layers — Knowledge, Inputs & Outputs
---

This document explains **what knowledge you need**, **what inputs you use**, and **what outputs you produce** at each [Gemara](https://gemara.openssf.org/) layer—so you can see where you operate without prior familiarity with roles or personas. **Tutorials:** [Threat Assessment](controls/threat-assessment-guide) and [Control Catalog Guide](controls/control-catalog-guide) (Layer 2). Layer 1 and Layer 3 tutorials coming later. Optional: [OSSF Toolbelt personas](https://github.com/ossf/toolbelt/tree/main/personas) map to these layers for narrative examples.

---

## Gemara layers: knowledge, inputs, outputs, and roles

One table to see **what you need to know**, **what you consume**, **what you produce**, and **who typically authors or consumes** at each layer.

| Layer | Name | Knowledge needed | Inputs | Outputs | Who typically authors | Who typically consumes |
|-------|------|-------------------|--------|--------|------------------------|--------------------------|
| **1** | Vectors & Guidance | Standards/frameworks (OWASP, NIST, HIPAA); expressing guidelines and mapping-references | — | Guidance catalogs (guidelines, families, mapping-references) | Org/regulator authors of guidance | Policy authors (L3); control authors (L2); developers |
| **2** | Threats & Controls | Threats and mitigations; how capabilities can be misused; technical controls and assessment requirements | L1 (optional) | Threat catalogs; Control catalogs (controls, assessment-requirements) | Security engineers; threat/control authors | Policy authors (L3); evaluators (L5); implementers |
| **3** | Risk & Policy | Risk appetite; scope (technologies, sensitivity); referencing L1/L2 and defining adherence | L1, L2 | Policy documents (scope, imports, adherence, risks, implementation-plan) | GRC/policy owners | Implementers (L4); evaluators (L5); auditors (L7) |
| **4** | Sensitive Activities | Implementing securely within policy (bridge: policy → implementation) | L3 | Actions that may introduce risk (design, build, deploy, operate) | Implementers (dev, infra, data) | Evaluators (L5) |
| **5** | Intent & Behavioral Evaluation | Running checks against controls and policy; assessment requirements and evidence | L2, L3 | Evaluation logs (control-id, result, assessment-logs) | Evaluators, QA, tooling (e.g. Privateer) | Enforcement (L6); auditors (L7) |
| **6** | Preventive & Remediative Enforcement | Gating or remediating from evaluation results and policy | L3, L5 | Gates; remediative actions | Platform/infra, security, CI/CD | Auditors (L7); policy owners |
| **7** | Audit & Monitoring | Assessing compliance and effectiveness of the full stack | L3, L5, L6 | Audit logs; continuous compliance monitoring | Auditors, CISO, compliance | Executive; policy owners; regulators |

**Projects and tooling:** [FINOS CCC](https://www.finos.org/common-cloud-controls-project) (CCC), [OSPS Baseline](https://baseline.openssf.org/), [Privateer](https://github.com/privateerproj/privateer) (Layer 5). **Tutorials:** Layer 2 → [Threat Assessment](controls/threat-assessment-guide), [Control Catalog Guide](controls/control-catalog-guide); Layer 1 and Layer 3 coming later. See [References](#references) for schemas.

---

## Mapping by Role and Layer

### 1. Security Engineer → Layers 2, 3, 5

- **Users who author or adopt threat catalogs**
  - Typically bake security into backlogs, use standards (e.g. SLSA, SAMM), and integrate tooling with CI/CD.
  - **Layer 2**: Author or adopt **control catalogs** with assessment requirements; **Layer 5**: Use evaluation tooling (e.g. Privateer) that produces logs keyed to `control-id` and assessment requirements (see `pvtr-baseline-scan.yaml`).

- **User who already performed a threat assessment - *still in Layer 2***
  - Typically author or adopt **control catalogs** with assessment requirements and can modify assessment requirements to fit the needs of the organization.
- **Users who define policy and risk baselines**
  - Typically define "secure enough," policy-as-code, and baselines.
  - **Layer 3**: **Policy documents** that reference controls and risks; **Layer 2**: Reusable control catalogs and threat mappings.

- **Users who focus on vulnerabilities and disclosure**
  - Typically have less direct authorship of Gemara artifacts but feed into **threat** and **control** refinement (Layer 2) and evidence for **evaluation/audit** (Layers 5–7).

### 2. Open Source Professional (OSPO, Supplier, Consumer) → Layers 2, 3

- **Users who supply or distribute OSS**
  - Typically need to understand upstream OSS and comply with regulations; use Scorecard, SLSA, S2C2F, OSV, OpenVEX.
  - **Layer 2**: Consume **threat/control catalogs** (e.g. OSPS, CCC) to assess and communicate posture; **Layer 3**: Organizational **policies** that reference those controls.

- **Users who consume OSS**
  - Typically must **understand the security posture** of the open source software their project consumes and want the software they build to **comply with regulatory requirements** and **manage risks associated with consumption**.
  - **Layer 2**: Consume **control catalogs** and threat catalogs (e.g. OSPS, CCC) to assess posture; **Layer 5**: Evaluation results (e.g. Pass/Fail per control) as in `pvtr-baseline-scan.yaml`.

- **Users in regulated environments**
  - Typically rely on **Layer 3** (policy, risk) and **Layer 2** (controls that satisfy regulators).

### 3. Infrastructure/Platform Engineer → Layers 2, 5, 6

- **Users who secure infrastructure and build/deploy**
  - Typically integrate supply chain checks into build/deploy and need "secure enough" OSS.
  - **Layer 2**: Implement **controls** (e.g. from CCC, OSPS); **Layer 5**: Config/behavior **evaluations**; **Layer 6**: **Enforcement** (gates, remediation).

- **Users who own CI/CD and platform tooling**
  - Typically want reusable, secure patterns (e.g. Terraform, Argo).
  - Same layer pattern: **controls** in catalogs, **evaluation** in pipelines, **enforcement** on non-compliance.

### 4. Data Operations Practitioner → Layers 1, 2, 3

- **Users who work with data science and ML**
  - Typically care about reproducibility, model provenance, and alignment with regulations.
  - **Layer 1**: **Guidance** (e.g. secure ML, model cards); **Layer 2**: Controls for data/model integrity; **Layer 3**: Data protection **policy**.

- **Users who own data pipelines**
  - Typically care about pipeline security, provenance, and compliance.
  - **Layer 2**: **Controls** for pipelines (e.g. signing, validation); **Layer 3**: **Policy** for data traceability and compliance.

- **Users who focus on data governance and privacy**
  - Typically care about classification, auditability, and regulatory alignment (e.g. GDPR, HIPAA).
  - **Layer 3**: **Policy** (GDPR, HIPAA); **Layer 2**: Controls and schemas for classification/lineage; **Layer 7**: **Audit** evidence.

### 5. Software Developer → Layers 1, 4, 5

- **Users who implement features and ship code**
  - Typically consume **guidance** (Layer 1) and work inside **sensitive activities** (Layer 4); their work is **evaluated** (Layer 5) via control checks (e.g. OSPS in Privateer).

- **Users who design architecture, AI/ML systems, or test pipelines**
  - Typically align architecture and pipelines with **guidance** and **controls** (Layers 1–2); tests and pipelines contribute to **evaluation** (Layer 5).

### 6. Executive / C-Suite → Layers 3, 7

- Typically own **risk appetite**, **policy** (Layer 3), and consume **audit** and **continuous monitoring** (Layer 7). They approve scope and compliance posture rather than authoring catalogs.

### 7. Product Marketing & Community → Layer 1 (messaging)

- Typically communicate adoption of **guidance** and baselines (e.g. "aligned with OSPS/OWASP") without necessarily authoring Gemara artifacts.

---

## Schema and validation

- **Schemas:** Tutorials conform to **layer-1.cue** (Guidance Catalog), **layer-2.cue** (Threat/Control Catalog), **layer-3.cue** (Policy). Validation uses the corresponding CUE definition (e.g. `cue vet -c -d '#GuidanceCatalog' …`).
- **Optional:** [OSSF Toolbelt personas](https://github.com/ossf/toolbelt/tree/main/personas) map to these layers for narrative examples (e.g. L1/L2/L3 authors and consumers).

---

## References

- **Gemara model**: [docs/model](https://github.com/gemaraproj/gemara/tree/main/docs/model) (Layers 1–7). Layer 1 narrative (Vectors, Guidance, "without foreknowledge of implementation details," authors: internal / industry / government / standards bodies): [05.1-Layer-1.md](../model/05.1-Layer-1).
- **Layer 1 schema**: [layer-1.cue](https://github.com/gemaraproj/gemara/blob/main/layer-1.cue) (guidance catalogs, guidelines, catalog type, vectors).
- **Layer 2 schema**: [layer-2.cue](https://github.com/gemaraproj/gemara/blob/main/layer-2.cue) (threat catalogs, control catalogs). See also **projects using Gemara at Layer 2**: [FINOS CCC](https://www.finos.org/common-cloud-controls-project), [OSPS Baseline](https://baseline.openssf.org/).
- **Layer 3 schema**: [layer-3.cue](https://github.com/gemaraproj/gemara/blob/main/layer-3.cue) (policy documents: scope, imports, adherence, etc.).
- **Tutorials**: [Threat Assessment](controls/threat-assessment-guide) and [Control Catalog Guide](controls/control-catalog-guide) (Layer 2). Layer 1 and Layer 3 tutorials coming later.
- **Test-data**: `good-ccc.yaml`, `good-osps.yml` (Layer 2), `pvtr-baseline-scan.yaml` (Layer 5). For Layer 3, use the [layer-3.cue](https://github.com/gemaraproj/gemara/blob/main/layer-3.cue) schema as the source of truth (test-data policy examples may not be up-to-date).
- **OSSF Toolbelt personas**: [personas](https://github.com/ossf/toolbelt/tree/main/personas) (README + softwaredeveloper, securityengineer, opensourcepro, dataoperationspractitioner, infrastructureandplatformengineer).
- **Tutorial primary personas (canonical descriptions)**:
  - **Sachiko** (Layer 1): [Software Developer personas → Sachiko the Solution Architect](https://github.com/ossf/toolbelt/blob/main/personas/softwaredeveloper.md#name-sachiko-the-solution-architect).
  - **Grear** (Layer 1, regulator/governing-body lens): [Data Operations Practitioner → Grear GRC (Data Governance Analyst)](https://github.com/ossf/toolbelt/blob/main/personas/dataoperationspractitioner.md#name-noura-the-data-governance-analyst).
  - **Pang** (Layer 2): [Security Engineer personas → Pang the Product Security Engineer](https://github.com/ossf/toolbelt/blob/main/personas/securityengineer.md#name-pang-the-product-security-engineer).
  - **Guinevere** (Layer 3): [Security Engineer personas → Guinevere GRC (Security Governance Lead)](https://github.com/ossf/toolbelt/blob/main/personas/securityengineer.md#name-guinevere-grc-security-governance-lead).
