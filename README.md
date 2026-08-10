
# 🏥 Healthcare Lab Results: Data Cleaning and Standardization Pipeline

## 📌 Project Overview
This project focuses on cleaning, standardizing, and mathematically transforming a highly unstructured healthcare dataset aggregated from multiple hospital systems. The objective is to prepare chaotic, contradictory lab results into a mathematically sound, clinically accurate dataset ready for advanced medical analytics and dashboarding.

## 📊 Dataset Origin & Context
* **Source:** downloaded from kaggle ([https://www.kaggle.com/datasets/nudratabbas/multi-hospital-lab-results-messy-data/data])

* **Initial Structure:** The raw dataset consisted of **[Insert row count, e.g., 1,000]** rows and **7** columns (`hospital`, `patient_id`, `test_name`, `test_value`, `unit`, `reference_range`, `collection_date`).
## 🧰 Tools Used
* **Microsoft Power Query / Excel:** Data Profiling, Conditional Logic (M Code), Type Conversion, and Structural Cleaning.

## ⚠️ The "Dirty Data" Challenge 
During the initial data profiling phase, several critical inconsistencies were identified:

1. **Measurement Unit Conflicts:** The dataset mixed international standards (`mmol/L`) with US standards (`mg/dL`) within the exact same column, rendering aggregate statistics invalid.

<img width="1327" height="688" alt="Screenshot 2026-08-09 182352" src="https://github.com/user-attachments/assets/c39227ec-de3e-4ae3-bac5-b50ac46ebfcd" />

2. **Sentinel Values in Numeric Columns:** Missing data was recorded as `-999` or explicit text (`not collected`, `N/A`), forcing numeric columns into text data types.

<img width="1334" height="699" alt="Screenshot 2026-08-09 182317" src="https://github.com/user-attachments/assets/8115acbe-0aaa-40f1-a899-19244ad7708a" />


3. **Inconsistent Nomenclature:** Identical medical tests were logged under different colloquial and clinical names depending on the hospital.

<img width="1331" height="687" alt="Screenshot 2026-08-09 182221" src="https://github.com/user-attachments/assets/72da9f32-a7cb-4105-a856-5a42f1b86f46" />


4. **Contradictory Reference Ranges:** Row-level discrepancies occurred where a test value was recorded in one measurement unit, but the reference range provided was for a completely different scale. For instance, a glucose test value recorded in `mmol/L` was incorrectly paired with a reference range of `70-99` (the `mg/dL` standard), which would algorithmically flag a perfectly healthy patient as critically abnormal.

## 🛠️ Step-by-Step Cleaning & Transformation Log

### Step 1: Initial Data Inspection & Profiling
Before applying any transformations, the raw data was systematically scoped to establish a baseline and identify structural anomalies.
* Activated Power Query's native profiling tools (**Column Quality**, **Column Distribution**, and **Column Profile**).
* Scanned the dataset structure to verify the total row count and review the initial data types assigned to each column.
* Analyzed the value distribution charts, which immediately exposed the critical issues: severe naming fragmentation in `test_name`, contradictory formats in `reference_range`, and the presence of text strings/sentinel values in the numeric `test_value` column.

### Step 2: Data Integrity & Duplicate Verification
* **Duplicate Check:** A strict check for exact row duplicates was done. Using column distribution profiling, the dataset was verified to contain zero identical rows. This confirmed that recurring `patient_id` numbers were legitimate clinical events (representing panel tests run on the same sample or subsequent follow-up visits) rather than data entry errors.
* **Identifier Standardization:** Converted `patient_id` from a numeric format to a Text data type to prevent accidental mathematical aggregations.

### Step 3: Standardizing Clinical Terminology
* Utilized the **Conditional Column** feature to build `IF/THEN` logic, unifying inconsistent text entries into standard clinical terms.
    * Mapped `Blood Sugar` and `Fasting Glucose` -> `Glucose`
    * Mapped `Total Chol` and `Serum Cholesterol` -> `Cholesterol`
    * Mapped `Glycated Hemoglobin` -> `Hemoglobin A1c`

<img width="902" height="526" alt="Screenshot 2026-08-09 184723" src="https://github.com/user-attachments/assets/eb9de7f2-3d46-4349-afe6-9d0209cbf118" />

### Step 4: Numeric Cleaning & Sentinel Value Handling
* Forced a Decimal data type conversion on the `test_value` column to intentionally trigger `Error` values for text strings like `N/A`.
* Replaced all generated errors and the `-999` sentinel values with true `null` (empty) values to restore the mathematical integrity of the column.

### Step 5: Mathematical Unit Standardization (Localization)
* **Conversion to SI Standard:** Standardized the dataset to the Nigerian / International standard (`mmol/L`). 
* Generated a **Custom Column** using M Code (`if/then/else` statements) to isolate values recorded in `mg/dL` and apply the exact molecular weight division factor without altering the existing `mmol/L` or `%` values:
    * `Glucose`: Divided by 18
    * `Cholesterol`: Divided by 38.67
* Rounded the final unified `mmol/L` values to 2 decimal places to meet clinical reporting standards.
* Replaced all instances of `mg/dL` in the `unit` column to accurately reflect the transformation.

<img width="1318" height="675" alt="Screenshot 2026-08-10 101328" src="https://github.com/user-attachments/assets/98777c3d-5501-47f3-902a-f2655816abdc" />

### Step 6: Reference Range Optimization
* **Master Overwrite:** Created a Conditional Column to overwrite the chaotic, mixed-format metadata (e.g., `Normal: <100`) with standardized `mmol/L` minimum and maximum thresholds based on the specific test name.
* **Isolating Variables (Splitting Columns):** Split the newly standardized reference ranges using a hyphen delimiter to create two distinct, independent numeric columns (`min_reference_range` and `max_reference_range`). 
* **The Structural Logic (Why this was done):** Analytical software cannot perform mathematical evaluations on a text string like `3.9-5.5`. By isolating the upper and lower boundaries into dedicated Decimal data types, the dataset is now structurally optimized for programmatic logic (e.g., `IF test_value > ref_max THEN 'High Risk'`) in future dashboarding or machine learning phases.
* Dropped redundant and obsolete original columns to optimize database size.

<img width="1363" height="700" alt="Screenshot 2026-08-09 204939" src="https://github.com/user-attachments/assets/15e32f39-40e5-44c2-bf1e-f762c9b92531" />



<img width="1358" height="705" alt="image" src="https://github.com/user-attachments/assets/a076d32e-7aca-4520-a7f4-20c4fed0a896" />


## 🧠 Key Clinical Logic Decision: Preserving Null Values
After standardizing the `test_value` column, rows containing `null` values were deliberately kept in the dataset rather than deleted. 

In a healthcare environment, a missing lab result is a valid and crucial medical event (e.g., a spoiled blood sample, machine failure, or a patient missing their appointment). Deleting rows containing a `null` test value would completely erase that patient's visit record, hospital origin, and test request history. Preserving these rows maintains the integrity of the total patient count and administrative records.
