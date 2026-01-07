# Target Architecture  
## RetailOps Intelligence Platform (ROIP)

---

## 1. Architecture Goal

The RetailOps Intelligence Platform (ROIP) implements a layered analytics architecture that separates ingestion, modeling, and consumption to ensure traceability, testability, and predictable performance for retail operations reporting.

The architecture is designed to support incremental, sprint-based delivery while maintaining clear ownership boundaries across data layers.

---

## 2. Architecture Principles

- **Separation of concerns:** Source, staging, warehouse, and BI layers have clearly defined responsibilities.
- **Traceability:** Analytical datasets and KPIs must be traceable to source systems and load events.
- **Auditability:** Data loads and transformations are logged, repeatable, and version-controlled.
- **Test-driven delivery:** Validation is built into the platform through unit, functional, and negative test cases.
- **Performance awareness:** Baselines and tuning decisions are measured and documented.
- **Incremental release:** Each sprint delivers a releasable increment with supporting documentation and evidence.

---

## 3. Logical Architecture

### 3.1 Source Systems

Primary source system:
- **WideWorldImporters** (reference retail source system). Acquisition and installation are tracked as a delivery dependency.

Secondary / fallback source:
- **AdventureWorks2022 / AdventureWorksDW2022**, available locally and suitable for implementing ingestion patterns if required during early sprints.

Optional future sources:
- A MySQL-based source (OMS-style) to demonstrate cross-platform ingestion patterns in later phases.

---

### 3.2 Staging Layer

**Purpose**
- Persist source data with minimal transformation.
- Provide a controlled landing zone for reconciliation, auditing, and reprocessing.
- Isolate source volatility from downstream analytical models.

**Characteristics**
- Tables closely align to source system entities.
- Each load captures audit metadata, including load timestamp, record counts, and load status.
- Staging enables source-to-target reconciliation and early anomaly detection.

---

### 3.3 Warehouse Layer (Analytics Model)

**Purpose**
- Transform staged data into analytics-optimized structures.
- Provide consistent, governed foundations for KPI calculations.

**Modeling approach**
- Star schema design with clearly defined dimensions and fact tables.
- Explicit grain definitions for each fact (e.g., one row per order line per day).

**Planned dimensions (examples)**
- `dim_date`
- `dim_product`
- `dim_customer`
- `dim_location` (warehouse or region)
- `dim_channel` (if applicable)

**Planned facts (examples)**
- `fact_sales`
- `fact_inventory_snapshot`
- `fact_fulfillment`
- `fact_returns`

**Design considerations**
- Referential integrity is validated between facts and dimensions.
- Slowly changing dimensions may be introduced where justified in later sprints.

---

### 3.4 BI and Consumption Layer

**Purpose**
- Deliver business-ready reporting through a governed semantic model.

**Planned outputs**
- Power BI semantic model with certified measures aligned to documented KPIs.
- Executive dashboard providing high-level operational health indicators.
- Operations dashboard focused on inventory, fulfillment, and returns drivers.
- Data health views summarizing freshness, load success, and validation results.

---

### 3.5 Quality, Performance, and Operational Controls

**Testing**
- Unit tests for transformation logic and key business rules.
- Functional tests for reconciliation and end-to-end integrity.
- Negative tests for failure scenarios (missing keys, duplicates, invalid values).

**Performance**
- Baseline timings for representative queries and loads.
- Indexing and tuning with before-and-after evidence.
- Performance testing using JMeter for selected workloads in later sprints.

**Operational readiness**
- Deployment guidance for database objects and scripts.
- Monitoring checks for load failures, data freshness, and anomalies.

---

## 4. Physical Architecture (Local Implementation)

**Primary components**
- **SQL Server 2022:** Staging and warehouse schemas.
- **VS Code:** Development environment for scripts and documentation.
- **GitHub:** Version control, sprint artifacts, and delivery evidence.
- **Power BI:** Semantic model and dashboards.

**Database organization**
- A dedicated database for ROIP analytics objects is recommended.
- Staging and warehouse layers are separated by schema naming conventions.

---

## 5. High-Level Data Flow

1. Data is extracted from source systems.
2. Source data is persisted in staging tables with load auditing.
3. Warehouse transformations generate dimensions and fact tables.
4. BI semantic models consume warehouse structures.
5. Validation and performance evidence is captured alongside delivery.

---

## 6. Dependencies and Risks

**Dependencies**
- Installation and availability of the WideWorldImporters source system.

**Risks**
- Local environment performance constraints may limit benchmark scalability.
- Reference dataset limitations may require documented assumptions.

**Mitigation**
- Constraints and assumptions are explicitly documented.
- Performance results are reported comparatively (baseline vs tuned).

---

## 7. Sprint Alignment

- **Sprint 1:** Source ingestion and staging foundation.
- **Sprint 2:** Warehouse modeling (dimensions and facts).
- **Sprint 3:** KPI certification and BI semantic layer.
- **Sprint 4:** Performance testing and production readiness evidence.

---

## 8. Status

**Status:** Approved — Sprint 0  
**Next Milestone:** Sprint 1 Planning (Source Ingestion & Staging)