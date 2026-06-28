# End-to-End Medallion Warehouse

**Transform raw operational data into reliable, analyst-ready analytics infrastructure.**

A production-grade SQL Server medallion data warehouse pipeline that ingests heterogeneous data from CRM and ERP systems, standardizes quality across transformation layers, and surfaces clean, actionable insights through a dimensional star schema designed for analytics and reporting.

---

## The Problem

Organizations generate data across siloed systems; CRM platforms track customer interactions and sales, ERPs manage operations and product hierarchies. But these sources speak different languages: inconsistent schemas, varying data types, incomplete records, and conflicting business logic.

---

## The Solution

This project implements a **medallion architecture**,a proven pattern for enterprise data warehousing that systematically transforms raw, inconsistent operational data into a clean, dimensional analytics layer. 
Three distinct transformation stages ensure data flows from **raw → standardized → business-ready** with full lineage and accountability at every step.

**Key outcomes:**
- ✅ Single source of truth for customer, product, and sales analytics
- ✅ Reproducible, auditable transformations with clear layer contracts  
- ✅ Analyst-ready star schema optimized for BI, dashboarding, and ad-hoc queries
- ✅ Extensible foundation for future use cases (ML pipelines, advanced analytics)

---

## Architecture Overview

![image](https://github.com/anikaa20/End-to-End-Medallion-Warehouse/blob/main/doc/Data%20Architecture.png)


### Architectural Principles

| Layer | Purpose | Transformation | Data Model |
|-------|---------|-----------------|------------|
| **Bronze** | Exact replica of operational systems | **None**.Raw data ingested as-is | Flat, no keys |
| **Silver** | Cleaned, deduplicated, type-standardized | Schema validation, null handling, referential integrity | Normalized staging tables |
| **Gold** | Dimensional layer optimized for analytics | Business logic, aggregations, dimensional joins | Star schema (facts + dimensions) |

---

## Data Model

### Star Schema (Gold Layer)

![image](https://github.com/anikaa20/End-to-End-Medallion-Warehouse/blob/main/doc/Star%20Schema.png)

The gold layer implements a **star schema** centered on sales transactions, with conformed dimensions for customers and products.



---

## Data Layers Explained

![image](https://github.com/anikaa20/End-to-End-Medallion-Warehouse/blob/main/doc/Data%20Flow.png)

### Bronze Layer: Raw Data

**Purpose:** Exact replica of operational systems; single source of truth for raw data.

**Characteristics:**
- No transformations applied
- One-to-one mapping with source systems (CRM tables, ERP tables)
- Retains all fields, including nulls and inconsistencies
- No primary keys enforced


---

### Silver Layer: Cleaned & Standardized

**Purpose:** High-quality, conformed staging layer; single version of truth for business metrics.

**Transformations applied:**
- Remove duplicates (e.g., multiple customer records per customer ID)
- Standardize data types (ensure dates, decimals consistent)
- Validate referential integrity (all customer IDs must exist in customer table)
- Handle nulls (COALESCE, imputation, or explicit documentation)
- Enrich from multiple sources (e.g., merge customer birthdate from ERP into CRM customer record)



---

### Gold Layer: Dimensional Analytics

**Purpose:** Optimized for analyst queries, BI dashboards, and ML pipelines.

**Structure:**
- **Dimensions:** Slowly changing reference data (customers, products)
- **Facts:** Transactional events (sales orders)

**Characteristics:**
- Denormalized for query performance
- Foreign keys link facts to dimensions
- Conformed dimensions ensure consistent business logic across analyses
- Surrogate keys (integer PKs) decouple warehouse from source systems

**Why star schema?**
- Analysts write simple queries without complex joins
- BI tools generate better performance execution plans
- Supports incremental loads efficiently
- Enables slowly changing dimension (SCD) logic for historical tracking


---

## Key Design Decisions

### Why Medallion Architecture?

✅ **Separation of concerns:** Each layer has a single responsibility (raw, clean, analytical)  
✅ **Auditability:** Track data lineage from source → warehouse  
✅ **Extensibility:** Add new layers (e.g., silver-to-silver transformations) without rewriting  
✅ **Recovery:** Corrupt gold layer? Recompute from silver. Corrupt silver? Restore from bronze.

### Why Star Schema (not Snowflake)?

✅ **Analyst experience:** Fewer joins, faster queries  
✅ **BI tool compatibility:** Tableau, Power BI optimize for stars  
✅ **Maintainability:** Easier to document and govern conformed dimensions  
⚠️ **Trade-off:** Slight redundancy in dimension storage (acceptable for warehouse use case)



---

## Future Enhancements

- [ ] **Incremental loads:** Implement change data capture (CDC) for efficient refreshes
- [ ] **Slowly Changing Dimensions (SCD Type 2):** Track historical customer/product attributes
- [ ] **Data lineage tracking:** Integrate with metadata tools (e.g., Apache Atlas, open-source alternatives)
- [ ] **Automated data quality framework:** dbt tests or custom validation procedures
- [ ] **dbt integration:** Migrate SQL scripts to dbt for version control & lineage
- [ ] **Cloud migration:** Port to Azure Synapse or Databricks for scalability
- [ ] **Real-time ingestion:** Explore streaming via Kafka for sub-hour latency



