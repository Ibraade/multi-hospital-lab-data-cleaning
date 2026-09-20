# 🏥 Multi-Hospital Laboratory Dataset: End-to-End Data Engineering & Visualization

## 📖 Project Overview
This project demonstrates a comprehensive data engineering and visualization workflow applied to a messy, unstructured healthcare dataset containing laboratory results from multiple hospitals. 

To showcase versatility across the modern data stack, this repository features a raw data cleaning pipeline executed through two distinct methodologies (Python/Pandas and Power Query), which then feeds into a multi-page interactive Power BI application designed for both clinical and operational stakeholders.

## 🔀 The End-to-End Pipeline

### 🐍 Phase 1A: Python & Pandas (Programmatic Cleaning)
This approach utilizes a Jupyter Notebook to build a reproducible, vectorized data cleaning script. It relies on custom logical functions, NumPy masking, and automated data type conversions to process the dataset efficiently.
* **Core Tools:** Python, Pandas, NumPy, Jupyter Notebook
* **Key File:** `data_cleaning_pandas.ipynb`
  <img width="1212" height="530" alt="Screenshot 2026-09-17 180100" src="https://github.com/user-attachments/assets/958449ac-5b1c-4f50-b06b-c00e280efeba" />


### 📊 Phase 1B: Excel & Power Query (GUI-Based Cleaning)
This approach utilizes Power Query to build a step-by-step graphical transformation pipeline. It demonstrates proficiency in standard enterprise analytics tools and produces a mathematically pure, standardized dataset.
* **Core Tools:** Microsoft Excel, Power Query
* **Key File:** `multi_hospital_lab_results.xlsx`
  <img width="1355" height="720" alt="Screenshot 2026-09-20 114649" src="https://github.com/user-attachments/assets/ab796aea-40c9-4895-8e7e-3f10c8ab691d" />


### 📈 Phase 2: Power BI (Clinical & Operations Suite)
To bridge the gap between raw data engineering and executive decision-making, the standardized dataset was deployed into an interactive Power BI report featuring a dual-page architecture:

**1. Hospital Operations & Workload Tracker (Executive View)**
Designed for hospital administrators to monitor testing volume, resource allocation, and seasonal trends across branches.
<img width="1213" height="514" alt="adminstrators_view" src="https://github.com/user-attachments/assets/1dbcab8d-2747-4645-b97a-feddcd0ff3d9" />


**2. Clinical Alert & Triage System (Clinical View)**
Designed for medical staff to instantly break down data silos, search cross-network patient history, and triage critical lab anomalies using automated DAX alert logic.
<img width="1217" height="498" alt="clinical_alert_view" src="https://github.com/user-attachments/assets/501df193-92ba-449f-a80c-f0508b86c353" />

* **Core Tools:** Power BI, DAX, Data Modeling
* **Key File:** `Hospital_Lab_Operations_Dashboard.pbix`

## 🧹 Universal Data Cleaning Methodology
Regardless of the technology stack utilized, the raw laboratory data required rigorous standardization before visualization. The following core transformations were applied in both pipelines:

* **Standardizing Clinical Terminology:** Consolidated inconsistent and colloquial test names (e.g., 'Fasting Glucose', 'Blood Sugar') into unified, standard clinical terms ('Glucose').
* **Mathematical Unit Conversion:** Identified rows recorded in mg/dL and converted them to the standard mmol/L by applying test-specific molecular weights (dividing Glucose by 18; Cholesterol by 38.67).
* **Handling Missing & Sentinel Values:** Coerced hidden text artifacts into nulls and replaced invalid `-999` placeholder values with `NaN` / `null` to prevent skewed statistical aggregations.
* **Extracting Reference Ranges:** Split the unstructured `reference_range` string column (e.g., '3.9-5.5') into two distinct, numeric `min_range` and `max_range` columns to allow for mathematical querying and conditional formatting.

## 📂 Repository Structure
```text
├── raw_multi_hospital_lab_results.csv             # The original, uncleaned dataset
├── data_cleaning_pandas.ipynb                     # Jupyter Notebook with Python pipeline
├── Cleaned_lab_data_with_pandas.csv               # Standardized dataset exported via Python
├── cleaned_lab_data_with_powerquery.xlsx          # Excel workbook containing Power Query pipeline
├── Hospital_Lab_Operations_Dashboard.pbix         # Interactive Power BI dashboard source file
├── administrators_view.png                        # Operations dashboard screenshot
└── clinical_alert_view.png                        # Clinical triage dashboard screenshot
