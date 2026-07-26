---
title: "Lakehouse Cloud Topology"
project: "mohamad hafiz "
status: "draft"
created: "2026-07-26"
tags:
  - cloud
  - topology
  - lakehouse
  - azure
---
# Lakehouse Cloud Topology — mohamad hafiz 

> Template assumes **Microsoft Azure**. Adapt labels for AWS (S3, Glue, Redshift) or GCP (GCS, Dataform, BigQuery) as needed.

---

## Cloud Topology Diagram

```mermaid
graph TB
    subgraph sources["Source Systems (external)"]
        CRM["CRM / ERP"]
        Events["Event Hub
(Streaming)"]
    end

    subgraph ingestion["Ingestion — Resource Group: rg-ingestion"]
        ADF["Azure Data Factory
(Batch pipelines)"]
        Flink["Azure Stream Analytics
/ Flink (Streaming)"]
    end

    subgraph lake["Data Lake — Resource Group: rg-data"]
        ADLS["ADLS Gen2
Bronze / Silver / Gold
(Delta Lake format)"]
    end

    subgraph compute["Compute — Resource Group: rg-compute"]
        Spark["Azure Databricks
(Spark / dbt)"]
        Synapse["Azure Synapse
(Serverless SQL)"]
    end

    subgraph serve["Serving — Resource Group: rg-serve"]
        Purview["Microsoft Purview
(Data Catalogue)"]
        PowerBI["Power BI
(BI reports)"]
        DataAPI["Data API
(Container Apps)"]
    end

    subgraph ops["Operations"]
        Monitor["Azure Monitor
+ Log Analytics"]
        KV["Key Vault
(Secrets)"]
        MI["Managed Identity"]
    end

    CRM -->|"JDBC batch"| ADF
    Events -->|"AMQP stream"| Flink
    ADF -->|"Parquet"| ADLS
    Flink -->|"Delta"| ADLS
    ADLS --> Spark
    Spark -->|"dbt transforms"| ADLS
    ADLS --> Synapse
    Synapse --> PowerBI
    ADLS --> DataAPI
    ADLS --> Purview
    Spark & ADF & Synapse --> Monitor
    MI --> ADF & Spark & Synapse & DataAPI
    KV --> ADF & DataAPI
```

---

## Resource Inventory

| Resource | SKU / Config | Region | Purpose |
|----------|-------------|--------|---------|
| ADLS Gen2 | Standard LRS / ZRS | *[Region]* | Lakehouse storage — all zones |
| Azure Data Factory | *[Standard]* | *[Region]* | Batch orchestration |
| Azure Databricks | *[Premium / Standard cluster]* | *[Region]* | Spark compute for transforms |
| Azure Synapse Analytics | *[Serverless SQL]* | *[Region]* | Ad-hoc analyst queries |
| Microsoft Purview | *[Standard]* | *[Region]* | Data governance + catalogue |
| Azure Monitor | *[Pay-as-you-go]* | *[Region]* | Pipeline + infra monitoring |
| Key Vault | Standard | *[Region]* | Source credentials, connection strings |

---

## Network & Security Controls

- **Private endpoints:** ADLS, Key Vault, Databricks accessed via private endpoints only.
- **Managed Identity:** All services authenticate to ADLS and Key Vault via MI — no stored credentials.
- **Encryption:** ADLS encrypted with Microsoft-managed keys (upgrade to CMK for regulated data).
- **Network isolation:** Databricks deployed in *[VNet injection]* mode; no public IP on clusters.
- **RBAC on ADLS:** ACLs enforced at the container and folder level per zone and role.
