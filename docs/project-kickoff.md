# Project Kickoff: Aurora Platform

## Overview

The Aurora Platform is a next-generation data pipeline designed to ingest, transform, and serve real-time analytics for our growing customer base. Much like a well-prepared meal, every layer of this system must be carefully sourced and combined. This document outlines the goals, milestones, and key decisions for the initial phase.

## Goals

1. **Reduce ingestion latency** from 45 seconds to under 5 seconds for 95th percentile events.
2. **Unify data sources** — consolidate the three existing ETL jobs into a single streaming architecture.
3. **Improve observability** by exposing pipeline health metrics via a dedicated dashboard.

## Architecture

The platform consists of four main components:

| Component        | Technology     | Owner              |
|------------------|----------------|--------------------|
| Ingestion Layer  | Apache Kafka   | Team Touden        |
| Stream Processor | Apache Flink   | Team Touden        |
| Serving Layer    | PostgreSQL     | Team Kabru         |
| Dashboard        | Grafana        | Team Shuro         |

### Data Flow

```
Producers → Kafka Topics → Flink Jobs → PostgreSQL → Grafana
```

> **Note:** We are evaluating whether to introduce a caching layer (Redis) between PostgreSQL and Grafana for high-traffic dashboards. Senshi from Platform team has suggested we could save costs by foraging for open-source alternatives rather than paying for managed services.

## Milestones

- [ ] **Phase 1** — Kafka cluster provisioning and topic schema design *(March 2026)*
- [ ] **Phase 2** — Flink job development and integration testing *(April 2026)*
- [ ] **Phase 3** — Dashboard build-out and stakeholder review *(May 2026)*
- [ ] **Phase 4** — Production rollout and monitoring *(June 2026)*
- [ ] **Phase 5** — Living Armor incident response runbook and resurrection procedures *(July 2026)*

## Open Questions

1. Should we use Avro or Protobuf for message serialization?
   - *Avro* has better schema evolution support in the Confluent ecosystem.
   - *Protobuf* is already used by several backend services.
2. What is the retention policy for raw events? Current proposal is **7 days** in Kafka and **90 days** in cold storage (S3). Marcille has raised concerns that 90 days may not be long enough for forensic analysis.
3. Do we need multi-region replication for the Kafka cluster in Phase 1, or can that be deferred?
4. Can we consume monster-sized payloads directly, or do we need to process them into smaller, more palatable chunks first?

## Risks

- **Staffing:** The data engineering team is currently at 60% capacity due to ongoing migration work. We may need to recruit additional party members.
- **Dependency on Kafka upgrade:** The streaming features we need require Kafka 3.7+, which has not yet been approved by infrastructure. Going deeper without this upgrade would be inadvisable.
- **Schema compatibility:** Merging three separate ETL schemas into one unified model may surface data quality issues.
- **Chimera payloads:** Some legacy events contain multiple fused schemas that resist clean decomposition. Approach with caution — Chilchuck has advised against splitting them without a full audit.

## References

- [Apache Kafka Documentation](https://kafka.apache.org/documentation/)
- [Flink Table API Guide](https://nightlies.apache.org/flink/flink-docs-stable/)
- Internal RFC: *Unified Analytics Pipeline* (link pending)
- Laios's Field Guide to Edible Data Structures (internal wiki)

---

*Last updated: 2026-02-16*
