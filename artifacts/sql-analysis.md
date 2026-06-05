[sql-analysis.md](https://github.com/user-attachments/files/28626676/sql-analysis.md)
# SQL: Clinical Data Analysis & Reporting Queries

**Author:** Olu Adun, Product Manager  
**Context:** Sample queries used for utilization management reporting, KPI tracking, and data validation at Ochsner Health and State Farm.  
> All queries use anonymized/synthetic schemas. No PHI included.

---

## 1. Prior Authorization Processing Time Analysis

Tracks average processing time per authorization type, identifies outliers, and surfaces SLA breaches.

```sql
-- Average processing time by auth type and month
SELECT
    auth_type,
    DATE_TRUNC('month', submitted_at) AS month,
    COUNT(*) AS total_auths,
    AVG(DATEDIFF('minute', submitted_at, decision_at)) AS avg_processing_minutes,
    PERCENTILE_CONT(0.95) WITHIN GROUP (ORDER BY DATEDIFF('minute', submitted_at, decision_at)) AS p95_minutes,
    SUM(CASE WHEN DATEDIFF('hour', submitted_at, decision_at) > 72 THEN 1 ELSE 0 END) AS sla_breaches,
    ROUND(100.0 * SUM(CASE WHEN DATEDIFF('hour', submitted_at, decision_at) > 72 THEN 1 ELSE 0 END) / COUNT(*), 2) AS breach_rate_pct
FROM prior_auth_requests
WHERE submitted_at >= DATEADD('month', -12, CURRENT_DATE)
    AND status IN ('approved', 'denied', 'withdrawn')
GROUP BY 1, 2
ORDER BY 2 DESC, 1;
```

**Business use:** Tracked weekly to monitor IDP automation impact on processing time. 28% reduction validated using this query comparing pre- and post-automation cohorts.

---

## 2. Product Adoption Funnel Analysis

Measures user activation, engagement, and retention for a digital health product.

```sql
-- Weekly activation and retention funnel
WITH user_events AS (
    SELECT
        user_id,
        MIN(CASE WHEN event_type = 'account_created' THEN event_date END) AS signup_date,
        MIN(CASE WHEN event_type = 'first_feature_use' THEN event_date END) AS activation_date,
        MAX(CASE WHEN event_type = 'feature_use' 
            AND event_date >= DATEADD('day', -7, CURRENT_DATE) THEN event_date END) AS last_7d_activity,
        MAX(CASE WHEN event_type = 'feature_use'
            AND event_date >= DATEADD('day', -30, CURRENT_DATE) THEN event_date END) AS last_30d_activity
    FROM user_event_log
    GROUP BY user_id
),
cohorts AS (
    SELECT
        DATE_TRUNC('week', signup_date) AS signup_week,
        COUNT(DISTINCT user_id) AS signups,
        COUNT(DISTINCT CASE WHEN activation_date IS NOT NULL THEN user_id END) AS activated,
        COUNT(DISTINCT CASE WHEN last_7d_activity IS NOT NULL THEN user_id END) AS wau,
        COUNT(DISTINCT CASE WHEN last_30d_activity IS NOT NULL THEN user_id END) AS mau
    FROM user_events
    GROUP BY 1
)
SELECT
    signup_week,
    signups,
    activated,
    ROUND(100.0 * activated / NULLIF(signups, 0), 1) AS activation_rate_pct,
    wau,
    mau,
    ROUND(100.0 * wau / NULLIF(activated, 0), 1) AS wau_retention_pct
FROM cohorts
ORDER BY signup_week DESC;
```

**Business use:** Used to identify the 17–22% adoption lift achieved after workflow optimizations. Cohort comparison showed which provider segments activated fastest.

---

## 3. KPI Dashboard — Utilization Management

Executive-facing summary of utilization management metrics for operational review.

```sql
-- Monthly UM scorecard
SELECT
    DATE_TRUNC('month', review_date) AS month,
    
    -- Volume
    COUNT(*) AS total_reviews,
    SUM(CASE WHEN review_type = 'prior_auth' THEN 1 ELSE 0 END) AS prior_auth_reviews,
    SUM(CASE WHEN review_type = 'concurrent' THEN 1 ELSE 0 END) AS concurrent_reviews,
    
    -- Outcomes
    ROUND(100.0 * SUM(CASE WHEN decision = 'approved' THEN 1 ELSE 0 END) / COUNT(*), 1) AS approval_rate_pct,
    ROUND(100.0 * SUM(CASE WHEN decision = 'denied' THEN 1 ELSE 0 END) / COUNT(*), 1) AS denial_rate_pct,
    ROUND(100.0 * SUM(CASE WHEN decision = 'peer_to_peer' THEN 1 ELSE 0 END) / COUNT(*), 1) AS p2p_rate_pct,
    
    -- Timeliness
    ROUND(AVG(DATEDIFF('hour', submitted_at, decision_at)), 1) AS avg_turnaround_hours,
    SUM(CASE WHEN DATEDIFF('hour', submitted_at, decision_at) > 72 THEN 1 ELSE 0 END) AS late_decisions,
    
    -- Quality
    SUM(CASE WHEN appeal_filed = TRUE THEN 1 ELSE 0 END) AS appeals_filed,
    SUM(CASE WHEN appeal_filed = TRUE AND appeal_outcome = 'overturned' THEN 1 ELSE 0 END) AS overturned

FROM utilization_reviews
WHERE review_date >= DATEADD('month', -13, CURRENT_DATE)
GROUP BY 1
ORDER BY 1 DESC;
```

---

## 4. Cost Savings Identification — Insurance (State Farm)

Identified $1M+ in cost-saving opportunities by analyzing claims processing inefficiencies.

```sql
-- Identify high-cost rework patterns in claims processing
WITH rework_claims AS (
    SELECT
        claim_id,
        COUNT(*) AS touch_count,
        SUM(handling_cost) AS total_handling_cost,
        MIN(received_date) AS first_touch,
        MAX(closed_date) AS final_close,
        DATEDIFF('day', MIN(received_date), MAX(closed_date)) AS cycle_days,
        LISTAGG(DISTINCT rework_reason, ' | ') AS rework_reasons
    FROM claims_activity_log
    WHERE activity_type IN ('rework', 'reassign', 'escalate', 'reopen')
    GROUP BY claim_id
    HAVING COUNT(*) > 1
),
cost_summary AS (
    SELECT
        rework_reasons,
        COUNT(*) AS claim_count,
        SUM(total_handling_cost) AS total_cost,
        AVG(total_handling_cost) AS avg_cost_per_claim,
        AVG(cycle_days) AS avg_cycle_days
    FROM rework_claims
    GROUP BY rework_reasons
)
SELECT
    rework_reasons,
    claim_count,
    ROUND(total_cost, 0) AS total_cost_usd,
    ROUND(avg_cost_per_claim, 0) AS avg_cost_per_claim_usd,
    ROUND(avg_cycle_days, 1) AS avg_cycle_days,
    -- Annualized savings if rework eliminated
    ROUND(total_cost * 12 / 
        DATEDIFF('month', 
            (SELECT MIN(received_date) FROM claims_activity_log), 
            CURRENT_DATE), 0) AS annualized_savings_if_fixed
FROM cost_summary
ORDER BY total_cost DESC
LIMIT 20;
```

**Business use:** This analysis surfaced the top 5 rework patterns responsible for 80% of excess handling cost — directly informing the process improvements that delivered $1M+ in identified savings.

---

## Notes on My SQL Practice

- I write queries to answer business questions, not to show off syntax
- Every query I run in production has a documented business question, expected output, and stakeholder owner
- I validate query results against manual spot-checks before using them in executive reporting
- I build reusable views and CTEs rather than duplicating logic across reports
