---
title: "High-Level Solution Design"
project: "mohamad hafiz "
status: "draft"
created: "2026-07-26"
tags:
  - hld
  - data
  - platform
---
# High-Level Solution Design — mohamad hafiz 

> **Platform type:** Data & Analytics Platform

---

## Architecture Overview

```mermaid
graph TB
    subgraph sources["Source Systems"]
        CRM["CRM"]
        ERP["ERP"]
        Events["Event Stream
(Kafka / Event Hub)"]
        Files["File Drops
(SFTP / Blob)"]
    end

    subgraph ingestion["Ingestion Layer"]
        Batch["Batch Ingestion
(ADF / Airflow / Glue)"]
        Stream["Stream Ingestion
(Kafka Connect / Flink)"]
    end

    subgraph lake["Data Lake / Lakehouse"]
        Bronze["🟫 Bronze
(Raw)"]
        Silver["⬜ Silver
(Cleansed)"]
        Gold["🟡 Gold
(Served)"]
    end

    subgraph transform["Transformation"]
        dbt["dbt / Spark
(SQL Transforms)"]
        Quality["Data Quality
(Great Expectations
/ Soda)"]
    end

    subgraph serve["Serving"]
        DW["Data Warehouse
(Synapse / Snowflake)"]
        Catalog["Data Catalogue
(Purview / Datahub)"]
        API["Data API
(REST / GraphQL)"]
    end

    subgraph consume["Consumers"]
        BI["📊 BI / Dashboards
(Power BI / Looker)"]
        DS["🤖 Data Science
/ ML"]
        Apps["📱 Applications"]
    end

    CRM & ERP --> Batch
    Events --> Stream
    Files --> Batch
    Batch & Stream --> Bronze
    Bronze --> dbt --> Silver
    Silver --> Quality
    Quality --> Gold
    Gold --> DW & Catalog & API
    DW --> BI
    Gold --> DS
    API --> Apps
```

---

## Component Summary

| Component | Technology | Responsibility |
|-----------|-----------|----------------|
| Batch Ingestion | *[ADF / Apache Airflow / AWS Glue]* | Scheduled extraction from source systems |
| Stream Ingestion | *[Kafka Connect / Azure Event Hubs + Flink]* | Real-time event capture |
| Data Lake | *[ADLS Gen2 / S3 / GCS]* | Raw and cleansed data storage (Bronze/Silver/Gold) |
| Transformation | *[dbt / Apache Spark]* | Data modelling and business logic |
| Data Quality | *[Great Expectations / Soda / dbt tests]* | Schema and rule validation at each zone |
| Data Warehouse | *[Synapse Analytics / Snowflake / BigQuery]* | Optimised analytical query engine |
| Data Catalogue | *[Microsoft Purview / DataHub / Alation]* | Discoverability, lineage, and governance |
| Data API | *[FastAPI / Azure API Management]* | Programmatic access for downstream apps |

---

## Key Design Decisions

- *[Decision 1 — e.g., "Medallion (Bronze/Silver/Gold) architecture chosen over a two-tier approach for data quality isolation"]*
- *[Decision 2 — e.g., "dbt chosen over Spark for Silver transforms due to SQL-first team skillset and lower operational overhead"]*
- *[Decision 3 — e.g., "All PII masked at Bronze→Silver boundary using deterministic tokenisation"]*
