# Sales & Banking Data Analysis Pipeline (Azure Databricks)

## 📌 Project Overview
This project demonstrates an end-to-end modern data engineering pipeline for processing large-scale banking and sales data. Utilizing the Medallion Architecture, we ingest raw data from on-premises SQL Server (via Azure Data Factory) into Azure Data Lake Storage (ADLS) Gen2, perform transformations using PySpark in Azure Databricks, and deliver curated datasets for visualization in Power BI.

The primary motivation for using Databricks is its ability to handle massive data volumes efficiently through distributed processing and the Delta Lake format.

## 🏗️ Architecture Diagram
The pipeline follows a modern cloud data architecture:
![new_on_prem_to_adl_to_databricks](https://github.com/user-attachments/assets/9aefd7d4-408a-4830-9d83-41635d2c0827)


1. **Source:** On-premises SQL Server (Ingested via ADF).

2. **Storage:** Azure Data Lake Storage Gen2.

3. **Processing:** Azure Databricks (PySpark).

4. **Security:** Azure Key Vault for managing Service Principals and Access Keys.

5. **Layers:** Bronze (Raw), Silver (Cleaned), and Gold (Business-ready).

6. **Schema Management:** SCD Type 1 for dimensional integrity.

## Repository Structure

```text
├── Code/
│   ├── Bronze_To_Silver/                            # Notebooks for ADLS mounting/connection
│   ├── Silverlayer_Transformation_ScdType1/         # logic for Deduplication, Null checks, and SCD
│   └── Step 3 - highest cost and product/           # DDL/DML for Customer, Product, Sales, and Category tables
├── Documentation/                                   # Architecture diagrams and step-by-step process docs
├── Screenshots/                                     # Validation screenshots of successful pipeline runs
└── README.md                                        # Project documentation
```



## Pipeline Workflow
**Step 1:** ADLS Gen2 Connectivity
To ensure modularity, connectivity is handled in a dedicated notebook. This notebook is called by transformation notebooks using %run.

Method Used: Service Principal

Security: Integration with Azure Key Vault to fetch secrets, ensuring no hardcoded credentials.

**Step 2:** Silver Layer Transformations
Raw data from the Bronze Layer is cleaned and standardized. We processed 5 tables (e.g., Customers, Transactions, Branches, etc.) through the following logic:

Null Checks & Date Validations: Standardizing date formats and handling missing values.

Feature Engineering: Added a Timestamp column for auditing.

Data Integrity: Duplicate removal and column renaming for business readability.

Output: Managed Delta Tables in the Silver Layer.

**Step 3:** Sales Performance Analysis
A custom analysis notebook was created to process sales data:

Calculations: Derived Revenue and Profit from manual/ingested DataFrames.

## Insights:

Product Analysis: Identification of the highest cost per product.

Customer Analysis: Identification of the top customer by item quantity purchased.

Storage: Results saved to a dedicated Silver/Gold Delta Table.

**Step 4:** Gold Layer & SCD Type 1
For the dimensional data (e.g., Customer or Product master tables), we implemented Slowly Changing Dimension (SCD) Type 1.

Logic: Overwriting existing records with updated information to maintain the most current state of the data.

Output: Final curated tables in the Gold Layer, optimized for Power BI reporting.

## 👤 Author and Contributors
Bhim Sen
