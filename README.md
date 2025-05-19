## Executive Summary

This project demonstrates a comprehensive data warehousing and analytics solution that implements industry best practices in data engineering and analytics. The solution follows a three-layer Medallion Architecture (Bronze, Silver, and Gold) to transform raw data into actionable business insights. The project demonstrates proper data ingestion, transformation, quality control, and presentation techniques for a retail company.

## **Table of Contents**

1. [Project Overview](#project-overview)
2. [Data Architecture](#data-architecture)
3. [Technical Implementation](#technical-implementation)
   - [Bronze Layer Implementation](#bronze-layer-implementation)
   - [Silver Layer Implementation](#silver-layer-implementation)
   - [Gold Layer Implementation](#gold-layer-implementation)
4. [Data Quality Checks](#data-quality-checks)
5. [Repository Structure](#repository-structure)

## Project Overview

### Business Context
The data warehouse integrates data from two source systems (ERP and CRM) to provide a unified view of customer information, product details, and sales transactions. This enables advanced analytics and reporting capabilities for business decision-making.

### Project Scope
- Design a modern data warehouse using Medallion Architecture
- Develop ETL pipelines for data ingestion and transformation
- Create an optimized data model with fact and dimension tables
- Implement data quality checks and validation
- Provide a data catalog for analytics users

## Data Architecture
![Data Architecture layout](/docs/data_architecture.png)

### Medallion Architecture
The project implements a three-layer architecture:

1. **Bronze Layer**: Raw data ingested as-is from source systems
2. **Silver Layer**: Cleansed, standardized, and normalized data
3. **Gold Layer**: Business-ready data modeled in a star schema for reporting and analytics

![Medallion Architecture](https://img.shields.io/badge/Bronze-Raw%20Data-brown) → 
![Silver Layer](https://img.shields.io/badge/Silver-Cleansed%20Data-silver) → 
![Gold Layer](https://img.shields.io/badge/Gold-Business%20Ready-gold)

### Data Sources
The project integrates data from two source systems:

**CRM System**:
- `crm_cust_info`: Customer information (cst_id, cst_key, cst_firstname, cst_lastname cst_marital_status, cst_gndr)
- `crm_prod_info`: Product information (prd_id, prd_key, prd_name, prd_cost, prd_line, prd_start_dt, prd_end_dt)
- `crm_sales_detales`: Sales transactions (sls_ord_num, sls_prd_key, sls_cust_id, sls_order_dt, sls_ship_dt, sls_due_dt, sls_sales, sls_quantity, sls_price)

**ERP System**:
- `erp_cust_az12`: Additional customer information (cid, bdate, gen)
- `erp_loc_a101`: Customer location information (cid, cntry)
- `erp_px_cat_g1v2`: Additional product information (id, cat, subcat, maintenance)

## Technical Implementation
### Database and Schema Creation
The project begins by creating a new database named 'DataWarehouse' with the three schemas `init_database.sql`

### Bronze Layer Implementation
1. **Table Creation**:`ddl_bronze.sql`
   - Creates tables matching the original data sources
   - Maintains original column names and data types
2. **Data Loading**: `proc_load_bronze.sql`
   - Implements a stored procedure to load data from CSV files
   - Truncates target tables if exist before loading to ensure clean data
   - Uses BULK INSERT commands for efficient data loading
   - Tracks processing time for performance monitoring

  ### Silver Layer Implementation
1. **Table Creation**:`ddl_silver.sql`
   - Creates tables with improved data types and structure for data cleansing
   - Adds tracking columns like dwh_create_date to know when table last updated
3. **Data Transformation**: `proc_load_bronze.sql`
     - Implements a stored procedure to load data from bronze with transformation
     - Implements complex transformation logic:
         - Trimming spaces from text fields
         - Normalizing values to more readable format (gender and marital status)
         - Extracting category IDs and product keys
         - Mapping product line codes to descriptive values
         - Fixing date fields and recalculating incorrect sales amounts
         - Handling missing values and invalid data

  ### Gold Layer Implementation
1. **View Creation**:`ddl_gold.sql`
   - Creates a star schema with dimension and fact tables:
      - `gold.dim_customers`: Customer dimension
      - `gold.dim_products`: Product dimension
      - `gold.fact_sales`: Sales fact table
3. **Data Enrichment**:
   - Renames columns to be more descriptive
   - Creates surrogate keys for dimension tables
   - Combines data from multiple source ERP and CRM for complete business view
  
![Data Flow layout](/docs/data_flow.png)
  
## Data Quality Checks
1. **Silver Layer Quality Checks**:
   - Validates null or duplicate primary keys
   - Checks for unwanted spaces in string fields
   - Ensures data standardization and consistency
   - Validates date ranges and order
   - Verifies data consistency between related fields
2. **Gold Layer Quality Checks**: 
   - Ensures uniqueness of surrogate keys in dimension tables
   - Validates referential integrity between fact and dimension tables
   - Confirms relationships in the data model
  
#### Table and Column Naming Conventions[naming conventions](/docs/naming_conventions.md)
#### Gold Layer Data Catalog and Description[data catalog](/docs/data_catalog.md)
  
This data warehouse project demonstrates a comprehensive approach to data integration and analytics. By following the Medallion Architecture and implementing robust data quality checks, the solution provides reliable business intelligence capabilities for a retail company selling bikes and related products. The scalable design ensures that additional data sources can be integrated in the future, and the standardized naming conventions make the system maintainable and understandable for all stakeholders.

  
---
## Repository Structure
```
data-warehouse-project/
│
├── datasets/                           # Raw datasets used for the project (ERP and CRM data)
│
├── docs/                               # Project documentation and architecture details
│   ├── data_architecture.png           # Draw.io file shows the project's architecture
│   ├── data_catalog.md                 # Catalog of datasets, including field descriptions and metadata
│   ├── data_flow.png                   # Draw.io file for the data flow diagram
│   ├── data_models.png                 # Draw.io file for data models (star schema)
│   ├── naming_conventions.md           # Consistent naming guidelines for tables, columns, and files
│
├── scripts/                            # SQL scripts for ETL and transformations
│   ├── bronze/                         # Scripts for extracting and loading raw data
│   ├── silver/                         # Scripts for cleaning and transforming data
│   ├── gold/                           # Scripts for creating analytical models
│
├── tests/                              # Test scripts and quality files
│   ├── quality_checks_gold.sql         # Gold layer quality checks
│   ├── quality_checks_silver.sql       # Silver layer quality checks
│
├── README.md                           # Project overview and instructions
└── LICENSE                             # License information for the repository

```
---

     
     
  
   


