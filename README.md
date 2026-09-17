# 🏥 Multi-Hospital Laboratory Dataset: Dual-Tool Cleaning Pipeline

## 📖 Project Overview
This project demonstrates a comprehensive data engineering and cleaning workflow applied to a messy, unstructured healthcare dataset containing laboratory results from multiple hospitals. 

To showcase versatility in data transformation, this repository features the exact same cleaning logic executed through two distinct methodologies: a fully programmatic **Python** pipeline and a GUI-based **Power Query** pipeline. Both methods result in a mathematically pure, standardized dataset ready for clinical analysis, SQL querying, or BI dashboard integration.

---

## 🔀 The Dual-Tool Pipeline

### 🐍 Method 1: Python & Pandas (Programmatic Approach)
This approach utilizes a Jupyter Notebook to build a reproducible, vectorized data cleaning script. It relies on custom logical functions, NumPy masking, and automated data type conversions to process the dataset efficiently.
* **Core Tools:** Python, Pandas, NumPy, Jupyter Notebook
* **Key File:** `data_cleaning_pandas.ipynb`

*(Code Snippet: Final data validation and export)*
<img width="1212" height="530" alt="Screenshot 2026-09-17 180100" src="https://github.com/user-attachments/assets/1f0a7fac-fa2e-4bd2-a502-99671be20b8b" />


### 📊 Method 2: Excel & Power Query (GUI-Based Approach)
This approach utilizes Power Query to build a step-by-step graphical transformation pipeline. It is optimized for direct integration into Excel workbooks or Power BI dashboards, demonstrating proficiency in standard enterprise analytics tools.
* **Core Tools:** Microsoft Excel, Power Query
* **Key File:** `multi_hospital_lab_results.xlsx`

*(Power Query Editor: Applied steps and column profiling)*
<img width="1358" height="705" alt="medical_powerquery" src="https://github.com/user-attachments/assets/ef7c97a8-2d38-4838-b1b9-bf7f2b968cf2" />


---

## 🧹 Universal Data Cleaning Methodology
Regardless of the technology stack utilized, the raw laboratory data required rigorous standardization. The following core transformations were applied in both pipelines:

1. **Standardizing Clinical Terminology:** 
   * Consolidated inconsistent and colloquial test names (e.g., *'Fasting Glucose'*, *'Blood Sugar'*) into unified, standard clinical terms (*'Glucose'*).
2. **Mathematical Unit Conversion:** 
   * Identified rows recorded in `mg/dL` and converted them to the standard `mmol/L` by applying test-specific molecular weights (dividing Glucose by 18; Cholesterol by 38.67).
3. **Handling Missing & Sentinel Values:** 
   * Coerced hidden text artifacts into nulls and replaced invalid `-999` placeholder values with `NaN` / `null` to prevent skewed statistical aggregations.
4. **Extracting Reference Ranges:** 
   * Split the unstructured `reference_range` string column (e.g., *'3.9-5.5'*) into two distinct, numeric `min_range` and `max_range` columns to allow for mathematical querying and conditional formatting.

---

## 📂 Repository Structure
* `raw_multi_hospital_lab_results.csv` — The original, uncleaned dataset.
* `data_cleaning_pandas.ipynb` — The Jupyter Notebook containing the documented Python pipeline.
* `cleaned_dataset_pandas.csv` — The final standardized dataset exported via Python.
* `multi_hospital_lab_results.xlsx` — The Excel workbook containing the Power Query pipeline and final clean table.

---

## 📬 Contact
**Ibrahim Yusuf**  
Data Analyst & Educator  
Email: ibrahimadeyemiyusuf@gmail.com
