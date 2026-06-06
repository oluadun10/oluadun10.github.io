[idp-prior-auth.md](https://github.com/user-attachments/files/28671740/idp-prior-auth.md)
**Live Prototype:** [View Dashboard →](../prototypes/prior-auth-dashboard.html)
# Case Study: Intelligent Document Processing for Prior Authorization

**Company:** Ochsner Health  
**Role:** Senior Product Manager  
**Timeline:** Q3 2022 – Q2 2023  
**Type:** AI/ML Product · Clinical Operations · 0→1 Feature Expansion

---

## The Problem

Prior authorization is one of the most friction-heavy processes in healthcare. Clinical staff at Ochsner were manually reviewing hundreds of authorization requests daily — reading PDFs, extracting clinical criteria, cross-referencing payer rules, and entering data into disparate systems. The process was:

- **Slow:** Average processing time exceeded internal SLA targets by 40%
- **Error-prone:** Manual data entry introduced inconsistencies that delayed approvals
- **Expensive:** High-volume manual review consumed significant FTE hours
- **Clinically harmful:** Delays in prior auth directly delayed patient care

---

## My Role

I owned this product end-to-end — from initial discovery through deployment and post-launch iteration.

**Discovery & Scoping**
- Facilitated 12+ stakeholder workshops with clinical operations, revenue cycle, IT, and compliance teams
- Mapped the existing prior auth workflow in BPMN 2.0, identifying 8 distinct decision points suitable for automation
- Built the business case: projected ROI, FTE savings, and error reduction estimates for executive approval

**Requirements & Design**
- Wrote 40+ user stories with clinical-grade acceptance criteria, mapping to HEDIS quality goals
- Defined API integration requirements between the IDP engine, EHR (Epic), and payer systems
- Partnered with data scientists to define the training data strategy and model validation thresholds
- Established compliance guardrails with Legal and IT to meet HIPAA requirements throughout

**Delivery**
- Led cross-functional squad (engineering, data science, clinical SMEs, QA) through 6 two-week sprints
- Maintained 96% sprint completion rate across the engagement
- Ran UAT with clinical operations team, iterating on edge cases flagged during testing

**Post-Launch**
- Monitored adoption and accuracy metrics via Tableau dashboard built with SQL data pipeline
- Facilitated retrospectives and backlog grooming to prioritize enhancements in subsequent quarters

---

## Outcome

| Metric | Before | After | Change |
|---|---|---|---|
| Prior auth processing time | Baseline | -28% | ✅ |
| Annual cost savings | — | $3M+ | ✅ |
| Manual review cycle time | ~8 hrs | <2 hrs | ✅ |
| Clinical claim fulfillment rate | Baseline | Improved | ✅ |
| Sprint completion rate | — | 96% | ✅ |

---

## Key Artifacts

- [PRD: IDP Automation](../artifacts/prd-idp-automation.md)
- [BPMN: Prior Auth Workflow](../artifacts/bpmn-prior-auth.md) *(coming soon)*
- User Story set (sanitized) — available on request

---

## Lessons Learned

**Clinical SME access is the critical path.** The most expensive delays came from inability to get clinical sign-off on edge cases. In future sprints I restructured ceremonies to guarantee dedicated clinical review time.

**Model validation needs a PM voice.** Data scientists optimized for accuracy; clinical operations needed to optimize for explainability. Bridging that gap required me to be deeply involved in model review sessions — not just accepting the handoff.

**The real product isn't the AI.** The IDP engine was a component. The product was the workflow change, the adoption, the training, and the reporting that made people trust it enough to rely on it daily.
