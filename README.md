# BeautyWorld

## Stage 1) Data Sources

Data from BeautyWorld (BW), a FMCG company selling beauty products for different categories. 

BW gets syndicated data on a regular basis from an agency across a number of countries and categories: France (Skincare), Poland (Razors & Blades), UK (Wipes).

The datasets consist of the following data files, which are TXT files compressed as GZIP:
- Aggregated Data
- Product
- Market
- Period

## Stage 2) Ingestion (Bronze Layer)

GOAL --> Bring data to Fabric from data sources

Ideally, flat files would normally be shared through cloud-based online storage accounts (e.g. ADLS, Amazon S3, Google Cloud Storage) or FTP servers. However for the purposes of this exercise, they are stored on my local computer.

Given the limitations of working with local files (i.e installing data gateways for pipelines and DFG2, notebooks running on Spark clusters), Antigravity recommended a Powershell script using Azcopy, a command-line utility.

## Stage 3) Transformation (Silver Layer)

GOAL--> Structuring, cleaning and validating data (i.e. schemas, management of nulls/blanks/duplicates)

Once the files are within the Fabric ecosystem, there are multiple workloads available to carry out data preparation:

1. **Fabric Notebooks (PySpark / Spark SQL)**: Highly Recommended. Best for flexibility, scalability, and implementing custom "quarantine" paths for failed quality checks.
2. **Dataflows Gen2 (Power Query)**: Ideal low-code/no-code visual workflow for column renaming, datatype casting, and filtering.
3. **dbt (Data Build Tool)**: Best if you want a SQL-first approach with automated schema/data tests (not_null, unique, etc.).
4. **Data Factory Copy Activity**: Useful for direct mappings, but has limited support for complex quality verification rules.
5. **Data Warehouse T-SQL (Staging + INSERT)**: Bulk-load raw files into a staging table using COPY INTO and then execute an INSERT/SELECT statement with TRY_CAST and WHERE clauses to validate numbers, assign datatypes, and write to your silver tables.

It is worth mentioning that these flat files ingested as-is contain mostly unusable column names and would require the correct datatypes in place for further analysis.

## Stage 4) Reporting (Gold Layer)
