# Retail Acquisition Data Pipeline

This repository contains the design and implementation of a data pipeline for a retail acquisition scenario. A large retail company acquires a smaller company, and the objective is to consolidate data from both organizations into a unified Lakehouse architecture.

The pipeline ingests heterogeneous data sources, standardizes schemas, aligns metrics, and resolves missing or inconsistent data to enable integrated analytics. The result is a scalable foundation that supports unified reporting and downstream data products for the newly merged organization.

This implementation focuses on delivering a **scalable MVP** that streamlines insights while maintaining an extensible architecture for future growth.

## Problem Context

Before the acquisition, both companies had very different data architectures.

The **parent company** already operates a structured Lakehouse architecture built on Databricks, with a layered data model supporting analytics and reporting.

<img alt="Captura de pantalla 2026-03-06 a la(s) 1 03 07 p m" src="https://github.com/user-attachments/assets/24d6aa4b-fe2a-4998-9ada-7f8ef8373176" />

The existing architecture follows a standard **Medallion Architecture** pattern:

- **OLTP Systems** provide operational data.

- Data is ingested into **Databricks**.

- Data is processed through the **Bronze → Silver → Gold** layers.

- Curated **Gold datasets** feed the **Serving Layer** used for dashboards, reporting, and analytics.

## Child Company Situation

The acquired company had a significantly simpler setup.

**Previous architecture:**

OLTP → Spreadsheets → Dashboarding / Reporting

This setup introduced several limitations:

- Limited scalability

- Lack of centralized data governance

- Manual reporting workflows

- No standardized transformation pipeline

To integrate this company into the parent ecosystem, a new data pipeline is required.

## Unified Lakehouse Architecture

The integration strategy introduces a **dedicated ingestion and transformation pipeline** for the child company while preserving the existing architecture of the parent company.

<img alt="Captura de pantalla 2026-03-06 a la(s) 1 09 30 p m" src="https://github.com/user-attachments/assets/8accec87-bca8-4dd8-b298-04a3c86a0ef4" />

Key design principles:

#### 1. Raw Data Ingestion

- Data from the child company OLTP systems is first stored in **Amazon S3** as raw files.

- Raw data is then ingested into Databricks as **Delta Tables**.

- A **metadata layer** is added through **Unity Catalog** for governance and discoverability.

#### 2. Medallion Processing Pipeline

- The child company data is processed through:

  - **Bronze Layer**– raw structured ingestion

  - **Silver Layer** – cleaned and standardized datasets

  - **Gold Layer** – business-ready tables

#### 3. Data Archival

- After ingestion, the original raw files are archived in **S3** for traceability and reproducibility.

#### 4. Data Consolidation

- The Gold tables from the child company are **merged** with the Gold layer of the parent company.

- This merged dataset becomes the **single source of truth** for analytics.

#### 5. Serving Layer

- Downstream analytics and reporting systems consume the **merged Gold tables**.

Pipeline orchestration and governance rely on:

- **Databricks Lakeflow Jobs** for scheduling and orchestration

- **Unity Catalog** for data governance and metadata management

## Data Ingestion Strategy

The ingestion process follows two stages.

<img alt="Captura de pantalla 2026-03-06 a la(s) 1 10 45 p m" src="https://github.com/user-attachments/assets/4730b1e1-b2de-4a7c-9304-292060634fbd" />

### Historical Backfill

A **batch historical load** will ingest the **last five months of data** from the child company into the parent Lakehouse.

This ensures that analytics can immediately operate on a meaningful historical window after the integration.

### Incremental Loads

After the historical load is complete, the pipeline transitions to **daily incremental ingestion**.

These updates are orchestrated through **Lakeflow Jobs**, ensuring that:

- New records are captured daily

- Transformations are applied consistently

- Gold tables remain synchronized with operational systems

All datasets are registered and governed within **Unity Catalog**.

## Technologies Used

- Databricks

- Delta Lake

- Unity Catalog

- Lakeflow Jobs

- Amazon S3

- SQL / Python data transformations

## Key Outcomes

This architecture enables:

- Scalable ingestion of newly acquired data sources

- Consistent governance through Unity Catalog

- Separation of raw, refined, and curated datasets

- Seamless integration of the child company's data into the parent analytics platform

- A maintainable and extensible Lakehouse foundation for future acquisitions
