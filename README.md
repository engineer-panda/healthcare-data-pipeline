# End-to-End Healthcare Data Pipeline (ADF + Databricks)

Designed and implemented a production-grade healthcare data pipeline using Azure Data Factory and Databricks to process multiple datasets (patients, providers, visits, billings).

The solution leverages hybrid incremental processing (ingestion_time watermark + control table + file-level tracking) to ensure idempotent, rerunnable data ingestion with full auditability and operational reliability.

---

## Architecture
![Architecture](architecture.jpg)

- Azure Data Factory for orchestration and pipeline control  
- Azure Data Lake Storage Gen2 implementing Medallion Architecture (Raw, Bronze, Silver)  
- Azure Databricks (PySpark) for scalable data transformation  
- Delta Lake for ACID-compliant storage and optimized reads  
- Azure SQL Database for metadata-driven control tables and audit logging  
- Azure Key Vault for secure secret and credential management  

---

## Data Flow
Source → Raw → Bronze → Silver

- Ingested data from on-prem and external sources into Raw layer without transformation  
- Standardized and organized data in Bronze layer using ADF pipelines  
- Implemented hybrid incremental logic using ingestion_time watermark combined with control tables and file tracking  
- Applied data validation, cleansing, and deduplication using PySpark window functions in Databricks  
- Loaded curated, analytics-ready data into Silver layer  
- Maintained end-to-end tracking using SQL-based control and audit tables  

---

## Key Features
- Hybrid incremental processing (watermark + file tracking + control tables)  
- Idempotent and rerunnable pipeline design  
- Metadata-driven orchestration using control tables  
- Robust audit logging for success, failure, and no-data scenarios  
- Parameterized pipelines supporting multiple datasets  
- Secure secret management via Azure Key Vault integration  
- Master pipeline for scalable orchestration  

---

## ADF Pipeline

![ADF Pipeline](docs/master_pipeline.png)

### Bronze to Silver Orchestration
ADF master pipeline orchestrates dataset-specific pipelines using Execute Pipeline activities with parameterized execution.

Implements dependency management, sequencing, and hybrid incremental logic while ensuring audit tracking and failure handling through control tables.

---

## Control Tables

Provides centralized metadata management and execution tracking for reliable pipeline operations.

- **Pipeline Control Table**: Drives dynamic pipeline execution (source paths, load type, watermark column, active flags)  
- **Files Table**: Enables file-level incremental processing and prevents duplicate ingestion  
- **Audit Log Table**: Captures execution metadata (pipeline, dataset, status, timestamps, errors) for monitoring and troubleshooting  

![Audit Table](docs/audit_table.png)

---

## Databricks Transformation

![Databricks Notebook](docs/databricks_notebook.png)

PySpark-based transformations enforce schema consistency, handle nulls, and perform deduplication using window functions.

Implements incremental processing using ingestion timestamps and writes optimized Delta tables to the Silver layer.

Secure connectivity to ADLS and Azure SQL is achieved via OAuth and Azure Key Vault-backed secrets.

---

## Pipeline Triggering

- Supports both manual execution for testing and scheduled triggers for production runs  
- Automated scheduling ensures consistent ingestion aligned with data availability  
- Handles no-data scenarios gracefully without pipeline failure  

---

## Detailed Documentation

For a full walkthrough of the pipeline, including architecture, pipeline design, transformations, and additional screenshots:

[View Full Documentation](docs/healthcare-data-pipeline-documentation.pdf)
