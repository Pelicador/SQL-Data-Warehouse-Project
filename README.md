## Executive Summary

This project demonstrates a comprehensive data warehousing and analytics solution that implements industry best practices in data engineering and analytics. The solution follows a three-layer Medallion Architecture (Bronze, Silver, and Gold) to transform raw data into actionable business insights. The project demonstrates proper data ingestion, transformation, quality control, and presentation techniques for a retail company.

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
- `crm_cust_info`: Customer information (ID, name, marital status, gender)
- `crm_prod_info`: Product information (ID, name, cost, product line, dates)
- `crm_sales_detales`: Sales transactions (order number, customer, dates, quantities, prices)

**ERP System**:
- `erp_cust_az12`: Additional customer information (customer ID, birthdate, gender)
- `erp_loc_a101`: Customer location information (customer ID, country)
- `erp_px_cat_g1v2`: Additional product information (ID, category, subcategory, maintenance)

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
  
   


