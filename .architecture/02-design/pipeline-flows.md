---
title: "Data Pipeline Flows"
project: "mohamad hafiz "
status: "draft"
created: "2026-07-26"
tags:
  - pipeline
  - data-flow
  - transformation
---
# Data Pipeline Flows — mohamad hafiz 

---

## Pipeline Overview

```mermaid
graph TB
    subgraph daily["Daily Batch Pipelines (02:00–06:00 UTC)"]
        P1["ingest_source_a
(02:00)"]
        P2["ingest_source_b
(02:30)"]
        P3["transform_silver
(04:00 — depends on P1, P2)"]
        P4["load_gold_dims
(05:00 — depends on P3)"]
        P5["load_gold_facts
(05:30 — depends on P4)"]
        P6["refresh_dw
(06:00 — depends on P5)"]
    end

    subgraph stream["Streaming Pipelines (continuous)"]
        S1["stream_events
(real-time)"]
        S2["enrich_events
(real-time)"]
    end

    P1 --> P3
    P2 --> P3
    P3 --> P4
    P4 --> P5
    P5 --> P6
    S1 --> S2
```

---

## Pipeline Details

### Pipeline: `ingest_source_a`

| Attribute | Value |
|-----------|-------|
| Schedule | Daily at 02:00 UTC |
| Source | DS-001 — *[Source A]* |
| Target | Bronze zone — `raw/source_a/` |
| Mode | Incremental (watermark: `updated_at`) |
| Expected duration | *[< 30 min]* |
| SLA | Complete by 03:00 UTC |
| On failure | Alert + retry ×3 |

---

### Pipeline: `transform_silver`

| Attribute | Value |
|-----------|-------|
| Schedule | Daily at 04:00 UTC (after ingest pipelines) |
| Source | Bronze zone |
| Target | Silver zone |
| Engine | *[dbt / Spark]* |
| Models | *[List dbt models or Spark jobs]* |
| PII handling | Mask / tokenise before writing Silver |
| Quality checks | dbt tests: not_null, unique, accepted_values, relationships |
| Expected duration | *[< 45 min]* |

---

### Pipeline: `stream_events`

| Attribute | Value |
|-----------|-------|
| Trigger | Continuous |
| Source | *[Kafka topic: `events.raw`]* |
| Target | Bronze zone — Delta table `raw/events/` (partitioned by day) |
| Checkpoint | *[ADLS / S3 — checkpoint dir]* |
| Micro-batch interval | *[10 s]* |
| Late arrival tolerance | *[1 h watermark]* |

---

## Dependency Graph

```mermaid
graph LR
    DS1["DS-001 data available"] --> T_Silver["transform_silver"]
    DS2["DS-002 data available"] --> T_Silver
    T_Silver --> Gold_Dims["load_gold_dims"]
    Gold_Dims --> Gold_Facts["load_gold_facts"]
    Gold_Facts --> DW_Refresh["refresh_data_warehouse"]
    DW_Refresh --> BI_Ready["✅ BI dashboards fresh"]
```
