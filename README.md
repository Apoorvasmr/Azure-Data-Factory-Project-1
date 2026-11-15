<img width="1024" height="765" alt="image" src="https://github.com/user-attachments/assets/ccc54fc6-46c7-4443-aad8-1ebc5683fabf" />

## 📌 Project Overview

This project implements a modern **Medallion Architecture** using **Azure Data Factory (ADF)** to orchestrate and automate data movement from multiple on-premises and cloud sources into **Azure Data Lake Storage Gen2 (ADLS Gen2)**, featuring robust CI/CD and automated monitoring.

### 🔌 Multi-Source Data Ingestion
* **On-Premises Files**: Securely ingested using a **Self-Hosted Integration Runtime (SHIR)** to handle local directory data. 
* **REST APIs / GitHub**: Automated retrieval of JSON datasets using ADF's native HTTP and **Web Activities**.
* **Azure SQL Database**: Real-time business data migration. Implemented **Incremental Data Loading (Watermarking)** via metadata validation (stored in JSON files on ADLS Gen2) rather than legacy watermark database tables.

### 🏅 Medallion Lakehouse Architecture (ADLS Gen2)
* **Bronze (Raw)**: Captures and dumps unaltered data in its native formats (CSV, JSON).
* **Silver (Refined)**: Transformed and schema-enforced into the **Delta Lake** format using ADF **Mapping Data Flows** (e.g., lowercase conversion, derived columns, regex replacement, and custom splits). Rows are incrementally updated or inserted using `upsert` logic.
* **Gold (Business Views)**: Final aggregated datasets and performance rankings (e.g., generating high-value business metrics using `Window` and `Rank` functions) to enable direct downstream analytics.

### 🛡️ Enterprise DevOps & Automation
* **Version Control & Collaboration**: Full **Azure DevOps (ADO)** Git integration. Development is isolated in feature branches and merged via Pull Requests into the `main` branch. 
* **Infrastructure as Code**: Publishing to ADF automatically generates **ARM (Azure Resource Manager) templates** in the `adf_publish` branch for continuous deployment.
* **Orchestration**: End-to-end automation managed via a parent-child pipeline pattern using Schedule Triggers.

### 🚨 Real-time Monitoring & Alerting
* Custom failure paths trigger **Azure Logic Apps** via HTTP POST, sending email alerts with dynamic telemetry (pipeline name, run ID, and exact error message) to ensure immediate operational response.
