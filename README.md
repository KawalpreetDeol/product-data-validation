# Product Data Validation
Validate product data using Azure Databricks, Azure Datalake Gen2, Azure Data Factory, Azure Key Vault and Azure SQL Database.

## Architecture
![Architecture Diagram](./ArchitectureDiagram.png)

### Azure Data Lake Gen2
Used to injest and stage the data.
1. Landing: blob files are uploaded directly here for ingestion 
2. Staging: stores/stages the data after being validated by the Azure Databricks Notebook
3. Rejected: stores the blob files that were rejected by the Azure Databricks Notebook

### Azure Data Factory
Used to build ETL pipelines to Move data from one location to another.
1. Invoke Notebook: Used to trigger the Azure Databricks Notebook when a new blob is added to the landing folder in Azure Data Lake Gen2
    - Storage Event Based Trigger: triggered when a new file is added to the Landing folder in Data Lake Gen2

### Azure Databricks
Used to validate the blob file from Azure Data Lake Gen2 with the Schema in Azure SQL Database. Places successful files in the Staging folder and the unsuccessful files in the Rejected folder on Azure Data Lake Gen2.

### Azure SQL Server
Runs the Azure SQL Database.

### Azure SQL Database
Stores the correct schema in a table.

### Azure Key Vault
Stores the Azure SQL Database and Azure Datalake Gen2 keys.

## Steps to Reproduce
1. Create an Azure Data Lake Storage Account (enable hierarchical namespace)
2. Create an Azure Databricks Workspace
3. Create an Azure SQL Server and an Azure SQL Database
4. Access the Azure SQL Database using SQL Server Management Studio (SSMS)
5. Create the schema table in Azure SQL Database through SSMS
6. Create an Azure Key Vault
7. Add the SQL Server Password and Data Lake Gen2 SAS Token to the Azure Key Vault as secret keys
8. Create a secrets scrope in Databricks to use the secret keys from Azure Key Vault
9. Create a Spark Cluster in Azure Databricks
10. Create a Notebook, then copy and paste the code from the repo (`databricks/Project0002.ipynb`) into the notebook or load the notebook from the repo directly into databricks
11. Create an Azure Data Facotry Workspace
12. Create a pipeline to trigger the Databricks Notebook on new file uploads to the landing folder in Azure Data Lake Gen2 (Storage Event Trigger) using a Linked Service
13. Perform End-to-End testing to ensure the data flows correctly.

## Improvements
- It may be better to use parquet files, especially for files with more rows to allow for faster processing times. Parquet is a compressed column based file format.
