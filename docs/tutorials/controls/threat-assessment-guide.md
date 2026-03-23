---
layout: page
title: Threat Assessment Guide
description: Step-by-step guide to performing Gemara-compatible threat assessments
---

## What This Is

This guide walks through a threat assessment using the [Gemara](https://gemara.openssf.org/) project.

**The basic idea:** Think of a project like a house. First, you identify what the house can do: its **capabilities** (e.g., "allow entry/exit", "store belongings"). Then, you identify **threats**, what could go wrong with those capabilities (e.g., "unauthorized entry through unlocked door", "theft of stored belongings").

In technical terms:
* **Capabilities** define what the technology can do. These form a primary component of the **attack surface** because every intended function represents a potential path for unintended use. In Gemara, capabilities you author for a scope live in a **Capability Catalog** (`#CapabilityCatalog` in [capabilitycatalog.cue](https://github.com/gemaraproj/gemara/blob/main/capabilitycatalog.cue)).
* **Threats** define specific ways those capabilities could be misused or exploited. They are authored in a **Threat Catalog** (`#ThreatCatalog` in [threatcatalog.cue](https://github.com/gemaraproj/gemara/blob/main/threatcatalog.cue)): `metadata`, optional top-level **`imports`** as a list of `#MultiEntryMapping` values (not keyed by `capabilities` / `threats`), **`groups`**, and **`threats`** where each threat lists related capabilities under `capabilities`.

This exercise helps you systematically identify what could go wrong so you can build appropriate defenses.

## Walkthrough

### Step 0: Define Scope

Select a component or technology to assess (service, API, infrastructure component, or technology stack).

**Leverage existing resources**: Gemara supports importing threats and capabilities from external catalogs so you don't have to start from scratch. The FINOS Common Cloud Controls (CCC) Core [catalog](https://github.com/finos/common-cloud-controls/releases/download/v2025.10/CCC.Core_v2025.10.yaml) defines well-vetted capabilities and threats that apply broadly across cloud services. These pre-built items can help accelerate your assessment.

We will explore how this is leveraged below as we dive into our container management tool example (i.e., SEC.SLAM.CM).

### Step 1: Setting Up Metadata

Declare your scope and mapping references. Key fields:

| Field                               | What It Is                                                   | Why                                                                                       |
|-------------------------------------|--------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| `title`                             | Display name for the threat catalog (top-level field)        | Human-readable label used in reports and tooling output                                   |
| `mapping-references` with `id: CCC` | Pointer to the CCC Core catalog release                      | Resolve imported CCC capability and threat IDs used in `imports` and in threat `capabilities` |
| `mapping-references` for local capabilities | Pointer to your **Capability Catalog** artifact (this tutorial: `SEC.SLAM.CM.CAP`) | Lets threats reference custom capability IDs (e.g. `SEC.SLAM.CM.CAP01`) |
| `imports` (optional)                | List of `#MultiEntryMapping` rows                            | Declare which CCC (or other) entries you reuse—capabilities and threats share the same list shape |

**Example (YAML):**

```yaml
title: Container Management Tool Security Threat Catalog
metadata:
  id: SEC.SLAM.CM
  type: ThreatCatalog
  gemara-version: "0.20.0"
  description: Threat catalog for container management tool security assessment
  version: 1.0.0
  author:
    id: example
    name: Example
    type: Human
  mapping-references:
    - id: CCC
      title: Common Cloud Controls Core
      version: v2025.10
      url: https://github.com/finos/common-cloud-controls/releases
      description: |
        Foundational repository of reusable security controls, capabilities,
        and threat models maintained by FINOS.
    - id: SEC.SLAM.CM.CAP
      title: Container Management Tool Security Capability Catalog
      version: "1.0.0"
      url: file://capability-catalog-example.yaml
      description: Local capabilities (CAP01, CAP02) for this assessment.
```

### Step 2: Identify Capabilities (Capability Catalog)

Capabilities are the core functions or features within the scope. Gemara stores them in a **Capability Catalog**, not inside the threat catalog file.

**Start with CCC.** A container management tool pulls images from registries—CCC Core models that as **CP29** (Active Ingestion). Tags resolve to images—**CP18** (Resource Versioning). You will reference those IDs from the **threat** catalog’s `imports` and from each threat’s `capabilities` mappings in later steps.

**Then define capabilities that are specific to your target** in a separate YAML artifact (`metadata.type: CapabilityCatalog`). Required fields:

| Field         | Required | Description                                                        |
|---------------|----------|--------------------------------------------------------------------|
| Capability ID | Yes      | Unique identifier (e.g. `SEC.SLAM.CM.CAP01`)                       |
| Title         | Yes      | A clear, concise name that describes the capability                |
| Description   | Yes      | A specific explanation of what this capability does                |
| `group`       | Yes      | Must match a `groups` entry in the same capability catalog           |

A complete, schema-valid example for this tutorial is [capability-catalog-example.yaml](capability-catalog-example.yaml).

**Example (YAML) — excerpt**

```yaml
title: Container Management Tool Security Capability Catalog
metadata:
  id: SEC.SLAM.CM.CAP
  type: CapabilityCatalog
  gemara-version: "0.20.0"
  # ... author, description, version ...
groups:
  - id: SEC.SLAM.CM.FAM01
    title: Image Retrieval and Resolution
    description: Capabilities for how the tool retrieves images and resolves references.
capabilities:
  - id: SEC.SLAM.CM.CAP01
    title: Image Retrieval by Tag
    description: |
      Ability to retrieve container images from registries using mutable tag names.
    group: SEC.SLAM.CM.FAM01
```

### Step 3: Identify Threats

Threats are specific ways capabilities can be misused, exploited, or cause problems. For each capability, identify potential threats.

**Check for imported threats first.** As with capabilities, review the CCC Core catalog for threats linked to the capabilities you imported.
If a threat fits your scope, import it. In this example, CCC Core defines **TH14** ("Older Resource Versions are Used") which is linked to **CP18**.
It applies because mutable image tags let the tool resolve to a stale or compromised version.

**Example (YAML)** — add CCC rows you import (capabilities and threats use the **same** top-level `imports` list in the threat catalog; each item is one `reference-id` with an `entries` array):

```yaml
imports:
  - reference-id: CCC
    entries:
      - reference-id: CCC.Core.CP29
        remarks: Active Ingestion
      - reference-id: CCC.Core.CP18
        remarks: Resource Versioning
      - reference-id: CCC.Core.TH14
        remarks: Older Resource Versions are Used
      - reference-id: CCC.Core.TH02
        remarks: Related CCC threat for transport / interception scenarios
      - reference-id: CCC.Core.CP01
        remarks: Encryption in Transit Enabled by Default
```

**Then, define specific threats** unique to your target. Required fields:

| Field             | Required | Description                                                                        |
|-------------------|----------|------------------------------------------------------------------------------------|
| Threat ID         | Yes      | Unique identifier following the pattern `ORG.PROJ.COMPONENT.THR##`                 |
| Title             | Yes      | A clear, concise name describing the threat                                        |
| Description       | Yes      | A specific explanation of what goes wrong and why it matters                       |
| `group`           | Yes      | Id of a threat `groups` entry in this catalog                                      |
| Capabilities      | Yes      | `capabilities` mappings to CCC and your capability catalog (`reference-id` must match `mapping-references`) |

**Example (YAML)**

Example: a custom threat (Container Image Tampering or Poisoning) linked to the capabilities it exploits — CCC CP29 (Active Ingestion), CP18 (Resource Versioning), and SEC.SLAM.CM CAP01 (Image Retrieval by Tag).

```yaml
threats:
  - id: SEC.SLAM.CM.THR01
    title: Container Image Tampering or Poisoning
    description: |
      Attackers may replace a legitimately published image tag with a malicious image
      by exploiting tag mutability in image registries, especially when the container
      management tool retrieves images by tag name rather than digest. This enables
      unauthorized access, data exfiltration, and system compromise.
    group: SEC.SLAM.CM.FAM01
    capabilities:
      - reference-id: CCC
        entries:
          - reference-id: CCC.Core.CP29
          - reference-id: CCC.Core.CP18
      - reference-id: SEC.SLAM.CM.CAP
        entries:
          - reference-id: SEC.SLAM.CM.CAP01
```

Use **`SEC.SLAM.CM.CAP`** (your capability catalog’s `metadata.id`) as the `reference-id` for local capabilities—not the threat catalog id.

### Step 4: Validate

A full, schema-valid **threat** catalog for this walkthrough is [threat-catalog-example.yaml](threat-catalog-example.yaml). It includes `imports`, `groups`, and all five example threats with correct `capabilities` references to CCC and `SEC.SLAM.CM.CAP`.

Validate the **capability** catalog and **threat** catalog separately:

**Validation commands:**

```bash
go install cuelang.org/go/cmd/cue@latest
cue vet -c -d '#CapabilityCatalog' github.com/gemaraproj/gemara@latest your-capability-catalog-example.yaml
cue vet -c -d '#ThreatCatalog' github.com/gemaraproj/gemara@latest your-threats-example.yaml
```

From a **clone of this repository** (run from the repo root; replace the placeholders with your file paths):

```bash
cue vet -c -d '#CapabilityCatalog' . your-capability-catalog-example.yaml
cue vet -c -d '#ThreatCatalog' . your-threats-example.yaml
```

Reference copies of these tutorials live at [capability-catalog-example.yaml](capability-catalog-example.yaml) and [threat-catalog-example.yaml](threat-catalog-example.yaml).

## What's Next

Create a Gemara Control Catalog that maps security controls to the identified threats using the [Control Catalog Guide](control-catalog-guide). See the [Layer 2 schema documentation](https://gemara.openssf.org/schema/layer-2.html), [threatcatalog.cue](https://github.com/gemaraproj/gemara/blob/main/threatcatalog.cue), and [capabilitycatalog.cue](https://github.com/gemaraproj/gemara/blob/main/capabilitycatalog.cue) for the full specification.
