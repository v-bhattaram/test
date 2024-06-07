# Data Reconciliation between Redshift and DynamoDB Tables

The goal is to ensure data consistency between EData tables in Redshift and user-facing tables in DynamoDB.

## Reconciliation Script Design

### Component: Configuration Variables
- **AWS DynamoDB Table Owner**
- **AWS DynamoDB Table ARN**
- **Redshift Table Name**

### Export Location for DynamoDB
- **S3 Bucket** (`s3_bucket`)
- **S3 DynamoDB Export Path** (`s3_dynamo_export_path`)
- **S3 Parquet Path** (`s3_parquet_path`)

### Glue DB Settings
- **Catalog Database** (`catalog_database`)
- **Catalog Table Name** (`catalog_table_name`)

### Reconciliation Results
- **Reconciliation Table Name** (`recon_table_name`)

### Reconciliation Log Entry
- **INSERT Operation**: Capture the reconciliation date, table, and start datetime.

### DynamoDB Table Data Export Steps
1. **Purge Existing Dynamo Parquet Files**
2. **Export DynamoDB Data to S3**

### Reconciliation SQL Process
- The reconciliation query consists of three components:
  1. **Primary Key (PK) Columns**: From both the DynamoDB export (in Redshift Spectrum) and the Redshift tables.
  2. **Reconciliation Columns**: From both the DynamoDB export (in Redshift Spectrum) and the Redshift tables.
  
- Perform a **Full Outer Join** between the DynamoDB export (in Redshift Spectrum) and the Redshift table to identify missing rows in either table.
- Perform an **Inner Join** to find mismatched columns.

### Reconciliation Log Entry
- **UPDATE Operation**: Capture the end datetime and row count for reconciliation issues identified by the SQL queries.
