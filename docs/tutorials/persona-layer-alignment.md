# OSSF Toolbelt Personas ↔ Gemara Layer Alignment

This document maps [OSSF Toolbelt personas](https://github.com/ossf/toolbelt/tree/main/personas) to the [Gemara](https://gemara.openssf.org/) seven-layer model, using the project’s tutorials, test-data, and schemas to ground the analysis.

---

## Tutorial primary personas (for coherence)

To keep the tutorials consistent and easy to follow, **one named persona is designated as the primary lens for each definition layer** that the tutorials cover:

| Layer | Primary tutorial persona | Role (Toolbelt) | Use in tutorials |
|-------|--------------------------|------------------|-------------------|
| **Layer 1 – Guidance** | **Sachiko** (Sachiko the Solution Architect) | Principal Software Architect; defines secure design patterns, blueprints, and alignment with frameworks (SLSA, SAMM); shares internal guidance for reuse. | Reference Sachiko when authoring or adopting **guidance catalogs** (vectors, guidelines, families) for org-specific or domain-specific reuse. Example: *“As Sachiko, you’re capturing secure-by-design guidelines and mapping them to OWASP/NIST so teams can reuse this guidance internally.”* For regulator/governing-body contexts, use **Grear** or **Regulated Ragnar** (see Layer 1 section below). |
| **Layer 2 – Controls** | **Pang** (Pang the Product Security Engineer) | Product Security Engineer; security champion embedded in engineering; bakes controls into backlogs, uses SLSA/SAMM, integrates tooling with CI/CD. | Reference Pang when authoring or adopting **threat catalogs** and **control catalogs** (threat assessment, control catalog guide). Example: *“As Pang, you’re defining controls with assessment requirements so teams can be evaluated against them.”* |
| **Layer 3 – Policy** | **Guinevere** (Guinevere GRC – Security Governance Lead) | Security Policy / GRC lead; defines “secure enough,” policy-as-code, baselines; aligns internal policy with controls and risk. | Reference Guinevere when authoring or consuming **policy documents** that reference risks and controls (per `layer-3.cue`: scope, imports, adherence, etc.). Example: *“As Guinevere, you’re turning organizational risk appetite and control selections into a policy document with scope, imports, and adherence.”* |

Use **Sachiko** for Layer 1 (Guidance), **Pang** for Layer 2 (Controls), and **Guinevere** for Layer 3 (Policy) consistently across the guidance, control-catalog, threat-assessment, and policy tutorials so readers have a single, coherent persona thread.

---

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

**Projects and tooling using Gemara (Layer 2):** [FINOS Common Cloud Controls](https://www.finos.org/common-cloud-controls-project) (CCC) and the [Open Source Project Security Baseline](https://baseline.openssf.org/) (OSPS Baseline) are projects that produce Layer 2 artifacts (control catalogs and, for CCC, threat catalogs) conforming to the Gemara schemas. The tutorials and test-data reference them (e.g. `good-ccc.yaml`, `good-osps.yml`). [Privateer](https://github.com/privateerproj/privateer) (Layer 5) evaluates repositories against the OSPS Baseline.

---

## Layer 1: Vectors and Guidance — persona fit

### Consistency with the Layer 1 schema and model

The alignment below is consistent with the [Layer 1 model](https://gemara.openssf.org/) and the **layer-1.cue** schema:

- **Why Layer 1 exists:** The need for generic, high-level risk assessment is often surfaced by factors or requirements **far removed from the scope of the activity being assessed**. Sometimes the need is made clear by a **Rule** (e.g. legislation); other times it is a **precursor for Controls** in a new technology category not yet fully assessed (e.g. artificial intelligence). Personas that author or adopt Layer 1 guidance often act in response to one or both of these drivers (regulators/governing bodies → Rules; orgs and industry → new tech or unique use-cases).

- **Vectors** (referenced by guidelines via `vector-mappings` in the schema): Focus on the **opportunity for mistake or malice**, not on technological intricacies. They can be documented independently or in a catalog and published as standalone artifacts or alongside related Guidance (e.g. MITRE ATT&CK techniques). The schema allows guidelines to link to vectors via `"vector-mappings"` in `#Guideline`.

- **Guidance / Guidelines:** The constituent parts of Guidance are **guidelines**, which do not typically stand alone and are most often published in a **Guidance Catalog** (`#GuidanceCatalog`). Each guideline includes explanatory context and recommendations for **designing optimal outcomes without foreknowledge of implementation details** (per the schema comment and model). Catalogs have a **type** (`#GuidanceType`): Standard, Regulation, Best Practice, or Framework—aligning with legislation (Regulation), standards bodies (Standard), and industry or internal use (Best Practice, Framework).

- **Who authors Guidance:** Guidance may be written **internally for unique circumstances** or by **industry groups, government agencies, or international standards bodies** (e.g. OWASP, NIST, HIPAA, GDPR, CRA, PCI, ISO). The personas below map to these authors: regulators/governing bodies (Regulation/Standard); org-specific reuse (Best Practice/Framework).

### Personas for Layer 1

Layer 1 artifacts (vectors, guidance catalogs) are authored or adopted by **regulators**, **governing bodies**, or **organizations for unique use-cases** so that guidance can be reused internally or across an ecosystem.

Best-fit OpenSSF Toolbelt personas for Layer 1, by lens:

| Lens | Persona(s) | Rationale |
|------|------------|-----------|
| **Regulators / governing bodies** | **Grear** (Grear GRC – Data Governance Analyst), **Regulated Ragnar** (Open Source Pro) | **Grear** works in a government agency; ensures data/ML respects privacy regulations (GDPR, HIPAA) and maintains traceability. She authors or adopts **guidance** that reflects regulatory and governing-body requirements. **Regulated Ragnar** operates in a regulated industry and must align to regulator and governing-body guidance; he often translates external guidance into org-specific adoption. |
| **Organization-specific guidance (unique use-case, internal reuse)** | **Sachiko** (Solution Architect), **Guinevere** (GRC), **Olga** (OSPO), **Daniel / Dibby** (Data Ops) | **Sachiko** defines secure design patterns and blueprints, aligns with SLSA/SAMM, and shares internal guidance for reuse—ideal for authoring a **guidance catalog** with families and guideline-mappings. **Guinevere** defines baselines and “secure enough”; she can author or adopt guidance catalogs that feed into policy (Layer 3). **Olga** (OSPO) curates OpenSSF Security Baseline and org standards; adopts/adapts guidance for the org. **Daniel** (Data Scientist) and **Dibby** (Data Engineer) author or adopt domain-specific guidance (e.g. secure ML, model cards, pipeline provenance) for internal reuse. |

**Tutorial primary for Layer 1:** Use **Sachiko** when the tutorial focuses on **authoring or adopting a guidance catalog** for org-specific or domain-specific reuse (e.g. secure development, supply chain, or ML). Use **Grear** or **Regulated Ragnar** when the scenario is **regulator- or governing-body-aligned** guidance (e.g. data protection, regulated industry).

---

## Persona ↔ Layer Alignment

### Strongest fits (primary layer engagement)

| Persona | Best-fit Gemara layer(s) | Rationale |
|---------|-------------------------|-----------|
| **Security Engineer** (Pang, Guinevere, Finn) | **Layer 2, Layer 3, Layer 5** | Defines and consumes **controls** (threat-informed, assessment requirements); aligns policy with standards (Layer 3); runs or interprets **evaluations** (config scans, pentests). Test-data: `good-ccc.yaml`, `good-osps.yml`, `pvtr-baseline-scan.yaml`. |
| **Open Source Professional** (Siddharth, Carl, OSPO, Regulated Ragnar) | **Layer 2, Layer 3** | **Consumers/suppliers** need to assess OSS posture (Scorecard, SLSA, S2C2F) and meet regulatory/compliance requirements. They consume **control catalogs** (e.g. OSPS Baseline, CCC) and **policies** that reference them (Layer 2: `layer-2.cue`; Layer 3: `layer-3.cue`). |
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

- **Carl (Consumer)**  
  - Wants scalable assessment of OSS security and compliance.  
  - **Layer 2**: Consumes **control catalogs** and evaluation outputs; **Layer 5**: Evaluation results (e.g. Pass/Fail per control) as in `pvtr-baseline-scan.yaml`.

- **Regulated Ragnar**  
  - Strong **Layer 3** (policy, risk) and **Layer 2** (controls that satisfy regulators).

### 3. Infrastructure/Platform Engineer → Layers 2, 5, 6

- **Chinmay (Cloud Platform Admin)**  
  - Secures infra, integrates supply chain checks into build/deploy, needs “secure enough” OSS.  
  - **Layer 2**: Implements **controls** (e.g. from CCC, OSPS); **Layer 5**: Config/behavior **evaluations**; **Layer 6**: **Enforcement** (gates, remediation).

- **Ophelia (IT Infrastructure Engineer)**  
  - CI/CD, Terraform, Argo; wants reusable, secure patterns.  
  - Same layer pattern: **controls** in catalogs, **evaluation** in pipelines, **enforcement** on non-compliance.

Tutorials (threat assessment → control catalog) and test-data (`good-ccc.yaml`, `good-osps.yml`) describe exactly the kind of catalogs these personas consume or help implement.

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

The **guidance tutorial** and **guidance-example.yaml** (Layer 1) and the **control-catalog** flow (Layer 2) plus **pvtr-baseline-scan** (Layer 5) show how developers sit between guidance consumption and being evaluated.

### 6. Executive / C-Suite → Layers 3, 7

- Own **risk appetite**, **policy** (Layer 3), and consume **audit** and **continuous monitoring** (Layer 7). They approve scope and compliance posture rather than authoring catalogs.

### 7. Product Marketing & Community → Layer 1 (messaging)

- Communicate adoption of **guidance** and baselines (e.g. “aligned with OSPS/OWASP”) without necessarily authoring Gemara artifacts.

---

## Summary Table: Best Persona–Layer Fit

| Gemara Layer | Tutorial primary persona | Primary personas (broad) | Supporting personas |
|--------------|-------------------------|--------------------------|----------------------|
| **Layer 1 – Guidance** | **Sachiko** (org-specific); **Grear** / **Regulated Ragnar** (regulator/governing-body) | Data Ops (governance/ML), Security (GRC), Open Source Pro (OSPO) | Software Developer, Product Marketing |
| **Layer 2 – Controls** | **Pang** | Security Engineer, Open Source Pro, Infra/Platform, Data Ops | Software Developer (consumer) |
| **Layer 3 – Policy** | **Guinevere** | Security Engineer (GRC), Open Source Pro, Data Ops (GRC), Executive | — |
| **Layer 4 – Sensitive Activities** | — | Software Developer, Infra/Platform, Data Ops | — |
| **Layer 5 – Evaluation** | — | Security Engineer, Infra/Platform, Open Source Pro (consumer) | Software Developer (subject of evaluation) |
| **Layer 6 – Enforcement** | — | Infra/Platform, Security Engineer | — |
| **Layer 7 – Audit & Monitoring** | — | Executive, Security/GRC, Data Ops (governance) | Open Source Pro |

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
