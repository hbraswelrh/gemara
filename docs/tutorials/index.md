---

## layout: page
title: Tutorials

## Start here

**Gemara Layers — Knowledge, Inputs & Outputs** — Start here if you're new to the model.

---

## Find Your Tutorial

Pick your goal — each path leads to the right guide.

### Performing a threat assessment

For a system or component → [Threat Assessment Guide](controls/threat-assessment-guide) — capability catalog + threat catalog, CCC imports, and threat `capabilities` mappings (Layer 2). Validated examples: [capability-catalog-example.yaml](controls/capability-catalog-example.yaml), [threat-catalog-example.yaml](controls/threat-catalog-example.yaml).

### Defining security controls

That mitigate those threats → [Control Catalog Guide](controls/control-catalog-guide) — control catalog with assessment requirements and `threats` mappings; top-level `imports` uses `#MultiEntryMapping` (Layer 2).

### Understanding what threats and controls exist

Before writing policy → [Threat Assessment Guide](controls/threat-assessment-guide)

→ **COMING SOON:** Review or author threat-informed controls that your policy will reference (Layer 2).

### Reviewing the controls to reference in a policy

→ **COMING SOON:** Understand the control catalog structure and assessment requirements (Layer 2).

### Understanding the security posture of consumed software

→ [Threat Assessment Guide](controls/threat-assessment-guide) — review threat catalogs for your dependencies (Layer 2).

→ **COMING SOON:** Use control catalogs (e.g. OSPS, CCC) as hardening guides (Layer 2).

### Creating a guidance catalog from best practices

From a spreadsheet or checklist — create a guidance catalog (guidelines, groups, `mapping-references`) that controls can reference; align to other frameworks with a [Mapping Document](https://gemara.openssf.org/schema/mapping.html) (example YAML: [mapping-document.yaml](guidance/mapping-document.yaml)). → [Guidance Catalog Guide](guidance/guidance-guide).

### Creating organizational policy

Create a policy document that translates risk appetite into mandatory rules — [Policy Guide](policy/policy-guide) — scope, imports, adherence, and risks (Layer 3).

---

## What You'll Build


| Layer / cross-cutting  | Artifact                               | Guide or example                                                                                                                                                                                               |
| ---------------------- | -------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Layer 1** — Guidance | Guidance Catalog                       | [Guidance Catalog Guide](guidance/guidance-guide)                                                                                                                                                              |
| **Layer 1** — Guidance | Principle Catalog (principles, groups) | [Principle Catalog schema](https://gemara.openssf.org/schema/principlecatalog.html)                                                                                                                            |
| **Layer 1** — Guidance | Vector Catalog                         | [Vector Catalog schema](https://gemara.openssf.org/schema/vectorcatalog.html)                                                                                                                                  |
| **Layer 2** — Controls | Capability Catalog                     | [Threat Assessment Guide](controls/threat-assessment-guide) + [capability-catalog-example.yaml](controls/capability-catalog-example.yaml)                                                                      |
| **Layer 2** — Controls | Threat Catalog                         | [Threat Assessment Guide](controls/threat-assessment-guide) + [threat-catalog-example.yaml](controls/threat-catalog-example.yaml)                                                                              |
| **Layer 2** — Controls | Control Catalog                        | [Control Catalog Guide](controls/control-catalog-guide)                                                                                                                                                        |
| **Layer 3** — Policy   | Policy Document                        | [Policy Guide](policy/policy-guide)                                                                                                                                                                            |
| **Layer 3** — Risks    | Risk Catalog                           | [Risk Catalog schema](https://gemara.openssf.org/schema/riskcatalog.html)                                                                                                                                      |
| **Cross-artifact**     | Mapping Document                       | [Mapping schema](https://gemara.openssf.org/schema/mapping.html) · [mapping-document.yaml](guidance/mapping-document.yaml)                                                                                     |
| **Layers 5–7**         | Evaluation / Enforcement / Audit logs  | [Evaluation Log](https://gemara.openssf.org/schema/evaluationlog.html), [Enforcement Log](https://gemara.openssf.org/schema/enforcementlog.html), [Audit Log](https://gemara.openssf.org/schema/auditlog.html) |


Schemas are defined in the repo’s CUE package (e.g. `guidancecatalog.cue`, `threatcatalog.cue`, `controlcatalog.cue`, `policy.cue`, `riskcatalog.cue`, `mappingdocument.cue`, `principlecatalog.cue`, `auditlog.cue`)—not the retired monolithic `layer-*.cue` files.

## What You'll Need`go` installed

- `cue` installed for validation (e.g. `cue vet -c -d '#ThreatCatalog' . your-threats-example.yaml` from a clone, or the same placeholder filename with `github.com/gemaraproj/gemara@latest` for the published module)

## Have Ideas?

- Reach out via Slack in `#gemara`
- Discuss in one of our bi-weekly meetings on the [OpenSSF calendar](https://calendar.google.com/calendar/u/0?cid=czYzdm9lZmhwNWk5cGZsdGI1cTY3bmdwZXNAZ3JvdXAuY2FsZW5kYXIuZ29vZ2xlLmNvbQ)
- Open a [GitHub Issue](https://github.com/gemaraproj/gemara/issues)

