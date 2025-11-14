# 📘 Azure Data Factory for Data Engineers — Covid-19 Analytics Project

This repository contains a full **end-to-end Azure Data Engineering project** built around real Covid-19 datasets from **ECDC (European Centre for Disease Prevention and Control)** and **Eurostat**.

The project demonstrates real-world enterprise data engineering skills:

- Designing & orchestrating data pipelines
- Ingesting data from HTTP APIs & Blob Storage
- Applying transformations using ADF Mapping Data Flows & Databricks
- Creating analytics-ready star schema tables in Azure SQL
- Building a Power BI report for Covid-19 insights
- Implementing scheduled pipelines, validations, monitoring, and alerts

---

# 🏗️ Solution Architecture

![Covid-19 Architecture](assets/Untitled%20Diagram-Page-6.drawio.png)

---

# 📁 Repository Structure
azure-data-factory-covid19-project/
│
├── assets/                             # Architecture diagrams & pipeline screenshots
│
├── databricks_notebooks/
│   ├── nb_transform_population_data.ipynb
│   └── nb_transform_testing_data.ipynb
│
├── data/
│   ├── input_data/
│   │   ├── ecdc_data/
│   │   └── eurostat_data/
│   │
│   ├── lookup_data/
│   │   ├── country_lookup.csv
│   │   └── dim_date.csv
│   │
│   ├── raw/
│   │   └── ecdc_data/
│   │
│   ├── processed/
│   │   ├── ecdc/
│   │   └── population/
│   │
│   ├── sql_scripts/
│       └── create_covid_tables_ddl.sql
│
├── power_bi_reports/
│   └── Covid-19 Trends By Country.pbix
│
└── README.md

---

# 1️⃣ Environment Setup

To run this project end-to-end, the following Azure resources were provisioned:

---

### 🔹 Azure Storage Account (Blob + ADLS Gen2)

- Enabled **hierarchical namespace**
- Used for:
  - `/input_data` (raw files before ingestion)
  - `/raw` (ADF ingestion zone)
  - `/processed` (transformed zone)
  - `/lookup_data` (reference files)

---

### 🔹 Azure Data Factory

Acts as the **orchestration engine**, responsible for:

- Ingesting ECDC & Eurostat files
- Executing Databricks notebooks
- Running Mapping Data Flows
- Loading data into SQL DB
- Pipeline scheduling & monitoring

**ADF Pipelines:**  
![Pipelines](assets/Screenshot%202025-11-14%20220923.png)

---

### 🔹 Azure Databricks

Used for transformations that require heavy compute:

- Parsing population dataset
- Cleaning testing dataset
- Creating aggregated age groups
- Writing curated outputs to ADLS

![Databricks pipeline activity](assets/Screenshot%202025-11-14%20221008.png)

---

### 🔹 Azure SQL Database

Serves as the **analytics database** supporting Power BI dashboards. Tables include:

- `dim_country`
- `dim_date`
- `fact_cases_deaths`
- `fact_hospital_admissions_daily`
- `fact_hospital_admissions_weekly`
- `fact_testing`
- `dim_population_age_group`

SQL DDL script located here:  
`/data/sql_scripts/create_covid_tables_ddl.sql`

---

### 🔹 Azure Key Vault

Securely stores:

- Service Principal credentials
- Storage account keys
- SQL credentials

---

# 2️⃣ Raw Data Ingestion Pipelines

---

## ✔ Ingest ECDC Data (HTTP → ADLS Gen2)

ADF dynamically retrieves all available CSV files using:

- **Lookup**
- **ForEach**
- **Copy Activity**

![ECDC pipeline](assets/Screenshot%202025-11-14%20221208.png)

---

## ✔ Ingest Population Data (Blob → ADLS Gen2)

Pipeline performs:

- File existence check
- Metadata validation
- Copy to raw zone
- Delete source file if valid

![Population pipeline](assets/Screenshot%202025-11-14%20231740.png)

---

# 3️⃣ Data Transformation (ETL/ELT)

---

## 📌 ADF Mapping Data Flows

### 🔹 Transform Hospital Admissions

Creates both **Daily** and **Weekly** datasets:

![Hospital Data Flow](assets/Screenshot%202025-11-14%20221122.png)

Includes:

- Pivot
- Aggregations
- Join with country lookup
- Weekly rollups

---

### 🔹 Transform Cases & Deaths

![Cases Data Flow](assets/Screenshot%202025-11-14%20221152.png)

Steps include:

- Filter to European region
- Pivot indicators
- Lookup country metadata
- Output curated dataset

---

## 📌 Databricks Transformations

Stored in `/databricks_notebooks/`

---

### 🔹 Population Transformation Notebook

`nb_transform_population_data.ipynb`

Key steps:

- Load `.tsv.gz`
- Unpivot mixed columns
- Map geo codes
- Aggregate to age groups
- Write processed output

---

### 🔹 Testing Data Notebook

`nb_transform_testing_data.ipynb`

- Clean daily testing data
- Add ISO week numbers
- Join dim tables
- Write curated datasets

---

# 4️⃣ Loading Processed Data into SQL

Each fact/dimension load is handled by ADF using simple Copy Activities:

### ✔ Example: Hospital Admissions (Daily)

![SQL hospital load](assets/Screenshot%202025-11-14%20232705.png)

### ✔ Example: Cases & Deaths

![SQL cases load](assets/Screenshot%202025-11-14%20232646.png)

### ✔ Example: Testing Data

![SQL testing load](assets/Screenshot%202025-11-14%20232625.png)

---

# 5️⃣ Orchestration & Automation

A master pipeline runs all ingestion and transformations:

- Input ingestion
- Transformations (ADF Data Flows + Databricks)
- SQL load
- Power BI refresh trigger (optional)

### ✔ Trigger

Pipeline is scheduled using:

- **Daily Schedule Trigger**
- **Tumbling Window Trigger (for backfills)**

---

# 6️⃣ Monitoring & Alerting

### ✔ ADF Monitor

- Pipeline run views
- Activity failures
- Trigger history

### ✔ Log Analytics + Alerts

Alerts configured for:

- Pipeline failures
- Pipeline duration threshold
- Missing files

---

## 7️⃣ Power BI Dashboard – Covid-19 Trends

Located in:

`power_bi_reports/Covid-19 Trends By Country.pbix`

### **Dashboard Includes:**

- Country-wise Covid-19 spread  
- Daily & weekly cases  
- ICU & hospital occupancy  
- Testing volume & positivity rate  
- Age-group impact analytics  
- Multi-country comparison  

---

## 📊 End-to-End Project Flow (Summary)

### ✔ **Extract**

- HTTP API → ADLS  
- Blob Storage → ADLS  

### ✔ **Validate**

- File existence  
- Column count check  
- Metadata checks  

### ✔ **Transform**

- ADF Data Flows → pivot, aggregate, filter  
- Databricks → heavy transformations  

### ✔ **Load**

- ADLS → Azure SQL Database (Star Schema)  

### ✔ **Visualize**

- Azure SQL → Power BI Dashboards  

