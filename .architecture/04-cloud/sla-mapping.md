---
title: "SLA & Data Contract Mapping"
project: "mohamad hafiz "
status: "draft"
created: "2026-07-26"
tags:
  - sla
  - data-contract
  - operations
---
# SLA & Data Contract Mapping — mohamad hafiz 

---

## Platform SLA Targets

| Metric | Target | Measurement Window | Alert Threshold |
|--------|--------|-------------------|----------------|
| Batch pipeline completion | 95% of pipelines complete within SLA | Monthly | 3 consecutive failures |
| Data freshness — Gold zone | Data no older than *[X hours]* by *[time]* | Daily | > *[X+1 hours]* |
| Query response time (Synapse) | p95 < *[10 s]* for standard analyst queries | Weekly | p95 > 15 s |
| API availability | 99.5% uptime | Monthly | 3 consecutive health probe failures |
| Ingestion failure rate | < 1% of pipeline runs fail | Monthly | > 3% in any 7-day window |

---

## Source Data Contracts

> A data contract is an agreement between the upstream source system team
> and the data platform on the quality and delivery guarantees of the data.

### Contract: DS-001 — *[Source Name]*

| Term | Agreement |
|------|-----------|
| Delivery schedule | Daily extract available by *[03:00 UTC]* |
| Format | *[Parquet / CSV]* with schema version *[X]* |
| Schema change notice | *[5 business days]* minimum notice before breaking changes |
| Acceptable null rate | < *[2%]* on required fields |
| Duplicate rate | < *[0.1%]* on primary key |
| Contact | *[Source team DL / Slack channel]* |
| Escalation | *[Manager name / runbook link]* |

---

### Contract: DS-002 — *[Source Name]*

> *Duplicate this block for each data source.*

---

## Operational Runbooks

| Scenario | Runbook |
|----------|---------|
| Batch pipeline SLA breach | *[Link to runbook]* |
| Schema drift detected | *[Link to runbook]* |
| Data quality gate failure | *[Link to runbook]* |
| Source system unavailable | *[Link to runbook]* |
| ADLS storage quota alert | *[Link to runbook]* |
