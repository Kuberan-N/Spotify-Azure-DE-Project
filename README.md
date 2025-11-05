# 🎧 Spotify Azure Data Engineering Project

A **comprehensive end-to-end Azure Data Engineering pipeline** implementing the **Medallion Architecture (Bronze → Silver → Gold)** using **Azure Data Factory (ADF)**, **Databricks**, **Delta Live Tables (DLT)**, and **Azure Data Lake Storage Gen2**.

## 🏗️ Architecture Overview
<p align="center">
  <img src="architecture/Azure_Data_Project_Architecture.png" width="820" />
</p>

## ⚙️ Tech Stack
| Component | Technology | Purpose |
|---|---|---|
| Orchestration | Azure Data Factory | Incremental ingestion & scheduling |
| Storage | ADLS Gen2 | Bronze, Silver, Gold zones |
| Processing | Azure Databricks (PySpark) | Transformations & Delta format |
| Streaming & Automation | LakeFlow + Autoloader | Dynamic, parameterized loads |
| Curation | Delta Live Tables (DLT) | Gold tables, SCD2, expectations |
| Security | Azure Key Vault | Secret management |
| Alerting | Logic Apps | Failure notifications |

## 🟤 Bronze Layer – Raw Ingestion (ADF)
- Incremental copy from **Azure SQL** to **ADLS Bronze** in **Parquet**
- **Watermark JSON** per table (CDC column tracked)
- Dynamic datasets and parameterized pipeline:
  - `PL_Master_Incremental_Ingestion_Loop` → ForEach → Lookup (watermark) → Copy → If (dataRead) → Update watermark / Delete empty
- Alerting: Logic App webhook on failure

👉 Details: [docs/01_BronzeLayer.md](docs/01_BronzeLayer.md)

## ⚪ Silver Layer – Transform (Databricks)
- **LakeFlow Job** + **Autoloader** in a **dynamic** (folder-driven) design
- A utility notebook receives `folder_name`, reads `/bronze/{folder}` via Autoloader, cleans/dedups with PySpark, writes **Delta** to `/silver/{folder}` and registers `silver.{folder}`

👉 Details: [docs/02_SilverLayer.md](docs/02_SilverLayer.md)

## 🟡 Gold Layer – Curated (DLT)
- **Delta Live Tables** pipeline with:
  - **Staging → Final** gold tables
  - **Append flow** for continuous loads
  - **SCD Type 2** (history) and **sequence by** for dedupe ordering
  - **Expectations** for data quality

👉 Details: [docs/03_GoldLayer.md](docs/03_GoldLayer.md)

## 📁 Data Lake Snapshot


## 🧱 Repo Structure


## ✅ Highlights
- Incremental CDC ingestion with watermark JSON
- Parameterized + reusable design end-to-end
- Delta format, schema evolution, expectations
- DLT SCD Type 2 with sequence-by dedupe
- Alerting & operational readiness

## 🔭 Future Enhancements
- Publish to **Synapse** or **Power BI**
- CI/CD with **GitHub Actions**
- Tests for PySpark transforms

**Author:** Kuberan Natarajan — [LinkedIn](https://www.linkedin.com/in/kuberan-n) • [GitHub](https://github.com/Kuberan-N)
