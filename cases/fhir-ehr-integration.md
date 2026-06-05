[fhir-ehr-integration.md](https://github.com/user-attachments/files/28626897/fhir-ehr-integration.md)
# Case Study: FHIR-Compliant EHR Integration & Integrated Care MVP

**Company:** Ochsner Health  
**Role:** Product Manager  
**Timeline:** Q3 2022 – Q2 2023  
**Type:** Platform · Interoperability · API Strategy · Health IT

---

## The Problem

Ochsner operated across multiple regional care sites, each running different EHR systems that couldn't talk to each other. Patient data lived in silos. A patient seen at one facility was effectively invisible to a provider at another. Clinical staff were manually calling between facilities, faxing records, and re-entering data — creating delays, errors, and care gaps that directly harmed patients.

The root cause was a fragmented data architecture with no interoperability layer. Without a shared data model, there was no way to build coordinated care workflows, population health tooling, or any meaningful AI application on top of patient data.

The mandate: define and deliver a FHIR-compliant API integration strategy that connected disparate EHR systems and enabled an Integrated Care MVP to close regional care gaps.

---

## My Role

**Technical Discovery & Architecture Input**
- Facilitated technical discovery workshops with clinical informatics, IT architecture, and EHR vendors to map current data flows, integration points, and gaps
- Documented the as-is state in BPMN 2.0 — 14 distinct data handoff points identified, 6 flagged as high-risk for patient safety
- Evaluated FHIR R4 compliance readiness across three EHR platforms in use across the health system
- Defined the target state architecture in partnership with IT: a FHIR-compliant API layer serving as the canonical data exchange standard across all facilities

**API Requirements & Vendor Alignment**
- Wrote detailed API product requirements specifying FHIR R4 resource types, authentication standards (SMART on FHIR), data validation rules, and error handling protocols
- Led vendor negotiation sessions to align EHR vendors on integration timelines, API contract standards, and testing requirements
- Defined SLAs for API availability, latency, and data freshness — establishing the reliability baseline needed before clinical workflows could depend on the integration

**Integrated Care MVP**
- Scoped the MVP to the highest-impact use case: giving primary care providers real-time visibility into specialist visits and hospital admissions for their attributed patient panel
- Wrote 35+ user stories covering patient data retrieval, care gap identification, provider notifications, and cross-facility referral workflows
- Partnered with compliance and Legal to ensure all data exchange met HIPAA requirements and the 2026 HIPAA-2 privacy mandate
- Ran UAT with a pilot group of primary care providers — 3 rounds of testing, 47 edge cases resolved before launch

**HIPAA-2 Compliance**
- Led the product compliance workstream during the 2026 HIPAA-2 transition — auditing all patient-facing data flows, implementing technical data controls, and ensuring 100% compliance across applications before the mandate deadline

---

## Outcome

| Metric | Result |
|---|---|
| Regional care gaps | Measurably closed post-MVP launch |
| HIPAA-2 compliance | 100% across all patient-facing applications |
| EHR systems integrated | 3 disparate platforms connected via FHIR API layer |
| High-risk data handoff points resolved | 6 of 6 identified risk points addressed |
| API availability SLA | Met consistently post-launch |

---

## Key Artifacts

- BPMN: Data flow mapping (sanitized) — coming soon
- API requirements template — available on request
- FHIR resource type specification — available on request

---

## Lessons Learned

**Interoperability is a political problem as much as a technical one.** EHR vendors have commercial incentives to keep data locked in their systems. Getting three vendors to agree on a shared API standard required executive sponsorship, careful framing around patient safety, and persistent relationship management. The technical work was the easier half.

**FHIR is a standard, not a guarantee.** "FHIR-compliant" means different things to different vendors. We learned early to specify not just the standard but the exact resource types, profiles, and extensions required — and to test against real data, not vendor demos.

**Scope the MVP to one clinical workflow, not the entire vision.** The temptation was to build a unified patient record viewer. We resisted it. The MVP gave PCPs one thing: visibility into what happened to their patients at other facilities. That one thing was enough to demonstrate value, build trust, and earn the budget for the next phase.
