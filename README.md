# Retail Sales Data Warehouse – ETL & Data Quality Validation Pipeline

## Project Overview

The **Retail Sales Data Warehouse** project is an end-to-end enterprise-style ETL pipeline developed using **AWS S3, Databricks SQL, Python, Delta Lake, and Databricks Workflows**.

The project simulates a real-world retail data warehouse solution that ingests retail sales data from CSV files, processes it through Medallion Architecture layers, performs data quality validations, maintains historical data using SCD Type 2, and automates orchestration through Databricks Workflow pipelines.

---

# Objectives

* Build an automated ETL pipeline using Databricks
* Implement Medallion Architecture (Bronze, Silver, Gold)
* Process incremental retail sales files from AWS S3
* Implement data quality validations
* Perform reject handling for invalid records
* Implement Slowly Changing Dimension (SCD Type 2)
* Create audit logging and monitoring
* Automate workflows using Databricks Jobs & Pipelines
* Build a star-schema based data warehouse

---

# Technology Stack

| Component            | Technology             |
| -------------------- | ---------------------- |
| Cloud Storage        | AWS S3                 |
| Data Processing      | Databricks SQL         |
| Scripting            | Python                 |
| Data Format          | Delta Lake             |
| Orchestration        | Databricks Workflows   |
| Version Control      | GitHub                 |
| ETL Architecture     | Medallion Architecture |
| Data Warehouse Model | Star Schema            |

---

# Architecture

```
AWS S3 (SFTP Zone)
        ↓
Bronze Layer (Raw Data)
        ↓
Silver Layer (Cleaned & Transformed Data)
        ↓
Gold Layer (Business Warehouse)
        ↓
Fact & Dimension Tables
```

---

# S3 Bucket Structure

```
sneha-retail-sales-data-warehouse/
│
├── sftp/
│   ├── customers/
│   ├── products/
│   ├── stores/
│   └── sales/
│
├── raw/
│   ├── customers/
│   ├── products/
│   ├── stores/
│   └── sales/
│
├── processed/
│   ├── customers/
│   ├── products/
│   ├── stores/
│   └── sales/
│
├── archive/
│   ├── sftp/
│   └── raw/
│
├── rejected/
└── logs/
```

---

# Medallion Architecture

## Bronze Layer

The Bronze layer stores raw ingested data directly from source CSV files.

### Tables

* customers_raw
* products_raw
* stores_raw
* sales_raw

### Features

* Raw ingestion
* Source metadata tracking
* File name tracking
* Load timestamp tracking
* Delta table storage

---

## Silver Layer

The Silver layer performs cleansing, transformation, and validation.

### Tables

* silver_customers
* silver_products
* silver_stores
* silver_sales

### Transformations

* Trim whitespace
* Convert names to proper case
* Convert email to lowercase
* Remove duplicate records
* Handle invalid records
* Standardize date formats
* Filter invalid quantities

---

## Gold Layer

The Gold layer contains the final business-ready warehouse tables.

### Dimension Tables

* DimCustomer
* DimProduct
* DimStore

### Fact Table

* FactSales

### Features

* Star schema implementation
* Surrogate keys
* Fact and dimension modeling
* Historical tracking using SCD Type 2
* Business-ready analytics layer

---

# Slowly Changing Dimension (SCD Type 2)

SCD Type 2 is implemented in the `DimCustomer` table to maintain historical customer information.

### Logic

When customer attributes such as:

* City
* Address

change:

* Existing record becomes inactive
* EndDate is updated
* New active record is inserted

### Benefits

* Historical tracking
* Time-based analytics
* Accurate reporting

---

# Incremental Load Processing

The project supports incremental processing using file timestamp-based ingestion.

### File Naming Convention

```
customers_DDMMYYYYHHMMSS.csv
```

### Incremental Logic

* Latest file remains active
* Older files move to archive
* Only new data is processed

### Benefits

* Faster processing
* Reduced compute cost
* Real-time style ingestion

---

# Data Quality Validation

The project implements enterprise-level data quality checks.

## Validation Checks

### Null Validation

Checks mandatory fields for null values.

### Duplicate Validation

Detects duplicate records using SQL window functions.

### Referential Integrity Validation

Ensures fact table references valid dimension records.

### Invalid Data Validation

Detects:

* Negative quantities
* Invalid dates
* Missing foreign keys

### Transformation Validation

Validates:

* Proper case conversion
* Lowercase email conversion
* Date standardization
* Derived amount calculations

---

# Reject Handling

Invalid records are isolated into reject tables instead of failing the entire pipeline.

## Reject Tables

* rejected_sales_qty
* rejected_products
* rejected_sales_customer

## Benefits

* Prevents complete pipeline failure
* Improves data quality
* Simplifies debugging and correction

---

# Audit Logging

Audit logging is implemented to monitor ETL execution.

## Logged Information

* Pipeline layer
* Table name
* Record counts
* Execution status
* Start and end time
* Error messages

## Benefits

* Pipeline monitoring
* Operational tracking
* Easier debugging
* Production-level observability

---

# Workflow Orchestration

Databricks Workflow pipeline automates the entire ETL process.

## Pipeline Flow

```
7_archival_process.py
        ↓
1_bronze_ingestion.sql
        ↓
2_silver_transform.sql
        ↓
4_reject_handling.sql
        ↓
3_gold_load.sql
        ↓
5_audit_logging.sql
        ↓
6_validation_checks.sql
```

## Features

* Sequential orchestration
* Automated execution
* Failure monitoring
* Retry mechanism
* Scheduling support

---

# ETL Testing

ETL testing is implemented across all layers.

## Testing Performed

* Source-to-target validation
* Row count validation
* Column mapping validation
* Transformation testing
* Data type validation
* Referential integrity testing
* SCD validation

---

# Project Features

* End-to-end ETL automation
* AWS S3 cloud integration
* Delta Lake implementation
* Medallion architecture
* Incremental load processing
* Historical data tracking
* Data quality validation
* Reject handling
* Audit logging
* Workflow orchestration
* Star schema warehouse modeling

---

# Repository Structure

```
Retail-sales-data-warehouse/
│
├── notebooks/
│   ├── 01_bronze_ingestion.sql
│   ├── 02_silver_transform.sql
│   ├── 03_gold_load.sql
│   ├── 04_reject_handling.sql
│   ├── 05_audit_logging.sql
│   ├── 06_validation_checks.sql
│   └── 07_archival_process.py
│
└── README.md
```

---

# Key Learning Outcomes

Through this project, the following concepts were implemented and understood:

* Cloud-based ETL architecture
* Databricks SQL processing
* Delta Lake implementation
* Data warehouse design
* Star schema modeling
* Workflow orchestration
* Incremental ETL processing
* SCD Type 2 implementation
* Data quality engineering
* ETL testing strategies
* Audit and monitoring techniques

---

# Future Enhancements

* Real-time streaming ingestion
* Power BI/Tableau dashboard integration
* Advanced monitoring and alerting
* CI/CD deployment automation
* Machine learning-based anomaly detection

---

# Conclusion

This project demonstrates a production-style enterprise ETL pipeline for retail sales analytics using AWS S3 and Databricks. The implementation includes Medallion Architecture, incremental processing, SCD Type 2, audit logging, reject handling, data quality validation, and workflow orchestration to simulate a scalable real-world data engineering solution.

---

# Author

**Sneha Metla**

Retail Sales Data Warehouse Project
