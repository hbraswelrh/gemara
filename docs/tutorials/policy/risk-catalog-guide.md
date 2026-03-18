---
layout: page
title: Risk Catalog Guide
---

## What This Is

This guide walks through creating a **Risk Catalog** using the [Gemara](https://gemara.openssf.org/) project. The document conforms to the **RiskCatalog** schema in `layer-3.cue`.

A risk catalog is a structured collection of documented risks that may affect an organization, system, or service. It provides a centralized reference for risks that can be mapped to threats and referenced by policies when documenting how those risks are mitigated or accepted.

Terms to know:
* **Groups**: Groupings of risks with a defined [risk appetite](https://gemara.openssf.org/glossary/risk-appetite/) and optional maximum severity. Each risk belongs to one group (schema field `groups` / `group`).
* **Risks**: Individual risk entries with id, title, description, severity, and optional links to Layer 2 threats and RACI owner.
* **Appetite**: The acceptable level of risk for a group (`Minimal`, `Low`, `Moderate`, `High`) per `#RiskAppetite` in `layer-3.cue`.
* **Severity**: Impact level for a risk or group cap (`Low`, `Medium`, `High`, `Critical`).
* **Mapping-references**: External threat catalogs (or other artifacts) that risks reference via `threats`. Each `reference-id` used in `threats` must appear in `metadata.mapping-references`.

This exercise produces a risk catalog that you can reference from a [Policy](policy-guide) when documenting mitigated and accepted risks.

The example risks in this guide are drawn from the same scope as the [Threat Assessment Guide](../controls/threat-assessment-guide): the container management tool (SEC.SLAM.CM) and the CCC Core catalog. They correspond to the imported threat **TH14** (Older Resource Versions are Used) and the custom threat **SEC.SLAM.CM.THR01** (Container Image Tampering or Poisoning) defined there.

## Walkthrough

### Step 0: Metadata and mapping-references

Set `title` and `metadata` (see [metadata.cue](https://github.com/gemaraproj/gemara/blob/main/metadata.cue) for the standard metadata fields). Use `type: RiskCatalog`. Include `mapping-references` for every external threat catalog (or other document) you reference in risks under `threats` (by `reference-id`). Key fields (see [layer-3.cue](https://github.com/gemaraproj/gemara/blob/main/layer-3.cue) and [metadata.cue](https://github.com/gemaraproj/gemara/blob/main/metadata.cue):

| Field                         | What It Is                                                                 | Why                                                                 |
|-------------------------------|----------------------------------------------------------------------------|---------------------------------------------------------------------|
| `title`                       | Display name for the catalog (top-level)                                  | Human-readable label in reports and tooling                         |
| `metadata.id`                 | Unique identifier for this catalog                                       | Used when policies or other documents reference this catalog       |
| `metadata.description`       | High-level summary of the catalog's purpose and scope                     | Required by schema; clarifies intent                               |
| `metadata.author`             | Actor (id, name, type) primarily responsible for this catalog            | Required by schema; identifies the author                           |
| `metadata.mapping-references` | Pointers to external threat catalogs (or artifacts) referenced in risks   | Required for `threats`; each `reference-id` used there must match here |

> **Note:** Include a `mapping-references` entry for every external catalog you reference in any risk's `threats` (by `reference-id`).

**Example (YAML):**

```yaml
title: "Organization Risk Catalog for Cloud and Container Workloads"
metadata:
  id: "org-risk-catalog-001"
  type: RiskCatalog
  gemara-version: "0.20.0"
  description: "Risks relevant to cloud and container management; threats linked to CCC Core and SEC.SLAM.CM threat catalog."
  version: "1.0.0"
  author:
    id: security-team
    name: "Security Team"
    type: Human
  mapping-references:
    - id: "CCC.Core"
      title: "FINOS Common Cloud Controls Core"
      version: "2025.10"
      description: "CCC Core threat and capability catalog."
    - id: "SEC.SLAM.CM"
      title: "Container Management Tool Security Threat Catalog"
      version: "1.0.0"
      description: "Threat catalog from the Threat Assessment Guide (SEC.SLAM.CM)."
```


### Step 1: Groups

Define at least one **group** when the catalog has risks. Groups classify risks and set appetite boundaries. Each entry is a `#RiskCategory` (extends `#Group`) with:

| Field            | Required | What It Is                                                                 |
|------------------|----------|----------------------------------------------------------------------------|
| `id`             | Yes      | Unique identifier; risks reference this id in their `group` field          |
| `title`          | Yes      | Short name for the group                                                   |
| `description`    | Yes      | Explains what kinds of risks belong in this group                          |
| `appetite`       | Yes      | Acceptable level of risk: `Minimal`, `Low`, `Moderate`, or `High`          |
| `max-severity`   | No       | Highest allowed severity in this group: `Low`, `Medium`, `High`, `Critical` |

**Example (YAML):**

```yaml
groups:
  - id: "infrastructure"
    title: "Infrastructure and Operations"
    description: "Risks related to cloud infrastructure, container platforms, and operational security."
    appetite: "Low"
    max-severity: "High"
  - id: "data"
    title: "Data and Privacy"
    description: "Risks related to data exposure, residency, and compliance."
    appetite: "Minimal"
    max-severity: "Critical"
```


### Step 2: Risks

Define **risks** with required fields and optional links to Layer 2 threats and RACI owner. Each risk must reference a group id defined in Step 1.

| Field           | Required | What It Is                                                                 |
|-----------------|----------|----------------------------------------------------------------------------|
| `id`            | Yes      | Unique identifier for this risk; used when policies reference it          |
| `title`         | Yes      | Clear, concise name for the risk                                          |
| `description`   | Yes      | Explains the risk scenario and context                                    |
| `group`         | Yes      | Id of a group in this catalog (from Step 1)                               |
| `severity`      | Yes      | Impact level: `Low`, `Medium`, `High`, or `Critical`                       |
| `owner`         | No       | RACI roles (responsible, accountable, consulted, informed) for this risk   |
| `impact`        | No       | Business or operational impact description                                 |
| `threats`       | No       | Links to Layer 2 threat entries via reference-id and entries (see below)   |

For `threats`, use the same structure as in control and threat catalogs: a list of multi-entry mappings. Each mapping has a `reference-id` (must match `metadata.mapping-references`) and `entries` (list of mapping entries). Each entry has `reference-id` identifying the threat (e.g. threat catalog id or threat id) and optional `remarks`.

**Example (YAML):**

```yaml
risks:
  - id: "R01"
    title: "Older or Compromised Container Images in Use"
    description: "Mutable image tags or lack of verification can lead to pulling stale or compromised images, increasing supply chain and runtime risk."
    group: "infrastructure"
    severity: "High"
    impact: "Supply chain compromise, unauthorized code execution, or data exfiltration."
    owner:
      responsible:
        - name: "Platform Engineering"
          affiliation: "Engineering"
      accountable:
        - name: "CISO"
          affiliation: "Security"
    threats:
      - reference-id: "CCC.Core"
        entries:
          - reference-id: "CCC.Core.TH14"
            remarks: "Older Resource Versions are Used"
  - id: "R02"
    title: "Container Image Tampering or Poisoning"
    description: "Images may be tampered with in transit or at rest, or built from poisoned dependencies or build pipelines."
    group: "infrastructure"
    severity: "Critical"
    threats:
      - reference-id: "CCC.Core"
        entries:
          - reference-id: "CCC.Core.TH14"
            remarks: "Older Resource Versions are Used"
      - reference-id: "SEC.SLAM.CM"
        entries:
          - reference-id: "SEC.SLAM.CM.THR01"
            remarks: "Container Image Tampering or Poisoning"
```


### Step 3: Validation

The catalog must conform to the RiskCatalog definition in the CUE module. Validate with CUE:

**Validation commands:**

Using the **published** module:

```bash
go install cuelang.org/go/cmd/cue@latest
cue vet -c -d '#RiskCatalog' github.com/gemaraproj/gemara@latest your-risk-catalog.yaml
```

Fix any errors (e.g. missing required fields, invalid `group` reference, severity or appetite not in allowed enums, or `threats` reference-id not in mapping-references) so the catalog is schema-valid.


## Minimal Full Example

A complete, schema-valid example is in [risk-catalog-example.yaml](risk-catalog-example.yaml) in this directory. The following matches that file (omit optional sections such as `owner`, `impact`, or `threats` in your own catalogs if you do not need them).

```yaml
title: "Organization Risk Catalog for Cloud and Container Workloads"
metadata:
  id: "org-risk-catalog-001"
  type: RiskCatalog
  gemara-version: "0.20.0"
  description: "Risks relevant to cloud and container management; threats linked to CCC Core and SEC.SLAM.CM threat catalog."
  version: "1.0.0"
  author:
    id: security-team
    name: "Security Team"
    type: Human
  mapping-references:
    - id: "CCC.Core"
      title: "FINOS Common Cloud Controls Core"
      version: "2025.10"
      description: "CCC Core threat and capability catalog."
    - id: "SEC.SLAM.CM"
      title: "Container Management Tool Security Threat Catalog"
      version: "1.0.0"
      description: "Threat catalog from the Threat Assessment Guide (SEC.SLAM.CM)."

groups:
  - id: "infrastructure"
    title: "Infrastructure and Operations"
    description: "Risks related to cloud infrastructure, container platforms, and operational security."
    appetite: "Low"
    max-severity: "High"
  - id: "data"
    title: "Data and Privacy"
    description: "Risks related to data exposure, residency, and compliance."
    appetite: "Minimal"
    max-severity: "Critical"

risks:
  - id: "R01"
    title: "Older or Compromised Container Images in Use"
    description: "Mutable image tags or lack of verification can lead to pulling stale or compromised images, increasing supply chain and runtime risk."
    group: "infrastructure"
    severity: "High"
    impact: "Supply chain compromise, unauthorized code execution, or data exfiltration."
    owner:
      responsible:
        - name: "Platform Engineering"
          affiliation: "Engineering"
      accountable:
        - name: "CISO"
          affiliation: "Security"
    threats:
      - reference-id: "CCC.Core"
        entries:
          - reference-id: "CCC.Core.TH14"
            remarks: "Older Resource Versions are Used"
  - id: "R02"
    title: "Container Image Tampering or Poisoning"
    description: "Images may be tampered with in transit or at rest, or built from poisoned dependencies or build pipelines."
    group: "infrastructure"
    severity: "Critical"
    threats:
      - reference-id: "CCC.Core"
        entries:
          - reference-id: "CCC.Core.TH14"
            remarks: "Older Resource Versions are Used"
      - reference-id: "SEC.SLAM.CM"
        entries:
          - reference-id: "SEC.SLAM.CM.THR01"
            remarks: "Container Image Tampering or Poisoning"
```


## What's Next

- Reference this risk catalog from a **Policy** document: in the policy's `risks` section, use `mitigated` and `accepted` entries that reference risk ids (and, for accepted risks, justification and optional scope). See the [Policy Guide](policy-guide) and [layer-3.cue](https://github.com/gemaraproj/gemara/blob/main/layer-3.cue) (`#Risks`, `#MitigatedRisk`, `#AcceptedRisk`).
- Use **Layer 5** evaluations to assess whether controls and implementations address the threats linked to these risks.
- Use **Layer 7** audit and continuous monitoring to review risk posture and policy effectiveness.

See the [Layer 3 schema documentation](https://gemara.openssf.org/schema/layer-3.html) and [layer-3.cue](https://github.com/gemaraproj/gemara/blob/main/layer-3.cue) for the full specification.
