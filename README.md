# Azure Data Factory for Data Engineers – Covid-19 Analytics Project

This project showcases an **end-to-end Data Engineering project** built on **Azure Data Factory (ADF)** to design and orchestrate a scalable Covid-19 data platform.  
The project demonstrates real-world data ingestion, transformation, orchestration, and reporting workflows used by Data Engineers in production systems.

---

## 🚀 Project Overview

During the Covid-19 pandemic, global agencies required **timely, trusted, and analytics-ready data** to track trends such as:

- Case numbers & deaths  
- Hospital & ICU occupancy  
- Testing levels and positivity  
- Country responses  
- Population impact by age groups  

This project builds a modern **Azure Data Engineering pipeline** to ingest, clean, transform, and serve Covid-19 datasets using enterprise Azure tools.

---

## 🎯 Project Objectives

- Ingest Covid-19 data from **ECDC, Eurostat, and API sources**
- Store raw and curated data in **Azure Data Lake Storage Gen2**
- Transform data using:
  - **ADF Mapping Data Flows**
  - **Azure Databricks (PySpark)**
- Load curated data into **Azure SQL Database**  
- Build **Power BI dashboards** for analytics  
- Automate workflow execution with **ADF Pipelines & Triggers**  

---

## 🧱 High-Level Architecture



---

### Data Sources
- Covid-19 Cases & Deaths (ECDC)
- Hospital admissions, ICU occupancy (ECDC)
- Covid testing & positivity rates (ECDC)
- Country response indicators (ECDC)
- Population by age group (Eurostat)

### Azure Components Used
- **Azure Blob Storage**
- **Azure Data Lake Gen2**
- **Azure Data Factory**
- **Azure Databricks**
- **Azure SQL Database**
- **Azure Monitor & Log Analytics**

---

## 🧰 Tech Stack

- **Data Storage:** ADLS Gen2, Blob, SQL DB  
- **Orchestration:** Azure Data Factory  
- **Transformations:** Data Flows, PySpark (Databricks)
- **Compute:** Databricks Cluster,
- **Analytics:** Power BI  

---

## 📂 Repository Structure




---

## 1️⃣ Environment Setup

Provisioned Azure resources:

- Storage Account (Blob + ADLS Gen2)
- Azure Data Factory Instance
- Azure SQL Database
- Databricks Workspace + Cluster
- Key Vault for credential management

---

## 2️⃣ Raw Data Ingestion

### Population Data (Blob → ADLS Gen2)

- Source: `population_by_age.tsv.gz`
- ADF Copy Activity extracts GZip file into ADLS Raw Zone.
- Validates file structure and size.

### ECDC Data (HTTP → ADLS Gen2)

Pipeline ingests:

- Cases & Deaths  
- Hospital Admissions  
- Testing Data  
- Country Response Indicators  

Key Features:

- Reusable pipelines with parameters  
- HTTP Linked Service  
- Metadata-driven ingestion (ForEach + Lookup)  
- Writes to:  
  - raw/ecdc/
  - raw/population/

---

## 3️⃣ Transformations

### ✔ ADF Mapping Data Flows

#### a) Cases & Deaths
- Cleans dataset  
- Joins country reference data  
- Pivot indicators into columns  
- Writes curated output  

#### b) Hospital & ICU Occupancy
- Cleans raw data  
- Creates daily & weekly versions  
- Computes start/end week dates  
- Writes both datasets to curated zone  

---

### ✔ HDInsight Hive Transformations (Testing Data)
- Cleans testing data  
- Derives week information  
- Joins reference dimensions  
- Writes curated dataset  

---

### ✔ Databricks PySpark (Population Data)
Steps include:

1. Mount ADLS using service principal  
2. Read population TSV  
3. Split, clean, and reshape data  
4. Map geo codes → country names  
5. Aggregate populations into age groups  
6. Write curated output  

---

## 4️⃣ Loading into Azure SQL Database

Using ADF Copy Activities:

- Load curated data into star schema:
- `dim_country`
- `dim_calendar`
- `dim_population_age_group`
- `fact_cases_deaths`
- `fact_hospital_admissions_daily`
- `fact_hospital_admissions_weekly`
- `fact_testing`

This enables downstream BI reporting & analytics.

---

## 5️⃣ Orchestration & Scheduling

### Pipelines include:
- Ingestion Pipelines  
- Transformation Pipelines  
- Master Orchestration Pipeline  

### Triggers Used:
- **Schedule Trigger** (daily or weekly)  
- **Tumbling Window Trigger** (for backfill)  
- **Event Trigger** (file arrival-based)  

Dependencies managed using:

- Pipeline chaining  
- Activity dependencies  
- Failure policies  

---

## 6️⃣ Monitoring & Alerting

Monitoring implemented using:

### ✔ ADF Monitor
- Pipeline run history  
- Activity-level failures  
- Trigger health checks  

### ✔ Azure Monitor + Log Analytics
- Logs pipeline failures  
- Alerts on failure count thresholds  
- Custom dashboards for SLA tracking  

---

# 📊 Power BI Analytics

A Power BI Report (PBIX) is created using SQL DB as the source.

### Report Tabs Include:
- Overview Dashboard  
- Cases & Deaths Trend Analysis  
- Hospital vs ICU Occupancy  
- Testing & Positivity Rate  
- Country Comparison  
- Age Group Distribution  

This makes insights accessible to leadership, analysts, and decision-makers.

---

## 📬 Contact

**Name:** Rishikesh Gundla  
**LinkedIn:** https://linkedin.com/in/rishikeshgundla 

---

