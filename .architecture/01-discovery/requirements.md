---
title: "Requirements — Data & Analytics Platform"
project: "mohamad hafiz "
status: "draft"
created: "2026-07-26"
author: "enter"
tags:
  - requirements
  - data
  - analytics
---
# Requirements — mohamad hafiz 

> **Domain:** Data & Analytics Platform
> Focus: data ingestion SLAs, schema governance, data quality, access control, and lineage.

---

## Functional Requirements

| ID | Requirement | Priority | Source |
|----|-------------|----------|--------|
| FR-001 | *[The platform shall ingest data from source X at a frequency of Y]* | Must | |
| FR-002 | *[The platform shall enforce schema validation on all ingested datasets]* | Must | |
| FR-003 | *[The platform shall provide a queryable data catalogue with column-level lineage]* | Should | |
| FR-004 | *[The platform shall support self-service SQL access for approved analysts]* | Should | |
| FR-005 | *[The platform shall mask PII fields before exposing to non-privileged roles]* | Must | |
| FR-006 | | | |

---

## Data Quality Requirements

| Rule | Dimension | Threshold | Action on Breach |
|------|-----------|-----------|-----------------|
| Null rate | Completeness | *[e.g., < 2% nulls in key columns]* | *[Alert + quarantine]* |
| Duplicate rate | Uniqueness | *[e.g., < 0.1% duplicate records]* | *[Dedup + alert]* |
| Schema drift | Consistency | *[Zero unexpected column changes]* | *[Fail pipeline + alert]* |
| Freshness | Timeliness | *[e.g., Data no older than X hours]* | *[Alert SRE]* |
| Referential integrity | Validity | *[e.g., All FK references resolve]* | *[Quarantine bad records]* |

---

## Non-Functional Requirements

| Category | Requirement | Target |
|----------|-------------|--------|
| Ingestion SLA | End-to-end latency (batch) | *[e.g., < 2 h from source to query-ready]* |
| Ingestion SLA | End-to-end latency (stream) | *[e.g., < 60 s from event to query-ready]* |
| Query performance | Analyst query p99 | *[e.g., < 10 s for ad-hoc queries on 1 TB]* |
| Storage scale | Data volume Year 1 | *[e.g., 10 TB raw / 3 TB curated]* |
| Retention | Raw data | *[e.g., 7 years — regulatory]* |
| Retention | Curated / served | *[e.g., 3 years rolling]* |
| Availability | Platform uptime | *[e.g., 99.5% — batch-tolerant]* |
| Compliance | Data classification | *[PII, Confidential, Public — GDPR/CCPA]* |

---

## Access Control Requirements

| Role | Access Level | Datasets |
|------|-------------|---------|
| Data Engineer | Full write | All raw and curated zones |
| Data Analyst | Read-only | Curated and served zones (PII masked) |
| Data Scientist | Read-only + compute | Curated zone + ML sandbox |
| External Partner | Read-only | *[Specific datasets only via API]* |
| Auditor | Read-only metadata | Lineage and access logs |
