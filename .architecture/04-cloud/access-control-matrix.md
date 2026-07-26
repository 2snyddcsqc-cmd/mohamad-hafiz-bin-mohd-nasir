---
title: "Access Control Matrix"
project: "mohamad hafiz "
status: "draft"
created: "2026-07-26"
tags:
  - security
  - access-control
  - rbac
---
# Access Control Matrix — mohamad hafiz 

---

## Role Definitions

| Role | Description | Typical Users |
|------|-------------|--------------|
| Platform Admin | Full control of infrastructure and pipelines | Data Platform team |
| Data Engineer | Read/write access to all lake zones + pipelines | Engineering team |
| Data Scientist | Read-only lake access + write to ML sandbox | ML / DS team |
| Data Analyst | Read-only curated (Silver/Gold) — PII masked | Analytics / BI team |
| BI Developer | Read-only Gold + Synapse + Power BI | BI team |
| External Consumer | Read-only via Data API — specific datasets only | Partner / external app |
| Auditor | Read-only to audit logs and metadata | Compliance team |

---

## ADLS Zone Access Matrix

| Role | Bronze | Silver | Gold | ML Sandbox | Audit Logs |
|------|--------|--------|------|-----------|-----------|
| Platform Admin | ✅ R/W | ✅ R/W | ✅ R/W | ✅ R/W | ✅ R |
| Data Engineer | ✅ R/W | ✅ R/W | ✅ R/W | ✅ R/W | ❌ |
| Data Scientist | ❌ | ✅ R (masked) | ✅ R (masked) | ✅ R/W | ❌ |
| Data Analyst | ❌ | ✅ R (masked) | ✅ R (masked) | ❌ | ❌ |
| BI Developer | ❌ | ❌ | ✅ R | ❌ | ❌ |
| External Consumer | ❌ | ❌ | ❌ (API only) | ❌ | ❌ |
| Auditor | ❌ | ❌ | ❌ | ❌ | ✅ R |

> **R** = Read, **R/W** = Read/Write, **masked** = PII fields replaced with tokens.

---

## PII Handling Rules

| PII Field Type | Handling in Silver | Handling in Gold | Handling via API |
|---------------|-------------------|-----------------|-----------------|
| Name | Tokenised | Tokenised | Masked (**** or token) |
| Email | SHA-256 hash | SHA-256 hash | Excluded |
| Phone | Last 4 digits only | Excluded | Excluded |
| Address | Generalised to region | Generalised | Excluded |
| IP address | Anonymised | Excluded | Excluded |

---

## Service Identity Matrix (Managed Identities)

| Service | ADLS Permissions | Key Vault | Synapse | Notes |
|---------|-----------------|-----------|---------|-------|
| ADF Managed Identity | Storage Blob Data Contributor (Bronze) | Secret reader | N/A | Scoped to Bronze container only |
| Databricks Managed Identity | Storage Blob Data Contributor (Silver/Gold) | Secret reader | N/A | |
| Synapse Managed Identity | Storage Blob Data Reader (Gold) | N/A | N/A | Read-only Gold |
| Container Apps (Data API) | Storage Blob Data Reader (Gold) | Secret reader | N/A | |
