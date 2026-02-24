<!-- ---
title: Guidance Catalog Guide (Sachiko Persona)
description: Layer 1 guidance catalog tutorial through the lens of Sachiko the Solution Architect
tags: ["Inspector", "Layer 1", "Persona"]
--- -->

# Guidance Catalog Guide — As Sachiko (Layer 1 Persona)

This tutorial presents **Layer 1: Vectors and Guidance** through the lens of the best-fit persona for authoring or adopting guidance catalogs. Follow the same steps as in the [Guidance Catalog Guide](guidance-tutorial.md); here we frame them as **Sachiko the Solution Architect** would.

## Persona: Sachiko the Solution Architect

**Role:** Principal Software Architect at a cloud infrastructure company. She defines secure design patterns and blueprints, aligns with frameworks like SLSA and SAMM, and shares internal guidance so others can reuse it. She doesn’t write most of the code anymore but is the person teams go to when something needs to scale securely.

**Why she fits Layer 1:** Sachiko captures **secure-by-design guidelines** and maps them to external standards (e.g. OWASP, NIST) so the organization can reuse one coherent guidance catalog internally. She focuses on **optimal outcomes without foreknowledge of implementation details**—exactly what a Gemara guidance catalog is for.

> **Alternative personas for Layer 1:** For **regulator- or governing-body-aligned** guidance (e.g. data protection, regulated industry), use **Grear** (Data Governance Analyst in a government agency) or **Regulated Ragnar** (Open Source Pro in a regulated industry). See [Persona–Layer Alignment](../persona-layer-alignment.md#layer-1-vectors-and-guidance--persona-fit).

---

## As Sachiko: Your Goal

*As Sachiko, you’re capturing secure-by-design guidelines and mapping them to OWASP/NIST (or other standards) so teams can reuse this guidance internally. You’re producing a **guidance catalog**—families and guidelines with optional `guideline-mappings`—that downstream controls (Layer 2) and policy (Layer 3) can reference.*

---

## Walkthrough (with Sachiko in mind)

Use the full step-by-step flow in the [Guidance Catalog Guide](guidance-tutorial.md). While you do:

1. **Step 0 – Scope and catalog type**  
   Choose a scope that matches how you think about systems: e.g. secure development, supply chain, or secure ML. Pick **Best Practice** or **Framework** for org-specific reuse; use **Standard** or **Regulation** if you’re aligning to a formal or regulatory source.

2. **Step 1 – Metadata and mapping-references**  
   Add `mapping-references` to the external standards your org cares about (OWASP, NIST, SLSA, etc.). Sachiko’s catalogs are meant to be the single place teams look for “what good looks like,” so clear references matter.

3. **Step 2 – Families**  
   Group guidelines by theme (e.g. “Secure Dependencies and Supply Chain,” “Access Control”). These families are the same conceptual buckets you’d use in design reviews and architecture docs.

4. **Step 3 – Guidelines**  
   Write each guideline with an **objective** and optional **recommendations** and **applicability**. Use **`guideline-mappings`** to tie each guideline to specific entries in OWASP, NIST, or other references—so when Pang (Layer 2) or Guinevere (Layer 3) reference your catalog, the lineage to standards is explicit.

5. **Step 4 – Validate**  
   Validate against the Layer 1 schema as in the main guide.

---

## What’s Next

- Map your guidance to **Layer 2 controls** via control catalogs’ `guideline-mappings` (Pang’s world): [Threat Assessment Guide](../controls/threat-assessment.md) and [Control Catalog Guide](../controls/control-catalog-guide.md).
- Reference this catalog from **Layer 3 policy** (Guinevere’s world): [Organizational Policy Tutorial](../policy/organizational-policy.md) and the [Layer 3 persona tutorial](../policy/organizational-policy-tutorial-persona.md).

See the [Persona–Layer Alignment](../persona-layer-alignment.md) for how Sachiko, Grear, and Regulated Ragnar map to Layer 1.
